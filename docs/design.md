# Design

![](../.gitbook/assets/foremastarchitecture%20%282%29.png)

### Foremast-AI-API Service

Foremast-AI-API service offers several internal APIs that interacts with the underly Data Store, publish the request message to message bus, retrieve the request status health or un-health result to Foremast-Barrelmn, non Foremast client or UI. Foremast-AI-API can serve the requests from multiple clusters. 

Foremast-AI-API service can retrieve config from data store and trigger the re-occurring request.

In the future release Foremast-AI-API service will  also monitor and  schedule   the tasks based on request status.

### Foremast-AI-Engine

Foremast-AI-Engine is consumer of the message. It can scale to multiple consumers. 

Based on the configuration it will first query the historical metric from metric store, compute the machine learning/statistic algorithm model,  for canary pre-deployment stage it will query the baseline and current metric and perform pairwise algorithm to check if both have same distribution pattern,  if current and baseline has different distribution pattern, it will be lower threshold. and then use threshold to detect current anomaly data points based on  historical mode .

### Scale and Fault Tolerant

we can add more Foremast-AI-Engine pod to scale

If there is any request is processed more than X minute \(configurable\), other Foremast-AI-Engine will take over and reprocess the request.

### Monitoring/Alerting

We leverage ElasticSearch as datastore to store the request content and status. Foremast-Ai-Engine will not only update the request status but also provide reason.

You can locate the request status and detail via Kibana dashboard. You can also config alert.

### Algorithms 

For v1.0 we will pick different default algorithm base one number of metric types. User can overwrite the default algorithm via the environment variables.  

For future release we will introduce model advisory component.  If user does not define the algorithm , model advisory will periodically evaluate different algorithms and pick the best one. 

#### One metric Types:

Moving Average

Exponential Smoothing

Double Exponential Smoothing

Holt-Winters

Prophet \(facebook\)

#### Two metric Types:

Bivariate Normal Distribution

#### Three or more metric Types:

Deep Learning \(LSTM\)

#### We also support pairwise algorithm  \(baseline vs current\)

**Compare Two:**

Mann-Whitney , Wilcoxon, Kruskal  

**Compare Two more :**

 Kruskal  

Fried manchi square \(special case\)













