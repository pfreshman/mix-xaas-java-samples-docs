---
title: "Getting Started"
linkTitle: "Getting Started"
date: 2022-07-08T11:51:04-04:00
draft: true
---

## Overview

This collection of Mix XaaS Java Samples are java command-line apps to help you get up and running quickly with using [Mix run-time API's](https://docs.mix.nuance.com/runtime-services/#runtime-apis-quick-reference).

Each sample app provides a consistent CLI for:

* managing authentication
* setting connection parameters
* handling timeouts
* passing in API-specific parameters via json file
* logging API messages


> Sample apps targeting run-time API's that stream audio are designed to capture audio (e.g. ASR) and play audio (e.g. TTS) using your default microphone and speaker, respectively. Audio can also be streamed from or to file if that is preferred.

Leverage these sample apps to:

* verify Mix run-time credentials
* experiment with API parameters
* learn best practices on how to interact with Mix using the source code as reference
* bootstrap your CI/CD workflows and activities

## Pre-Requisites

In order to use these samples, you'll need to make sure you've:

* registered with [Mix](https://mix.nuance.com)
* created a set of [run-time credentials](https://docs.mix.nuance.com/authorization/#client-credentials) in `Mix.dashboard`
* installed [Java run-time 11 or greater](https://docs.microsoft.com/en-us/java/openjdk/install)

> Note on building the samples
>  * Runnable jars are provided for download, but for those who would like to build the jars from source, these projects were built using Gradle 6.0

## Quick Start

1. Navigate to the Run-Time API that you're interested in and download the runnable jar.

    **Core Run-Time API's**
   * [ASR](/docs/asr)
   * [NLU](/docs/nlu)
   * [ASR + NLU](/docs/asr+nlu)
   * [TTS](/docs/tts)
   * [DLG](/docs/dlg)

    **Custom Resource API's**
   * [ASR Train](/docs/asr-train)
   * [NLU Train](/docs/nlu-train)
   * [TTS Storage](/docs/tts-storage)


2. All sample clients support the `-H` command-line arg to get usage details

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

3. Sample clients require:

   * A `configuration file` containing client credentials

        **Sample config**
        ```yaml
        {
            "enabled": true,
            "client_id": "<Provide Your Mix Application's Client ID>",
            "client_secret": "<Provide Your Mix Application's Client Secret>",
            "token_url": "https://auth.crt.nuance.com/oauth2/token"
        }
        ```
  
   * A `params file` containing API parameters. Content will be a json-representation of the run-time specific API's input parameters.

        **Sample params file for ASR `RecognitionInit` parameters**
        ```yaml
        {
            "parameters": {
                "language": "eng-USA",
                "topic": "GEN",
                "audioFormat": {
                    "pcm": {
                    "sampleRateHz": 16000
                    }
                },
                "utteranceDetectionMode": "MULTIPLE",
                "resultType": "PARTIAL",
                "recognitionFlags": {
                    "autoPunctuate": true,
                    "filterProfanity": false,
                    "includeTokenization": true,
                    "maskLoadFailures": true
                },
                "noInputTimeoutMs": 750,
                "utteranceEndSilenceMs": 900,
                "recognitionTimeoutMs": 0,
                "speechDetectionSensitivity": ".75",
                "maxHypotheses": 5
            },
            "resources": [
                {
                    "externalReference": {
                    "type": "DOMAIN_LM",
                    "uri": "urn:nuance-mix:tag:model/XAAS_TEST/mix.asr?=language=eng-USA"
                    }
                },
                {
                    "externalReference": {
                    "type": "SPEAKER_PROFILE"
                    }
                },
                {
                    "inline_wordset": "{\"NAMES\":[{\"literal\":\"Harrison Fourd\",\"spoken\":[\"Harrison Ford\"]},{\"literal\":\"Michael Douglas\"}]}"
                }
            ],
            "clientData": {
                "client_version": "1.0",
                "client_app": "xaas_asr_java_sample_client"
            }
        }
        ```
4. Open a terminal and run the sample client

    **Example**
    ```shell
    java -jar asr_client.jar -c /path/to/your/config.json -p /path/to/your/params.json
    ```