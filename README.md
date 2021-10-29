# AWS RDS Terraform module

Terraform module which creates **RDS** resources on **AWS**.

## User Stories for this module

- AAOps I can create a Postgres, MariaDB or MySQL database with its name, subnet, version, disk (type and size), max connections number, with HA or no
- AAOps I can choose if I access my Database with SSL
- AAOps I can find my DB credentials in SecretManager
- AAOps AAOps I can give an existing KMS Key (Custom or AWS-managed) to the module or let the module create a KMS key for my RDS and my secrets
## Usage

```hcl
module "rds" {
  source = "https://github.com/padok-team/terraform-aws-rds.git?ref=v1.0.0"

  tags = {
    "Name" : "rds-poc-library-multi-az",
    "Port" : 5432,
    "Env" : "poc-library"
  }
  aws_region = "eu-west-3"

  identifier = "rds-poc-library-multi-az"

  ## DATABASE
  engine              = "postgres"
  engine_version      = "13.4"
  db_parameter_family = "postgres13"
  name                = "aws_rds_instance_poc_library_multi_az"
  username            = "aws_rds_instance_user_poc_library_multi_az"

  ## NETWORK
  subnet_ids = ["subnet-0f55d716e3746c4db", "subnet-0ced4e0a55a479422", "subnet-0005a41a2318130e5"]
  vpc_id     = "vpc-0fcea78a178762e3f"
}
```

## Examples

- [AAOps I deploy a postgres rds instance with multi AZ availability with custom KMS key for secrets and rds encryption](examples/multi_az_rds_instance_postgres/main.tf)
- [AAOps I Deploy a mysql rds instance without multi AZ and with auto generated KMS Keys](examples/one_az_rds_instance_mysql/main.tf)

<!-- BEGIN_TF_DOCS -->
## Modules

No modules.

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_db_parameter_family"></a> [db\_parameter\_family](#input\_db\_parameter\_family) | The family of the DB parameter group. Among postgres11, postgres12, postgres13, mysql5.6, mysql5.7, mysql8.0 for MySQL and Postgres | `string` | n/a | yes |
| <a name="input_engine"></a> [engine](#input\_engine) | Engine used for your RDS instance (mysql, postgres ...) | `string` | n/a | yes |
| <a name="input_engine_version"></a> [engine\_version](#input\_engine\_version) | Version of your engine | `string` | n/a | yes |
| <a name="input_identifier"></a> [identifier](#input\_identifier) | Unique identifier for your RDS instance. For example, aws\_rds\_instance\_postgres\_poc\_library\_break | `string` | n/a | yes |
| <a name="input_subnet_ids"></a> [subnet\_ids](#input\_subnet\_ids) | A list of VPC subnet IDs to create your db subnet group | `list(string)` | n/a | yes |
| <a name="input_tags"></a> [tags](#input\_tags) | Tags to attach to all resources created by this module | `map(any)` | n/a | yes |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | VPC id where the DB is | `string` | n/a | yes |
| <a name="input_allocated_storage"></a> [allocated\_storage](#input\_allocated\_storage) | Storage allocated to your RDS instance in Gigabytes | `number` | `10` | no |
| <a name="input_allow_major_version_upgrade"></a> [allow\_major\_version\_upgrade](#input\_allow\_major\_version\_upgrade) | Indicates that major version upgrades are allowed | `bool` | `false` | no |
| <a name="input_apply_immediately"></a> [apply\_immediately](#input\_apply\_immediately) | Specifies whether any database modifications are applied immediately, or during the next maintenance window | `bool` | `false` | no |
| <a name="input_arn_custom_kms_key"></a> [arn\_custom\_kms\_key](#input\_arn\_custom\_kms\_key) | Arn of your custom KMS Key. Useful only if custom\_kms\_key is set to true | `string` | `null` | no |
| <a name="input_arn_custom_kms_key_secret"></a> [arn\_custom\_kms\_key\_secret](#input\_arn\_custom\_kms\_key\_secret) | Encrypt AWS secret with CMK | `string` | `null` | no |
| <a name="input_authorized_security_groups"></a> [authorized\_security\_groups](#input\_authorized\_security\_groups) | List of the security group that are allowed to access RDS Instance | `list(string)` | `[]` | no |
| <a name="input_auto_minor_version_upgrade"></a> [auto\_minor\_version\_upgrade](#input\_auto\_minor\_version\_upgrade) | Indicates that minor engine upgrades will be applied automatically to the DB instance during the maintenance window | `bool` | `true` | no |
| <a name="input_availability_zone"></a> [availability\_zone](#input\_availability\_zone) | Availability zone to use when Multi AZ is disabled | `string` | `"eu-west-3a"` | no |
| <a name="input_backup_retention_period"></a> [backup\_retention\_period](#input\_backup\_retention\_period) | Backup retention period | `number` | `15` | no |
| <a name="input_deletion_protection"></a> [deletion\_protection](#input\_deletion\_protection) | If the DB instance should have deletion protection enabled. The database can't be deleted when this value is set to true | `bool` | `true` | no |
| <a name="input_force_ssl"></a> [force\_ssl](#input\_force\_ssl) | Force SSL for DB connections | `string` | `true` | no |
| <a name="input_iam_database_authentication_enabled"></a> [iam\_database\_authentication\_enabled](#input\_iam\_database\_authentication\_enabled) | Specifies whether or mappings of AWS Identity and Access Management (IAM) accounts to database accounts is enabled | `bool` | `false` | no |
| <a name="input_instance_class"></a> [instance\_class](#input\_instance\_class) | Instance class for your RDS instance | `string` | `"db.t3.micro"` | no |
| <a name="input_maintenance_window"></a> [maintenance\_window](#input\_maintenance\_window) | The window to perform maintenance in. Syntax: 'ddd:hh24:mi-ddd:hh24:mi'. Eg: 'Mon:00:00-Mon:03:00' | `string` | `"Mon:00:00-Mon:03:00"` | no |
| <a name="input_max_allocated_storage"></a> [max\_allocated\_storage](#input\_max\_allocated\_storage) | When configured, the upper limit to which Amazon RDS can automatically scale the storage of the DB instance | `number` | `50` | no |
| <a name="input_multi_az"></a> [multi\_az](#input\_multi\_az) | Set to true to deploy a multi AZ RDS instance | `bool` | `false` | no |
| <a name="input_name"></a> [name](#input\_name) | Name of your database in your RDS instance | `string` | `"aws_padok_database_instance"` | no |
| <a name="input_password_length"></a> [password\_length](#input\_password\_length) | Password length for db master user, Minimum length is 25 | `number` | `40` | no |
| <a name="input_performance_insights_enabled"></a> [performance\_insights\_enabled](#input\_performance\_insights\_enabled) | Set to true to enable performance insights on your RDS instance | `bool` | `true` | no |
| <a name="input_port"></a> [port](#input\_port) | The port on which the DB accepts connections. Default is chosen depeding on the engine | `number` | `null` | no |
| <a name="input_publicly_accessible"></a> [publicly\_accessible](#input\_publicly\_accessible) | Bool to control if instance is publicly accessible. | `bool` | `false` | no |
| <a name="input_rds_secret_recovery_window_in_days"></a> [rds\_secret\_recovery\_window\_in\_days](#input\_rds\_secret\_recovery\_window\_in\_days) | Secret recovery window in days | `number` | `10` | no |
| <a name="input_rds_skip_final_snapshot"></a> [rds\_skip\_final\_snapshot](#input\_rds\_skip\_final\_snapshot) | Determines whether a final DB snapshot is created before the DB instance is deleted | `bool` | `false` | no |
| <a name="input_storage_type"></a> [storage\_type](#input\_storage\_type) | One of 'standard' (magnetic), 'gp2' (general purpose SSD), or 'io1' (provisioned IOPS SSD) | `string` | `"gp2"` | no |
| <a name="input_username"></a> [username](#input\_username) | Name of the master user for the database in your RDS Instance | `string` | `"admin"` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_rds_endpoint"></a> [rds\_endpoint](#output\_rds\_endpoint) | Endpoint of the RDS Instance |
| <a name="output_rds_identifier"></a> [rds\_identifier](#output\_rds\_identifier) | Identifier of the RDS Instance |
<!-- END_TF_DOCS -->

## Next Steps
- Read Replicas
- RDS Proxy
