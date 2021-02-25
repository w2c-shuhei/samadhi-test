# feelnote Stack

## 構成

以下ディレクトリに環境毎の Makefile・master.yaml を配置
環境毎の Makefile は同階層の master.yaml を参照し、スタックを作成・更新
(※ 構成詳細は README.md を参照)

* stacks/aws-536459734415/feelnote-prod
* stacks/aws-536459734415/feelnote-stg

## スタック作成コマンド

Linux make コマンドを実行し、スタック作成・削除

```
# CloudFormationスタック作成
$ make -C stacks/aws-536459734415/feelnote-prod/ deploy

# CloudFormationスタック削除
$ make -C stacks/aws-536459734415/feelnote-prod/ delete
```

## Makefile（パラメータ・実行CLI）

環境毎の挙動の違いをMakefileの変数で制御

```
TEMPLATE_BUCKET：Clouformationテンプレートの配置用のSバケット
SERVICE_NAME：feelnote
ENVIRONMENT_TYPE：環境の識別子(prod/stg/stg/load)
LOAD_BALANCER_PUBLIC：パブリックアクセスの有無(true or false)
ZONE_NAME：feelnote.org
SUB_DOMAIN_NAME_1：v2.feelnote.org
SUB_DOMAIN_NAME_2：worldschool.feelnote.org
SUB_DOMAIN_NAME_3：admin.feelnote.org
VPC_ID：リソースを作成すするVPCのID
VPC_CIDER：リソースを作成すするVPCのCIDER
ELB01_SUBNET_A_ID：ALBを配置するサブネットのID
ELB01_SUBNET_C_ID：ALBを配置するサブネットのID
WEB01_SUBNET_C_ID：タスクを配置するサブネットのID
WEB01_SUBNET_A_ID：タスクを配置するサブネットのID
```

```
#
# EFS 作成フラグ
#
CREATE_EFS：EFSの作成有無(true or false)
```

```
#
# ALB 作成パラメータ
#
IDLE_TIMEOUT_SECONDS：アイドルタイムアウト(デフォルト：60秒)
LISTENER_CERTIFICATION_ARN：ACMのAmazonリソースネーム
ALB_LISTENER_SSL_POLICY：セキュリティポリシー(デフォルト：ELBSecurityPolicy-2016-08)
```

```
#
# Elastic Container Service(ECS) 作成パラメータ
#   - <サービス名> … v2 / worldschool / admin 
#
ECS_<サービス名>_TASK_CPU_UNIT：タスクが使用するCPUユニット数
ECS_<サービス名>_TASK_MEMORY：タスクが使用するメモリ量(MiB単位)
ECS_<サービス名>_CONTAINER_PORT：ホストポートにバインドされるコンテナポート番号
ECS_<サービス名>_IMAGE_NAME：ECRにアップロードされているコンテナイメージ
ECS_<サービス名>_TASK_DESIRED_COUNT：タスク実行数 (※V2_MIN_CAPACITYと同じ値を設定)
ECS_FILE_SYSTEM_ID：EFSのファイルシステムID
```

```
#
# Application Auto Scaling 作成パラメータ
#   - 「ターゲット追跡ポリシースケール」を使用してスケーリング
#   - <サービス名> … v2 / worldschool / admin 
#
<サービス名>_MAX_CAPACITY：スケールアウトするタスクの最大値
<サービス名>_MIN_CAPACITY：スケールインするタスクの最小値 (※ECS_<サービス名>_TASK_DESIRED_COUNTと同じ値を設定)
<サービス名>_TARGET_SCALING_POLICY_OUT_COOLDOWN：以前のスケーリングアクティビティが有効になるまで待機する時間(秒単位)
<サービス名>_TARGET_SCALING_POLICY_IN_COOLDOWN：以前のスケーリングアクティビティが有効になるまで待機する時間(秒単位)
<サービス名>_TARGET_VALUE_CPU：
<サービス名>_TARGET_VALUE_MEMORY：
```
