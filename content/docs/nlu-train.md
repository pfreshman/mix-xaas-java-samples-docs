---
title: A sample java gRPC client for NLUaaS.Train
linkTitle: NLU Train
---

This sample app uses the [Mix NLUaaS Training gRPC API](https://docs.mix.nuance.com/nlu-grpc/v1/#wordset-grpc-api) to train (ie. create compiled) wordsets.

[Download Jar](/downloads/nlu_train.jar)

[Sample Params](/downloads/params.nlu-train.json)

## Usage Details

```
$ java -jar build/libs/nlu_train.jar -H
Version: 1.0.0
usage: java -jar nlu_train.jar [-H|--help] [-cmd|--command <value>] [-h|--hostname <value>]
                                [-c|--configFile <value>] [-p|--params <value>] [-u|--userid <value>]
                                [-l|--logFile <value>] [-s|--insecure] [-tt|--trxnTimeout <value>] [-ct|--connectTimeout <value>]
                                Use Nuance ASRaaS.Train to create compiled wordsets available as ASR Resources for your app

Arguments:

 -H,--help                               Print this help information
 -cmd,--command <command>                Wordset Command. Available commands are 'compile', 'delete', and 'metadata'. Default: compile
 -h,--hostname <hostname>                ASRaaS server URL host:port. Default: nlu.api.nuance.com:443
 -u,--userID <userID>                    User ID to associate with the wordset. Otherwise, the wordset will be available app-wide for all users. Default:
 -c,--config <config>                    config file containing client credentials (client_id and
                                         client_secret) and oauth server URL. Default: config.json
 -p,--params <params>                    json file containing recognition parameters. Default: params.json
 -l,--logFile <logFile>                  log file path and name. Default: nlu-train.log
 -s,--insecure                           By default a secure TLS connection is established with ASRaaS. Specify this flag if connecting to a non-secure deployment of ASRaaS (e.g. on-prem). Default: false
 -tt,--trxnTimeout <trxnTimeout>         Time in ms to wait for nlu request to complete. Default: 60000
 -ct,--connectTimeout <connectTimeout>   Time in ms to wait for connection. Default: 2000
 ```

## Running an NLU.Train Request

### Pre-Requisites

A deployed NLU model will be required to perform the NLU training requests.

{{% mix-project-pre-reqs %}}

### Compile Wordset

```
java -jar build/libs/nlu_train.jar -c config.mix-sample-app.json -p params.nlu-train.json -cmd compile
2022-07-25 15:08:04.673 INFO    Version: 1.0.0
2022-07-25 15:08:04.776 INFO    CONNECTING ...
2022-07-25 15:08:04.777 INFO    AUTHENTICATING... 
2022-07-25 15:08:05.309 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED 
2022-07-25 15:08:06.132 INFO    CONNECTED 
2022-07-25 15:08:06.255 INFO     >>>>>>>>> {
  "wordset": "{\"COFFEE_SIZE\":[{\"canonical\":\"x-sm\",\"literal\":\"short\"},{\"canonical\":\"sm\",\"literal\":\"tall\"},{\"canonical\":\"md\",\"literal\":\"grandè\"},{\"canonical\":\"lg\",\"literal\":\"venti\"},{\"canonical\":\"x-lg\",\"literal\":\"trenta\"}]}",
  "companionArtifactReference": {
    "uri": "urn:nuance-mix:tag:model/QUICK_START_COFFEE_APP_V1/mix.nlu?\u003dlanguage\u003deng-USA"
  },
  "targetArtifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/QUICK_START_COFFEE_APP_V1/EXTRA_SIZES/eng-USA/mix.nlu"
  },
  "metadata": {
    "tenant": "nuance",
    "project": "quick start coffee app",
    "client_version": "1.0.0"
  },
  "clientData": {
    "transaction_id": "4c922bdf-b158-4e25-a340-c3188c75b4c2",
    "subscriber_id": "54d58d7ad7db42c1804a2ab2f827f87c",
    "user_id": "54d58d7ad7db42c1804a2ab2f827f87c",
    "deviceModel": "x86_64"
  }
}
2022-07-25 15:08:06.265 INFO    Adding x-client-request-id 059f4d2b-782c-420d-bef0-eb2e1f54703c
2022-07-25 15:08:06.345 INFO    Received x-request-id 01a5a540-5bd4-992d-8253-662003850482
2022-07-25 15:08:06.378 WARNING  <<<<<<<<< {"statusCode":"OK"}
2022-07-25 15:08:06.379 INFO     <<<<<<<<< {"status":"JOB_STATUS_PROCESSING"}
2022-07-25 15:08:07.083 WARNING  <<<<<<<<< {"statusCode":"OK"}
2022-07-25 15:08:07.084 INFO     <<<<<<<<< {"status":"JOB_STATUS_COMPLETE"}
2022-07-25 15:08:07.086 INFO    Channel Shutdown
2022-07-25 15:08:07.087 INFO    DISCONNECTED
```

> This compiled wordset can be referenced in the [`nlu sample client`](/docs/nlu) using the following params file `resources` entry:
> 
>  ```yaml
> {
>   "externalReference": {
>     "type": "COMPILED_WORDSET",
>     "uri": "urn:nuance-mix:tag:wordset:lang/QUICK_START_COFFEE_APP_V1/EXTRA_SIZES/eng-USA/mix.nlu"
>   }
> }
>  ```

### Get Wordset Metadata

```
java -jar build/libs/nlu_train.jar -c config.mix-sample-app.json -p params.nlu-train.json -cmd metadata
2022-07-25 15:09:39.589 INFO    Version: 1.0.0
2022-07-25 15:09:39.716 INFO    CONNECTING ...
2022-07-25 15:09:39.721 INFO    AUTHENTICATING... 
2022-07-25 15:09:39.740 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED 
2022-07-25 15:09:40.884 INFO    CONNECTED 
 >>>>>>>>> {
  "artifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/QUICK_START_COFFEE_APP_V1/EXTRA_SIZES/eng-USA/mix.nlu"
  }
}
2022-07-25 15:09:41.003 INFO    Adding x-client-request-id e78bfc50-c955-4107-a40f-7d39f64a518d
2022-07-25 15:09:41.084 INFO    Received x-request-id e10243a0-a6b6-9f18-9f35-37ebd44110e0
2022-07-25 15:09:41.110 WARNING  <<<<<<<<< {"statusCode":"OK"}
2022-07-25 15:09:41.111 INFO     <<<<<<<<< {"x_nuance_companion_checksum_sha256":"48da00e644d91c367180d6f8470598fa43b9d9b1683e7b5c5ad5dea3598918d9","x_nuance_compiled_wordset_last_update":"2022-07-25T19:08:07.001Z","project":"quick start coffee app","client_version":"1.0.0","tenant":"nuance","x_nuance_wordset_content_checksum_sha256":"e9efec369e8e1d6eb4e23b1f5f0d01f89da02a02dbed39cab911889c749f434b","x_nuance_compiled_wordset_checksum_sha256":"7df857dc2b248d9f96008ca4ce9366b3aee84d83df44df087cd92f3c291b1be9"}
2022-07-25 15:09:41.113 INFO    Channel Shutdown
2022-07-25 15:09:41.113 INFO    DISCONNECTED
```

### Delete Wordset

```
java -jar build/libs/nlu_train.jar -c config.mix-sample-app.json -p params.nlu-train.json -cmd delete
2022-07-25 15:11:10.842 INFO    Version: 1.0.0
2022-07-25 15:11:10.934 INFO    CONNECTING ...
2022-07-25 15:11:10.936 INFO    AUTHENTICATING... 
2022-07-25 15:11:10.951 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED 
2022-07-25 15:11:11.826 INFO    CONNECTED 
 >>>>>>>>> {
  "artifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/QUICK_START_COFFEE_APP_V1/EXTRA_SIZES/eng-USA/mix.nlu"
  }
}
2022-07-25 15:11:11.937 INFO    Adding x-client-request-id 1bb94dde-7e13-41ec-a95b-e32f580de080
2022-07-25 15:11:12.005 INFO    Received x-request-id 4a41db1f-c034-9393-9775-01ec98bd066d
2022-07-25 15:11:12.065 WARNING  <<<<<<<<< {"statusCode":"OK"}
2022-07-25 15:11:12.067 INFO    Channel Shutdown
2022-07-25 15:11:12.068 INFO    DISCONNECTED
```
