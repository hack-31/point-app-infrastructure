# codepipeline

1. `taskdef.json`の`<<ACCOUT_ID>>`は実際の12桁のアカウントIDに置換する
2. `appspec.yaml`と`taskdef.json`を圧縮して`point-app-task-def.zip`にする
3. [S3](https://s3.console.aws.amazon.com/s3/buckets/point-app-codepipeline?region=ap-northeast-1&tab=objects)に`point-app-task-def.zip`を配置する
