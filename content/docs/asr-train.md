---
title: A sample java gRPC client for ASRaaS.Train
linkTitle: ASR Train
---

This sample app uses the [Mix ASRaaS Training gRPC API](https://docs.mix.nuance.com/asr-grpc/v1/#training-api) to train (ie. create compiled) wordsets.

[Download Jar](/downloads/asr_train.jar)

[Sample Params](/downloads/params.asr-train.json)

## Usage Details

```
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

### Pre-Requisites

A deployed ASR model will be required to perform the ASR training requests.

{{% mix-project-pre-reqs api="asr" %}}

### Compile Wordset

```
java -jar build/libs/asr_train.jar -c config.mix-sample-app.json -p params.asr-train.json -cmd compile
2022-07-25 14:29:30.612 INFO    Version: 1.0.0
2022-07-25 14:29:30.732 INFO    CONNECTING ...
2022-07-25 14:29:30.734 INFO    AUTHENTICATING... 
2022-07-25 14:29:31.307 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED 
2022-07-25 14:29:32.485 INFO    CONNECTED 
2022-07-25 14:29:32.603 INFO     >>>>>>>>> {
  "wordset": "{\"COFFEE_SIZE\":[{\"literal\":\"short\",\"spoken\":[\"short\"]},{\"literal\":\"tall\",\"spoken\":[\"tall\"]},{\"literal\":\"grande\",\"spoken\":[\"grandè\"]},{\"literal\":\"venti\",\"spoken\":[\"venti\"]},{\"literal\":\"trenta\",\"spoken\":[\"trenta\"]}]}",
  "companionArtifactReference": {
    "uri": "urn:nuance-mix:tag:model/QUICK_START_COFFEE_APP_V1/mix.asr?\u003dlanguage\u003deng-USA"
  },
  "targetArtifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/QUICK_START_COFFEE_APP_V1/EXTRA_SIZES/eng-USA/mix.asr"
  },
  "metadata": {
    "tenant": "nuance",
    "project": "quick start coffee app",
    "client_version": "1.0.0"
  },
  "clientData": {
    "transaction_id": "e5d44845-b844-4ba7-afee-8e086946a9b9",
    "subscriber_id": "54d58d7ad7db42c1804a2ab2f827f87c",
    "user_id": "54d58d7ad7db42c1804a2ab2f827f87c",
    "deviceModel": "x86_64"
  }
}
2022-07-25 14:29:32.613 INFO    Adding x-client-request-id 51c58c79-016b-4502-b0d9-2050f3157a1d
2022-07-25 14:29:32.992 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
2022-07-25 14:29:32.993 INFO     <<<<<<<<< {"jobId":"ba8827d0-0c47-11ed-a78f-d1bbdb83bb3c","status":"JOB_STATUS_QUEUED","messages":[{"code":15,"message":"The following entities are not defined in the model:  COFFEE_SIZE."}]}
2022-07-25 14:29:43.000 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
2022-07-25 14:29:43.001 INFO     <<<<<<<<< {"jobId":"ba8827d0-0c47-11ed-a78f-d1bbdb83bb3c","status":"JOB_STATUS_PROCESSING"}
2022-07-25 14:29:52.925 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
2022-07-25 14:29:52.926 INFO     <<<<<<<<< {"jobId":"ba8827d0-0c47-11ed-a78f-d1bbdb83bb3c","status":"JOB_STATUS_PROCESSING"}
2022-07-25 14:29:55.395 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
2022-07-25 14:29:55.397 INFO     <<<<<<<<< {"jobId":"ba8827d0-0c47-11ed-a78f-d1bbdb83bb3c","status":"JOB_STATUS_COMPLETE"}
2022-07-25 14:29:55.398 INFO    Channel Shutdown
2022-07-25 14:29:55.399 INFO    DISCONNECTED 
```

> This compiled wordset can be referenced in the [`asr sample client`](/docs/asr) using the following params file `resources` entry:
> 
>  ```yaml
> {
>   "externalReference": {
>     "type": "COMPILED_WORDSET",
>     "uri": "urn:nuance-mix:tag:wordset:lang/QUICK_START_COFFEE_APP_V1/EXTRA_SIZES/eng-USA/mix.asr"
>   }
> }
>  ```

### Get Wordset Metadata

```
java -jar build/libs/asr_train.jar -c config.mix-sample-app.json -p params.asr-train.json -cmd metadata
2022-07-25 14:32:35.249 INFO    Version: 1.0.0
2022-07-25 14:32:35.361 INFO    CONNECTING ...
2022-07-25 14:32:35.364 INFO    AUTHENTICATING... 
2022-07-25 14:32:35.930 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED 
2022-07-25 14:32:36.968 INFO    CONNECTED 
 >>>>>>>>> {
  "artifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/QUICK_START_COFFEE_APP_V1/EXTRA_SIZES/eng-USA/mix.asr"
  }
}
2022-07-25 14:32:37.084 INFO    Adding x-client-request-id 4696b62f-7a04-4710-8818-abecf7a97ca2
2022-07-25 14:32:37.195 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
2022-07-25 14:32:37.196 INFO     <<<<<<<<< {"x_nuance_companion_checksum_sha256":"61f58496433492582e0a4652bb0aa10f3c41648566eb5553a5e42d7fbc4576d4","x_nuance_compiled_wordset_last_update":"2022-07-25T18:29:55.426Z","project":"quick start coffee app","content-type":"application/x-nuance-wordset-pkg","client_version":"1.0.0","tenant":"nuance","x_nuance_compiled_wordset_checksum_sha256":"705eef649cbc534deb67f5490870c2a4cf4e6ce6749a9be313dd5339df7f8894","x_nuance_wordset_content_checksum_sha256":"bae1ca22f07ef862e282d2d871fc7eae356f18db5b865dfb172a9aad068a6d25"}
2022-07-25 14:32:37.198 INFO    Channel Shutdown
2022-07-25 14:32:37.198 INFO    DISCONNECTED
```

### Delete Wordset

```
java -jar build/libs/asr_train.jar -c config.mix-sample-app.json -p params.asr-train.json -cmd delete
2022-07-25 14:42:44.171 INFO    Version: 1.0.0
2022-07-25 14:42:44.280 INFO    CONNECTING ...
2022-07-25 14:42:44.282 INFO    AUTHENTICATING... 
2022-07-25 14:42:44.897 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED 
2022-07-25 14:42:46.001 INFO    CONNECTED 
 >>>>>>>>> {
  "artifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/QUICK_START_COFFEE_APP_V1/EXTRA_SIZES/eng-USA/mix.asr"
  }
}
2022-07-25 14:42:46.120 INFO    Adding x-client-request-id 1bd36781-bcab-47da-b396-1e19b6abf76a
2022-07-25 14:42:46.255 INFO     <<<<<<<<< {"statusCode":"OK","httpTransCode":200}
2022-07-25 14:42:46.257 INFO    Channel Shutdown
2022-07-25 14:42:46.258 INFO    DISCONNECTED 
```
