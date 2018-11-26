# Installation

With an introduction to the core concepts in the previous section, lets move onto setting up K-Atlas and using it.

## Install All-in-one

Install everything to have a ready-to-use setup of Foremast:

```text

```

Point your browser to following URL to start using K-Atlas:

```text
http://ingress/
```

Or you can install individual components using below instructions.

### Install Barrelman

Barrelman should be installed to the Kubernetes cluster that you want to enable the deployment health check feature:

```bash
$ kubectl create -f 
```

Output:

```bash

```

### Install Foremast Service

{% hint style="info" %}
 Foremast service is a global service that only needs to be deployed to a control plane Kubernetes cluster.
{% endhint %}

```text
$ kubectl create -f deploy/foremast.yaml
```

Output:

```bash

```



