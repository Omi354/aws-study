# aws-study

## パラメータストア設定
以下のパラメータが必要です。
手動での設定が必要なため、別途マネジメントコンソール上から作成してください。

### DB接続情報
- 名前: /raisetech-aws-study/db-master-user
- 種類: String
- 値: 自身で設定

### DB接続情報
- 名前: /raisetech-aws-study/db-password
- 種類: SecureString
- 値: 自身で設定

### CloudWatchAgentでEC2からログを出力するための設定
- 名前: AmazonCloudWatch-linux
- 種類: String
- 値:
  ```json
  {
    "agent": {
      "metrics_collection_interval": 60,
      "run_as_user": "root"
    },
    "logs": {
      "logs_collected": {
        "files": {
          "collect_list": [
            {
              "file_path": "/var/log/messages",
              "log_group_class": "STANDARD",
              "log_group_name": "messages",
              "log_stream_name": "{instance_id}",
              "retention_in_days": 1
            }
          ]
        }
      }
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
            "io_time"
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
