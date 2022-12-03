# IaC

今回は、IaC として CloudFormation を利用する。

CloudFormation では、以下をリソースとして作成する。

- VPC およびサブネットの構築
- インターネットゲートウェイ
- ルートテーブル
- セキュリティグループ

## VPC とサブネット

マルチ AZ 構成を利用することで、データセンターの物理障害に対する可用性を高める

- Ingress
  - インターネットからリクエストを受け付ける Ingress(ALB)用サブネット
- app contaner
  - アプリ稼働用サブネット
- db
  - データベース用サブネット
- cache
  - キャッシュサーバー用サブネット
- management
  - 運用管理用サブネット
- egress
  - ECR など外部にリクエストするインターフェース型 VPC エンドポイント用サブネット

| name          | NW      | AZ  | CIDR          | subnet name                           |
| ------------- | ------- | --- | ------------- | ------------------------------------- |
| Ingress       | Public  | 1a  | 10.0.0.0/24   | point-app-subnet-public-ingress-1a    |
| Ingress       | Public  | 1c  | 10.0.1.0/24   | point-app-subnet-public-ingress-1c    |
| app container | Private | 1a  | 10.0.8.0/24   | point-app-subnet-private-container-1a |
| app container | Private | 1c  | 10.0.9.0/24   | point-app-subnet-private-container-1c |
| db            | Private | 1a  | 10.0.16.0/24  | point-app-subnet-private-db-1a        |
| db            | Private | 1c  | 10.0.17.0/24  | point-app-subnet-private-db-1c        |
| cache         | Private | 1a  | 10.0.24.0/24  | point-app-subnet-private-cache-1a     |
| cache         | Private | 1c  | 10.0.25.0/24  | point-app-subnet-private-cache-1c     |
| management    | Public  | 1a  | 10.0.240.0/24 | point-app-subnet-public-management-1a |
| management    | Public  | 1c  | 10.0.241.0/24 | point-app-subnet-public-management-1c |
| egress        | Private | 1a  | 10.0.248.0/24 | point-app-subnet-private-egress-1a    |
| egress        | Private | 1c  | 10.0.249.0/24 | point-app-subnet-private-egress-1c    |

## インターネットゲートウェイ

VPC 内のリソースがインターネット通信する際に必要なるネットワークリソース。
VPC ごとに 1 つだけひとつ紐づける。

public IP と private iP を 1 対 1 で変換し、通信する。

## ルートテーブル

ルートテーブルとは、ネットワークの経路を設定する他 m のリソース。

宛先としてデフォルトゲートウェイ（0.0.0.0/0）が指定された場合にインターネットゲートウェイへ向けるルールを設定する。

## セキュリティグループ

アクセスの制御を行う。

アウトバウンドルールは「0.0.0.0/0」を許可し、インバウンドルールは Ingres は最小限にする。

詳しくは、[こちら](https://dev.classmethod.jp/articles/ec2-cacess/)を見るとよいかも。
