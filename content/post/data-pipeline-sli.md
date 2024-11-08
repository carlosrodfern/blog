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


Those metrics are indeed very user-impact-driven. They help engineers quickly see the real impact of data going stale on users. For instance, freshness encompasses not only how quickly data changes and syncs into the final storage accessible to the user but also the number of users who query and use the out-of-sync data. Implementing some of these goals necessitates a substantial effort, such as modifying the system to precalculate metrics, which enables efficient detection of stale data in user queries. Nevertheless, whatever the end SLI is, it should be somewhat interpretable in a way that reflects with some accuracy what the users actually experience.

The Queuing Theory model presents another potential framework that can be used for the SLI design. In this model, each job can be a queuing system. The queuing system receives data processing requests that are put into a queue, such as Kafka or RabbitMQ, and uses servers to pull and service them. These servers are threads within the microservices belonging to the same deployment unit.

![](/blog/img/queuing-sys-model.drawio.png)

The waiting time is the duration a request spends in the queue, while the service time is the duration a server spends processing the request. The completion time is defined as the total time it takes to process the request and send the results. It is the difference between arrival time and departure time.

With this model in mind, we can set up a completion time counter metric that measures the number of requests and the total completion time. This can serve as an SLI. Additionally, we use the waiting time and service time counters separate from SLIs to assist with troubleshooting.

How does this translate to the user's experience? Engineers typically comprehend the most frequently used parts of the app and the consequences of outdated data. This will allow the engineers to classify services into SLO classes based on the completion time average. They could determine SLO classes like "near-realtime,"  "1-day,"  "4-days,"  etc. With this approach, engineers cannot yet measure the exact percentage of users that queried a specific set of staled data; however, they can intuitively understand the impact of staled data by feature based on generally known user usage patterns and known internal system dependencies.
