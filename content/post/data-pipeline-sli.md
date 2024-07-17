+++
title = "Defining SLI for Data Pipelines"
date = 2024-07-12
author = "Carlos R.F."
+++

Data Pipelines can be thought as an arrangement of jobs that recieve data processing requests.

The SRE workbook presents the following possible SLIs to measure in a Pipeline

|Metric|Description|
|------|-----------|
|Freshness|The proportion of the data that was updated more recently than some time threshold. Ideally this metric counts how many times a user accessed the data, so that it most accurately reflects the user experience.|
|Correctness|The proportion of records coming into the pipeline that resulted in the correct value coming out.|
|Coverage| For batch processing, the proportion of jobs that processed above some target amount of data. For streaming processing, the proportion of incoming records that were successfully processed within some time window.|

Those metrics are indeed very user-impact-driven. They help to quickly see the real impact of data going stale on users.
For example, freshness is not only about the difference between data changing, and getting synced into the final storage available to the user to query, but more specifically, how many users queried and used the out of sync data. Some of those goals require a significant effort to implement, e.g., by means of changing the system to be precalculating the metrics, to the point that it is efficient to know if the data that was queried by a user was stale. Nevertheless, whatever the end SLI is, should be somewhat interpretable in a way that reflects with some accuracy what the users actually experience.

Another possible model that can be used for SLI design is the one from Queuing Theory. In this model, each job can be then a queuing system.

The queuing system has a queue where data processing requests are received and servers that pull the requests and service them. These servers are threads within the microservices belonging to the same deployment unit.

![](/blog/img/queuing-sys-model.drawio.png)

The time the request is in the queue is the waiting time, the time the request is being serviced is the service time. The completion time is the total time it takes to process the request and send the results. It is the difference between the arrival time and the departure time.

With this model in mind, we can set up a completion time counter metric that measure the number of requests and the total completion time. This can serve as the SLI. Also, not as part of the SLI but just for further troubleshooting, we can make the waiting time and the service time also counters.

How does this translate to users experience? In most cases, engineers understand what parts of the app are used the most, and the impact of having staled data. This will allow the engineers to classify functionality into SLO classes based on the completion time average. They could determine SLO classes like "near-realtime", "1-day", "4-days", etc.... With this approach, engineers cannot measure yet the exact percentage of users that queried a specific set of staled data, however, they can intuitively understand the impact of staled data by feature based on generally known user usage patterns and known internal system dependencies.
