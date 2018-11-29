# Core Components

## 

## Foremast-Barrelman

## Foremast-Judgement Framework

![Foremast-Judgement Framework Sequence Diagram](../.gitbook/assets/foremastjudgementsequencediagram.png)

### Foremast-AI-API

Formast-AI-API provide internal Restful APIs . There are two main APIs. One is  create Foremast-Judgement request. Formast-AI-API will validate the request , store to data store, publish the request to message bus \(future release\) and then return jobID response.

Formast-AI-API client can based on jobID to invoke Formast-AI-API to retrieve the status of job. Once Foremost Judgement completed the application health judgement, it will return health, un-health or unknown\(if current metric is not there\).

### Foremast-AI-Engine

Foremast-AI-Engine is consumer of the Foremast Judgement request. It is brain of Foremast. 

V1.0 does not have message bus. Foremast-AI-Engine will retrieve the oldest  open request based on  last modified time and reserve the request to process.

There are 6 different status : initial , in progress , reprocess, completed health , complete un-health and completed unknown.

Abort is abort by client. 

![](../.gitbook/assets/foremastrequeststatediagram.png)



