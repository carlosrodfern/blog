+++
title = "Designing Alerts on an Availability SLO"
date = 2024-07-12
author = "Carlos R.F."
+++

The Google SRE Workbook originally explains this burn rate method in detail. Here, I describe it in a little bit different way, hoping to make it more understandable.

* $A_t$ : Availability target, e.g., 0.99
* $A_p$ : Availability period, e.g., 30 days
* $e$ : Error rate, e.g., 0.04
* $E=1-A_t$: Error budget
* $t_b$ : Time to burn all the error budget

$$
t_b = \frac{E}{e} \times A_p
$$

How long does it take to burn all the error budget at a 0.002 error rate for an availability target of 0.999 (99.9%) ?


$$
t_b = \frac{1-0.999}{0.002} \times 30d
= 15d
$$

In this scenario, for example, if we see a consistent error rate of 0.02 in a 3-day period, we can be alerted that if we continue with such a rate, in 12 days ($15d-3d$) we would be falling from the 99.9% target.

To simplify the math, let's define $B$ as the burn rate at which the error budget is burned. For example, for an availability of 0.999, the error budget is 0.001, so an error rate of 0.002 is a burn rate of 2.

$$B = \frac{e}{E}$$

We can then put the burn-rate in function of time to burn (substituting $e$):


$$
B = \frac{\frac{E}{t_b} \times Ap}{E}
 = \frac{E \times Ap }{t_b \times E}
 = \frac{Ap}{t_b}
$$

With this in mind, let's define alerts that can be used for different availability requirements and are based on $B$, the burn rate.
The alerts can be defined according to existing categories, e.g., routine, urgent, and critical, and for a 30-day period.

|$B$|$t_b$|When to alert|Time left after alert to fix|Ticket Type|
|---|---|---|---|---|
|1|30d|3d|27d|Routine|
|6|5d|1d|4d|Urgent|
|30|1d|6h|18h|Critical|

Now, let's see how those rates apply to different availability requirements. If our $A_t$ is 99.5%, then our $E$ is 0.5% ($E=1-A_t$), and we are defining the urgent alert ($B=6$), then, if we observe an error rate like:

$$e \geq 6 \times 0.005$$

...for a period of $1d$, then we alert that we are burning the budget to the point that in 4 days we won't meet the 99.5% availability of the 30 days. That is exactly $6 \times 0.005 = 0.03$, or 3% error rate in the last 24 hours.

The general alert formula is:

$$e \geq B \times E$$

With Prometheus, that urgent alert would look like this:

```
sum(
    delta(http_request_duration_seconds_count{code=~"5.."}[1d])
    /
    delta(http_request_duration_seconds_count[1d])
) by (app)
  >= 6*0.005
```

The routine alert would be:

```
sum(
    delta(http_request_duration_seconds_count{code=~"5.."}[3d])
    /
    delta(http_request_duration_seconds_count[3d])
) by (app)
  >= 1*0.005
```

And the critical would be:

```
sum(
    delta(http_request_duration_seconds_count{code=~"5.."}[6h])
    /
    delta(http_request_duration_seconds_count[6h])
) by (app)
  >= 30*0.005
```

If the service has a different availability target, then the only thing we would need to change is the $0.005$ to the corresponding $E$ for such a target.
