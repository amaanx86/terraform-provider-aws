---
subcategory: "CloudWatch Logs"
layout: "aws"
page_title: "AWS: aws_cloudwatch_log_s3_table_source_association"
description: |-
  Manages a CloudWatch Logs S3 Table Source Association.
---

# Resource: aws_cloudwatch_log_s3_table_source_association

Manages a CloudWatch Logs S3 Table Source Association. This resource associates a CloudWatch Logs Data Source (such as a collection of log groups) with an S3 Table Integration, enabling new log messages to be automatically written to S3 Tables for analytics.

For more information, see the [CloudWatch Logs S3 Tables integration documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/s3-tables-integration.html).

## Example Usage

### Associate All CloudWatch Logs Data Sources (Wildcard)

```terraform
resource "aws_cloudwatch_log_s3_table_source_association" "example" {
  integration_arn = aws_observabilityadmin_s3_table_integration.example.arn
  # datasource_name and datasource_type default to "*" (all sources)
}
```

### Associate a Specific Custom Data Source

Copy new messages streaming to one particular log group into an s3 table with name `namestring__typestring`, located in an Amazon-managed table bucket named `aws-cloudwatch`.

Note that the name and type strings of custom data sources can only contain lowercase letters, numbers and underscores.
See [data source documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/data-source-discovery-management.html#how-to-get-started-data-sources) for details.

```terraform
resource "aws_cloudwatch_log_group" "example" {
  name = "app_logs"
  tags = {
    "cw:datasource:name" = "namestring"
    "cw:datasource:type" = "typestring"
  }
}

resource "aws_cloudwatch_log_s3_table_source_association" "example" {
  integration_arn = aws_observabilityadmin_s3_table_integration.example.arn
  datasource_name = "namestring"
  datasource_type = "typestring"
}
```

## Argument Reference

This resource supports the following arguments:

* `datasource_name` - (Optional, Forces new resource) Name of the data source. Defaults to `*` to match all data source names.
* `datasource_type` - (Optional, Forces new resource) Type of the data source. Defaults to `*` to match all data source types.
* `integration_arn` - (Required, Forces new resource) ARN of the `aws_observabilityadmin_s3_table_integration` to associate the data source with.
* `region` - (Optional) Region where this resource will be [managed](https://docs.aws.amazon.com/general/latest/gr/rande.html#regional-endpoints). Defaults to the Region set in the [provider configuration](https://registry.terraform.io/providers/hashicorp/aws/latest/docs#aws-configuration-reference).

## Attribute Reference

This resource exports the following attributes in addition to the arguments above:

* `id` - Unique identifier for this source association.
* `status` - Current status of the association. Valid values: `ACTIVE`, `UNHEALTHY`, `FAILED`, `DATA_SOURCE_DELETE_IN_PROGRESS`.
* `status_reason` - Additional information about the current status.

## Import

In Terraform v1.5.0 and later, use an [`import` block](https://developer.hashicorp.com/terraform/language/import) to import CloudWatch Logs S3 Table Source Associations using `integration_arn,id`. For example:

```terraform
import {
  to = aws_cloudwatch_log_s3_table_source_association.example
  id = "arn:aws:observabilityadmin:us-east-1:123456789012:s3-table-integration/example-id,source-identifier"
}
```

Using `terraform import`, import CloudWatch Logs S3 Table Source Associations using `integration_arn,id`. For example:

```console
% terraform import aws_cloudwatch_log_s3_table_source_association.example arn:aws:observabilityadmin:us-east-1:123456789012:s3-table-integration/example-id,source-identifier
```
