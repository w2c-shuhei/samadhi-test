# CloudFormationテンプレート


## 作成方針

* AWS CloudFormation ベストプラクティスをベースに作成
https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/best-practices.html
* 今後の保守性を考え、テンプレートは YAML 形式で作成
* ライフサイクル・所有権の観点から、一定のルール(AWSアカウントやサービス単位(※1))で Stack を分け、  
適切なディレクトリ構成を定め管理
* Root Stack へ環境毎にテンプレートとパラメータを投入しAWSリソースを作成し、テンプレートは共通化
* AWS CLIでの操作や管理を想定、実際のコマンドや環境毎に必要となるパラメータ・環境変数を Stack毎に Makefile に記述
* テンプレート間はクロススタック参照を用いて、リソースを共有する
* 各テンプレートにて可能な限りサポートされている固有パラメータや疑似パラメータ(AWS::AccountId 等)を使用  
尚、AWS固有のパラメーター型が使えない場合、基本的には「AllowedPattern」を使う
* 作成するStakやAWSリソース、設定するタグ名について、命名規則を定め設定・管理  

※1 サービス単位…お客様のサービス単位(「Feelnoteサービス」「Feelnote管理画面」等)  
※ AWS CloudFormation ベストプラティクス  
https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/best-practices.html  
※ 固有パラメータ  
https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html  
※ 疑似パラメータ  
https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html  


## システム要件

* AWS CLI
* Bash
* GNU Make
* jq
* ...


## ディレクトリ構成

ライフサイクル・所有権の観点から、一定のルール(AWSアカウントやサービス単位)で Stack を分け、適切なディレクトリ構成を定め管理

* AWSアカウント
AWSの契約単位で階層化（「aws-************」

* サービス又は環境単位
サービス単位又は環境(本番環境/ステージング環境/開発環境)で階層化

* ディレクトリ構成

```
TopDir/
├─ stacks/
│     └─ aws-************/              // AWSアカウント・スタック毎に作成
│            ├─ feelnote-prod/             　// feelnote 本番用スタック
│            │     ├─ Makefile
│            │     └─ master.yaml
│            ├─ feelnote-stg/                // feelnote ステージング用スタック
│            │     ├─ Makefile
│            │     └─ master.yaml
│            ├─ infrastructure-all-prod/     // VPC + Endpoint + EFS 作成スタック
│            │     ├─ Makefile
│            │     └─ master.yaml
│            ├─ infrastructure-vpc-prod/     // VPC 作成スタック
│            │     ├─ Makefile
│            │     └─ master.yaml
│            ├─ infrastructure-endpoint-prod/     // Endpoint 作成スタック
│            │     ├─ Makefile
│            │     └─ master.yaml
│            └─ infrastructure-efs-prod/     // EFS 作成スタック
│                   ├─ Makefile
│                   └─ master.yaml
├─ templates/                               // Cloudformationテンプレート
│     ├─ infrastructure/                   // VPC, Endpoint, EFS 作成テンプレート
│     │      ├─ vpc.yaml
│     │      └─ efs.yaml
│     └─ service/                          // サービス関連のテンプレート
│            ├─ load-balancer.yaml
│            ├─ ecs-cluster.yaml
│            └─ auto-scaling.yaml
└─ Readme.md

```


## スタックの作成

### Makefile・master.yaml

スタック毎に Makefile を作成し、パラメータや環境変数を定義します。  
AWS CLIを利用して、スタックを作成するコマンドを記載。

* Makefile と master.yaml のサンプル

```
PROJECT_NAME_ID=feelnote-service
ENVIRONMENT_TYPE=prod
VPC_CIDER=172.16.3.0/24
PUBLIC_SUBNET_A_CIDER=172.16.3.0/27
PUBLIC_SUBNET_C_CIDER=172.16.3.32/27
PRIVATE_SUBNET_A_CIDER=172.16.3.64/27
PRIVATE_SUBNET_C_CIDER=172.16.3.96/27
SECURE_SUBNET_A_CIDER=172.16.3.128/27
SECURE_SUBNET_C_CIDER=172.16.3.160/27

.PHONY: deploy
deploy:
	aws cloudformation deploy \
			--stack-name ${PROJECT_NAME_ID}-${ENVIRONMENT_TYPE}-Root \
			--template-file $(PWD)/stacks/aws-************/feelnote-service-prod/master.yaml \
			--parameter-overrides \
					EnvironmentType=${PROJECT_NAME_ID} \
					ProjectNameId=${ENVIRONMENT_TYPE} \
					VPCCider=${VPC_CIDER} \
					PublicSubnetACider=${PUBLIC_SUBNET_A_CIDER} \
					PublicSubnetCCider=${PUBLIC_SUBNET_C_CIDER} \
					PrivateSubnetACider=${PRIVATE_SUBNET_A_CIDER} \
					PrivateSubnetCCider=${PRIVATE_SUBNET_C_CIDER} \
					SecureSubnetACider=${SECURE_SUBNET_A_CIDER} \
					SecureSubnetCCider=${SECURE_SUBNET_C_CIDER} \
			--tags \
					StackType=${PROJECT_NAME_ID}-${ENVIRONMENT_TYPE}-Root


```

```
AWSTemplateFormatVersion: '2010-09-09'

Description: >
  CloudFormation Nest Template Parent.

Parameters:
  VPCCommonUrl:
    Default: "https://aws-cloudformation-test-************.s3-ap-northeast-1.amazonaws.com/vpc.yaml"
    Type: String
  EnvironmentType:
    Type: String
  ProjectNameId:
    Type: String
  VPCCider:
    Type: String
  PublicSubnetACider:
    Type: String
  PublicSubnetCCider:
    Type: String
  PrivateSubnetACider:
    Type: String
  PrivateSubnetCCider:
    Type: String
  SecureSubnetACider:
    Type: String
  SecureSubnetCCider:
    Type: String
  CreateVPC:
    Type: String
    Default: Yes

Resources:
  NestVPCCommon:
    Type: "AWS::CloudFormation::Stack"
    Properties:
      TemplateURL:
        Ref: VPCCommonUrl
      Parameters:
        EnvironmentType: !Ref EnvironmentType
        ProjectNameId: !Ref ProjectNameId
        VPCCider: !Ref VPCCider
        PublicSubnetACider: !Ref PublicSubnetACider
        PublicSubnetCCider: !Ref PublicSubnetCCider
        PrivateSubnetACider: !Ref PrivateSubnetACider
        PrivateSubnetCCider: !Ref PrivateSubnetCCider
        SecureSubnetACider: !Ref SecureSubnetACider
        SecureSubnetCCider: !Ref SecureSubnetCCider
```

* 実行例

システム要件を満たした環境から、makeコマンドを実行し、スタックを操作します。  
(下記はスタック作成時のコマンド実行例)

```
$ make -C stacks/aws-************/feelnote-service-prod/ deploy
```


## パラメータ

* テンプレートに認証情報を埋め込まない  
Systems Manager パラメータストア又は Secrets Manager に設定・保存した値を参照
* AWS固有のパラメータタイプの使用  
https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html  
* AWS固有のパラメーター型が使えない場合、基本的には「AllowedPattern」を使う  
* 短縮形構文の使用（Fn::Sub → ！Sub 等）
* 組み込み関数・疑似パラメーターを使く  
    - 組み込み関数（Fn::ImportValue、Fn::GetAtt、Ref、Fn::Join 等）  
    - 疑似パラメータ（AWS::AccountId 等）  


## 命名規則

### Stack名

__${サービス名}-${環境}-Root__

* 終端の「Root」を除いて小文字で記載し、ハイフン(-)で繋げる
* サービス名 … 対象となるサービス・アプリケーションの名前、一意となる識別子
* 環境 … 本番(prod)、検証(stg)、開発(dev)、負荷試験(load) 等


### AWSリソース

__${サービス名}-${AWSサービス名/種別}-${環境}__

* 全て小文字で記載、ハイフン(-)で繋げる
* サービス名 … 対象となるサービス・アプリケーションの名前、一意となる識別子
* AWSサービス名 … VPC(VPC, Subnet, RouteTable), ELB, ECS(Cluster, Service等)
* 環境 … 本番(prod)、検証(stg)、開発(dev)、負荷試験(load) 等
* その他 … ネットワークのレイヤー(public, private, secure)、目的(log, backup等)


### 論理ID

* ハイフン(-)なしで記載する為、見た目でわかりやすく記載  
例えば、パブリックサブネットのリソース作成の場合「PublicSubnetA」のように 大文字・小文字を組み合わせる


### エキスポート名

__${サービス名}-${論理ID}-${環境}-export__

* いくつか制限あるので、注意して設定  
https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/outputs-section-structure.html


### タグ(Stack、各リソース)

基本、AWSリソース名と同じ命名規則で設定する
