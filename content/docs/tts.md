---
title: A sample java gRPC client for TTSaaS
linkTitle: TTS
---

This sample app uses the Mix3 TTSaaS gRPC API to make TTS requests.

[Download Jar](/downloads/tts_client.jar)

[Sample Params](/downloads/params.tts.json)

## Usage Details

```
$ java -jar build/libs/tts_client.jar -H
Version: 1.0.0
usage: java -jar tts_client.jar [-H|--help] [-h|--hostname <value>] [-c|--configFile <value>]
                                [-p|--params <value>] [-i|--textInput <value>] [-a|--audioSink <value>]
                                [-l|--logFile <value>] [-s|--insecure] [-tt|--trxnTimeout <value>] [-ct|--connectTimeout <value>]
                                [-i|--iterations <value>] [-b|--batchMode] [-v|--getVoices]

                                Use Nuance TTSaaS to vocalize text in your app

Arguments:

 -H,--help                               Print this help information
 -h,--hostname <hostname>                TTSaaS server URL host:port. Default: tts.api.nuance.com:443
 -a,--audioSink <audioSink>              Audio sink to send audio to. Options are 'microphone' and path to audio file. Default: microphone
 -ti,--textInput <textInput>             Text to vocalize. Only plain text is supported - use the params file for advanced vocalizations. Default:
 -c,--config <config>                    config file containing client credentials (client_id and
                                         client_secret) and oauth server URL. Default: config.json
 -p,--params <params>                    json file containing tts parameters. Default: params.json
 -l,--logFile <logFile>                  log file path and name. Default: tts.log
 -s,--insecure                           By default a secure TLS connection is established with NLUaaS. Specify this flag if connecting to a non-secure deployment of NLUaaS (e.g. on-prem). Default: false
 -v,--voices                             Get available voices instead of synthesize audio. Default: false
 -tt,--trxnTimeout <trxnTimeout>         Time in ms to wait for tts request to complete. Default: 20000
 -ct,--connectTimeout <connectTimeout>   Time in ms to wait for connection. Default: 2000
 -i,--iterations <iterations>            Number of iterations to run in parallel. Default: 1
 -b,--batchMode                          Batch mode reads the file specified in textInput and loops through processing each line of text. Default: false
 ```

## Running a TTS Request

### Synthesize a Single Phrase

```
$ java -jar build/libs/tts_client.jar --textInput  "This is a test using params dot json to specify Zoe as the voice"
2020-09-13 19:44:45.373 INFO  Version: 1.0.0
2020-09-13 19:44:45.458 INFO  CONNECTING (TTSAAS)...
2020-09-13 19:44:45.459 INFO  AUTHENTICATING... (TTSAAS)
2020-09-13 19:44:46.228 INFO  AUTHENTICATION SUCCEEDED - TOKEN CREATED (TTSAAS)
2020-09-13 19:44:46.668 INFO  CONNECTING (TTSAAS)...
 >>>>>>>>> {
  "voice": {
    "name": "Zoe-Ml",
    "model": "enhanced",
    "language": "en-US"
  },
  "audioParams": {
    "audioFormat": {
      "pcm": {
        "sampleRateHz": 22050
      }
    },
    "volumePercentage": 80,
    "audioChunkDurationMs": 60
  },
  "input": {
    "text": {
      "text": "This is a test using params dot json to specify Zoe as the voice"
    }
  },
  "eventParams": {
    "sendWordMarkerEvents": true
  },
  "clientData": {
    "transaction_id": "a023a3c2-5de8-4a45-8289-99336b6a9ef3",
    "subscriber_id": "44103596f97e493eba48d25e9f05c533",
    "user_id": "44103596f97e493eba48d25e9f05c533",
    "client_app": "xaas_tts_java_sample_client",
    "deviceModel": "x86_64",
    "client_version": "1.0"
  },
  "userId": "44103596f97e493eba48d25e9f05c533"
}
2020-09-13 19:44:46.727 INFO  PLAYING STARTED
2020-09-13 19:44:46.980 INFO  CONNECTED (TTSAAS)
2020-09-13 19:44:47.166 INFO  received 2646 bytes of audio
... log statements redacted for brevity ...
2020-09-13 19:44:47.444 INFO  received 1192 bytes of audio
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  }
}
2020-09-13 19:44:51.599 INFO  PLAYING STOPPED
2020-09-13 19:44:51.599 INFO  187156 bytes written to file [microphone]
```

### Synthesize a Batch of Phrases

> Specify a file containing phrases to synthesize for the `-ti` arg and include the `-b` arg

```
java -jar build/libs/tts_client.jar -c config.mix-beta.json -b -ti sample/input.txt 
2022-07-21 09:06:53.270 INFO    Version: 1.0.0
2022-07-21 09:06:53.378 INFO    CONNECTING (TTSAAS)...
2022-07-21 09:06:53.380 INFO    AUTHENTICATING... (TTSAAS)
2022-07-21 09:06:53.999 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (TTSAAS)
2022-07-21 09:06:54.471 INFO    Line #1: Hello world
2022-07-21 09:06:55.130 INFO    CONNECTED (TTSAAS)
 >>>>>>>>> {
  "voice": {
    "name": "Zoe-Ml",
    "model": "enhanced",
    "language": "en-US"
  },
  "audioParams": {
    "audioFormat": {
      "pcm": {
        "sampleRateHz": 22050
      }
    },
    "volumePercentage": 80,
    "audioChunkDurationMs": 60
  },
  "input": {
    "text": {
      "text": "Hello world"
    }
  },
  "eventParams": {
    "sendWordMarkerEvents": true
  },
  "clientData": {
    "transaction_id": "5b92a68d-8ee5-476a-9c0e-92cb0fa0f4b2",
    "subscriber_id": "664f758ae97543008195bdd6662888bf",
    "user_id": "664f758ae97543008195bdd6662888bf",
    "client_app": "xaas_tts_java_sample_client",
    "deviceModel": "x86_64",
    "client_version": "1.0"
  },
  "userId": "664f758ae97543008195bdd6662888bf"
}
2022-07-21 09:06:55.134 INFO    PLAYING STARTED
2022-07-21 09:06:55.144 INFO    Adding x-client-request-id d21bb4dc-c847-4a09-ad07-cc497d1680e0
2022-07-21 09:06:55.292 INFO    received 2646 bytes of audio
... log statements redacted for brevity ...
2022-07-21 09:06:55.343 INFO    received 626 bytes of audio
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  }
}
2022-07-21 09:06:55.346 INFO    PLAYING STOPPED
2022-07-21 09:06:55.346 INFO    56192 bytes written to file [Hello world-21.out]
2022-07-21 09:06:55.474 INFO    Line #2: What time is it?
 >>>>>>>>> {
  "voice": {
    "name": "Zoe-Ml",
    "model": "enhanced",
    "language": "en-US"
  },
  "audioParams": {
    "audioFormat": {
      "pcm": {
        "sampleRateHz": 22050
      }
    },
    "volumePercentage": 80,
    "audioChunkDurationMs": 60
  },
  "input": {
    "text": {
      "text": "What time is it?"
    }
  },
  "eventParams": {
    "sendWordMarkerEvents": true
  },
  "clientData": {
    "transaction_id": "b8aff52c-693b-4c61-a616-d11c0e56a9fc",
    "subscriber_id": "664f758ae97543008195bdd6662888bf",
    "user_id": "664f758ae97543008195bdd6662888bf",
    "client_app": "xaas_tts_java_sample_client",
    "deviceModel": "x86_64",
    "client_version": "1.0"
  },
  "userId": "664f758ae97543008195bdd6662888bf"
}
2022-07-21 09:06:55.477 INFO    Adding x-client-request-id ff1750c1-b079-44f3-ab36-e3127afb0784
2022-07-21 09:06:55.477 INFO    PLAYING STARTED
2022-07-21 09:06:55.600 INFO    received 2646 bytes of audio
... log statements redacted for brevity ...
2022-07-21 09:06:55.616 INFO    received 1280 bytes of audio
 <<<<<<<<< {
  "status": {
    "code": 200,
    "message": "OK"
  }
}
2022-07-21 09:06:55.617 INFO    PLAYING STOPPED
2022-07-21 09:06:55.618 INFO    59492 bytes written to file [What time is it?-21.out]
2022-07-21 09:06:55.799 INFO    Channel Shutdown
2022-07-21 09:06:55.800 INFO    DISCONNECTED (TTSAAS)
```