---
title:  "Adopt a zero trust network model using calico and istio"
date:   2024-06-16 12:00:00
tags: [Cloud, k8s, type/write, calico, istio]
---

## Install Calico.
```
helm repo add projectcalico https://docs.tigera.io/calico/charts

kubectl create namespace tigera-operator

helm install calico projectcalico/tigera-operator --version v3.28.1 --namespace tigera-operator
```

Wait for calico installed

## Install Istio and enable Calico integration.

1. Enable the Policy Sync API on Felix cluster-wide.

```
kubectl patch FelixConfiguration default --type=merge --patch '{"spec": {"policySyncPathPrefix": "/var/run/nodeagent"}}'
```

if you have installed Calico via the operator, you can optionally disable flexvolumes. Flexvolumes were used in earlier implementations and have since been deprecated.

```
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.1/manifests/csi-driver.yaml
```

2. Install Istio

Create ***PeerAuthentication*** Policy

```
kubectl create -f - <<EOF
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default-strict-mode
  namespace: istio-system
spec:
  mtls:
    mode: STRICT
EOF
```

Add Calico authorization services to the mesh

```
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.1/manifests/alp/istio-app-layer-policy-envoy-v3.yaml
```

Add namespace label

```
kubectl label namespace <your namespace name> istio-injection=enabled
```

## Establish workload identity by using Service Accounts.


## Write initial allow-list policies for each service.

Ref: [[https://docs.tigera.io/calico/latest/network-policy/istio/enforce-policy-istio#the-need-for-policy]]

Sample Policy
```
# Copyright (c) 2017 Tigera, Inc. All rights reserved.

# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: customer
spec:
  selector: app == 'customer'
  ingress:
    - action: Allow
      http:
        methods: ["GET"]
  egress:
    - action: Allow

---

apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: summary
spec:
  selector: app == 'summary'
  ingress:
    - action: Allow
      source:
        serviceAccounts:
          names: ["customer"]
  egress:
    - action: Allow

---
apiVersion: projectcalico.org/v3
kind: GlobalNetworkPolicy
metadata:
  name: database
spec:
  selector: app == 'database'
  ingress:
    - action: Allow
      source:
        serviceAccounts:
          names: ["summary"]
  egress:
    - action: Allow

---
```