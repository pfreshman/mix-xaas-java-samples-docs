---
title: A sample java gRPC client for NLUaaS
linkTitle: NLU
---

This sample app uses the [Mix NLUaaS gRPC API](https://docs.mix.nuance.com/nlu-grpc/v1/#nlu-as-a-service-grpc-api) to make NLU requests.

[Download Jar](/downloads/nlu_client.jar)

[Sample Params](/downloads/params.nlu.json)

## Usage Details

```
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

### Pre-Requisites

A deployed model will be required to perform an interpretation request. 

{{% mix-project-pre-reqs %}}

### Interpreting a Single Text Input 
```
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

### Interpreting Batch Text Input

> Specify a file containing phrases to interpret for the `-ti` arg and include the `-b` arg

```
java -jar build/libs/nlu_client.jar -c config.my-config.json -m "urn:nuance-mix:tag:model/HELLO_WORLD/mix.nlu?=language=eng-USA" -b -ti sample/input.txt 
2022-07-21 08:57:27.099 INFO    Version: 1.0.0
2022-07-21 08:57:27.195 INFO    CONNECTING (NLUAAS)...
2022-07-21 08:57:27.197 INFO    AUTHENTICATING... (NLUAAS)
2022-07-21 08:57:27.216 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS)
2022-07-21 08:57:27.739 INFO    Line #1: Hello World
2022-07-21 08:57:28.069 INFO    INTERPRETING STARTING...
2022-07-21 08:57:28.074 INFO    CONNECTED (NLU)
 >>>>>>>>> {
  "parameters": {
    "interpretationResultType": "SINGLE_INTENT",
    "maxInterpretations": 3
  },
  "model": {
    "type": "SEMANTIC_MODEL",
    "uri": "urn:nuance-mix:tag:model/HELLO_WORLD/mix.nlu?\u003dlanguage\u003deng-USA"
  },
  "clientData": {
    "client_version": "1.0",
    "client_app": "xaas_nlu_java_sample_client",
    "transaction_id": "7e59dfa7-b583-4252-be4f-b7f9b400d0a1",
    "subscriber_id": "664f758ae97543008195bdd6662888bf",
    "user_id": "664f758ae97543008195bdd6662888bf",
    "deviceModel": "x86_64"
  },
  "userId": "664f758ae97543008195bdd6662888bf",
  "input": {
    "text": "Hello World"
  }
}
2022-07-21 08:57:28.096 INFO    Adding x-client-request-id 8e8ee7d8-0887-4ecb-9e42-1940ccc287f1
2022-07-21 08:57:28.155 INFO    Received x-request-id 3624fc5d-18f0-98ae-b8c1-a8a990331ed2
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "Hello World",
    "interpretations": [{
      "singleIntentInterpretation": {
        "intent": "HI",
        "confidence": 0.99999964,
        "origin": "STATISTICAL"
      }
    }, {
      "singleIntentInterpretation": {
        "intent": "BYE",
        "confidence": 1.953E-7,
        "origin": "STATISTICAL"
      }
    }],
    "formattedLiteral": "Hello World"
  }
}
2022-07-21 08:57:28.716 INFO    INTERPRETING COMPLETE
2022-07-21 08:57:28.717 INFO    Line #2: Goodbye Everyone
2022-07-21 08:57:28.718 INFO    INTERPRETING STARTING...
 >>>>>>>>> {
  "parameters": {
    "interpretationResultType": "SINGLE_INTENT",
    "maxInterpretations": 3
  },
  "model": {
    "type": "SEMANTIC_MODEL",
    "uri": "urn:nuance-mix:tag:model/HELLO_WORLD/mix.nlu?\u003dlanguage\u003deng-USA"
  },
  "clientData": {
    "client_version": "1.0",
    "client_app": "xaas_nlu_java_sample_client",
    "transaction_id": "a19d52da-0fe7-44d4-b555-93e9bcb96792",
    "subscriber_id": "664f758ae97543008195bdd6662888bf",
    "user_id": "664f758ae97543008195bdd6662888bf",
    "deviceModel": "x86_64"
  },
  "userId": "664f758ae97543008195bdd6662888bf",
  "input": {
    "text": "Goodbye Everyone"
  }
}
2022-07-21 08:57:28.720 INFO    Adding x-client-request-id 0bc6bfd1-d3de-47d7-bb0e-722a1e44ab54
2022-07-21 08:57:28.754 INFO    Received x-request-id 499c2d78-0ee5-9031-8dd8-1ce77db7c5f7
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "Goodbye Everyone",
    "interpretations": [{
      "singleIntentInterpretation": {
        "intent": "BYE",
        "confidence": 0.99999887,
        "origin": "STATISTICAL"
      }
    }, {
      "singleIntentInterpretation": {
        "intent": "HI",
        "confidence": 6.451E-7,
        "origin": "STATISTICAL"
      }
    }],
    "formattedLiteral": "Goodbye Everyone"
  }
}
2022-07-21 08:57:28.797 INFO    INTERPRETING COMPLETE
2022-07-21 08:57:28.799 INFO    Channel Shutdown
2022-07-21 08:57:28.800 INFO    DISCONNECTED (NLUAAS)
```