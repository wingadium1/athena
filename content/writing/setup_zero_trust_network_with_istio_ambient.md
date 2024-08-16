---
title:  "Adopt a zero trust network model using istio ambient mode"
date:   2024-06-16 12:00:00
tags: [Cloud, k8s, type/write]
---

## Install Calico.
```
helm repo add projectcalico https://docs.tigera.io/calico/charts

kubectl create namespace tigera-operator

helm install calico projectcalico/tigera-operator --version v3.28.1 --namespace tigera-operator
```

Wait for calico installed

## Install Istio ambient

[[Ambient Overview|https://istio.io/latest/docs/ambient/overview/]]

### Optional install Kiali

Set data plane mode ambient to namespace
```
 kubectl label namespace default istio.io/dataplane-mode=ambient
```

## Demo

Create 2 httpbin service as target
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: httpbin
---
apiVersion: v1
kind: Service
metadata:
  name: httpbin
  labels:
    app: httpbin
    service: httpbin
spec:
  ports:
    - name: http
      port: 8000
      targetPort: 8080
  selector:
    app: httpbin
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: httpbin-2
---
apiVersion: v1
kind: Service
metadata:
  name: httpbin-2
  labels:
    app: httpbin-2
    service: httpbin-2
spec:
  ports:
    - name: http
      port: 8000
      targetPort: 8080
  selector:
    app: httpbin-2
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpbin
spec:
  replicas: 1
  selector:
    matchLabels:
      app: httpbin
      version: v1
  template:
    metadata:
      labels:
        app: httpbin
        version: v1
    spec:
      serviceAccountName: httpbin
      containers:
        - image: docker.io/kong/httpbin
          imagePullPolicy: IfNotPresent
          name: httpbin
          # Same as found in Dockerfile's CMD but using an unprivileged port
          command:
            - gunicorn
            - -b
            - "[::]:8080"
            - httpbin:app
            - -k
            - gevent
          env:
            # Tells pipenv to use a writable directory instead of $HOME
            - name: WORKON_HOME
              value: /tmp
          ports:
            - containerPort: 8080
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpbin-2
spec:
  replicas: 1
  selector:
    matchLabels:
      app: httpbin-2
      version: v1
  template:
    metadata:
      labels:
        app: httpbin-2
        version: v1
    spec:
      serviceAccountName: httpbin-2
      containers:
        - image: docker.io/kong/httpbin
          imagePullPolicy: IfNotPresent
          name: httpbin
          # Same as found in Dockerfile's CMD but using an unprivileged port
          command:
            - gunicorn
            - -b
            - "[::]:8080"
            - httpbin:app
            - -k
            - gevent
          env:
            # Tells pipenv to use a writable directory instead of $HOME
            - name: WORKON_HOME
              value: /tmp
          ports:
            - containerPort: 8080
```

Create sleep service as requester

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: sleep
---
apiVersion: v1
kind: Service
metadata:
  name: sleep
  labels:
    app: sleep
    service: sleep
spec:
  ports:
    - port: 80
      name: http
  selector:
    app: sleep
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sleep
spec:
  replicas: 1
  selector:
    matchLabels:
      app: sleep
  template:
    metadata:
      labels:
        app: sleep
    spec:
      terminationGracePeriodSeconds: 0
      serviceAccountName: sleep
      containers:
        - name: sleep
          image: curlimages/curl
          command: ["/bin/sleep", "infinity"]
          imagePullPolicy: IfNotPresent
          volumeMounts:
            - mountPath: /etc/sleep/tls
              name: secret-volume
      volumes:
        - name: secret-volume
          secret:
            secretName: sleep-secret
            optional: true
---
```

Apply **AuthorizationPolicy** for ALLOW and DENY traffic from `sleep`

```
apiVersion: security.istio.io/v1
kind: AuthorizationPolicy
metadata:
  name: allow-sleep-to-httpbin
spec:
  selector:
    matchLabels:
      app: httpbin
  action: ALLOW
  rules:
    - from:
        - source:
            principals:
              - cluster.local/ns/default/sa/sleep
---
apiVersion: security.istio.io/v1
kind: AuthorizationPolicy
metadata:
  name: allow-sleep-to-httpbin-2
spec:
  selector:
    matchLabels:
      app: httpbin-2
  action: DENY
  rules:
    - from:
        - source:
            principals:
              - cluster.local/ns/default/sa/sleep
```

Output

![alt text](../assets/img/istio_ambient_output.png)

## TODO
[] implement Waypoint proxies to reach zero trust network