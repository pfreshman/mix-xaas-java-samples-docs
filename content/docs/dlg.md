---
title: A sample java gRPC client for DLGaaS
linkTitle: DLG
---

This sample app uses the [Mix DLGaaS gRPC API](https://docs.mix.nuance.com/dialog-grpc/v1/#dialog-as-a-service-grpc-api) to make DLG requests.

[Download Jar](/downloads/dlg_client.jar)

[Sample Params](/downloads/params.dlg.json)

[Wiremock Jar](/downloads/wiremock-standalone-2.27.2.jar)

## Usage Details

```
$ java -jar build/libs/dlg_client.jar -H
Version: 1.0.0
usage: java -jar dlg_client.jar [-H|--help] [-h|--hostname <value>] [-c|--configFile <value>]
                                [-p|--params <value>] [-m|--modelUrn <modelUrn>] [-d|--dialogBackendEndpoint <value>]
                                [-l|--logFile <value>] [-s|--insecure] [-tt|--trxnTimeout <value>]

                                Use Nuance DLGaaS for end to end conversational AI experiences in your app

Arguments:

 -H,--help                                            Print this help information
 -h,--hostname <hostname>                             DLGaaS server URL host:port. Default: dlg.api.nuance.com:443
 -d,--dialogBackendEndpoint <dialogBackendEndpoint>   host:port to use for the dialog backend endpoint. Default: localhost:8080
 -c,--config <config>                                 config file containing client credentials (client_id and
                                                      client_secret) and oauth server URL. Default: config.json
 -p,--params <params>                                 json file containing dlg parameters. Default: params.json
 -l,--logFile <logFile>                               log file path and name. Default: dlg.log
 -s,--insecure                                        By default a secure TLS connection is established with DLGaaS. Specify this flag if connecting to a non-secure deployment of DLGaaS (e.g. on-prem). Default: false
 -tt,--trxnTimeout <trxnTimeout>                      Time in ms to wait for dlg request to complete. Default: 20000
 -ct,--connectTimeout <connectTimeout>                Time in ms to wait for connection. Default: 2000
 -m,--modelUrn <modelUrn>                             Model URN for dialog context
```

## Pre-Requisites

A deployed Dialog model will be required to use this sample client.

{{% mix-project-pre-reqs %}}

### Dialog Data Access Backend Endpoint

This client is capable of fetching data from a Data Provider upon a DLGaaS [Data Action (DA)](https://mix.nuance.com/v3/documentation/dialog-grpc/v1/index.html#data-access-actions)
or an [Escalation Action (EA)](https://mix.nuance.com/v3/documentation/dialog-grpc/v1/index.html#transfer-actions).

By default it will request data with an HTTP GET request to http://localhost:8080/. The host:port can be configured by using the command line option, --dialogBackendEndpoint. The URL path appended to http://localhost:8080/ will be the ID of the DA and/or EA.

If/when the DLGaaS sends data with the DA/EA request it will add these as query parameters. 

For example, when the DLG app receives this DA request from DLGaaS:
```yaml
{
  "payload": {
    "messages": [],
    "daAction": {
      "id": "get_price",
      "data": {
        "COFFEE_TYPE": "espresso",
        "COFFEE_SIZE": "lg"
      }
    }
  }
}
```

it will perform an HTTP GET on this URL: 
```
http://localhost:8080/get_price?COFFEE_TYPE=expresso&COFFEE_SIZE=lg
```

On success, HTTP status 200, the provider should provide a json response body containing the parameters to be sent back to the DLGaaS. 

From the example above the Data Provider may return:
```yaml
{ 
    "price" : "3.98" 
}
```

The DLGaaS app will add each key value pair returned in the json to the RequestData along with the returnCode, in the response payload sent back:
```yaml
{
  "payload": {
    "requested_data": {
      "id": "get_price",
      "data": {
        "price": "3.98",
        "returnCode": "0"
      }
    }
  }
}
```

A returnCode of 0 will be used for all successfull data provider responses and a returnCode of 1 will be used when not successful.

## Start the Dialog Backend Endpoint

This application comes bundled with a Data Provider using [WireMock](http://wiremock.org/docs/running-standalone/). Once it is running this DLG application can make GET requests to it whenever needed. Example, URL bindings and responses can be found in the "mappings" directory. Start the Data Provider server as follows.

```
$ java -jar build/libs/wiremock-standalone-2.27.2.jar 
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
 /$$      /$$ /$$                     /$$      /$$                     /$$      
| $$  /$ | $$|__/                    | $$$    /$$$                    | $$      
| $$ /$$$| $$ /$$  /$$$$$$   /$$$$$$ | $$$$  /$$$$  /$$$$$$   /$$$$$$$| $$   /$$
| $$/$$ $$ $$| $$ /$$__  $$ /$$__  $$| $$ $$/$$ $$ /$$__  $$ /$$_____/| $$  /$$/
| $$$$_  $$$$| $$| $$  \__/| $$$$$$$$| $$  $$$| $$| $$  \ $$| $$      | $$$$$$/ 
| $$$/ \  $$$| $$| $$      | $$_____/| $$\  $ | $$| $$  | $$| $$      | $$_  $$ 
| $$/   \  $$| $$| $$      |  $$$$$$$| $$ \/  | $$|  $$$$$$/|  $$$$$$$| $$ \  $$
|__/     \__/|__/|__/       \_______/|__/     |__/ \______/  \_______/|__/  \__/

port:                         8080
enable-browser-proxying:      false
disable-banner:               false
no-request-journal:           false
verbose:                      false
```

## Session data

[The DLG app can send session data](https://mix.nuance.com/v3/documentation/dialog-grpc/v1/index.html#exchanging-session-data) to DLGaaS when it initiates a StartRequest.

By default, the application will send the data contained in `session_data` node in the `params.json` file.

## Running a Dialog Conversation

### Without Data Access

```
$ java -jar build/libs/dlg_client.jar -m "urn:nuance-mix:tag:model/XAAS_TEST/mix.dialog"
2020-09-14 01:23:28.849 INFO  Version: 1.0.0
2020-09-14 01:23:28.935 INFO  AUTHENTICATING...
2020-09-14 01:23:28.937 INFO  AUTHENTICATION SUCCEEDED - TOKEN CREATED
2020-09-14 01:23:30.725 INFO  DIALOG STARTED [session id: 4ce6a7c8-4ac8-44b1-80f4-3c7c93c42c2e]
2020-09-14 01:23:31.068 INFO  SYSTEM PROMPT: Hello Jane Doe , and welcome to the sample coffee app! What can I get you today?
2020-09-14 01:23:35.847 INFO  USER PROMPT: Press enter to start recording audio (Type 'q' to quit):
i'd like a latte
2020-09-14 01:23:40.463 INFO  SYSTEM PROMPT: What size coffee would you like?
medium
2020-09-14 01:23:42.460 INFO  USER PROMPT: Press enter to start recording audio (Type 'q' to quit):
2020-09-14 01:23:43.543 INFO  SYSTEM PROMPT: Perfect, a medium latte coming right up! Thanks for stopping by. Glad to be of service! \u{1f44b}
2020-09-14 01:23:43.543 INFO  DIALOG FINISHED
```

### With Data Access

Set `triggerDataAction: true` in `params.json`.

```
$ java -jar build/libs/dlg_client.jar -m "urn:nuance-mix:tag:model/XAAS_TEST/mix.dialog"
2020-09-14 01:10:50.682 INFO  Version: 1.0.0
2020-09-14 01:10:50.773 INFO  AUTHENTICATING...
2020-09-14 01:10:51.451 INFO  AUTHENTICATION SUCCEEDED - TOKEN CREATED
2020-09-14 01:10:53.253 INFO  DIALOG STARTED [session id: c7ceab6d-af0d-47ec-b07f-815e38f57fef]
2020-09-14 01:10:53.653 INFO  SYSTEM PROMPT: Hello Jane Doe , and welcome to the sample coffee app! What can I get you today?
2020-09-14 01:10:58.425 INFO  USER PROMPT: Press enter to start recording audio (Type 'q' to quit):
i'd like a latte
2020-09-14 01:11:07.789 INFO  SYSTEM PROMPT: What size coffee would you like?
2020-09-14 01:11:09.720 INFO  USER PROMPT: Press enter to start recording audio (Type 'q' to quit):
medium
2020-09-14 01:11:15.083 INFO  SYSTEM PROMPT: That will be $ 2.98 for a medium latte . Coming right up! Thanks for stopping by. Glad to be of service! \u{1f44b}
2020-09-14 01:11:15.083 INFO  DIALOG FINISHED

2020-09-14 01:12:13.156 INFO  Version: 1.0.0
2020-09-14 01:12:13.251 INFO  AUTHENTICATING...
2020-09-14 01:12:13.254 INFO  AUTHENTICATION SUCCEEDED - TOKEN CREATED
2020-09-14 01:12:14.998 INFO  DIALOG STARTED [session id: 5b9dfb40-cbbd-4837-9255-19b1269beffd]
2020-09-14 01:12:15.325 INFO  SYSTEM PROMPT: Hello Jane Doe , and welcome to the sample coffee app! What can I get you today?
2020-09-14 01:12:20.085 INFO  USER PROMPT: Press enter to start recording audio (Type 'q' to quit):
i'd like a large latte
2020-09-14 01:12:23.307 INFO  SYSTEM PROMPT: That will be $ 3.98 for a large latte . Coming right up! Thanks for stopping by. Glad to be of service! \u{1f44b}
2020-09-14 01:12:23.308 INFO  DIALOG FINISHED
```

### With Data Access - Transfer

Set `triggerDataAction: true` in `params.json`.

```
$ java -jar build/libs/dlg_client.jar -m "urn:nuance-mix:tag:model/XAAS_TEST/mix.dialog"
2020-09-14 01:18:30.715 INFO  Version: 1.0.0
2020-09-14 01:18:30.808 INFO  AUTHENTICATING...
2020-09-14 01:18:30.810 INFO  AUTHENTICATION SUCCEEDED - TOKEN CREATED
2020-09-14 01:18:32.506 INFO  DIALOG STARTED [session id: 03cfd555-01ae-442d-b258-4363889c9ac1]
2020-09-14 01:18:32.814 INFO  SYSTEM PROMPT: Hello Jane Doe , and welcome to the sample coffee app! What can I get you today?
2020-09-14 01:18:37.589 INFO  USER PROMPT: Press enter to start recording audio (Type 'q' to quit):
blah
2020-09-14 01:18:39.949 INFO  SYSTEM PROMPT: Sorry, didn't understand that. Can I interest you in a beverage? What can I get you today?
2020-09-14 01:18:44.392 INFO  USER PROMPT: Press enter to start recording audio (Type 'q' to quit):
blah
2020-09-14 01:18:46.056 INFO  SYSTEM PROMPT: Sorry, I couldn't understand. Let me get someone for you.
2020-09-14 01:18:46.605 INFO  SYSTEM PROMPT: Shoot, sorry we weren't able to make things work. Hope it works out next time! \u{1f44b}
2020-09-14 01:18:46.605 INFO  DIALOG FINISHED
```
