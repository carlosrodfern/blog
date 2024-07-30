+++
title = "CentOS Blog / Managing Internal CI Tests with TMT for CentOS Stream Updates"
date = 2024-01-23
author = "Carlos R.F."
+++

[Link to the article on blog.centos.org](https://blog.centos.org/2024/01/managing-internal-ci-tests-with-tmt-for-centos-stream-updates/)

The article presents the work I did at HealthTrio which demonstrates how to implement safe updates for a Linux distro that behaves like a rolling distro within the major releases. The approach simply involves taking snapshots of repositories at a scheduled time, making them available for internal CI testing, applying them to canary machines, and finally applying them to the rest.

An improved version of it would be using the [compose ID](https://composes.stream.centos.org/production/) concept found in EPEL and CentOS Stream repositories. While the testing of a compose ID is implicit in the snapshot-in-time approach, a step forward would be to explicitly point to such a compose ID as the target. Facebook moved away from snapshot-in-time to compose-id-centric approach and presented their work and benefits in [this video](https://www.youtube.com/watch?v=20iZEJFARZs).
