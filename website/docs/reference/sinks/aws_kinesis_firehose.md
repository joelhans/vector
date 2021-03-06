---
last_modified_on: "2020-04-22"
delivery_guarantee: "at_least_once"
component_title: "AWS Kinesis Firehose"
description: "The Vector `aws_kinesis_firehose` sink batches `log` events to Amazon Web Service's Kinesis Data Firehose via the `PutRecordBatch` API endpoint."
event_types: ["log"]
function_category: "transmit"
issues_url: https://github.com/timberio/vector/issues?q=is%3Aopen+is%3Aissue+label%3A%22sink%3A+aws_kinesis_firehose%22
operating_systems: ["Linux","MacOS","Windows"]
sidebar_label: "aws_kinesis_firehose|[\"log\"]"
source_url: https://github.com/timberio/vector/tree/master/src/sinks/aws_kinesis_firehose.rs
status: "prod-ready"
title: "AWS Kinesis Firehose Sink"
unsupported_operating_systems: []
---

import Fields from '@site/src/components/Fields';
import Field from '@site/src/components/Field';
import SVG from 'react-inlinesvg';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

The Vector `aws_kinesis_firehose` sink
[batches](#buffers--batches) [`log`][docs.data-model.log] events to [Amazon Web
Service's Kinesis Data Firehose][urls.aws_kinesis_firehose] via the
[`PutRecordBatch` API
endpoint](https://docs.aws.amazon.com/firehose/latest/APIReference/API_PutRecordBatch.html).

<!--
     THIS FILE IS AUTOGENERATED!

     To make changes please edit the template located at:

     website/docs/reference/sinks/aws_kinesis_firehose.md.erb
-->

## Configuration

<Tabs
  block={true}
  defaultValue="common"
  values={[{"label":"Common","value":"common"},{"label":"Advanced","value":"advanced"}]}>
<TabItem value="common">

```toml title="vector.toml"
[sinks.my_sink_id]
  # General
  type = "aws_kinesis_firehose" # required
  inputs = ["my-source-id"] # required
  healthcheck = true # optional, default
  region = "us-east-1" # required, required when endpoint = ""
  stream_name = "my-stream" # required

  # Encoding
  encoding.codec = "json" # required
```

</TabItem>
<TabItem value="advanced">

```toml title="vector.toml"
[sinks.my_sink_id]
  # General
  type = "aws_kinesis_firehose" # required
  inputs = ["my-source-id"] # required
  assume_role = "arn:aws:iam::123456789098:role/my_role" # optional, no default
  endpoint = "127.0.0.0:5000/path/to/service" # optional, no default, relevant when region = ""
  healthcheck = true # optional, default
  region = "us-east-1" # required, required when endpoint = ""
  stream_name = "my-stream" # required

  # Batch
  batch.max_events = 500 # optional, default, events
  batch.timeout_secs = 1 # optional, default, seconds

  # Buffer
  buffer.type = "memory" # optional, default
  buffer.max_events = 500 # optional, default, events, relevant when type = "memory"
  buffer.max_size = 104900000 # required, bytes, required when type = "disk"
  buffer.when_full = "block" # optional, default

  # Encoding
  encoding.codec = "json" # required
  encoding.except_fields = ["timestamp", "message", "host"] # optional, no default
  encoding.only_fields = ["timestamp", "message", "host"] # optional, no default
  encoding.timestamp_format = "rfc3339" # optional, default

  # Request
  request.in_flight_limit = 5 # optional, default, requests
  request.rate_limit_duration_secs = 1 # optional, default, seconds
  request.rate_limit_num = 5 # optional, default
  request.retry_attempts = -1 # optional, default
  request.retry_initial_backoff_secs = 1 # optional, default, seconds
  request.retry_max_duration_secs = 10 # optional, default, seconds
  request.timeout_secs = 30 # optional, default, seconds
```

</TabItem>
</Tabs>

<Fields filters={true}>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[]}
  groups={[]}
  name={"batch"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"table"}
  unit={null}
  warnings={[]}
  >

### batch

Configures the sink batching behavior.



<Fields filters={false}>
<Field
  common={true}
  defaultValue={500}
  enumValues={null}
  examples={[500]}
  groups={[]}
  name={"max_events"}
  path={"batch"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"events"}
  warnings={[]}
  >

#### max_events

The maximum size of a batch, in events, before it is flushed.

 See [Buffers & Batches](#buffers--batches) for more info.


</Field>
<Field
  common={true}
  defaultValue={1}
  enumValues={null}
  examples={[1]}
  groups={[]}
  name={"timeout_secs"}
  path={"batch"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"seconds"}
  warnings={[]}
  >

#### timeout_secs

The maximum age of a batch before it is flushed.

 See [Buffers & Batches](#buffers--batches) for more info.


</Field>
</Fields>

</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[]}
  groups={[]}
  name={"buffer"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"table"}
  unit={null}
  warnings={[]}
  >

### buffer

Configures the sink specific buffer behavior.



<Fields filters={false}>
<Field
  common={true}
  defaultValue={"memory"}
  enumValues={{"memory":"Stores the sink's buffer in memory. This is more performant, but less durable. Data will be lost if Vector is restarted forcefully.","disk":"Stores the sink's buffer on disk. This is less performant, but durable. Data will not be lost between restarts."}}
  examples={["memory","disk"]}
  groups={[]}
  name={"type"}
  path={"buffer"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

#### type

The buffer's type and storage mechanism.




</Field>
<Field
  common={true}
  defaultValue={500}
  enumValues={null}
  examples={[500]}
  groups={[]}
  name={"max_events"}
  path={"buffer"}
  relevantWhen={{"type":"memory"}}
  required={false}
  templateable={false}
  type={"int"}
  unit={"events"}
  warnings={[]}
  >

#### max_events

The maximum number of [events][docs.data-model] allowed in the buffer.

 See [Buffers & Batches](#buffers--batches) for more info.


</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[104900000]}
  groups={[]}
  name={"max_size"}
  path={"buffer"}
  relevantWhen={{"type":"disk"}}
  required={true}
  templateable={false}
  type={"int"}
  unit={"bytes"}
  warnings={[]}
  >

#### max_size

The maximum size of the buffer on the disk.




</Field>
<Field
  common={false}
  defaultValue={"block"}
  enumValues={{"block":"Applies back pressure when the buffer is full. This prevents data loss, but will cause data to pile up on the edge.","drop_newest":"Drops new data as it's received. This data is lost. This should be used when performance is the highest priority."}}
  examples={["block","drop_newest"]}
  groups={[]}
  name={"when_full"}
  path={"buffer"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

#### when_full

The behavior when the buffer becomes full.




</Field>
</Fields>

</Field>
<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={[]}
  groups={[]}
  name={"encoding"}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"table"}
  unit={null}
  warnings={[]}
  >

### encoding

Configures the encoding specific sink behavior.



<Fields filters={false}>
<Field
  common={true}
  defaultValue={null}
  enumValues={{"json":"Each event is encoded into JSON and the payload is represented as a JSON array.","text":"Each event is encoded into text via the `message` key and the payload is new line delimited."}}
  examples={["json","text"]}
  groups={[]}
  name={"codec"}
  path={"encoding"}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

#### codec

The encoding codec used to serialize the events before outputting.




</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[["timestamp","message","host"]]}
  groups={[]}
  name={"except_fields"}
  path={"encoding"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"[string]"}
  unit={null}
  warnings={[]}
  >

#### except_fields

Prevent the sink from encoding the specified labels.




</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[["timestamp","message","host"]]}
  groups={[]}
  name={"only_fields"}
  path={"encoding"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"[string]"}
  unit={null}
  warnings={[]}
  >

#### only_fields

Limit the sink to only encoding the specified labels.




</Field>
<Field
  common={false}
  defaultValue={"rfc3339"}
  enumValues={{"rfc3339":"Format as an RFC3339 string","unix":"Format as a unix timestamp, can be parsed as a Clickhouse DateTime"}}
  examples={["rfc3339","unix"]}
  groups={[]}
  name={"timestamp_format"}
  path={"encoding"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

#### timestamp_format

How to format event timestamps.




</Field>
</Fields>

</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["arn:aws:iam::123456789098:role/my_role"]}
  groups={[]}
  name={"assume_role"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

### assume_role

The ARN of an [IAM role][urls.aws_iam_role] to assume at startup.

 See [AWS Authentication](#aws-authentication) for more info.


</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["127.0.0.0:5000/path/to/service"]}
  groups={[]}
  name={"endpoint"}
  path={null}
  relevantWhen={{"region":""}}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

### endpoint

Custom endpoint for use with AWS-compatible services. Providing a value for
this option will make [`region`](#region) moot.




</Field>
<Field
  common={true}
  defaultValue={true}
  enumValues={null}
  examples={[true,false]}
  groups={[]}
  name={"healthcheck"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"bool"}
  unit={null}
  warnings={[]}
  >

### healthcheck

Enables/disables the sink healthcheck upon start.

 See [Health Checks](#health-checks) for more info.


</Field>
<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["us-east-1"]}
  groups={[]}
  name={"region"}
  path={null}
  relevantWhen={{"endpoint":""}}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

### region

The [AWS region][urls.aws_regions] of the target service. If [`endpoint`](#endpoint) is
provided it will override this value since the endpoint includes the region.




</Field>
<Field
  common={true}
  defaultValue={null}
  enumValues={null}
  examples={["my-stream"]}
  groups={[]}
  name={"stream_name"}
  path={null}
  relevantWhen={null}
  required={true}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

### stream_name

The [stream name][urls.aws_cloudwatch_logs_stream_name] of the target Kinesis
Firehose delivery stream.




</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={[]}
  groups={[]}
  name={"request"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"table"}
  unit={null}
  warnings={[]}
  >

### request

Configures the sink request behavior.



<Fields filters={false}>
<Field
  common={true}
  defaultValue={5}
  enumValues={null}
  examples={[5]}
  groups={[]}
  name={"in_flight_limit"}
  path={"request"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"requests"}
  warnings={[]}
  >

#### in_flight_limit

The maximum number of in-flight requests allowed at any given time.

 See [Rate Limits](#rate-limits) for more info.


</Field>
<Field
  common={true}
  defaultValue={1}
  enumValues={null}
  examples={[1]}
  groups={[]}
  name={"rate_limit_duration_secs"}
  path={"request"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"seconds"}
  warnings={[]}
  >

#### rate_limit_duration_secs

The time window, in seconds, used for the [`rate_limit_num`](#rate_limit_num) option.

 See [Rate Limits](#rate-limits) for more info.


</Field>
<Field
  common={true}
  defaultValue={5}
  enumValues={null}
  examples={[5]}
  groups={[]}
  name={"rate_limit_num"}
  path={"request"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={null}
  warnings={[]}
  >

#### rate_limit_num

The maximum number of requests allowed within the [`rate_limit_duration_secs`](#rate_limit_duration_secs)
time window.

 See [Rate Limits](#rate-limits) for more info.


</Field>
<Field
  common={false}
  defaultValue={-1}
  enumValues={null}
  examples={[-1]}
  groups={[]}
  name={"retry_attempts"}
  path={"request"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={null}
  warnings={[]}
  >

#### retry_attempts

The maximum number of retries to make for failed requests.

 See [Retry Policy](#retry-policy) for more info.


</Field>
<Field
  common={false}
  defaultValue={1}
  enumValues={null}
  examples={[1]}
  groups={[]}
  name={"retry_initial_backoff_secs"}
  path={"request"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"seconds"}
  warnings={[]}
  >

#### retry_initial_backoff_secs

The amount of time to wait before attempting the first retry for a failed
request. Once, the first retry has failed the fibonacci sequence will be used
to select future backoffs.




</Field>
<Field
  common={false}
  defaultValue={10}
  enumValues={null}
  examples={[10]}
  groups={[]}
  name={"retry_max_duration_secs"}
  path={"request"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"seconds"}
  warnings={[]}
  >

#### retry_max_duration_secs

The maximum amount of time, in seconds, to wait between retries.




</Field>
<Field
  common={true}
  defaultValue={30}
  enumValues={null}
  examples={[30]}
  groups={[]}
  name={"timeout_secs"}
  path={"request"}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"int"}
  unit={"seconds"}
  warnings={[]}
  >

#### timeout_secs

The maximum time a request can take before being aborted. It is highly
recommended that you do not lower value below the service's internal timeout,
as this could create orphaned requests, pile on retries, and result in
duplicate data downstream.

 See [Buffers & Batches](#buffers--batches) for more info.


</Field>
</Fields>

</Field>
</Fields>

## Env Vars

<Fields filters={true}>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["AKIAIOSFODNN7EXAMPLE"]}
  groups={[]}
  name={"AWS_ACCESS_KEY_ID"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

### AWS_ACCESS_KEY_ID

Used for AWS authentication when communicating with AWS services. See relevant
[AWS components][pages.aws_components] for more info.

 See [AWS Authentication](#aws-authentication) for more info.


</Field>
<Field
  common={false}
  defaultValue={null}
  enumValues={null}
  examples={["wJalrXUtnFEMI/K7MDENG/FD2F4GJ"]}
  groups={[]}
  name={"AWS_SECRET_ACCESS_KEY"}
  path={null}
  relevantWhen={null}
  required={false}
  templateable={false}
  type={"string"}
  unit={null}
  warnings={[]}
  >

### AWS_SECRET_ACCESS_KEY

Used for AWS authentication when communicating with AWS services. See relevant
[AWS components][pages.aws_components] for more info.

 See [AWS Authentication](#aws-authentication) for more info.


</Field>
</Fields>


## Examples

```http
POST / HTTP/1.1
Host: firehose.<region>.<domain>
Content-Length: <byte_size>
Content-Type: application/x-amz-json-1.1
Connection: Keep-Alive
X-Amz-Target: Firehose_20150804.PutRecordBatch
{
    "DeliveryStreamName": "<stream_name>",
    "Records": [
        {
            "Data": "<base64_encoded_log>",
        },
        {
            "Data": "<base64_encoded_log>",
        },
        {
            "Data": "<base64_encoded_log>",
        },
    ]
}
```

## How It Works

### AWS Authentication

Vector checks for AWS credentials in the following order:

1. Environment variables [`AWS_ACCESS_KEY_ID`](#aws_access_key_id) and [`AWS_SECRET_ACCESS_KEY`](#aws_secret_access_key).
2. The [`credential_process` command][urls.aws_credential_process] in the AWS config file. (usually located at `~/.aws/config`)
3. The [AWS credentials file][urls.aws_credentials_file]. (usually located at `~/.aws/credentials`)
4. The [IAM instance profile][urls.iam_instance_profile]. (will only work if running on an EC2 instance with an instance profile/role)

If credentials are not found the [healtcheck](#healthchecks) will fail and an
error will be [logged][docs.monitoring#logs].

#### Obtaining an access key

In general, we recommend using instance profiles/roles whenever possible. In
cases where this is not possible you can generate an AWS access key for any user
within your AWS account. AWS provides a [detailed guide][urls.aws_access_keys] on
how to do this.

#### Assuming Roles

Vector can assume an AWS IAM role via the [`assume_role`](#assume_role) option. This is an
optional setting that is helpful for a variety of use cases, such as cross
account access.

### Buffers & Batches

<SVG src="/img/buffers-and-batches-serial.svg" />

The `aws_kinesis_firehose` sink buffers & batches data as
shown in the diagram above. You'll notice that Vector treats these concepts
differently, instead of treating them as global concepts, Vector treats them
as sink specific concepts. This isolates sinks, ensuring services disruptions
are contained and [delivery guarantees][docs.guarantees] are honored.

*Batches* are flushed when 1 of 2 conditions are met:

1. The batch age meets or exceeds the configured [`timeout_secs`](#timeout_secs).
2. The batch size meets or exceeds the configured [`max_events`](#max_events).

*Buffers* are controlled via the [`buffer.*`](#buffer) options.

### Environment Variables

Environment variables are supported through all of Vector's configuration.
Simply add `${MY_ENV_VAR}` in your Vector configuration file and the variable
will be replaced before being evaluated.

You can learn more in the
[Environment Variables][docs.configuration#environment-variables] section.

### Health Checks

Health checks ensure that the downstream service is accessible and ready to
accept data. This check is performed upon sink initialization.
If the health check fails an error will be logged and Vector will proceed to
start.

#### Require Health Checks

If you'd like to exit immediately upon a health check failure, you can
pass the `--require-healthy` flag:

```bash
vector --config /etc/vector/vector.toml --require-healthy
```

#### Disable Health Checks

If you'd like to disable health checks for this sink you can set the
`healthcheck` option to `false`.

### Rate Limits

Vector offers a few levers to control the rate and volume of requests to the
downstream service. Start with the [`rate_limit_duration_secs`](#rate_limit_duration_secs) and
`rate_limit_num` options to ensure Vector does not exceed the specified
number of requests in the specified window. You can further control the pace at
which this window is saturated with the [`in_flight_limit`](#in_flight_limit) option, which
will guarantee no more than the specified number of requests are in-flight at
any given time.

Please note, Vector's defaults are carefully chosen and it should be rare that
you need to adjust these. If you found a good reason to do so please share it
with the Vector team by [opening an issue][urls.new_aws_kinesis_firehose_sink_issue].

### Retry Policy

Vector will retry failed requests (status == `429`, >= `500`, and != `501`).
Other responses will _not_ be retried. You can control the number of retry
attempts and backoff rate with the [`retry_attempts`](#retry_attempts) and
`retry_backoff_secs` options.


[docs.configuration#environment-variables]: /docs/setup/configuration/#environment-variables
[docs.data-model.log]: /docs/about/data-model/log/
[docs.data-model]: /docs/about/data-model/
[docs.guarantees]: /docs/about/guarantees/
[docs.monitoring#logs]: /docs/administration/monitoring/#logs
[pages.aws_components]: /components/?providers%5B%5D=aws/
[urls.aws_access_keys]: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html
[urls.aws_cloudwatch_logs_stream_name]: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Working-with-log-groups-and-streams.html
[urls.aws_credential_process]: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sourcing-external.html
[urls.aws_credentials_file]: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html
[urls.aws_iam_role]: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html
[urls.aws_kinesis_firehose]: https://aws.amazon.com/kinesis/data-firehose/
[urls.aws_regions]: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.RegionsAndAvailabilityZones.html
[urls.iam_instance_profile]: https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html
[urls.new_aws_kinesis_firehose_sink_issue]: https://github.com/timberio/vector/issues/new?labels=sink%3A+aws_kinesis_firehose
