---
title: AWS Metrics
description: Each AWS service provides some metrics by default, but to get better insight into your EC2 instance, you may need to add additional metrics using the Cloudwatch Agent
published: true
date: 2022-06-13T23:23:24.332Z
tags: aws, cloudwatch
editor: markdown
dateCreated: 2022-06-13T23:23:24.332Z
---

# AWS Metrics
## EC2
Cloudwatch provides some EC2 metrics by default, such as CPU utilization, Network throughput and EBS throughput.  A glaring omission is Memory utilization.  I dove into the exercise of adding Memory utilization at Data.World; these are my notes.

1. First and formost, the Cloudwatch Agent needs to be installed: `yum install -y -R 15 amazon-cloudwatch-agent` (as root).  This installs a suite of tools at `/opt/aws/amazon-cloudwatch-agent`.
1. Second, the Cloudwatch Agent must be configured.  You have two options here:
2.1 The act of starting the daemon will produce a default configuration.  Simply run `/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s`. This will create a default configuration at `/etc/amazon/amazon-cloudwatch-agent/`
2.2. Alternaively, you can use the `amazon-cloudwatch-agent-config-wizard` wizard to step you through the configuration.  You will be promted to report on metrics produced by the open source project *collectd*.  If you wish to use *collectd* metrics, you will need to install *collectd*... `sudo amazon-linux-extras install collectd`
2.3. Perhaps someone has already made a configuration and stored it in *System Manager*'s *Parameter Store*.  In this case, you can both download the configuration and start the daemon with `/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c ssm:<nameOfParameterStore>`
