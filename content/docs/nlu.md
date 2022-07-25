---
title: A sample java gRPC client for NLUaaS
linkTitle: NLU
---

This sample app uses the [Mix NLUaaS gRPC API](https://docs.mix.nuance.com/nlu-grpc/v1/#nlu-as-a-service-grpc-api) to make NLU requests.

[Download Jar](/downloads/nlu_client.jar)

[Quick Start Coffee App Sample NLU Params](/downloads/params.nlu.quick-start-coffee-sample.json) (_contains inline wordsets_)

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
                                         urn:nuance-mix:tag:model/QUICK_START_COFFEE_APP_V1/mix.nlu?=language=eng-USA). Default:
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
java -jar build/libs/nlu_client.jar -c config.mix-sample-app.json -p params.nlu.quick-start-coffee-sample.json -m urn:nuance-mix:tag:model/QUICK_START_COFFEE_APP_V1/mix.nlu?=language=eng-USA -ti "I'd like a trenta Caffe Misto"
2022-07-25 14:11:34.775 INFO    Version: 1.0.0
2022-07-25 14:11:34.883 INFO    CONNECTING (NLUAAS)...
2022-07-25 14:11:34.884 INFO    AUTHENTICATING... (NLUAAS)
2022-07-25 14:11:34.901 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS)
2022-07-25 14:11:36.028 INFO    INTERPRETING STARTING...
2022-07-25 14:11:36.034 INFO    CONNECTED (NLU)
 >>>>>>>>> {
  "parameters": {
    "interpretationResultType": "SINGLE_INTENT",
    "maxInterpretations": 3
  },
  "model": {
    "type": "SEMANTIC_MODEL",
    "uri": "urn:nuance-mix:tag:model/QUICK_START_COFFEE_APP_V1/mix.nlu?\u003dlanguage\u003deng-USA"
  },
  "resources": [{
    "inlineWordset": "{\"COFFEE_TYPE\":[{\"canonical\":\"Macchiato\",\"literal\":\"Macchiato\"},{\"canonical\":\"Caffè Misto\",\"literal\":\"Caffè Misto\"},{\"canonical\":\"Caffè Misto\",\"literal\":\"Caffe Misto\"},{\"canonical\":\"Cinnamon Dolce Latte\",\"literal\":\"Cinnamon Dolce Latte\"}]}"
  }, {
    "inlineWordset": "{\"COFFEE_SIZE\":[{\"canonical\":\"x-sm\",\"literal\":\"short\"},{\"canonical\":\"sm\",\"literal\":\"tall\"},{\"canonical\":\"md\",\"literal\":\"grandè\"},{\"canonical\":\"lg\",\"literal\":\"venti\"},{\"canonical\":\"x-lg\",\"literal\":\"trenta\"}]}"
  }],
  "clientData": {
    "client_version": "1.0",
    "client_app": "xaas_nlu_java_sample_client",
    "transaction_id": "73a4b90c-24f6-43b8-b9fb-cbe088fa133f",
    "subscriber_id": "54d58d7ad7db42c1804a2ab2f827f87c",
    "user_id": "54d58d7ad7db42c1804a2ab2f827f87c",
    "deviceModel": "x86_64"
  },
  "userId": "54d58d7ad7db42c1804a2ab2f827f87c",
  "input": {
    "text": "I\u0027d like a trenta Caffe Misto"
  }
}
2022-07-25 14:11:36.057 INFO    Adding x-client-request-id ad560787-5626-469b-8a94-221563d4ba07
2022-07-25 14:11:36.136 INFO    Received x-request-id ded0beaf-f504-9701-b46d-44b8d17b9bcf
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "I\u0027d like a trenta Caffe Misto",
    "interpretations": [{
      "singleIntentInterpretation": {
        "intent": "ORDER_COFFEE",
        "confidence": 1.0,
        "origin": "GRAMMAR",
        "entities": {
          "COFFEE_TYPE": {
            "entities": [{
              "textRange": {
                "startIndex": 18,
                "endIndex": 29
              },
              "confidence": 1.0,
              "origin": "GRAMMAR",
              "stringValue": "Caffè Misto",
              "literal": "Caffe Misto",
              "formattedLiteral": "Caffe Misto",
              "formattedTextRange": {
                "startIndex": 18,
                "endIndex": 29
              }
            }]
          },
          "COFFEE_SIZE": {
            "entities": [{
              "textRange": {
                "startIndex": 11,
                "endIndex": 17
              },
              "confidence": 1.0,
              "origin": "GRAMMAR",
              "stringValue": "x-lg",
              "literal": "trenta",
              "formattedLiteral": "trenta",
              "formattedTextRange": {
                "startIndex": 11,
                "endIndex": 17
              }
            }]
          }
        }
      }
    }],
    "formattedLiteral": "I\u0027d like a trenta Caffe Misto"
  }
}
2022-07-25 14:11:36.243 INFO    INTERPRETING COMPLETE
2022-07-25 14:11:36.245 INFO    Channel Shutdown
2022-07-25 14:11:36.246 INFO    DISCONNECTED (NLUAAS)
```

### Interpreting Text Input from Batch File
```
java -jar build/libs/nlu_client.jar -c config.mix-sample-app.json -p params.nlu.quick-start-coffee-sample.json -m urn:nuance-mix:tag:model/QUICK_START_COFFEE_APP_V1/mix.nlu?=language=eng-USA -ti input.txt -b
2022-07-25 14:02:29.539 INFO    Version: 1.0.0
2022-07-25 14:02:29.642 INFO    CONNECTING (NLUAAS)...
2022-07-25 14:02:29.644 INFO    AUTHENTICATING... (NLUAAS)
2022-07-25 14:02:29.663 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS)
2022-07-25 14:02:30.195 INFO    Line #1: I want a Large Iced Latte
2022-07-25 14:02:30.850 INFO    INTERPRETING STARTING...
2022-07-25 14:02:30.855 INFO    CONNECTED (NLU)
 >>>>>>>>> {
  "parameters": {
    << ...input redacted for brevity... >>
  }
}
2022-07-25 14:02:30.874 INFO    Adding x-client-request-id effd2dac-9da7-414e-9a28-fdccb725fe3e
2022-07-25 14:02:30.937 INFO    Received x-request-id 26e755e3-b825-92a9-908a-2c36596617c0
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "I want a Large Iced Latte",
    "interpretations": [{
      "singleIntentInterpretation": {
        << ...response redacted for brevity... >>
      }
    }],
    "formattedLiteral": "I want a Large Iced Latte"
  }
}
2022-07-25 14:02:31.458 INFO    INTERPRETING COMPLETE
2022-07-25 14:02:31.459 INFO    Line #2: Please give me an americano
2022-07-25 14:02:31.460 INFO    INTERPRETING STARTING...
 >>>>>>>>> {
  "parameters": {
    << ...input redacted for brevity... >>
  }
}
2022-07-25 14:02:31.464 INFO    Adding x-client-request-id 1893f8dd-1932-4d58-b9c1-7c26b1fa24d7
2022-07-25 14:02:31.496 INFO    Received x-request-id 396379e7-3e34-9fd3-a794-193caeba771d
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "Please give me an americano",
    "interpretations": [{
      "singleIntentInterpretation": {
        << ...response redacted for brevity... >>
      }
    }],
    "formattedLiteral": "Please give me an americano"
  }
}
2022-07-25 14:02:31.593 INFO    INTERPRETING COMPLETE
2022-07-25 14:02:31.594 INFO    Line #3: I would like a small Coffee
2022-07-25 14:02:31.638 INFO    INTERPRETING STARTING...
 >>>>>>>>> {
  "parameters": {
    << ...input redacted for brevity... >>
  }
}
2022-07-25 14:02:31.642 INFO    Adding x-client-request-id 03ba6b68-dd6d-4749-8b38-4c15203d1c53
2022-07-25 14:02:31.680 INFO    Received x-request-id 6a9c4474-29e3-9f6b-a385-845b5c612edc
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "I would like a small Coffee",
    "interpretations": [{
      "singleIntentInterpretation": {
        << ...response redacted for brevity... >>
      }
    }],
    "formattedLiteral": "I would like a small Coffee"
  }
}
2022-07-25 14:02:31.775 INFO    INTERPRETING COMPLETE
2022-07-25 14:02:31.776 INFO    Channel Shutdown
2022-07-25 14:02:31.777 INFO    DISCONNECTED (NLUAAS)
```