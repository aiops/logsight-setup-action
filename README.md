<a href="https://logsight.ai/"><img src="https://logsight.ai/assets/img/logol.png" width="150"/></a>

**Your Ally for Intelligent DevOps Pipelines**

logsight.ai infuses deep learning and AI-powered analytics to enable continuous software delivery and proactive troubleshooting.

# log-setup-action

This action is used to initialize connection to logsight.ai, create an application, and start collecting logs with FluentBit. The action used in combination with https://github.com/aiops/logsight-verification-action enables comparison of logs from a new deployment of a service with the logs from the previous running deployment using
logsight.ai Stage Verifier (https://docs.logsight.ai/#/monitor_deployments/stage_verifier)

## Inputs
```text
inputs:
  username:
    description: 'Basic auth username.'
    required: true
  password:
    description: 'Basic auth password.'
    required: true
  application_name:
    description: 'Application name.'
    required: false
    default: ${{ github.ref }}
  tag:
    description: 'Tag that refers to the current version of your software (e.g., {{ $github.sha }})'
    required: false
    default: ${{ github.sha }}
  fluentbit_enable:
    description: 'Flag that enables or disables FluentBit log collection. If disabled, please make sure you have your own input that forwards logs to logsight.ai (https://docs.logsight.ai/#/./send_logs/fluentbit).'
    required: false
    default: 1
  fluentbit_inputname:
    description: 'FluentBit inputs (https://docs.fluentbit.io/manual/pipeline/inputs)'
    required: false
    default: 'tail'
  fluentbit_filelocation:
    description: 'FluentBit file location if using tail as input (https://docs.fluentbit.io/manual/pipeline/inputs/tail)'
    required: false
    default: '/containers/*/*.log'
  fluentbit_matchpattern:
    description: 'FluentBit match pattern. Use * if you want match all messages (https://docs.fluentbit.io/manual/concepts/key-concepts#match)'
    required: false
    default: '*'
  fluentbit_message:
    description: 'FluentBit message string that specifies the Key that contains the log message. (e.g., if using Tail messages automatically have the Key message)'
    required: false
    default: 'message'
  fluentbit_host:
    description: 'Logsight URL. Use logsight.ai for the web service, or 127.0.0.1 for an on-premise solution'
    required: false
    default: 'demo.logsight.ai'
  fluentbit_port:
    description: 'Logsight PORT. Use 443'
    required: false
    default: '443'
  fluentbit_config:
    description: 'If you want your own fluentbit config, use ONLY this entry to specifiy the config file location. If specified, this will override all other fluentbit related variables.'
    required: false
    default: ''

```

If you provide your own fluentbit config file please refer to https://docs.fluentbit.io/manual/.

## Outputs

#### `application_id`
The ID of the application that was created. This id is used by https://github.com/aiops/logsight-verification-action.

## Example usage

```
uses: actions/logsight-init@master
with:
  username: LOGSIGHT_USERNAME
  password: LOGSIGHT_PASSWORD
  application-name: {{ github.ref }}  # here can be any string name
  
#the rest of the input fields can be set optionally.
```