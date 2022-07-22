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
$ java -jar build/libs/nlu_train.jar -c config.my-config.json -cmd compile
2021-05-14 09:59:19.930 INFO    Version: 1.0.0
2021-05-14 09:59:20.039 INFO    CONNECTING (NLUAAS.WORDSET)...
2021-05-14 09:59:20.040 INFO    AUTHENTICATING... (NLUAAS.WORDSET)
2021-05-14 09:59:20.044 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS.WORDSET)
2021-05-14 09:59:20.923 INFO    CONNECTING (NLUAAS.WORDSET)...
2021-05-14 09:59:20.935 INFO     >>>>>>>>> {
  "wordset": "{\"NAME\":[{\"literal\":\"Spiderman\",\"canonical\":\"Spider-man\",\"spoken\":[\"spiderman\"]}, {\"literal\":\"Vision\",\"canonical\":\"Vi-sion\",\"spoken\":[\"vision\"]}]}",
  "companionArtifactReference": {
    "uri": "urn:nuance-mix:tag:model/HELLO_WORLD/mix.nlu?\u003dlanguage\u003deng-USA"
  },
  "targetArtifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/HELLO_WORLD/EXTRA_NAMES/eng-USA/mix.nlu"
  },
  "metadata": {
    "tenant": "nuance",
    "project": "test",
    "client_version": "1.0.0"
  },
  "clientData": {
    "transaction_id": "7ea4f330-3926-4de1-9ce8-dc8868082851",
    "subscriber_id": "4a9817feea354f6aa39acf02ec723794",
    "user_id": "4a9817feea354f6aa39acf02ec723794",
    "deviceModel": "x86_64"
  }
}
2021-05-14 09:59:22.477 INFO    CONNECTED (NLU.WORDSET)
2021-05-14 09:59:23.323 WARNING  <<<<<<<<< {"statusCode":"OK"}
2021-05-14 09:59:23.324 INFO     <<<<<<<<< {"status":"JOB_STATUS_PROCESSING"}
2021-05-14 09:59:27.327 WARNING  <<<<<<<<< {"statusCode":"OK"}
2021-05-14 09:59:27.329 INFO     <<<<<<<<< {"status":"JOB_STATUS_COMPLETE"}
```

### Get Wordset Metadata

```
$ java -jar build/libs/nlu_train.jar -c config.my-config.json -cmd metadata
2021-05-14 09:59:35.052 INFO    Version: 1.0.0
2021-05-14 09:59:35.146 INFO    CONNECTING (NLUAAS.WORDSET)...
2021-05-14 09:59:35.147 INFO    AUTHENTICATING... (NLUAAS.WORDSET)
2021-05-14 09:59:35.150 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS.WORDSET)
2021-05-14 09:59:35.796 INFO    CONNECTING (NLUAAS.WORDSET)...
 >>>>>>>>> {
  "artifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/HELLO_WORLD/EXTRA_NAMES/eng-USA/mix.nlu"
  }
}
2021-05-14 09:59:36.481 INFO    CONNECTED (NLU.WORDSET)
2021-05-14 09:59:36.714 WARNING  <<<<<<<<< {"statusCode":"OK"}
2021-05-14 09:59:36.715 INFO     <<<<<<<<< {"x_nuance_companion_checksum_sha256":"11bf71c9e89a6d68c278ae9d5009d679e192e4b83048852cc1bc65d71e64b7c9","x_nuance_compiled_wordset_last_update":"2021-05-14T13:59:26.992Z","project":"test","client_version":"1.0.0","tenant":"nuance","x_nuance_wordset_content_checksum_sha256":"6aee26b1c5f902db43bb4c85aeca80c06aa332cf68eeaefc094ef250802bee1c","x_nuance_compiled_wordset_checksum_sha256":"0440bdc06815de9d17f07e5b990dcdbd22bc33ea53cd20e118e0c2c27f49ee7f"}
```

### Delete Wordset

```
$ java -jar build/libs/nlu_train.jar -c config.my-config.json -cmd delete
2021-05-14 12:10:56.746 INFO    Version: 1.0.0
2021-05-14 12:10:56.852 INFO    CONNECTING (NLUAAS.WORDSET)...
2021-05-14 12:10:56.853 INFO    AUTHENTICATING... (NLUAAS.WORDSET)
2021-05-14 12:10:57.400 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED (NLUAAS.WORDSET)
2021-05-14 12:10:58.073 INFO    CONNECTING (NLUAAS.WORDSET)...
 >>>>>>>>> {
  "artifactReference": {
    "uri": "urn:nuance-mix:tag:wordset:lang/HELLO_WORLD/EXTRA_NAMES/eng-USA/mix.nlu"
  }
}
2021-05-14 12:10:59.388 INFO    CONNECTED (NLU.WORDSET)
2021-05-14 12:10:59.549 WARNING  <<<<<<<<< {"statusCode":"OK"}
```
