+++
title = "Designing Alerts for SLOs"
date = 2024-07-12
author = "Carlos R.F."
+++

The burn rate method was originally explained in the Google SRE Workbook. Here, I describe it in a slightly different way, hoping to make it easier to understand.

* \\(R_t\\) : reliability target, e.g., `0.99` (availability or latency)
* \\(R_p\\) : reliability period, e.g., 30-day (`30d`)
* \\(m\\) : target misses rate, e.g., `0.04`. It is the error rate in case of availability or the rate of requests taking longer than the target in case of latency.
* \\(M=1-R_t\\): misses budget
* \\(t_b\\) : time-to-burn all the misses budget

Let \\(m\\) be:

* \\(t\\) : time, now
* \\(p\\) : period, e.g. `1d` (1 day)
* \\(m_{t}\\) : current misses count
* \\(m_{t-p}\\) : misses count \\(p\\) time ago.
* \\(r_{t}\\) : current request count
* \\(r_{t-p}\\) : request count \\(p\\) time ago.

$$
m = \frac{m_{t}-m_{t-p}}{r_{t}-r_{t-p}}
$$

Let \\(t_b\\) be:

$$
t_b = \frac{M}{m} \times R_p
$$



How long does it take to burn all the error budget at a `0.002` error rate for an availability target of `0.999` (99.9%) ?


\\begin{align}
t_b &= \frac{M}{m} \times R_p \\\\
&= \frac{1-R_t}{m} \times R_p \\\\
&= \frac{1-0.999}{0.002} \times 30d \\\\
&= 15d
\\end{align}

In this scenario, for example, if we see a consistent error rate of `0.002` in a 3-day period, we can be alerted that if we continue with such a rate, we would be falling from the 99.9% target in 12 days (\\(15d-3d\\)).

To simplify the math, let's define \\(B\\) as the burn rate at which the misses budget is burned. For example, for an availability of `0.999`, the error budget is `0.001`, so an error rate of `0.002` is a burn rate of `2`.

$$B = \frac{m}{M}$$

We can then put the burn rate in function of time-to-burn (substituting \\(m\\)):


\\begin{align}
B &= \frac{\frac{M}{t_b} \times Rp}{M} \\\\
 &= \frac{M \times Rp }{t_b \times M} \\\\
 &= \frac{Rp}{t_b}
\\end{align}

With this in mind, let's define alerts that can be used for different availability requirements and are based on \\(B\\).
The alerts can be defined according to existing categories, e.g., routine, urgent, and critical, and for a 30-day period.

|\\(B\\)|\\(t_b\\)|Evaluation period|Time left to fix|Ticket type|
|---|---|---|---|---|
|`1`|`30d`|`3d`|`27d`|Routine|
|`6`|`5d`|`1d`|`4d`|Urgent|
|`30`|`1d`|`6h`|`18h`|Critical|

The evaluation period is the time frame you will use to calculate \\(m\\) early to detect if there is an issue before \\(t_b\\) arrives.

Now, let's see how those rates apply to different reliability requirements. If our \\(R_t\\) is 99.5%, then our \\(M\\) is 0.5% (\\(M=1-R_t\\)). If we are defining the urgent alert (\\(B=6\\)), then our maximum error rate for a period of `1d` before alerting would be:

$$m \geq 6 \times 0.005 = 0.03$$

If the error rate in the evaluation period of `1d` is greater than `0.03`, we can send an alert indicating that we are depleting the budget in `4d`, because the \\(t_b\\) is `5d`.

How would this apply to latency targets? If we have a reliability target of 90% of requests with a latency of 100 ms or less, then the \\(R_t\\) is `0.9`, and \\(M\\) is `0.1`. The urgent alert (\\(B=6\\)) would trigger with a misses rate of `0.6` (\\(6 \times 0.1\\)) over a period of `1d`:


The general alert formula is:

$$m \geq B \times M$$

Essentially, the \\(B\\), which is \\(\frac{Rp}{t_b}\\), is chosen together with an evaluation period taking into account how long you want to have to fix the issue before the 30-day misses budget depletion happens at \\(t_b\\).

The urgent alert for availability would look like this in Prometheus:

```
sum(rate(http_request_duration_seconds_count{code=~"5.."}[1d])) by (app)
/
sum(rate(http_request_duration_seconds_count[1d])) by (app)
  >= 6*0.005
```

If the service has a different availability target, then the only thing we would need to change is the `0.005` to the corresponding \\(M\\).

In the case of latency, it would be like this for the target of 90% of requests with a latency of 100 ms or less:

```
sum(
  rate(http_request_duration_seconds_count{code!~"5.."}[1d])
  -rate(http_request_duration_seconds_bucket{code!~"5..", le="0.1"}[1d])
  ) by (app)
/
sum(rate(http_request_duration_seconds_count{code!~"5.."}[1d])) by (app)
  >= 6*0.1
```

To get the expressions for the other burn rates, we simply replace the `6` with the desired \\(B\\), and adjust the period (the `[1d]`) accordingly.
