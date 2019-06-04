<!-- This file was automatically generated by the `build-harness`. Make all changes to `README.yaml` and run `make readme` to rebuild this file. -->
[![README Header][readme_header_img]][readme_header_link]

[![Cloud Posse][logo]](https://cpco.io/homepage)

# testing.cloudposse.co [![Codefresh Build Status](https://g.codefresh.io/api/badges/build?repoOwner=cloudposse&repoName=testing.cloudposse.co&branch=master&pipelineName=build&accountName=cloudposse&type=cf-1)](https://g.codefresh.io/pipelines/build/builds?repoOwner=cloudposse&repoName=testing.cloudposse.co&serviceName=cloudposse%2Ftesting.cloudposse.co&filter=trigger:build~Build;branch:master;pipeline:5b4d6e94cb00eb3e7efb81de~build) [![Latest Release](https://img.shields.io/github/release/cloudposse/testing.cloudposse.co.svg)](https://github.com/cloudposse/testing.cloudposse.co/releases) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


Terraform Reference Architecture that implements a [Geodesic](https://github.com/cloudposse/geodesic) Module for an Automated Testing Organization in AWS.

This account is suitable for running automated tests of Terraform modules. 

__NOTE:__ Before creating the Testing infrastructure, you need to provision the [Parent ("Root") Organization](https://github.com/cloudposse/root.cloudposse.co) in AWS (because it creates resources needed for all other accounts). Follow the steps in [README](https://github.com/cloudposse/root.cloudposse.co) first. You need to do it only once.


---

This project is part of our comprehensive ["SweetOps"](https://cpco.io/sweetops) approach towards DevOps. 
[<img align="right" title="Share via Email" src="https://docs.cloudposse.com/images/ionicons/ios-email-outline-2.0.1-16x16-999999.svg"/>][share_email]
[<img align="right" title="Share on Google+" src="https://docs.cloudposse.com/images/ionicons/social-googleplus-outline-2.0.1-16x16-999999.svg" />][share_googleplus]
[<img align="right" title="Share on Facebook" src="https://docs.cloudposse.com/images/ionicons/social-facebook-outline-2.0.1-16x16-999999.svg" />][share_facebook]
[<img align="right" title="Share on Reddit" src="https://docs.cloudposse.com/images/ionicons/social-reddit-outline-2.0.1-16x16-999999.svg" />][share_reddit]
[<img align="right" title="Share on LinkedIn" src="https://docs.cloudposse.com/images/ionicons/social-linkedin-outline-2.0.1-16x16-999999.svg" />][share_linkedin]
[<img align="right" title="Share on Twitter" src="https://docs.cloudposse.com/images/ionicons/social-twitter-outline-2.0.1-16x16-999999.svg" />][share_twitter]




It's 100% Open Source and licensed under the [APACHE2](LICENSE).












## Introduction

We use [geodesic](https://github.com/cloudposse/geodesic) to define and build world-class cloud infrastructures backed by AWS and powered by Kubernetes.

`geodesic` exposes many tools that can be used to define and provision AWS and Kubernetes resources.

Here is the list of tools we use to provision the `testing.cloudposse.co` infrastructure:

* [aws-vault](https://github.com/99designs/aws-vault)
* [chamber](https://github.com/segmentio/chamber)
* [terraform](https://www.terraform.io/)


## Quick Start


### Setup AWS Role

__NOTE:__ You need to do it only once.

Configure AWS profile in `~/.aws/config`. Make sure to change username (username@cloudposse.co) to your own.

```bash
[profile cpco-testing-admin]
region=us-west-2
role_arn=arn:aws:iam::126450723953:role/OrganizationAccountAccessRole
mfa_serial=arn:aws:iam::323330167063:mfa/username@cloudposse.co
source_profile=cpco
```

### Install and setup aws-vault

__NOTE:__ You need to do it only once.

We use [aws-vault](https://docs.cloudposse.com/tools/aws-vault/) to store IAM credentials in your operating system's secure keystore and then generates temporary credentials from those to expose to your shell and applications.

Install [aws-vault](https://docs.cloudposse.com/tools/aws-vault/) on your local computer first.

On MacOS, you may use `homebrew cask`

```bash
brew cask install aws-vault
```

Then setup your secret credentials for AWS in `aws-vault`
```bash
aws-vault add --backend file cpco
```

__NOTE:__ You should set `AWS_VAULT_BACKEND=file` in your shell rc config (e.g. `~/.bashrc`) so it persists.

For more info, see [aws-vault](https://docs.cloudposse.com/tools/aws-vault/)


## Examples

### Build Docker Image

```
# Initialize the project's build-harness
make init

# Build docker image
make docker/build
```

### Install the wrapper shell
```bash
make install
```

### Run the shell
```bash
testing.cloudposse.co
```

### Login to AWS with your MFA device
```bash
assume-role
```

__NOTE:__ Before provisioning AWS resources with Terraform, you need to create `tfstate-backend` first (S3 bucket to store Terraform state and DynamoDB table for state locking).

Follow the steps in this [README](https://github.com/cloudposse/terraform-root-modules/blob/master/aws/tfstate-backend/). You need to do it only once.

After `tfstate-backend` has been provisioned, follow the rest of the instructions in the order shown below.


### Provision `dns` with Terraform

Change directory to `dns` folder
```bash
cd /conf/dns
```

Run Terraform
```bash
init-terraform
terraform plan
terraform apply
```

For more info, see [geodesic-with-terraform](https://docs.cloudposse.com/geodesic/module/with-terraform/)

### Provision `cloudtrail` with Terraform

```bash
cd /conf/cloudtrail
init-terraform
terraform plan
terraform apply
```

### Provision `chamber` with Terraform

```bash
cd /conf/chamber
init-terraform
terraform plan
terraform apply
```




## Makefile Targets
```
Available targets:

  all                                 Initialize build-harness, install deps, build docker container, install wrapper script and run shell
  build                               Build docker image
  deps                                Install dependencies (if any)
  help                                Help screen
  help/all                            Display help for all targets
  help/short                          This help short screen
  install                             Install wrapper script from geodesic container
  push                                Push docker image to registry
  run                                 Start the geodesic shell by calling wrapper script

```



## Share the Love 

Like this project? Please give it a ★ on [our GitHub](https://github.com/cloudposse/testing.cloudposse.co)! (it helps us **a lot**) 

Are you using this project or any of our other projects? Consider [leaving a testimonial][testimonial]. =)


## Related Projects

Check out these related projects.

- [Packages](https://github.com/cloudposse/packages) - Cloud Posse installer and distribution of native apps
- [Build Harness](https://github.com/cloudposse/dev) - Collection of Makefiles to facilitate building Golang projects, Dockerfiles, Helm charts, and more
- [terraform-root-modules](https://github.com/cloudposse/terraform-root-modules) - Collection of Terraform "root module" invocations for provisioning reference architectures
- [root.cloudposse.co](https://github.com/cloudposse/root.cloudposse.co) - Example Terraform Reference Architecture of a Geodesic Module for a Parent ("Root") Organization in AWS.
- [audit.cloudposse.co](https://github.com/cloudposse/audit.cloudposse.co) - Example Terraform Reference Architecture of a Geodesic Module for an Audit Logs Organization in AWS.
- [prod.cloudposse.co](https://github.com/cloudposse/prod.cloudposse.co) - Example Terraform Reference Architecture of a Geodesic Module for a Production Organization in AWS.
- [staging.cloudposse.co](https://github.com/cloudposse/staging.cloudposse.co) - Example Terraform Reference Architecture of a Geodesic Module for a Staging Organization in AWS.
- [dev.cloudposse.co](https://github.com/cloudposse/dev.cloudposse.co) - Example Terraform Reference Architecture of a Geodesic Module for a Development Sandbox Organization in AWS.




## References

For additional context, refer to some of these links. 

- [Cloud Posse Documentation](https://docs.cloudposse.com) - Complete documentation for the Cloud Posse solution


## Help

**Got a question?**

File a GitHub [issue](https://github.com/cloudposse/testing.cloudposse.co/issues), send us an [email][email] or join our [Slack Community][slack].

[![README Commercial Support][readme_commercial_support_img]][readme_commercial_support_link]

## Commercial Support

Work directly with our team of DevOps experts via email, slack, and video conferencing. 

We provide [*commercial support*][commercial_support] for all of our [Open Source][github] projects. As a *Dedicated Support* customer, you have access to our team of subject matter experts at a fraction of the cost of a full-time engineer. 

[![E-Mail](https://img.shields.io/badge/email-hello@cloudposse.com-blue.svg)][email]

- **Questions.** We'll use a Shared Slack channel between your team and ours.
- **Troubleshooting.** We'll help you triage why things aren't working.
- **Code Reviews.** We'll review your Pull Requests and provide constructive feedback.
- **Bug Fixes.** We'll rapidly work to fix any bugs in our projects.
- **Build New Terraform Modules.** We'll [develop original modules][module_development] to provision infrastructure.
- **Cloud Architecture.** We'll assist with your cloud strategy and design.
- **Implementation.** We'll provide hands-on support to implement our reference architectures. 




## Slack Community

Join our [Open Source Community][slack] on Slack. It's **FREE** for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build totally *sweet* infrastructure.

## Newsletter

Signup for [our newsletter][newsletter] that covers everything on our technology radar.  Receive updates on what we're up to on GitHub as well as awesome new projects we discover. 

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/testing.cloudposse.co/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://cpco.io/help-out) with our other projects, we would love to hear from you! Shoot us an [email][email].

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2019 [Cloud Posse, LLC](https://cpco.io/copyright)



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









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know by [leaving a testimonial][testimonial]!

[![Cloud Posse][logo]][website]

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We ❤️  [Open Source Software][we_love_open_source].

We offer [paid support][commercial_support] on all of our projects.  

Check out [our other projects][github], [follow us on twitter][twitter], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.




[![README Footer][readme_footer_img]][readme_footer_link]
[![Beacon][beacon]][website]

  [logo]: https://cloudposse.com/logo-300x69.svg
  [docs]: https://cpco.io/docs
  [website]: https://cpco.io/homepage
  [github]: https://cpco.io/github
  [jobs]: https://cpco.io/jobs
  [hire]: https://cpco.io/hire
  [slack]: https://cpco.io/slack
  [linkedin]: https://cpco.io/linkedin
  [twitter]: https://cpco.io/twitter
  [testimonial]: https://cpco.io/leave-testimonial
  [newsletter]: https://cpco.io/newsletter
  [email]: https://cpco.io/email
  [commercial_support]: https://cpco.io/commercial-support
  [we_love_open_source]: https://cpco.io/we-love-open-source
  [module_development]: https://cpco.io/module-development
  [terraform_modules]: https://cpco.io/terraform-modules
  [readme_header_img]: https://cloudposse.com/readme/header/img?repo=cloudposse/testing.cloudposse.co
  [readme_header_link]: https://cloudposse.com/readme/header/link?repo=cloudposse/testing.cloudposse.co
  [readme_footer_img]: https://cloudposse.com/readme/footer/img?repo=cloudposse/testing.cloudposse.co
  [readme_footer_link]: https://cloudposse.com/readme/footer/link?repo=cloudposse/testing.cloudposse.co
  [readme_commercial_support_img]: https://cloudposse.com/readme/commercial-support/img?repo=cloudposse/testing.cloudposse.co
  [readme_commercial_support_link]: https://cloudposse.com/readme/commercial-support/link?repo=cloudposse/testing.cloudposse.co
  [share_twitter]: https://twitter.com/intent/tweet/?text=testing.cloudposse.co&url=https://github.com/cloudposse/testing.cloudposse.co
  [share_linkedin]: https://www.linkedin.com/shareArticle?mini=true&title=testing.cloudposse.co&url=https://github.com/cloudposse/testing.cloudposse.co
  [share_reddit]: https://reddit.com/submit/?url=https://github.com/cloudposse/testing.cloudposse.co
  [share_facebook]: https://facebook.com/sharer/sharer.php?u=https://github.com/cloudposse/testing.cloudposse.co
  [share_googleplus]: https://plus.google.com/share?url=https://github.com/cloudposse/testing.cloudposse.co
  [share_email]: mailto:?subject=testing.cloudposse.co&body=https://github.com/cloudposse/testing.cloudposse.co
  [beacon]: https://ga-beacon.cloudposse.com/UA-76589703-4/cloudposse/testing.cloudposse.co?pixel&cs=github&cm=readme&an=testing.cloudposse.co
