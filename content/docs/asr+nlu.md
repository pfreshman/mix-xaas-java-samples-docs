---
title: A sample java gRPC client for ASRaaS + NLUaaS
linkTitle: ASR + NLU
---

This sample app uses the Mix ASRaaS and NLUaaS gRPC API's to make chained ASR + NLU requests.

[Download Jar](/downloads/asr_nlu_client.jar)

[Quick Start Coffee App Sample ASR Params](/downloads/params.asr.quick-start-coffee-sample.json) (_contains inline wordsets_)

[Quick Start Coffee App Sample NLU Params](/downloads/params.nlu.quick-start-coffee-sample.json) (_contains inline wordsets_)

## Usage Details

```
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

### Pre-Requisites

A deployed ASR and NLU model will be required to perform the following speech recognition and natural language interpretation requests.

{{% mix-project-pre-reqs %}}

### Streaming Audio from Microphone

```
java -jar build/libs/asr_nlu_client.jar -c config.mix-sample-app.json -ap params.asr.quick-start-coffee-sample.json -np params.nlu.quick-start-coffee-sample.json -m urn:nuance-mix:tag:model/QUICK_START_COFFEE_APP_V1/mix.nlu?=language=eng-USA
2022-07-22 12:23:35.178 INFO    Version: 1.0.0
2022-07-22 12:23:35.302 INFO    CONNECTING (ASR)...
2022-07-22 12:23:35.304 INFO    AUTHENTICATING... (ASRAAS)
2022-07-22 12:23:35.322 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (ASRAAS)
2022-07-22 12:23:36.050 INFO    CONNECTING (NLUAAS)...
2022-07-22 12:23:36.051 INFO    AUTHENTICATING... (NLUAAS)
2022-07-22 12:23:36.051 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS)
2022-07-22 12:23:36.140 INFO    Press enter to quit capturing audio...
2022-07-22 12:23:36.244 INFO    RECORDING...
2022-07-22 12:23:36.266 INFO    CONNECTED (ASR)
2022-07-22 12:23:36.266 INFO    CONNECTED (NLU)
2022-07-22 12:23:36.272 INFO    Adding x-client-request-id b904f0f4-d028-42fa-be7d-aecd1c71c3f2
2022-07-22 12:23:36.374 INFO    [null] Start of Speech Detected: 0
2022-07-22 12:23:36.559 INFO    Received x-request-id 3186daaa-146f-45de-a3e7-538cd1f68787
2022-07-22 12:23:36.841 INFO    SOS Event: 40ms
2022-07-22 12:23:43.088 INFO    Transcription [FINAL]: [conf: 0.77] I'd like a trenta Misto Caffè.
2022-07-22 12:23:43.346 INFO    RECORDING STOPPED
2022-07-22 12:23:43.348 INFO    RECOGNITION COMPLETE
2022-07-22 12:23:43.402 INFO    INTERPRETING STARTING...
 >>>>>>>>> {
  "parameters": {
    << ...input redacted for brevity... >>
  }
}
2022-07-22 12:23:43.425 INFO    Adding x-client-request-id 030553f9-8957-4263-8d1a-86bfead83fd1
2022-07-22 12:23:43.462 INFO    Received x-request-id a1ed95b0-c229-9fa2-a6cf-9a9842d32918
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "I\u0027d like a trenta Misto Caffè.",
    "interpretations": [{
      "singleIntentInterpretation": {
        "intent": "ORDER_COFFEE",
        "confidence": 1.0,
        "origin": "STATISTICAL",
        "entities": {
          "COFFEE_TYPE": {
            "entities": [{
              "textRange": {
                "startIndex": 18,
                "endIndex": 23
              },
              "confidence": 0.3547698,
              "origin": "STATISTICAL",
              "stringValue": "",
              "literal": "Misto",
              "formattedLiteral": "Misto",
              "formattedTextRange": {
                "startIndex": 18,
                "endIndex": 23
              },
              "audioRange": {
                "startTimeMs": 2200,
                "endTimeMs": 2560
              }
            }]
          },
          "COFFEE_SIZE": {
            "entities": [{
              "textRange": {
                "startIndex": 11,
                "endIndex": 17
              },
              "confidence": 0.70017946,
              "origin": "STATISTICAL",
              "stringValue": "x-lg",
              "literal": "trenta",
              "formattedLiteral": "trenta",
              "formattedTextRange": {
                "startIndex": 11,
                "endIndex": 17
              },
              "audioRange": {
                "startTimeMs": 1000,
                "endTimeMs": 1480
              }
            }]
          }
        }
      }
    }],
    "formattedLiteral": "I\u0027d like a trenta Misto Caffè."
  }
}
2022-07-22 12:23:43.862 INFO    INTERPRETING COMPLETE
2022-07-22 12:23:43.863 INFO    Channel Shutdown
2022-07-22 12:23:43.864 INFO    DISCONNECTED (ASRAAS)
2022-07-22 12:23:43.866 INFO    Channel Shutdown
2022-07-22 12:23:43.866 INFO    DISCONNECTED (NLUAAS)
```
### Streaming Audio From File
```
$ java -jar build/libs/asr_nlu_client.jar -c config.mix-sample-app.json -ap params.asr.quick-start-coffee-sample.json -np params.nlu.quick-start-coffee-sample.json -m urn:nuance-mix:tag:model/QUICK_START_COFFEE_APP_V1/mix.nlu?=language=eng-USA -a sample/audio.pcm
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
    << ...input redacted for brevity... >>
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
