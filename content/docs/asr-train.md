---
title: A sample java gRPC client for ASRaaS.Train
linkTitle: ASR Train
---

This sample app uses the Mix3 ASRaaS gRPC API to train (ie. create compiled) wordsets.

[Download Jar](/downloads/asr_train.jar)

## Usage Details

```shell
$ java -jar build/libs/asr_train.jar -H
Version: 1.0.0
usage: java -jar asr_train.jar [-H|--help] [-cmd|--command <value>] [-h|--hostname <value>]
                                [-c|--configFile <value>] [-p|--params <value>] [-u|--userid <value>]
                                [-l|--logFile <value>] [-s|--insecure] [-tt|--trxnTimeout <value>] [-ct|--connectTimeout <value>]
                                Use Nuance ASRaaS.Train to create compiled wordsets available as ASR Resources for your app

Arguments:

 -H,--help                               Print this help information
 -cmd,--command <command>                Wordset Command. Available commands are 'compile', 'delete', and 'metadata'. Default: compile
 -h,--hostname <hostname>                ASRaaS server URL host:port. Default: asr.api.nuance.com:443
 -u,--userID <userID>                    User ID to associate with the wordset. Otherwise, the wordset will be available app-wide for all users. Default:
 -c,--config <config>                    config file containing client credentials (client_id and
                                         client_secret) and oauth server URL. Default: config.json
 -p,--params <params>                    json file containing recognition parameters. Default: params.json
 -l,--logFile <logFile>                  log file path and name. Default: asr-train.log
 -s,--insecure                           By default a secure TLS connection is established with ASRaaS. Specify this flag if connecting to a non-secure deployment of ASRaaS (e.g. on-prem). Default: false
 -tt,--trxnTimeout <trxnTimeout>         Time in ms to wait for asr request to complete. Default: 60000
 -ct,--connectTimeout <connectTimeout>   Time in ms to wait for connection. Default: 2000
 ```

## Running an ASR.Train Request

### Compile Wordset

```
$ java -jar build/libs/asr_train.jar -c config.mix-beta.json -cmd compile
2021-05-13 16:28:14.531 INFO    Version: 1.0.0
2021-05-13 16:28:14.627 INFO    CONNECTING (ASR.WORDSET)...
2021-05-13 16:28:14.629 INFO    AUTHENTICATING... (ASRAAS)
2021-05-13 16:28:14.632 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (ASRAAS)
2021-05-13 16:28:16.309 INFO    CONNECTING (ASR.WORDSET)...
2021-05-13 16:28:16.323 INFO     >>>>>>>>> {
  "wordset": "{\"NAME\":[{\"literal\":\"Spiderman\",\"spoken\":[\"spiderman\"]}, {\"literal\":\"Vision\",\"spoken\":[\"vision\"]}]}",
  "companionArtifactReference": {
    "uri": "urn:nuance-mix:tag:model/HELLO_WORLD/mix.asr?\u003dlanguage\u003deng-USA"
  },
  "targetArtifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/HELLO_WORLD/EXTRA_NAMES/eng-USA/mix.asr"
  },
  "metadata": {
    "tenant": "nuance",
    "project": "test",
    "client_version": "1.0.0"
  },
  "clientData": {
    "transaction_id": "e4dca311-1a29-4973-b843-3d08af4b8623",
    "subscriber_id": "4a9817feea354f6aa39acf02ec723794",
    "user_id": "4a9817feea354f6aa39acf02ec723794",
    "deviceModel": "x86_64"
  }
}
2021-05-13 16:28:17.626 INFO    CONNECTED (ASR.WORDSET)
2021-05-13 16:28:17.804 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
2021-05-13 16:28:17.805 INFO     <<<<<<<<< {"jobId":"c04617d0-b429-11eb-ae12-9978e130c174","status":"JOB_STATUS_QUEUED"}
2021-05-13 16:28:18.183 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
2021-05-13 16:28:18.183 INFO     <<<<<<<<< {"jobId":"c04617d0-b429-11eb-ae12-9978e130c174","status":"JOB_STATUS_COMPLETE"}
```

### Get Wordset Metadata

```
$ java -jar build/libs/asr_train.jar -c config.mix-beta.json -cmd metadata
2021-05-13 16:42:16.400 INFO    Version: 1.0.0
2021-05-13 16:42:16.500 INFO    CONNECTING (ASRAAS.WORDSET)...
2021-05-13 16:42:16.501 INFO    AUTHENTICATING... (ASRAAS.WORDSET)
2021-05-13 16:42:16.504 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (ASRAAS.WORDSET)
2021-05-13 16:42:17.195 INFO    CONNECTING (ASRAAS.WORDSET)...
 >>>>>>>>> {
  "artifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/HELLO_WORLD/EXTRA_NAMES/eng-USA/mix.asr"
  }
}
2021-05-13 16:42:17.525 INFO    CONNECTED (ASR.WORDSET)
2021-05-13 16:42:17.636 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
2021-05-13 16:42:17.638 INFO     <<<<<<<<< {"x_nuance_companion_checksum_sha256":"067c446ebe28fbd344f9d5407723b98ab0fc2a24657b151ea6cb22902b304df8","x_nuance_compiled_wordset_last_update":"2021-05-13T20:41:37.974Z","project":"test","content-type":"application/x-nuance-wordset-pkg","client_version":"1.0.0","tenant":"nuance","x_nuance_compiled_wordset_checksum_sha256":"ff60c99cb0a8d35291c670f64ab881875bf23b1992a42dc1e154ea22e2d0b5dd","x_nuance_wordset_content_checksum_sha256":"ab2ceb21f086ae5bb7fbdb2547a49e19db05ce77206e134c9a6cc7992602f475"}
```

### Delete Wordset

```
$ java -jar build/libs/asr_train.jar -c config.mix-beta.json -cmd delete
2021-05-13 16:42:53.546 INFO    Version: 1.0.0
2021-05-13 16:42:53.645 INFO    CONNECTING (ASRAAS.WORDSET)...
2021-05-13 16:42:53.646 INFO    AUTHENTICATING... (ASRAAS.WORDSET)
2021-05-13 16:42:53.650 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (ASRAAS.WORDSET)
2021-05-13 16:42:54.281 INFO    CONNECTING (ASRAAS.WORDSET)...
 >>>>>>>>> {
  "artifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/HELLO_WORLD/EXTRA_NAMES/eng-USA/mix.asr"
  }
}
2021-05-13 16:42:54.659 INFO    CONNECTED (ASR.WORDSET)
2021-05-13 16:42:54.800 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
```
