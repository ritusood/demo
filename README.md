# Setup

## Pre-requisites

PowerDNS is configured

## Add Customer

Create a values.yaml file like below. 
More examples in the examples folder

```
name: customer1
namespace: c1ns 
domain: customer1.com
idp: 192.168.121.11:30232

```
To create a customer configutation:
```

./addCustomer.sh -v examples/customer1/customer.yaml create

```

This step perfoms following steps:

i.	Add Gateway and VirtualService resource for the outer Istio Ingress Proxy (LB)
ii.	Create namespace for the customer
iii. Deploy keycloak broker for the customer
iv.	Deploy Istio Ingress gateway in the new namespace
v.	Deploy oauth2-proxy in the new namespace
vi.	Configure oauth2-proxy
vii. Configure Keycloak
viii. Edit Istio configmap to add customer oauth2-proxy as an extensionProvider


## Add Application for the customer

```
name: customer1
namespace: c1ns 
domain: customer1.com
app: app1
host: app1.internalcustomer1.com
port: 8000

```

```

./addApp.sh -v examples/customer1/app1.yaml create

```

## Add Service

For each application deployed on user clusters add Service to provide connectivity info

```
name: app1
host: app1.internalcustomer1.com
port: 31166
internal: 8000
address: 240.0.0.2

```

```

./addService.sh -v examples/service.yaml create

```

## Add Authentication for roles

```
name: customer1
namespace: c1ns
# this is used for internal purposes only
# must be unique per role 
index: 1
app: app1
role: general
url: /image 

```

```

./addAppAuthz.sh -v examples/customer1/role1.yaml create

```

