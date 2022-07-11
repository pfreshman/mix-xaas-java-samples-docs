---
title: A sample java gRPC client for TTSaaS
linkTitle: TTS
---

This sample app uses the Mix3 TTSaaS gRPC API to make TTS requests.

[Download Jar](/downloads/tts_client.jar)

## Usage Details

```shell
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

```shell
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
2020-09-13 19:44:47.168 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.169 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.173 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.174 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.176 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.177 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.178 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.180 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.181 INFO  received 1658 bytes of audio
2020-09-13 19:44:47.195 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.198 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.198 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.214 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.215 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.216 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.217 INFO  received 914 bytes of audio
2020-09-13 19:44:47.232 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.233 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.234 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.235 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.236 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.253 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.255 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.256 INFO  received 742 bytes of audio
2020-09-13 19:44:47.260 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.261 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.262 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.277 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.278 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.279 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.281 INFO  received 214 bytes of audio
2020-09-13 19:44:47.314 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.315 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.317 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.333 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.334 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.335 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.337 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.338 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.340 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.357 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.359 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.362 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.364 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.366 INFO  received 2202 bytes of audio
2020-09-13 19:44:47.368 INFO  received 1476 bytes of audio
2020-09-13 19:44:47.370 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.372 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.381 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.383 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.384 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.385 INFO  received 1112 bytes of audio
2020-09-13 19:44:47.386 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.386 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.387 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.389 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.390 INFO  received 364 bytes of audio
2020-09-13 19:44:47.397 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.399 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.401 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.402 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.404 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.406 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.417 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.419 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.420 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.422 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.423 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.425 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.427 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.429 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.437 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.441 INFO  received 2646 bytes of audio
2020-09-13 19:44:47.443 INFO  received 2646 bytes of audio
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
