+++
title = "Designing Alerts for SLOs"
date = 2024-07-12
author = "Carlos R.F."
+++

This burn rate method was originally explained in detail in the Google SRE Workbook. Here, I describe it in a slightly different way, hoping to make it easier to understand.

* $R_t$ : reliability target, e.g., 0.99 (availability or latency)
* $R_p$ : reliability period, e.g., 30 days
* $m$ : target misses rate, e.g., 0.04. It is the error rate in case of availability or the requests taking longer than the target in case of latency.
* $M=1-R_t$: misses-budget
* $t_b$ : time-to-burn all the misses budget

$$
t_b = \frac{M}{m} \times R_p
$$

How long does it take to burn all the error budget at a 0.002 error rate for an availability target of 0.999 (99.9%) ?


$$
t_b = \frac{1-0.999}{0.002} \times 30d
= 15d
$$

In this scenario, for example, if we see a consistent error rate of 0.02 in a 3-day period, we can be alerted that if we continue with such a rate, in 12 days ($15d-3d$) we would be falling from the 99.9% target.

To simplify the math, let's define $B$ as the burn rate at which the misses budget is burned. For example, for an availability of 0.999, the error budget is 0.001, so an error rate of 0.002 is a burn rate of 2.

$$B = \frac{m}{M}$$

We can then put the burn-rate in function of time-to-burn (substituting $m$):


$$
B = \frac{\frac{M}{t_b} \times Rp}{M}
 = \frac{M \times Rp }{t_b \times M}
 = \frac{Rp}{t_b}
$$

With this in mind, let's define alerts that can be used for different availability requirements and are based on $B$, the burn rate.
The alerts can be defined according to existing categories, e.g., routine, urgent, and critical, and for a 30-day period.

|$B$|$t_b$|When to alert|Time left after alert to fix|Ticket Type|
|---|---|---|---|---|
|1|30d|3d|27d|Routine|
|6|5d|1d|4d|Urgent|
|30|1d|6h|18h|Critical|

Now, let's see how those rates apply to different reliability requirements. If our $R_t$ is 99.5%, then our $M$ is 0.5% ($M=1-R_t$), and we are defining the urgent alert ($B=6$), then our maximum error rate for a period of $1d$ before alerting would be the following:

$$m \geq 6 \times 0.005$$

If the error rate is greater, we can send an alert indicating that we are depleting the budget to the point where we won't meet the 99.5% availability of the 30-day period in 4 days. The $m$ is exactly $6 \times 0.005 = 0.03$, or 3% error rate in the last 24 hours.

How would this apply to latency targets? If we have a reliability target that 90% of requests must be 100 ms or less, then the $R_t$ is 0.9, and $M$ is 0.1. The urgent alert ($B=6) would then trigger when we observe a misses rate over the period of $1d$ like the following:

$$m \geq 6 \times 0.1$$

The general alert formula is:

$$m \geq B \times M$$

With Prometheus, that urgent alert for availability would look like this:

```
sum(rate(http_request_duration_seconds_count{code=~"5.."}[1d])) by (app)
/
sum(rate(http_request_duration_seconds_count[1d])) by (app)
  >= 6*0.005
```

If the service has a different availability target, then the only thing we would need to change is the $0.005$ to the corresponding $M$ for such a target.

In the case of latency, it would be like this for the target of 90% of requests at 100 ms or less response time:

```
sum(
  rate(http_request_duration_seconds_count{code!~"5.."}[1d])
  -rate(http_request_duration_seconds_bucket{code!~"5..", le="0.1"}[1d])
  ) by (app)
/
sum(rate(http_request_duration_seconds_count{code!~"5.."}[1d])) by (app)
  >= 6*0.1
```

To get the expressions for the other burn rates, we simply replace the $6$ with the desired $B$, and adjust the period (the `[1d]`) accordingly
