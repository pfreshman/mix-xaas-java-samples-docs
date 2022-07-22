---
title: A sample java gRPC client for ASRaaS + NLUaaS
linkTitle: ASR + NLU
---

This sample app uses the Mix ASRaaS and NLUaaS gRPC API's to make chained ASR + NLU requests.

[Download Jar](/downloads/asr_nlu_client.jar)

[Sample ASR Params](/downloads/params.asr.json)

[Sample NLU Params](/downloads/params.nlu.json)

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
java -jar build/libs/asr_nlu_client.jar -c config.my-config.json -m "urn:nuance-mix:tag:model/HELLO_WORLD/mix.nlu?=language=eng-USA"
2022-07-21 09:55:34.962 INFO    Version: 1.0.0
2022-07-21 09:55:35.010 INFO    OPTIONS: {"asrParamsFile":"asr_params.json","nluParamsFile":"nlu_params.json","asrHostname":"asr.api.nuance.com:443","nluHostname":"nlu.api.nuance.com:443","audioSource":"microphone","endpointerEnabled":false,"secure":true,"modelUrn":"urn:nuance-mix:tag:model/HELLO_WORLD/mix.nlu?=language=eng-USA","logFile":"asr+nlu.log","configFile":"config.mix-beta.json","connectTimeout":2000,"batchMode":false,"trxnTimeout":60000}
2022-07-21 09:55:35.080 INFO    CONNECTING (ASR)...
2022-07-21 09:55:35.083 INFO    AUTHENTICATING... (ASRAAS)
2022-07-21 09:55:35.661 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (ASRAAS)
2022-07-21 09:55:36.179 INFO    CONNECTING (NLUAAS)...
2022-07-21 09:55:36.180 INFO    AUTHENTICATING... (NLUAAS)
2022-07-21 09:55:36.180 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS)
2022-07-21 09:55:36.240 INFO    Press enter to quit capturing audio...
2022-07-21 09:55:36.348 INFO    RECORDING...
2022-07-21 09:55:36.476 INFO    [null] Start of Speech Detected: 0
2022-07-21 09:55:36.763 INFO    CONNECTED (ASR)
2022-07-21 09:55:36.764 INFO    CONNECTED (NLU)
2022-07-21 09:55:36.769 INFO    Adding x-client-request-id 83d1431f-c230-41e8-9aa8-c5e982c304e4
2022-07-21 09:55:37.054 INFO    Received x-request-id 271818e8-b0c2-416c-841a-539a6b33cddb
2022-07-21 09:55:37.206 INFO    SOS Event: 0ms
2022-07-21 09:55:37.443 INFO    Transcription [PARTIAL]: [conf: 0.00] Global
2022-07-21 09:55:37.460 INFO    Transcription [PARTIAL]: [conf: 0.00] Hello work
2022-07-21 09:55:37.491 INFO    Transcription [PARTIAL]: [conf: 0.00] Hello world
2022-07-21 09:55:38.369 INFO    Transcription [FINAL]: [conf: 0.75] Hello world
2022-07-21 09:55:38.561 INFO    RECORDING STOPPED
2022-07-21 09:55:38.564 INFO    RECOGNITION COMPLETE
2022-07-21 09:55:38.618 INFO    INTERPRETING STARTING...
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
    "client_app": "xaas_asrnlu_java_sample_client",
    "transaction_id": "b6022997-f645-4e2b-accb-061e78db7af9",
    "subscriber_id": "664f758ae97543008195bdd6662888bf",
    "user_id": "664f758ae97543008195bdd6662888bf",
    "deviceModel": "x86_64"
  },
  "userId": "664f758ae97543008195bdd6662888bf",
  "input": {
    "asrResult": {
      "@type": "type.googleapis.com/nuance.asr.v1.Result",
      "absEndMs": 1820,
      "utteranceInfo": {
        "durationMs": 1820,
        "dsp": {
          "snrEstimateDb": 23.0,
          "level": 25089.0,
          "numChannels": 1,
          "initialSilenceMs": 40,
          "initialEnergy": -61.357,
          "finalEnergy": -42.6136,
          "meanEnergy": 64.9448
        }
      },
      "hypotheses": [{
        "confidence": 0.26,
        "averageConfidence": 0.749,
        "formattedText": "Hello world",
        "minimallyFormattedText": "Hello world",
        "words": [{
          "text": "Hello",
          "confidence": 0.552,
          "startMs": 20,
          "endMs": 160
        }, {
          "text": "world",
          "confidence": 0.802,
          "startMs": 160,
          "endMs": 680
        }],
        "encryptedTokenization": "Xd25/yUlHz/OFUK84tXlL8VykVMY3BwAwvAf6XXp/0KJUPI27qNZQCORFkkC/m2cAikD0yuIVjSVmP71sqUGU5nb3A2aWdgftd0FZJAkpayWMXOJtwxzafUQ7qGc/ZrRuUI1EOI/Yct/JVxsfxtjaI8ljwQ/PMKkW3iTEMuvZvVRgCbb4VqlAKvPinA8IixByK9I+50gW0Zd1rPHrW6dbBsTFRX2GjonU2eSEjfycBKrvebdXiLQd37IEdcKlytLgf0mYo9+UBK+91dr88VeEANoBRW1r0+p1BHQXt9MfEZ64WzK7z2hdA5SvTniXd8dj8Fqh9/4+cXajAMq0lzH5Q\u003d\u003duu5KkFZ9fRWpYHQ5tgU5/7xVi8j7dwyXEIk7aKdQBRSqPDn5y8vNpdx4C5BsCo2xp4EnLk9j3rYk+99MtyB71EpNJbS/dI560PEkkwaDTpYspV90Kb34uUrF8bSFuEy3/0+ojFG1/u3o4DFKl0QDaG88z/4dmVMTqceygHmIPPdWoLfYyMsiDIUpBed/kh0t9acBEdUa1PrkdxqsMNcWAKP5TDW3DAV/9yLQg9OcdmE\u003d"
      }, {
        "averageConfidence": 0.769,
        "formattedText": "Hello world",
        "minimallyFormattedText": "Hello world",
        "words": [{
          "text": "Hello",
          "confidence": 0.553,
          "startMs": 40,
          "endMs": 160
        }, {
          "text": "world",
          "confidence": 0.819,
          "startMs": 160,
          "endMs": 680
        }],
        "encryptedTokenization": "Xd25/yUlHz/OFUK84tXlL8VykVMY3BwAwvAf6XXp/0KJUPI27qNZQCORFkkC/m2cAikD0yuIVjSVmP71sqUGU5nb3A2aWdgftd0FZJAkpayWMXOJtwxzafUQ7qGc/ZrRuUI1EOI/Yct/JVxsfxtjaI8ljwQ/PMKkW3iTEMuvZvVRgCbb4VqlAKvPinA8IixByK9I+50gW0Zd1rPHrW6dbBsTFRX2GjonU2eSEjfycBKrvebdXiLQd37IEdcKlytLgf0mYo9+UBK+91dr88VeEANoBRW1r0+p1BHQXt9MfEZ64WzK7z2hdA5SvTniXd8dj8Fqh9/4+cXajAMq0lzH5Q\u003d\u003dIWwuw/B4W4Q77Muk9SB/Ea8XjfKxINtfkVNlSsNfKhAuW6qJivTZlKZpeif8YdcfRX2RIb1NT8rW+erojbYazezigQPIJaZipqOovkPzBACKPe3Tr5IEdcqzA2QqrQa6VZaPEO/cpNNRWEOUWPXNJNhS6CgjF0iB77R3OBoHF+edxQI8hx8UeKVDY1pu1Zv/bWvhFBuuw1u4jY7rIZx4jKgBAMSi2JZRfHYIhC+qAKo\u003d"
      }, {
        "averageConfidence": 0.927,
        "formattedText": "World",
        "minimallyFormattedText": "World",
        "words": [{
          "text": "World",
          "confidence": 0.927,
          "startMs": 300,
          "endMs": 680
        }],
        "encryptedTokenization": "Xd25/yUlHz/OFUK84tXlL8VykVMY3BwAwvAf6XXp/0KJUPI27qNZQCORFkkC/m2cAikD0yuIVjSVmP71sqUGU5nb3A2aWdgftd0FZJAkpayWMXOJtwxzafUQ7qGc/ZrRuUI1EOI/Yct/JVxsfxtjaI8ljwQ/PMKkW3iTEMuvZvVRgCbb4VqlAKvPinA8IixByK9I+50gW0Zd1rPHrW6dbBsTFRX2GjonU2eSEjfycBKrvebdXiLQd37IEdcKlytLgf0mYo9+UBK+91dr88VeEANoBRW1r0+p1BHQXt9MfEZ64WzK7z2hdA5SvTniXd8dj8Fqh9/4+cXajAMq0lzH5Q\u003d\u003ds3m1e7MJX9FD+0pu7pInhcDRp4M8ED3oHIQRmHvGH/FLFff1VgxbSUo75UUkhOsXKZCm4CEqXJkKHQdLjCtQSeGynQD1xeXexABgzZskPpgyJjU3l1F6LYDwYJ0gZu0/"
      }, {
        "averageConfidence": 0.742,
        "formattedText": "Club world",
        "minimallyFormattedText": "Club world",
        "words": [{
          "text": "Club",
          "confidence": 0.36,
          "startMs": 40,
          "endMs": 240
        }, {
          "text": "world",
          "confidence": 0.915,
          "startMs": 240,
          "endMs": 680
        }],
        "encryptedTokenization": "Xd25/yUlHz/OFUK84tXlL8VykVMY3BwAwvAf6XXp/0KJUPI27qNZQCORFkkC/m2cAikD0yuIVjSVmP71sqUGU5nb3A2aWdgftd0FZJAkpayWMXOJtwxzafUQ7qGc/ZrRuUI1EOI/Yct/JVxsfxtjaI8ljwQ/PMKkW3iTEMuvZvVRgCbb4VqlAKvPinA8IixByK9I+50gW0Zd1rPHrW6dbBsTFRX2GjonU2eSEjfycBKrvebdXiLQd37IEdcKlytLgf0mYo9+UBK+91dr88VeEANoBRW1r0+p1BHQXt9MfEZ64WzK7z2hdA5SvTniXd8dj8Fqh9/4+cXajAMq0lzH5Q\u003d\u003dzmM5bgm1hu6WfPoQ34mbIz5PMb7prF3Th4B8A5bFE8qM4ib/DKZKy8dCdEym81l0EzFPzgYSt0uN7ROwo4XG+ahParePx40Ri7+a9xtze5t36LnFnKuWPDhpaQIIcXkB3qHei5IIbH3hkaauuD8WKor204+TOaNRUSLUXNQebqBIBY/T5dFodaqWaBCL+FSL0dqZQWx1NtFqPiKdlrN9h9lt0axJ/SuoCXB9C5k1KSM\u003d"
      }, {
        "averageConfidence": 0.654,
        "formattedText": "Little world",
        "minimallyFormattedText": "Little world",
        "words": [{
          "text": "Little",
          "confidence": 0.347,
          "startMs": 20,
          "endMs": 240
        }, {
          "text": "world",
          "confidence": 0.807,
          "startMs": 240,
          "endMs": 680
        }],
        "encryptedTokenization": "Xd25/yUlHz/OFUK84tXlL8VykVMY3BwAwvAf6XXp/0KJUPI27qNZQCORFkkC/m2cAikD0yuIVjSVmP71sqUGU5nb3A2aWdgftd0FZJAkpayWMXOJtwxzafUQ7qGc/ZrRuUI1EOI/Yct/JVxsfxtjaI8ljwQ/PMKkW3iTEMuvZvVRgCbb4VqlAKvPinA8IixByK9I+50gW0Zd1rPHrW6dbBsTFRX2GjonU2eSEjfycBKrvebdXiLQd37IEdcKlytLgf0mYo9+UBK+91dr88VeEANoBRW1r0+p1BHQXt9MfEZ64WzK7z2hdA5SvTniXd8dj8Fqh9/4+cXajAMq0lzH5Q\u003d\u003djv6meqHaoL4KFInZxmubkvssUIBNOIeDQE4oz8HpYaxRpyby05WGrtggWu24SGK3VCkk0TfxKqKSTJCjwOto2ZE14sERkF+7bWVqzl53MokYNKf/c/5s7mcaWa7GXJVfD4e0C5U2eE6BN3c8YPN/3eM8YrOV3OpkQvJPqh2sxOSUKkibNW6suQVpFRTRCMGqvkZoE/sYFU/x4gfuEI6Y2tIscntXgRJG+hN78BdPVgE\u003d"
      }],
      "dataPack": {
        "language": "eng-USA",
        "topic": "GEN",
        "version": "4.7.3",
        "id": "GMT20220208215217"
      }
    }
  }
}
2022-07-21 09:55:38.636 INFO    Adding x-client-request-id 86e4a71c-520e-4c03-97a1-4a1da14a178c
2022-07-21 09:55:38.694 INFO    Received x-request-id c7c388a5-2727-9c79-b9e3-d502b77338d4
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  },
  "result": {
    "literal": "hello world",
    "interpretations": [{
      "singleIntentInterpretation": {
        "intent": "HI",
        "confidence": 0.9999816,
        "origin": "STATISTICAL"
      }
    }, {
      "singleIntentInterpretation": {
        "intent": "BYE",
        "confidence": 7.4769E-6,
        "origin": "STATISTICAL"
      }
    }],
    "formattedLiteral": "Hello world"
  }
}
2022-07-21 09:55:38.743 INFO    INTERPRETING COMPLETE
2022-07-21 09:55:38.744 INFO    Channel Shutdown
2022-07-21 09:55:38.750 INFO    DISCONNECTED (ASRAAS)
2022-07-21 09:55:38.752 INFO    Channel Shutdown
2022-07-21 09:55:38.753 INFO    DISCONNECTED (NLUAAS)
```
### Streaming Audio From File
```
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
