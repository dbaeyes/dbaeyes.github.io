---
layout: post
title: CloudWatch Agent for Mem, Disk and App logs
categories: [AWS]
tags: [AWS,Cloudwatch]
description: cmd
author: "Vince"
permalink: /:categories/:year/:month/:day/:title.html
---

You can use cloudwatch agent to collect the Mem and disk useage for the EC2 and alos the apps running logs. 

## 1. Set role
[AWS cloudwatch agent role](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-iam-roles-for-cloudwatch-agent.html)

## 2. Install

```
wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb

--for Ubantu
sudo dpkg -i -E ./amazon-cloudwatch-agent.deb
```
## 3. Config file
>  sudo vi /opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json

## 4. Start
> sudo systemctl restart amazon-cloudwatch-agent

Check log
> tail -f /opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log

or other method
```
sudo amazon-cloudwatch-agent-ctl -a stop

amazon-cloudwatch-agent-ctl -a status

sudo amazon-cloudwatch-agent-ctl -a start
```

Sample of config file
```
{
        "agent": {
                "metrics_collection_interval": 60,
                "run_as_user": "cwagent"
        },
        "metrics": {
                "append_dimensions": {
                                 "InstanceId": "${aws:InstanceId}"
                },
                "metrics_collected": {
                        "disk": {
                                "measurement": [
                                        "used_percent"
                                ],
                                "metrics_collection_interval": 300,
                                "resources": [
                                        "/"
                                ]
                        },
                        "mem": {
                                "measurement": [
                                        "mem_used_percent"
                                ],
                                "metrics_collection_interval": 300
                        }
                }
        },
        "logs": {
            "logs_collected": {
              "files": {
                "collect_list": [
                  {
                      "file_path": "/home/ubuntu/log_sync.log",
                      "log_group_name":  "/apps/webservers/crontab/log",
                      "log_stream_name": "{ip_address}_{instance_id}",
                      "timestamp_format": "%d/%b/%Y:%H:%M:%S %z",
                      "timezone": "Local"
                  }
                ]
              }
            }
          }
}
```

