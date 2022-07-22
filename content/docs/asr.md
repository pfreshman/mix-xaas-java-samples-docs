---
title: A sample Java gRPC client for ASRaaS
linkTitle: ASR
---

This sample app uses the [Mix ASRaaS gRPC API](https://docs.mix.nuance.com/asr-grpc/v1/#asr-as-a-service-grpc-api) to make ASR requests.

[Download Jar](/downloads/asr_client.jar)

[Sample Params](/downloads/params.asr.json)

## Usage Details

```
$ java -jar build/libs/asr_client.jar -H
usage: java -jar asr_client.jar [-H|--help] [-h|--hostname <value>] [-c|--configFile <value>]
                                [-p|--params <value>] [-a|--audioSource <value>] [-m|--modelUrn <value>]
                                [-l|--logFile <value>] [-s|--insecure] [-tt|--trxnTimeout <value>] [-ct|--connectTimeout <value>]
                                [-i|--iterations <value>] [-b|--batchMode]

                                Use Nuance ASRaaS to recognize speech in your app

Arguments:

 -H,--help                               Print this help information
 -h,--hostname <hostname>                ASRaaS server URL host:port. Default: asr.api.nuance.com:443
 -a,--audioSource <audioSource>          Audio source. Supported options are 'microphone' and a path to a raw PCM 16kHz audio file. Default: microphone
 -m,--modelUrn <modelUrn>                ASR Model (DLM) URN with the following schema:
                                         urn:nuance-mix:tag:model/<Context Tag>/mix.asr?=language=<Language Code> (e.g.
                                         urn:nuance-mix:tag:model/XAAS_TEST/mix.asr?=language=eng-USA). Default:
 -c,--config <config>                    config file containing client credentials (client_id and
                                         client_secret) and oauth server URL. Default: config.json
 -p,--params <params>                    json file containing recognition parameters. Default: params.json
 -l,--logFile <logFile>                  log file path and name. Default: asr.log
 -s,--insecure                           By default a secure TLS connection is established with ASRaaS. Specify this flag if connecting to a non-secure deployment of ASRaaS (e.g. on-prem). Default: false
 -tt,--trxnTimeout <trxnTimeout>         Time in ms to wait for asr request to complete. Default: 60000
 -ct,--connectTimeout <connectTimeout>   Time in ms to wait for connection. Default: 2000
 -i,--iterations <iterations>            Number of iterations to run in parallel. Ignored if audioSource is 'microphone'. Default: 1
 -b,--batchMode                          Batch mode reads the file specified in audioSource and loops through processing each line which points to an audio file. Ignored if audioSource is 'microphone'. Default: false

 ```

**Note**: Mix DLM's are optional and not necessary to perform ASR transactions.

## Running an ASR Request

### Streaming live audio from the microphone

```
java -jar build/libs/asr_client.jar -c config.my-config.json -p params.hello_world.json 
2022-07-21 08:34:18.633 INFO    Version: 1.0.0
2022-07-21 08:34:18.853 INFO    CONNECTING (ASR)...
2022-07-21 08:34:18.854 INFO    AUTHENTICATING... (ASRAAS)
2022-07-21 08:34:18.866 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (ASRAAS)
2022-07-21 08:34:19.554 INFO    Press enter to quit capturing audio...
2022-07-21 08:34:19.658 INFO    RECORDING...
2022-07-21 08:34:19.784 INFO    [null] Start of Speech Detected: 0
2022-07-21 08:34:20.180 INFO    CONNECTED (ASR)
2022-07-21 08:34:20.184 INFO    Adding x-client-request-id 7601ea8c-223c-451c-b5da-18c0cc0a9547
2022-07-21 08:34:20.515 INFO    Received x-request-id 9343d1e7-383d-4cdd-80f9-1466fa34ff73
2022-07-21 08:34:20.634 INFO    SOS Event: 0ms
2022-07-21 08:34:20.758 INFO    Transcription [PARTIAL]: [conf: 0.00] Lower
2022-07-21 08:34:20.775 INFO    Transcription [PARTIAL]: [conf: 0.00] Flowood
2022-07-21 08:34:20.831 INFO    Transcription [PARTIAL]: [conf: 0.00] Lower
2022-07-21 08:34:20.852 INFO    Transcription [PARTIAL]: [conf: 0.00] Hola world
2022-07-21 08:34:21.832 INFO    Transcription [FINAL]: [conf: 0.74] Hello world
2022-07-21 08:34:22.105 INFO    RECORDING STOPPED
2022-07-21 08:34:22.108 INFO    RECOGNITION COMPLETE
2022-07-21 08:34:22.109 INFO    Channel Shutdown
2022-07-21 08:34:22.109 INFO    DISCONNECTED (ASRAAS)
```

### Streaming audio from a file

```
$ java -jar build/libs/asr_client.jar -a sample/audio.pcm
2020-09-13 21:10:17.754	INFO	Version: 1.0.0
2020-09-13 21:10:17.868	INFO	CONNECTING (ASR)...
2020-09-13 21:10:17.869	INFO	AUTHENTICATING... (ASRAAS)
2020-09-13 21:10:18.514	INFO	AUTHENTICATION SUCCEEDED - TOKEN CREATED (ASRAAS)
2020-09-13 21:10:19.142	INFO	CONNECTING (ASR)...
2020-09-13 21:10:19.336	INFO	Press enter to quit capturing audio...
2020-09-13 21:10:19.338	INFO	RECORDING...
2020-09-13 21:10:19.430	INFO	CONNECTED (ASR)
2020-09-13 21:10:20.059	INFO	SOS Event: 0ms
2020-09-13 21:10:20.201	INFO	Transcription [PARTIAL]: [conf: 0.00] I
2020-09-13 21:10:20.203	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd love
2020-09-13 21:10:20.274	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like
2020-09-13 21:10:20.370	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like to
2020-09-13 21:10:20.482	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like a
2020-09-13 21:10:20.690	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like a lot
2020-09-13 21:10:20.795	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like a large
2020-09-13 21:10:21.018	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like a larger
2020-09-13 21:10:21.113	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like a large
2020-09-13 21:10:21.327	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like a large iced
2020-09-13 21:10:21.669	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like a large iced like
2020-09-13 21:10:21.767	INFO	Transcription [PARTIAL]: [conf: 0.00] I'd like a large iced latte
2020-09-13 21:10:22.729	INFO	RECORDING STOPPED
2020-09-13 21:10:22.920	INFO	Transcription [FINAL]: [conf: 0.91] I'd like a large iced latte
2020-09-13 21:10:22.941	INFO	RECOGNITION COMPLETE
```

### Streaming audio in batch mode from file

```
java -jar build/libs/asr_client.jar -c config.my-config.json -b -a batch.txt 
2022-07-20 15:56:05.510 INFO    Version: 1.0.0
2022-07-20 15:56:05.709 INFO    CONNECTING (ASR)...
2022-07-20 15:56:05.710 INFO    AUTHENTICATING... (ASRAAS)
2022-07-20 15:56:05.723 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (ASRAAS)
2022-07-20 15:56:06.175 INFO    Line #1: ./captain_america.pcm
2022-07-20 15:56:06.185 INFO    Press enter to quit capturing audio...
2022-07-20 15:56:06.187 INFO    RECORDING...
2022-07-20 15:56:06.188 INFO    [null] Start of Speech Detected: 0
2022-07-20 15:56:06.447 INFO    CONNECTED (ASR)
2022-07-20 15:56:06.448 INFO    Adding x-client-request-id 850fb845-4377-45ec-8b77-783d478abd8a
2022-07-20 15:56:06.728 INFO    Received x-request-id 60d7c884-b81d-9710-b587-2f2aca4c269a
2022-07-20 15:56:06.884 INFO    SOS Event: 0ms
2022-07-20 15:56:08.980 INFO    RECORDING STOPPED
2022-07-20 15:56:09.109 INFO    Transcription [FINAL]: [conf: 0.95] I am captain America
2022-07-20 15:56:09.140 INFO    Channel Shutdown
2022-07-20 15:56:09.140 INFO    DISCONNECTED (ASRAAS)
2022-07-20 15:56:09.144 INFO    RECOGNITION COMPLETE
```
