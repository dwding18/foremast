# Core Components

## 

## Foremast-Barrelman

## Foremast-Judgement Framework

![Foremast-Judgement Framework Sequence Diagram](../.gitbook/assets/foremastjudgementsequencediagram.png)

### Foremast-AI-API

Formast-AI-API provide internal Restful APIs . There are two APIs. One is  create Foremast-Judgement request. Formast-AI-API will validate the request , store to data store, publish the request to message bus and then return jobID response.

Formast-AI-API client can based on jobID to invoke Formast-AI-API to retrieve the status of job. Once Foremost Judgement completed the application health judgement, it will return health, un-health or unknown\(if current metric is not there\).

### Foremast-AI-Engine

Foremast-AI-Engine is consumer of the message. It can scale to multiple consumers. 

Based on the configuration it will first query the historical metric from metric store, compute the machine learning/statistic algorithm model,  for canary pre-deployment stage it will query the baseline and current metric and perform pairwise algorithm to check if both have same distribution pattern,  if current and baseline has different distribution pattern, it will be lower threshold. and then use threshold to detect current anomaly data points based on  historical mode .



