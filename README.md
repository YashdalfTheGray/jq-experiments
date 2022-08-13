# jq-experiments

fun with jq and its many functions

## Getting started

Generate your own sample JSON here - https://www.mockaroo.com/

## Experiments

### Merge two JSON files containing objects

```
jq -s '.[0] * .[1]' file1.json file2.json > result.json
```

### Join a particular JSON list into one string

The property should be a JSON array

```
jq -r '.<property_name> | join(" ")'
```

### Get the current runtime of ECS tasks

Requires the AWS CLI and appropriate credentials

```
aws ecs describe-tasks --tasks $(aws ecs list-tasks --cluster <CLUSTER_NAME> --desired-status RUNNING | jq -r '.taskArns | join(" ")') --cluster <CLUSTER_NAME> | jq '.tasks | [ .[] | { taskArn: .taskArn, containerInstanceArn: .containerInstanceArn?, startedAt: .startedAt, runningTimeInSeconds: ((now | localtime | mktime) - (.startedAt | split(".")[0] + "Z" | fromdate)) } ]'
```
