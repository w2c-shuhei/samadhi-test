# Infrastructure Stack (VPC・Endpoint・EFS)

## 構成

以下ディレクトリに環境毎の Makefile・master.yaml を配置
環境毎の Makefile は同階層の master.yaml を参照し、スタックを作成・更新
(※ 構成詳細は README.md を参照)

* stacks/aws-536459734415/infrastructure-all-prod (※ VPC+Endpoint+EFS 全て作成するスタック)
* stacks/aws-536459734415/infrastructure-vpc-prod (※ VPC のみ作成するスタック)
* stacks/aws-536459734415/infrastructure-endpoint-prod (※ Endpoint のみ全て作成するスタック)
* stacks/aws-536459734415/infrastructure-efs-prod (※ EFS のみ全て作成するスタック)

## スタック作成コマンド

Linux make コマンドを実行し、スタック作成・削除

```
# CloudFormationスタック作成
$ make -C stacks/aws-536459734415/infrastructure-all-prod/ deploy

# CloudFormationスタック削除
$ make -C stacks/aws-536459734415/infrastructure-all-prod/ delete
```

## Makefile（パラメータ・実行CLI）

環境毎の挙動の違いをMakefileの変数で制御

```
TEMPLATE_BUCKET：Clouformationテンプレートの配置用のSバケット
SERVICE_NAME：feelnote
ENVIRONMENT_TYPE：環境の識別子(prod/stg/stg/load)
```

```
VPC_CIDER：VPCのCIDER
ELB01_SUBNET_A_CIDER：ELB用サブネットのCIDER(AZ-a)
ELB02_SUBNET_C_CIDER：ELB用サブネットのCIDER(AZ-c)
WEB01_SUBNET_C_CIDER：EC2・タスク用サブネットのCIDER(AZ-c)
WEB02_SUBNET_A_CIDER：EC2・タスク用サブネットのCIDER(AZ-a)
RDS01_SUBNET_C_CIDER：RDS用サブネットのCIDER(AZ-c)
RDS02_SUBNET_A_CIDER：RDS用サブネットのCIDER(AZ-a)
ELASICACHE01_SUBNET_C_CIDER：Elasticahe用サブネットのCIDER(AZ-c)
ELASICACHE02_SUBNET_A_CIDER：Elasticahe用サブネットのCIDER(AZ-a)
JOB01_SUBNET_A_CIDER：JOBサーバ用サブネットのCIDER(AZ-a)
JOB02_SUBNET_C_CIDER：JOBサーバ用サブネットのCIDER(AZ-c)
```

