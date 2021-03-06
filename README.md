# argo-probes

This package includes probes for ARGO internal services. 

Currently, there are probes for:

- ARGO EGI Connectors
- ARGO Messaging service
- ARGO Web API
- POEM service

## ARGO Messaging service

Probe is inspecting AMS service by trying to publish and consume randomly
generated messages. Probe creates a topic and subscription, generates random
100 messages with payload about 500 bytes that tries to publish to service
following immediate try of consuming them. If the integrity of messages is
preserved on publishing and consuming side, service is working fine and probe
will return successfull status.

The usage is:

```sh
$ usage: ams-probe [-h] [--host HOST] --token TOKEN --project PROJECT
                   [--topic TOPIC] [--subscription SUBSCRIPTION]
                   [--timeout TIMEOUT]

```

where:
- (--host): the FQDN of the AMS service. Default: messaging-devel.argo.grnet.gr
- (--token): secret used to authenticate to AMS service
- (--project): project created on AMS service
- (--topic): topic that will be created in project and that will hold published
             messages. Default: nagios_sensor_topic.
- (--subscription): subscription created for topic from which messages will be
                    pulled. Default: nagios_sensor_sub
- (--timeout): Timeout after connection is considered dead. Default: 180 

### Usage example

```sh
$ ./ams-probe --token T0K3N --host messaging-devel.argo.grnet.gr --project EGI --topic probetest --subscription probetestsub --timeout 30
```

## ARGO Web API 

This is a probe for checking AR and status reports are properly working. 
It checks if there are available AR and status data for a selected day. 

The usage of the script is:
```sh
$ usage: web-api [-h] [-H HOSTNAME] [--tenant TENANT] [--rtype RTYPE] [--token TOKEN]
              [--day DAY] [--unused-reports Report1 Report2] [-t TIMEOUT] [-v DEBUG]
```

where:

 - (-H): the hostname of the web api 
 - (--tenant): the tenant name (ex. EGI)
 - (--rtype): the report type (ar or status)
 - (--token): the authorization token
 - (--unused-reports): Report names that are not used anymore. 
 - (--day): the day to check (1,2 ..3 means 1 or 2 or 3 days back)
 - (-t): the timeout
 - (-v): prints some debug data when is set to on  (by default off)
 
### Usage example

```sh
$ ./web-api -H web-api.test.com --tenant tenantname --rtype ar --token 12321312313123 --unused-reports  Report1 Report2  --day 1 -t 180 -v
```
