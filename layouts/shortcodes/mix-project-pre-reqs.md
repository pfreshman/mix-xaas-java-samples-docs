{{ $api := .Get "api" | default "none" }}

Perform the following steps in `Mix.dashboard` and then provide the appropriate URN:

1. Create a Project, Locale: en-US
2. Import the NLU Model: [`order_coffee.trsx`](https://docs.mix.nuance.com/downloads/mix-starting/order_coffee.trsx)
3. Import the Dialog Model: [`order_coffee.json`](https://docs.mix.nuance.com/downloads/mix-starting/order_coffee.json)
4. [Create Builds](https://docs.mix.nuance.com/builds/#create-builds) of your project's models
5. [Create and deploy an Application Configuration](https://docs.mix.nuance.com/app-configs/#create-and-deploy-applications-procedure), defining a context tag of `QUICK_START_COFFEE_APP_V1` 

{{ if ne $api "asr" }}
> In order to leverage dynamic wordsets, you will:
>  * first need to update the NLU model by configuring `COFFEE_SIZE` and `COFFEE_TYPE` [entities](https://docs.mix.nuance.com/mix-nlu/#entities) to be `dynamic`
>  * and then, for dialog, create two dynamic data entity [variables](https://docs.mix.nuance.com/dialog-app-spec/#variable). To follow the examples provided in this documentation, name them `coffee_types` and `coffee_sizes`
{{ end }}

>
Check the [Quick Start](https://mix.nuance.com/v3/documentation/mix-starting/#quick-start) for more information.
