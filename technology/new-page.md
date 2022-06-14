---
title: AWS Metrics
description: Each AWS service provides some metrics by default, but to get better insight into your EC2 instance, you may need to add additional metrics using the Cloudwatch Agent
published: true
date: 2022-06-14T00:44:39.357Z
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

1. Starting the agent (if you didn't choose #2.1 in the list above) is `/opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a start`.

### Editing the configuration JSON file
Of key interest to me is the *Namespace* (such as *AWS/EC2*) in which my metrics are written.  A namespace will group my metrics in a location seperated from the standard metrics.  As an example, I want the production run of Data.World's biweekly run to be seperate from all the other metrics that are produced -- such as the metrics produced by our lab servers.
```
{
	"agent": {
		"metrics_collection_interval": 60,
		"run_as_user": "root"
	},
	"metrics": {
		"aggregation_dimensions": [
			[
				"InstanceId"
			]
		],
		"append_dimensions": {
			"AutoScalingGroupName": "${aws:AutoScalingGroupName}",
			"ImageId": "${aws:ImageId}",
			"InstanceId": "${aws:InstanceId}",
			"InstanceType": "${aws:InstanceType}"
		},
		"metrics_collected": {
			"collectd": {
				"metrics_aggregation_interval": 60
			},
			"cpu": {
				"measurement": [
					"cpu_usage_idle",
					"cpu_usage_iowait",
					"cpu_usage_user",
					"cpu_usage_system"
				],
				"metrics_collection_interval": 60,
				"totalcpu": false
			},
			"disk": {
				"measurement": [
					"used_percent",
					"inodes_free"
				],
				"metrics_collection_interval": 60,
				"resources": [
					"*"
				]
			},
			"diskio": {
				"measurement": [
					"io_time",
					"write_bytes",
					"read_bytes",
					"writes",
					"reads"
				],
				"metrics_collection_interval": 60,
				"resources": [
					"*"
				]
			},
			"mem": {
				"measurement": [
					"mem_used_percent"
				],
				"metrics_collection_interval": 60
			},
			"netstat": {
				"measurement": [
					"tcp_established",
					"tcp_time_wait"
				],
				"metrics_collection_interval": 60
			},
			"statsd": {
				"metrics_aggregation_interval": 60,
				"metrics_collection_interval": 10,
				"service_address": ":8125"
			},
			"swap": {
				"measurement": [
					"swap_used_percent"
				],
				"metrics_collection_interval": 60
			}
		}
	}
}
```

