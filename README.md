<!--

  ** DO NOT EDIT THIS FILE
  **
  ** This file was automatically generated by the `build-harness`.
  ** 1) Make all changes to `README.yaml`
  ** 2) Run `make init` (you only need to do this once)
  ** 3) Run`make readme` to rebuild this file.
  **

  -->


[<img src="https://res.cloudinary.com/enter-at/image/upload/v1576145406/static/logo-svg.svg" alt="enter-at" width="100">][website]


# terraform-aws-lambda




 [![Build Status](https://github.com/enter-at/terraform-aws-lambda/workflows/Release/badge.svg)](https://github.com/enter-at/terraform-aws-lambda/actions) [![Latest Release](https://img.shields.io/github/release/enter-at/terraform-aws-lambda.svg)](https://github.com/enter-at/terraform-aws-lambda/releases/latest) [![Semantic Release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)


Terraform module designed to facilitate the creation of AWS Lambda functions.


---


It's 100% Open Source and licensed under the [APACHE2](LICENSE).





## Usage


**IMPORTANT:** The `master` branch is used in `source` just as an example. In your code, do not pin to `master` because there may be breaking changes between releases.
Instead pin to the release tag (e.g. `?ref=tags/x.y.z`) of one of our [latest releases](https://github.com/enter-at/terraform-aws-lambda/releases).


### Simple Example

```hcl
module "lambda" {
  source        = "git::https://github.com/enter-at/terraform-aws-lambda.git?ref=master"
  function_name = "test-service"
  handler       = "service/handler"
  dist_dir      = var.dist_dir
  source_dir    = var.source_dir
  runtime       = var.runtime

  rsync_pattern = [
    "--include=*.js"
  ]
}
```
### Advanced Example

```hcl
locals {
  service_dir = "account-data"
}

module "lambda" {
  source        = "git::https://github.com/enter-at/terraform-aws-lambda.git?ref=master"
  function_name = "test-service"
  handler       = "${local.service_dir}/handler"
  dist_dir      = var.dist_dir
  source_dir    = var.source_dir
  runtime       = var.runtime
  layers        = var.layers

  rsync_pattern = [
    "--include=lib/",
    "--include=domain/",
    "--include=${local.service_dir}/",
    "--include=*.js"
  ]

  policy = {
    json = data.aws_iam_policy_document.main.json
  }

  environment = {
    variables = {
      SM_SERVICE_CONFIG = var.pfs_secrets_manager_secret.arn
    }
  }

  vpc_config = {
    subnet_ids = var.private_subnet_ids

    security_group_ids = [
      var.security_group_id
    ]
  }

  tags = {
    "Team" = "XYZ"
  }
}
```






## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| dead_letter_config | (Optional) Nested block to configure the function's dead letter queue. | object | `null` | no |
| description | (Optional) Description of what your Lambda Function does. | string | `null` | no |
| environment | (Optional) The Lambda environment's configuration settings. | object | `null` | no |
| function_name | (Required) A unique name for your Lambda Function. | string | - | yes |
| handler | (Required) The function entrypoint in your code. | string | - | yes |
| layers | (Optional) List of Lambda Layer Version ARNs (maximum of 5) to attach to your Lambda Function. | list(string) | `null` | no |
| memory_size | (Optional) Amount of memory in MB your Lambda Function can use at runtime. Defaults to 128. | number | `128` | no |
| policy | (Optional) An additional policy to attach to the Lambda function role. | object | `null` | no |
| reserved_concurrent_executions | (Optional) The amount of reserved concurrent executions for this lambda function. | number | `null` | no |
| rsync_pattern | (Optional) A list of rsync pattern to include or exclude files and directories. | list(string) | `<list>` | no |
| runtime | (Required) The identifier of the function's runtime. | string | - | yes |
| source_dir | (Required) The location of the handler source code. | string | - | yes |
| tags | (Optional) A mapping of tags to assign to the object. | map(string) | `null` | no |
| timeout | (Optional) The amount of time your Lambda Function has to run in seconds. Defaults to 3. | number | `3` | no |
| tracing_config | (Optional) A child block with a single argument mode | object | `null` | no |
| vpc_config | (Optional) Provide this to allow your function to access your VPC. | object | `null` | no |

## Outputs

| Name | Description |
|------|-------------|
| arn | The Amazon Resource Name (ARN) identifying the Lambda function. |
| function_name | - |
| invoke_arn | The ARN to be used for invoking the Lambda function |
| role_arn | The ARN of the IAM role created for the Lambda function |
| role_name | The name of the IAM role created for the Lambda function |




## Share the Love

Like this project? Please give it a ★ on [our GitHub](https://github.com/enter-at/terraform-aws-lambda)! (it helps us **a lot**)

Are you using this project or any of our other projects? Consider [leaving a testimonial][testimonial]. =)


## Related Projects

Check out these related projects.

- [terraform-newrelic-alert-lambda](https://github.com/enter-at/terraform-newrelic-alert-lambda) - Terraform Module to define New Relic alerts for AWS Lambda functions.



## Help

**Got a question?**

File a GitHub [issue](https://github.com/enter-at/terraform-aws-lambda/issues).

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/enter-at/terraform-aws-lambda/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project, we would love to hear from you!

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!





## License

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.




### Contributors

|  [![Steffen Leistner][sleistner_avatar]][sleistner_homepage]<br/>[Steffen Leistner][sleistner_homepage] |
|---|

  [sleistner_homepage]: https://github.com/sleistner
  [sleistner_avatar]: https://res.cloudinary.com/enter-at/image/fetch/w_150,h_150,c_thumb/https://github.com/sleistner.png



  [website]: https://github.com/enter-at
