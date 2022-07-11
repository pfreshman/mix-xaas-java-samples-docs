---
title: A sample Java gRPC client for ASRaaS
linkTitle: ASR
---

This sample app uses the Mix3 ASRaaS gRPC API to make ASR requests.

[Download Jar](/downloads/asr_client.jar)

## Usage Details

```shell
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

## Running an ASR Request

**Note**: DLM's from Mix are not necessary to perform ASR transactions. DLM's are optional. Below is a plain (non-customized) request.

```shell
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
