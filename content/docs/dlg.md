---
title: A sample java gRPC client for DLGaaS
linkTitle: DLG
---

This sample app uses the [Mix DLGaaS gRPC API](https://docs.mix.nuance.com/dialog-grpc/v1/#dialog-as-a-service-grpc-api) to make DLG requests.

[Download Jar](/downloads/dlg_client.jar)

[Quick Start Coffee App Dialog Sample Params](/downloads/params.dlg.quick-start-coffee-sample.json)

[Quick Start Coffee App Dialog Sample Params (_with inline nlu wordsets_)](/downloads/params.dlg.quick-start-coffee-sample-with-inline-nlu-wordsets.json)

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

## Running a Dialog Conversation

```
java -jar build/libs/dlg_client.jar -c config.mix-sample-app.json -p params.dlg.quick-start-coffee-sample.json -m urn:nuance-mix:tag:model/QUICK_START_COFFEE_APP_V1/mix.dialog
2022-07-22 14:25:21.180 INFO    Version: 1.0.0
2022-07-22 14:25:21.274 INFO    AUTHENTICATING...
2022-07-22 14:25:21.289 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED
2022-07-22 14:25:22.149 INFO    Adding x-client-request-id 665dc3ab-c328-431c-95fc-82e38ede1200
2022-07-22 14:25:22.498 INFO    DIALOG STARTED [session id: 62eea0c5-7dde-42ba-8e49-45b669f56914]
2022-07-22 14:25:22.524 INFO    Adding x-client-request-id 5a6fb82f-2d4e-4061-84b0-38c35c1a4b9b
2022-07-22 14:25:22.770 INFO    SYSTEM PROMPT: Hello and welcome to the coffee app. What can I get you today?
2022-07-22 14:25:22.816 INFO    USER PROMPT: Press enter to start recording audio (Type 'q' to quit): 
2022-07-22 14:25:23.662 INFO    Adding x-client-request-id db722a8e-6ab0-4a56-bedc-db3d6b96e2e0
2022-07-22 14:25:23.792 INFO    RECORDING...
2022-07-22 14:25:25.197 INFO    [null] Start of Speech Detected: 1120
2022-07-22 14:25:26.948 INFO    [null] End of Speech Detected: 2880
2022-07-22 14:25:27.129 INFO    RECORDING STOPPED
2022-07-22 14:25:27.843 INFO    SYSTEM PROMPT: Perfect, a tall Caffè Misto coming right up!
2022-07-22 14:25:27.844 INFO    DIALOG FINISHED
2022-07-22 14:25:27.886 INFO    USER PROMPT: Dialog Finished (Type 'q' to quit or any key to restart): 
q
2022-07-22 14:25:31.554 INFO    Channel Shutdown
2022-07-22 14:25:31.554 INFO    DISCONNECTED
```


### Dialog Data Access Backend Endpoint

This sample client is capable of fetching data from a Data Provider upon a DLGaaS [Data Action (DA)](https://mix.nuance.com/v3/documentation/dialog-grpc/v1/index.html#data-access-actions) or an [Escalation Action (EA)](https://mix.nuance.com/v3/documentation/dialog-grpc/v1/index.html#transfer-actions).

By default the client will request data with an HTTP GET request to http://localhost:8080/. The host:port can be configured by using the command line option, --dialogBackendEndpoint. The URL path appended to http://localhost:8080/ will be the ID of the Data Action and/or Escalation Action.

If DLGaaS sends data back to the client with the DA/EA request, the client will add these data key/value pairs as query parameters to the HTTP requet. 

For example, when the client receives this DA request from DLGaaS:
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

it will construct the following URL: 

```
http://localhost:8080/get_price?COFFEE_TYPE=expresso&COFFEE_SIZE=lg
```

Upon success, the RESTful service should provide a json response body containing the parameters to be sent back to DLGaaS.

For example, continuing with the example above, the service may return back to the client:

```yaml
{ 
    "price" : "3.98" 
}
```

The `dlg_client` will add each key/value pair to the RequestData along with the returnCode, in the response payload sent back to DLGaaS. For example:

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

The `dlg_client` can send [session data](https://mix.nuance.com/v3/documentation/dialog-grpc/v1/index.html#exchanging-session-data) to DLGaaS when it initiates a StartRequest.

By default, the application will send the data contained in the `session_data` node in the `params.json` file.
