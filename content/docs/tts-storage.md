---
title: A sample java gRPC client for TTSaaS.Storage
linkTitle: TTS Storage
---

This sample app uses the [Mix TTSaaS Storage gRPC API](https://docs.mix.nuance.com/tts-grpc/v1/#storage-api) to upload storage.

[Download Jar](/downloads/tts_storage.jar)

## Usage Details

```
$ java -jar build/libs/tts_storage.jar -H
Version: 1.0.0
usage: java -jar tts_storage.jar [-H|--help] [-h|--hostname <value>] [-c|--configFile <value>]
                                -cmd|--command [UPLOAD, DELETE]
                                [-f|--file <value>] [-t|--contextTag <value>] [-n|--name <value>] [-tp|--type <value>]
                                [-lc|--languageCode <value>] [-v|--voice <value>] [-vm|--voiceModel <value>] [-vv|--voiceVersion <value>]
                                [-vsv|--visualStudioVersion <value>] [-u|--uri <value>
                                [-l|--logFile <value>] [-s|--insecure] [-tt|--trxnTimeout <value>] [-ct|--connectTimeout <value>]
                                Use Nuance TTSaaS.Storage to upload TTS Storage data.

Arguments:

 -H,--help                                                Print this help information
 -h,--hostname <hostname>                                 TTSaaS server URL host:port. Default: tts.api.nuance.com:443
 -c,--config <config>                                     config file containing client credentials (client_id and
                                                          client_secret) and oauth server URL. Default: config.json
 -p,--params <params>                                     params file containing params. Default: params.json
 -cmd,--command <command>                                 TTSaaS storage command. Available Comamands: [UPLOAD, DELETE]. Default: upload
 -f,--file <file>                                         File to upload. If an ActivePrompt Database, must be packaged as a zip.
 -t,--contextTag <contextTag>                             A group name, either existing or new. If it doesn't exist, it will be created.
 -n,--name <name>                                         A name for the resource within the context.
 -tp,--type <type>                                        The resource type, one of: [ACTIVEPROMPT, USER_DICTIONARY, TEXT_RULESET, WAV]
 -lc,--languageCode <languageCode>                        IETF language code. Required when type is [user_dictionary, text_ruleset]
 -v,--voice <voice>                                       A Nuance voice. Required when type is activeprompt.
 -vm,--voiceModel <voiceModel>                            The voice model. Required when type is activeprompt.
 -vv,--voiceVersion <voiceVersion>                        The version of the voice. Required when type is activeprompt.
 -vsv,--vocalizerStudioVersion <vocalizerStudioVersion>   The Nuance Vocalizer Studio version. Required when type is activeprompt.
 -u,--uri <uri>                                           For the delete operation, the URN of the object to delete.
 -l,--logFile <logFile>                                   log file path and name. Default: tts-storage.log
 -s,--insecure                                            By default a secure TLS connection is established with TTSaaS. Specify this flag if connecting to a non-secure deployment of TTSaaS (e.g. on-prem). Default: false
 -tt,--trxnTimeout <trxnTimeout>                          Time in ms to wait for request to complete. Default: 60000
 -ct,--connectTimeout <connectTimeout>                    Time in ms to wait for connection. Default: 2000
 ```

## Running a TTS.Storage Request

### Upload a Text Ruleset
```
$ java -jar build/libs/tts_storage.jar -cmd UPLOAD -c ../../config.my-config.json  -tp TEXT_RULESET -f ruleset.txt -t 12345 -name parms -lc en-US -ct 60000
2021-10-14 12:39:53.580 INFO    Version: 1.0.0
2021-10-14 12:39:53.740 INFO    CONNECTING ...
2021-10-14 12:39:53.742 INFO    AUTHENTICATING...
2021-10-14 12:39:53.767 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED
2021-10-14 12:39:56.930 INFO    CONNECTED
2021-10-14 12:39:57.045 INFO    Adding x-client-request-id 396d03c3-f81b-44a4-82f5-9f38f4db1407
2021-10-14 12:39:57.172 INFO     <<<<<<<<< {"statusCode":"OK"}
2021-10-14 12:39:57.177 INFO     <<<<<<<<< {"status":{"statusCode":"OK"},"uri":"urn:nuance-mix:tag:tuning:lang/12345/parms/en-us/mix.tts?type\u003dtextruleset"}
2021-10-14 12:39:57.185 INFO    Channel Shutdown
2021-10-14 12:39:57.185 INFO    DISCONNECTED
```

### Upload a User Dictionary

```
$ java -jar build/libs/tts_storage.jar -cmd UPLOAD -c ../../config.my-config.json  -tp USER_DICTIONARY -f hahaha.dcb -t 12345
 -name parms -lc en-US -ct 60000
2021-10-15 09:40:55.987 INFO    Version: 1.0.0
2021-10-15 09:40:56.082 INFO    CONNECTING ...
2021-10-15 09:40:56.083 INFO    AUTHENTICATING...
2021-10-15 09:40:56.096 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED
2021-10-15 09:40:58.495 INFO    CONNECTED
2021-10-15 09:40:58.608 INFO    Adding x-client-request-id 33878e36-3fbe-45af-aa75-f06c4da7b23b
2021-10-15 09:40:58.787 INFO     <<<<<<<<< {"statusCode":"OK"}
2021-10-15 09:40:58.790 INFO     <<<<<<<<< {"status":{"statusCode":"OK"},"uri":"urn:nuance-mix:tag:tuning:lang/12345/parms/en-us/mix.tts?type\u003duserdict"}
2021-10-15 09:40:58.796 INFO    Channel Shutdown
2021-10-15 09:40:58.797 INFO    DISCONNECTED
```

### Delete a Previously Uploaded User Dictionary

```
$ java -jar build/libs/tts_storage.jar -cmd DELETE -c ../../config.my-config.json -tp USER_DICTIONARY -lc en-US -u urn:nuance-mix:tag:tuning:lang/12345/parms/en-us/mix.tts?type\u003duserdict
2021-10-15 09:42:35.049 INFO    Version: 1.0.0
2021-10-15 09:42:35.139 INFO    CONNECTING ...
2021-10-15 09:42:35.141 INFO    AUTHENTICATING...
2021-10-15 09:42:36.273 INFO    AUTHENTICATION SUCCEEDED - TOKEN CREATED
2021-10-15 09:42:37.821 INFO    CONNECTED
 >>>>>>>>> {
  "uri": "urn:nuance-mix:tag:tuning:lang/12345/parms/en-us/mix.tts?typeu003duserdict"
}
2021-10-15 09:42:37.910 INFO    Adding x-client-request-id eaa9bc30-cb1d-4073-bc61-41dd04f803d0
2021-10-15 09:42:38.023 INFO     <<<<<<<<< {"statusCode":"OK"}
2021-10-15 09:42:38.032 INFO    Channel Shutdown
2021-10-15 09:42:38.032 INFO    DISCONNECTED
```

