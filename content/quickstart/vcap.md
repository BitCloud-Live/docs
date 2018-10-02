---
title: Start using vcap services
weight: 3
---

# VCAP_SERVICES
For bindable services, UVCloud adds connection details to the VCAP_SERVICES environment variable after binding a service instance to your application.

The results are returned as a JSON document that contains an object for each service for which one or more instances are bound to the application. The service object contains a child object for each service instance of that service that is bound to the application. The attributes that describe a bound service are defined in the table below.

The key for each service in the JSON document is the same as the value of the “label” attribute.

Attribute	Description
binding_name	The name assigned to the service binding by the user.
instance_name	The name assigned to the service instance by the user.
name	The binding_name if it exists; otherwise the instance_name.
label	The name of the service offering.
tags	An array of strings an app can use to identify a service instance.
plan	The service plan selected when the service instance was created.
credentials	A JSON object containing the service-specific credentials needed to access the service instance.


The example below shows the value of VCAP_SERVICES for bound instances of several services available in the Pivotal Web Services Marketplace.

VCAP_SERVICES=
```json
{
  "elephantsql": [
    {
      "name": "elephantsql-binding-c6c60",
      "binding_name": "elephantsql-binding-c6c60",
      "instance_name": "elephantsql-c6c60",
      "label": "elephantsql",
      "tags": [
        "postgres",
        "postgresql",
        "relational"
      ],
      "plan": "turtle",
      "credentials": {
        "uri": "postgres://exampleuser:examplepass@babar.elephantsql.com:5432/exampleuser"
      }
    }
  ],
  "sendgrid": [
    {
      "name": "mysendgrid",
      "binding_name": null,
      "instance_name": "mysendgrid",
      "label": "sendgrid",
      "tags": [
        "smtp"
      ],
      "plan": "free",
      "credentials": {
        "hostname": "smtp.sendgrid.net",
        "username": "QvsXMbJ3rK",
        "password": "HCHMOYluTv"
      }
    }
  ]
}
```

Of course there is some libraries out there for handle VCAP_SERVICES more smoothly, such as:  
https://www.npmjs.com/package/vcap_services for Nodejs  
https://pypi.org/project/vcap-services/ for python
