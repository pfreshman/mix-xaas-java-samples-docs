---
title: A sample java gRPC client for NLUaaS
linkTitle: NLU
---

This sample app uses the Mix3 NLUaaS gRPC API to make NLU requests.

[Download Jar](/downloads/nlu_client.jar)

## Usage Details

```shell
$ java -jar build/libs/nlu_client.jar -H
usage: java -jar nlu_client.jar [-H|--help] [-h|--hostname <value>] [-c|--configFile <value>]
                                [-p|--params <value>] [-ti|--textInput <value>] [-m|--modelUrn <value>]
                                [-l|--logFile <value>] [-s|--insecure] [-tt|--trxnTimeout <value>] [-ct|--connectTimeout <value>]
                                [-i|--iterations <value>] [-b|--batchMode]

                                Use Nuance NLUaaS to Understand Natural Language in your app

Arguments:

 -H,--help                               Print this help information
 -h,--hostname <hostname>                NLUaaS server URL host:port. Default: nlu.api.nuance.com:443
 -m,--modelUrn <modelUrn>                NLU Model URN with the following schema:
                                         urn:nuance-mix:tag:model/<Context Tag>/mix.nlu?=language=<Language Code> (e.g.
                                         urn:nuance-mix:tag:model/XAAS_TEST/mix.nlu?=language=eng-USA). Default:
 -ti,--textInput <textInput>             Text to perform interpretation on. If batchMode is enabled, the client treats this as a path to a file. Default:
 -c,--config <config>                    config file containing client credentials (client_id and
                                         client_secret) and oauth server URL. Default: config.json
 -p,--params <params>                    json file containing nlu parameters. Default: params.json
 -l,--logFile <logFile>                  log file path and name. Default: nlu.log
 -s,--insecure                           By default a secure TLS connection is established with NLUaaS. Specify this flag if connecting to a non-secure deployment of NLUaaS (e.g. on-prem). Default: false
 -tt,--trxnTimeout <trxnTimeout>         Time in ms to wait for nlu request to complete. Default: 2000
 -ct,--connectTimeout <connectTimeout>   Time in ms to wait for connection. Default: 2000
 -i,--iterations <iterations>            Number of iterations to run in parallel. Default: 1
 -b,--batchMode                          Batch mode reads the file specified in textInput and loops through processing each line of text. Default: false
```

## Running an Intepretation Request

A deployed model will be required to perform an interpretation request. 

Perform the following steps and provide the appropriate URN:

1. Create a Project, Locale: en-US
2. Import `sample/order_coffee.trsx`
3. Create a Build
4. Create and deploy an Application Configuration, defining a context tag of `XAAS_TEST` 

Check the [Quick Start](https://mix.nuance.com/v3/documentation/mix-starting/#quick-start) for more information.

```shell
$ java -jar build/libs/nlu_client.jar -m "urn:nuance-mix:tag:model/XAAS_TEST/mix.nlu?=language=eng-USA" -ti "i'd like an americano"
2020-09-13 19:26:26.996 INFO  Version: 1.0.0
2020-09-13 19:26:27.091 INFO  CONNECTING (NLUAAS)...
2020-09-13 19:26:27.092 INFO  AUTHENTICATING... (NLUAAS)
2020-09-13 19:26:27.869 INFO  AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS)
2020-09-13 19:26:28.277 INFO  INTERPRETING STARTING...
2020-09-13 19:26:28.281 INFO  CONNECTING (NLUAAS)...
 >>>>>>>>> {
  "parameters": {
    "interpretationResultType": "SINGLE_INTENT",
    "maxInterpretations": 3
  },
  "model": {
    "type": "SEMANTIC_MODEL",
    "uri": "urn:nuance-mix:tag:model/XAAS_TEST/mix.nlu?\u003dlanguage\u003deng-USA"
  },
  "clientData": {
    "transaction_id": "571b5bb8-eddc-4cae-bf84-bcabf3f94fd6",
    "subscriber_id": "55bad684ea9f4b6686f4c4d8afde5f35",
    "user_id": "55bad684ea9f4b6686f4c4d8afde5f35",
    "client_app": "xaas_nlu_java_sample_client",
    "deviceModel": "x86_64",
    "client_version": "1.0"
  },
  "userId": "55bad684ea9f4b6686f4c4d8afde5f35",
  "input": {
    "text": "i\u0027d like an americano"
  }
}
2020-09-13 19:26:28.684 INFO  CONNECTED (NLU)
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "i\u0027d like an americano",
    "interpretations": [{
      "singleIntentInterpretation": {
        "intent": "ORDER_COFFEE",
        "confidence": 1.0,
        "origin": "STATISTICAL",
        "entities": {
          "COFFEE_TYPE": {
            "entities": [{
              "textRange": {
                "startIndex": 12,
                "endIndex": 21
              },
              "confidence": 0.9480499,
              "origin": "STATISTICAL",
              "stringValue": "americano",
              "literal": "americano"
            }]
          }
        }
      }
    }]
  }
}
2020-09-13 19:26:29.277 INFO  INTERPRETING COMPLETE
```
