---
title: A sample java gRPC client for ASRaaS + NLUaaS
linkTitle: ASR + NLU
---

This sample app uses the Mix3 ASRaaS and NLUaaS gRPC API's to make chained ASR + NLU requests.

[Download Jar](/downloads/asr_train.jar)

## Usage Details

```shell
$ java -jar build/libs/asr_nlu_client.jar -H
Version: 1.0.0
usage: java -jar asr_nlu_client.jar [-H|--help] [-ah|--asrHostname <value>] [-nh|--nluHostname <value>] [-c|--configFile <value>]
                                [-ap|--asrParams <value>] [-np|--nluParams <value>] [-a|--audioSource <value>] [-m|--modelUrn <value>]
                                [-l|--logFile <value>] [-s|--insecure] [-tt|--trxnTimeout <value>] [-ct|--connectTimeout <value>]
                                [-i|--iterations <value>] [-b|--batchMode]

                                Use Nuance ASRaaS + NLUaaS to recognize speech and perform natural language understand in your app

Arguments:

 -H,--help                               Print this help information
 -ah,--asrHostname <asrHostname>         ASRaaS server URL host:port. Default: asr.api.nuance.com:443
 -nh,--nluHostname <nluHostname>         NLUaaS server URL host:port. Default: nlu.api.nuance.com:443
 -a,--audioSource <audioSource>          Audio source. Supported options are 'microphone' and a path to a raw PCM 16kHz audio file. Default: microphone
 -m,--modelUrn <modelUrn>                NLU Model URN with the following schema:
                                         urn:nuance-mix:tag:model/<Context Tag>/mix.nlu?=language=<Language Code> (e.g.
                                         urn:nuance-mix:tag:model/XAAS_TEST/mix.nlu?=language=eng-USA). Default:
 -c,--config <config>                    config file containing client credentials (client_id and
                                         client_secret) and oauth server URL. Default: config.json
 -ap,--asrParams <asrParams>             json file containing recognition parameters. Default: asr_params.json
 -np,--nluParams <nluParams>             json file containing nlu parameters. Default: nlu_params.json
 -l,--logFile <logFile>                  log file path and name. Default: asr+nlu.log
 -s,--insecure                           By default a secure TLS connection is established with ASRaaS. Specify this flag if connecting to a non-secure deployment of ASRaaS (e.g. on-prem). Default: false
 -tt,--trxnTimeout <trxnTimeout>         Time in ms to wait for asr request to complete. Default: 60000
 -ct,--connectTimeout <connectTimeout>   Time in ms to wait for connection. Default: 2000
 -i,--iterations <iterations>            Number of iterations to run in parallel. Ignored if audioSource is 'microphone'. Default: 1
 -b,--batchMode                          Batch mode reads the file specified in audioSource and loops through processing each line which points to an audio file. Ignored if audioSource is 'microphone'. Default: false
```

## Running an ASR+NLU Request

A deployed ASR and NLU model will be required to perform the following speech recognition and natural language interpretation requests.

Perform the following steps and provide the appropriate URNs:

1. Create a Project, Locale: en-US
2. Import `sample/order_coffee.trsx`
3. Create a Build
4. Create and deploy an Application Configuration, defining a context tag of `XAAS_TEST` 

Check the [Quick Start](https://mix.nuance.com/v3/documentation/mix-starting/#quick-start) for more information.

```shell
$ java -jar build/libs/asr_nlu_client.jar -c config.json -m "urn:nuance-mix:tag:model/XAAS_TEST/mix.nlu?=language=eng-USA" -a sample/audio.pcm
2020-09-13 18:30:17.865 INFO  Version: 1.0.0
2020-09-13 18:30:17.958 INFO  CONNECTING (ASR)...
2020-09-13 18:30:17.959 INFO  AUTHENTICATING... (ASRAAS)
2020-09-13 18:30:17.962 INFO  AUTHENTICATION SUCCEEDED - TOKEN CREATED (ASRAAS)
2020-09-13 18:30:18.523 INFO  CONNECTING (NLUAAS)...
2020-09-13 18:30:18.524 INFO  AUTHENTICATING... (NLUAAS)
2020-09-13 18:30:18.525 INFO  AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS)
2020-09-13 18:30:18.546 INFO  CONNECTING (ASR)...
2020-09-13 18:30:18.550 INFO  Press enter to quit capturing audio...
2020-09-13 18:30:18.550 INFO  RECORDING...
2020-09-13 18:30:18.840 INFO  CONNECTED (ASR)
2020-09-13 18:30:18.847 INFO  CONNECTED (NLU)
2020-09-13 18:30:19.579 INFO  SOS Event: 0ms
2020-09-13 18:30:19.702 INFO  Transcription [PARTIAL]: [conf: 0.00] I
2020-09-13 18:30:19.725 INFO  Transcription [PARTIAL]: [conf: 0.00] I'd like
2020-09-13 18:30:19.849 INFO  Transcription [PARTIAL]: [conf: 0.00] I'd like a lot
2020-09-13 18:30:19.967 INFO  Transcription [PARTIAL]: [conf: 0.00] I'd like a large
2020-09-13 18:30:20.177 INFO  Transcription [PARTIAL]: [conf: 0.00] I'd like a larger
2020-09-13 18:30:20.286 INFO  Transcription [PARTIAL]: [conf: 0.00] I'd like a large
2020-09-13 18:30:20.511 INFO  Transcription [PARTIAL]: [conf: 0.00] I'd like a large iced
2020-09-13 18:30:20.824 INFO  Transcription [PARTIAL]: [conf: 0.00] I'd like a large iced like
2020-09-13 18:30:20.936 INFO  Transcription [PARTIAL]: [conf: 0.00] I'd like a large iced latte
2020-09-13 18:30:21.899 INFO  RECORDING STOPPED
2020-09-13 18:30:22.004 INFO  Transcription [FINAL]: [conf: 0.91] I'd like a large iced latte
2020-09-13 18:30:22.106 INFO  RECOGNITION COMPLETE
2020-09-13 18:30:22.115 INFO  INTERPRETING STARTING...
2020-09-13 18:30:22.115 INFO  CONNECTING (NLUAAS)...
2020-09-13 18:30:22.116 INFO  CONNECTED (NLU)
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
    "transaction_id": "d5a20f97-546e-44f1-a98e-d4120a74982b",
    "subscriber_id": "96025c79755944b8820e0b76106a8a0c",
    "user_id": "96025c79755944b8820e0b76106a8a0c",
    "client_app": "xaas_asrnlu_java_sample_client",
    "deviceModel": "x86_64",
    "client_version": "1.0"
  },
  "userId": "96025c79755944b8820e0b76106a8a0c",
  "input": {
    "asrResult": {
      "@type": "type.googleapis.com/nuance.asr.v1.Result",
      "absEndMs": 3160,
      "utteranceInfo": {
        "durationMs": 3160,
        "dsp": {
          "snrEstimateDb": 18.0,
          "level": 22529.0,
          "numChannels": 1,
          "initialSilenceMs": 240,
          "initialEnergy": -52.1484,
          "finalEnergy": -57.3047,
          "meanEnergy": 81.7016
        }
      },
      "hypotheses": [{
        "confidence": 0.643,
        "averageConfidence": 0.913,
        "formattedText": "I\u0027d like a large iced latte",
        "minimallyFormattedText": "I\u0027d like a large iced latte",
        "words": [{
          "text": "I\u0027d",
          "confidence": 0.975,
          "startMs": 240,
          "endMs": 420
        }, {
          "text": "like",
          "confidence": 0.985,
          "startMs": 420,
          "endMs": 680
        }, {
          "text": "a",
          "confidence": 0.968,
          "startMs": 680,
          "endMs": 900
        }, {
          "text": "large",
          "confidence": 0.932,
          "startMs": 900,
          "endMs": 1260
        }, {
          "text": "iced",
          "confidence": 0.808,
          "startMs": 1260,
          "endMs": 1740
        }, {
          "text": "latte",
          "confidence": 0.929,
          "startMs": 1740,
          "endMs": 2140
        }],
        "encryptedTokenization": "..."
      }, {
        "averageConfidence": 0.892,
        "formattedText": "I\u0027d like a large ice latte",
        "minimallyFormattedText": "I\u0027d like a large ice latte",
        "words": [{
          "text": "I\u0027d",
          "confidence": 0.975,
          "startMs": 240,
          "endMs": 420
        }, {
          "text": "like",
          "confidence": 0.985,
          "startMs": 420,
          "endMs": 680
        }, {
          "text": "a",
          "confidence": 0.966,
          "startMs": 680,
          "endMs": 900
        }, {
          "text": "large",
          "confidence": 0.928,
          "startMs": 900,
          "endMs": 1260
        }, {
          "text": "ice",
          "confidence": 0.754,
          "startMs": 1260,
          "endMs": 1620
        }, {
          "text": "latte",
          "confidence": 0.871,
          "startMs": 1620,
          "endMs": 2140
        }],
        "encryptedTokenization": "..."
      }]
    }
  }
}
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "I\u0027d like a large iced latte",
    "interpretations": [{
      "singleIntentInterpretation": {
        "intent": "ORDER_COFFEE",
        "confidence": 1.0,
        "origin": "STATISTICAL",
        "entities": {
          "COFFEE_TYPE": {
            "entities": [{
              "textRange": {
                "startIndex": 17,
                "endIndex": 21
              },
              "confidence": 0.19360532,
              "origin": "STATISTICAL",
              "stringValue": "",
              "literal": "iced"
            }, {
              "textRange": {
                "startIndex": 22,
                "endIndex": 27
              },
              "confidence": 0.20336248,
              "origin": "STATISTICAL",
              "stringValue": "latte",
              "literal": "latte"
            }]
          },
          "COFFEE_SIZE": {
            "entities": [{
              "textRange": {
                "startIndex": 11,
                "endIndex": 16
              },
              "confidence": 0.6418865,
              "origin": "STATISTICAL",
              "stringValue": "lg",
              "literal": "large"
            }]
          }
        }
      }
    }]
  }
}
2020-09-13 18:30:24.056 INFO  INTERPRETING COMPLETE
```
