#
# サービス起動コマンド:
#   $ make -C stacks/aws-************/feelnote-prod/ deploy
#

TEMPLATE_BUCKET=aws-cloudformation-test-105426345459
SERVICE_NAME=feelnote
ENVIRONMENT_TYPE=prod
LOAD_BALANCER_PUBLIC=true
#ZONE_NAME=fellnote.org
ZONE_NAME=cloud-master.biz
#ZONE_ID=**********
SUB_DOMAIN_NAME_1=v2.${ZONE_NAME}
SUB_DOMAIN_NAME_2=worldschool.${ZONE_NAME}
SUB_DOMAIN_NAME_3=admin.${ZONE_NAME}
VPC_ID=vpc-06abe8a276b2af5e6
VPC_CIDER=172.31.0.0/16
ELB01_SUBNET_A_ID=subnet-0d63df8b26416f29c
ELB02_SUBNET_C_ID=subnet-05f42f0867fe6ed5b
WEB01_SUBNET_C_ID=subnet-07de7c27c789ccf08
WEB02_SUBNET_A_ID=subnet-01ccc813099b7dfcf

# EFSの作成する場合は "true" を指定
CREATE_EFS=false

IDLE_TIMEOUT_SECONDS=60
LISTENER_CERTIFICATION_ARN="arn:aws:acm:ap-northeast-1:105426345459:certificate/8006cfb0-0881-4f75-8caa-a2fc234babd8"
ALB_LISTENER_SSL_POLICY="ELBSecurityPolicy-2016-08"

ECS_V2_TASK_CPU_UNIT=256
ECS_V2_TASK_MEMORY=512
ECS_WORLDSCHOOL_TASK_CPU_UNIT=256
ECS_WORLDSCHOOL_TASK_MEMORY=512
ECS_ADMIN_TASK_CPU_UNIT=256
ECS_ADMIN_TASK_MEMORY=512
ECS_V2_CONTAINER_PORT=80
ECS_WORLDSCHOOL_CONTAINER_PORT=80
ECS_ADMIN_CONTAINER_PORT=80
#ECS_V2_CONTAINER_PORT=8080
#ECS_WORLDSCHOOL_CONTAINER_PORT=8081
#ECS_ADMIN_CONTAINER_PORT=8082
ECS_V2_IMAGE_NAME=105426345459.dkr.ecr.ap-northeast-1.amazonaws.com/test:latest
ECS_WORLDSCHOOL_IMAGE_NAME=105426345459.dkr.ecr.ap-northeast-1.amazonaws.com/test:latest
ECS_ADMIN_IMAGE_NAME=105426345459.dkr.ecr.ap-northeast-1.amazonaws.com/test:latest
ECS_V2_TASK_DESIRED_COUNT=1
ECS_WORLDSCHOOL_TASK_DESIRED_COUNT=1
ECS_ADMIN_TASK_DESIRED_COUNT=1
ECS_FILE_SYSTEM_ID=fs-7d9f535d

# オートスケーリングを有効にする場合は "true" を指定
AUTO_SCALING_ENABLE=true

V2_MAX_CAPACITY=2
V2_MIN_CAPACITY=1
V2_TARGET_SCALING_POLICY_OUT_COOLDOWN=300
V2_TARGET_SCALING_POLICY_IN_COOLDOWN=300
V2_TARGET_VALUE_CPU=30
V2_TARGET_VALUE_MEMORY=50

WORLDSCHOOL_MAX_CAPACITY=2
WORLDSCHOOL_MIN_CAPACITY=1
WORLDSCHOOL_SCALING_POLICY_COOLDOWN=300
WORLDSCHOOL_TARGET_SCALING_POLICY_OUT_COOLDOWN=300
WORLDSCHOOL_TARGET_SCALING_POLICY_IN_COOLDOWN=300
WORLDSCHOOL_TARGET_VALUE_CPU=30
WORLDSCHOOL_TARGET_VALUE_MEMORY=50

ADMIN_MAX_CAPACITY=2
ADMIN_MIN_CAPACITY=1
ADMIN_SCALING_POLICY_COOLDOWN=300
ADMIN_TARGET_SCALING_POLICY_OUT_COOLDOWN=300
ADMIN_TARGET_SCALING_POLICY_IN_COOLDOWN=300
ADMIN_TARGET_VALUE_CPU=30
ADMIN_TARGET_VALUE_MEMORY=50

.PHONY: deploy
deploy:
	make deploy-templates
	aws cloudformation deploy \
			--stack-name ${SERVICE_NAME}-${ENVIRONMENT_TYPE}-Root \
			--template-file $(PWD)/stacks/aws-105426345459/feelnote-${ENVIRONMENT_TYPE}/master.yaml \
			--parameter-overrides \
					TemplateBucket=${TEMPLATE_BUCKET} \
					ServiceName=${SERVICE_NAME} \
					EnvironmentType=${ENVIRONMENT_TYPE} \
					LoadBalancerPublic=${LOAD_BALANCER_PUBLIC} \
					SubDomainNmae1=${SUB_DOMAIN_NAME_1} \
					SubDomainNmae2=${SUB_DOMAIN_NAME_2} \
					SubDomainNmae3=${SUB_DOMAIN_NAME_3} \
					VpcId=${VPC_ID} \
					VpcCider=${VPC_CIDER} \
					ELB01SubnetAId=${ELB01_SUBNET_A_ID} \
					ELB02SubnetCId=${ELB02_SUBNET_C_ID} \
					WEB01SubnetCId=${WEB01_SUBNET_C_ID} \
					WEB02SubnetAId=${WEB02_SUBNET_A_ID} \
					CreateEFS=${CREATE_EFS} \
					IdleTimeoutSeconds=${IDLE_TIMEOUT_SECONDS} \
					ListenerCertificateArn=${LISTENER_CERTIFICATION_ARN} \
					AlbListenerSslPolicy=${ALB_LISTENER_SSL_POLICY} \
					ECSV2TaskCPUUnit=${ECS_V2_TASK_CPU_UNIT} \
					ECSV2TaskMemory=${ECS_V2_TASK_MEMORY} \
					ECSWordschoolTaskCPUUnit=${ECS_WORLDSCHOOL_TASK_CPU_UNIT} \
					ECSWordschoolTaskMemory=${ECS_WORLDSCHOOL_TASK_MEMORY} \
					ECSAdminTaskCPUUnit=${ECS_ADMIN_TASK_CPU_UNIT} \
					ECSAdminTaskMemory=${ECS_ADMIN_TASK_MEMORY} \
					ECSV2ContainerPort=${ECS_V2_CONTAINER_PORT} \
					ECSWordschoolContainerPort=${ECS_WORLDSCHOOL_CONTAINER_PORT} \
					ECSAdminContainerPort=${ECS_ADMIN_CONTAINER_PORT} \
					ECSV2ImageName=${ECS_V2_IMAGE_NAME} \
					ECSWorldschoolImageName=${ECS_WORLDSCHOOL_IMAGE_NAME} \
					ECSAdminImageName=${ECS_ADMIN_IMAGE_NAME} \
					ECSV2TaskDesiredCount=${ECS_V2_TASK_DESIRED_COUNT} \
					ECSWorldschoolTaskDesiredCount=${ECS_WORLDSCHOOL_TASK_DESIRED_COUNT} \
					ECSAdminTaskDesiredCount=${ECS_ADMIN_TASK_DESIRED_COUNT} \
					ECSFilesyStemId=${ECS_FILE_SYSTEM_ID} \
					AutoScalingEnable=${AUTO_SCALING_ENABLE} \
					V2MaxCapacity=${V2_MAX_CAPACITY} \
					V2MinCapacity=${V2_MIN_CAPACITY} \
					V2TargetScalingPolicyOutCooldown=${V2_TARGET_SCALING_POLICY_OUT_COOLDOWN} \
					V2TargetScalingPolicyInCooldown=${V2_TARGET_SCALING_POLICY_IN_COOLDOWN} \
					V2TargetValueCpu=${V2_TARGET_VALUE_CPU} \
					V2TargetValueMemory=${V2_TARGET_VALUE_MEMORY} \
					WorldschoolMaxCapacity=${WORLDSCHOOL_MAX_CAPACITY} \
					WorldschoolMinCapacity=${WORLDSCHOOL_MIN_CAPACITY} \
					WorldschoolTargetScalingPolicyOutCooldown=${WORLDSCHOOL_TARGET_SCALING_POLICY_OUT_COOLDOWN} \
					WorldschoolTargetScalingPolicyInCooldown=${WORLDSCHOOL_TARGET_SCALING_POLICY_IN_COOLDOWN} \
					WorldschoolTargetValueCpu=${WORLDSCHOOL_TARGET_VALUE_CPU} \
					WorldschoolTargetValueMemory=${WORLDSCHOOL_TARGET_VALUE_MEMORY} \
					AdminMaxCapacity=${ADMIN_MAX_CAPACITY} \
					AdminMinCapacity=${ADMIN_MIN_CAPACITY} \
					AdminTargetScalingPolicyOutCooldown=${ADMIN_TARGET_SCALING_POLICY_OUT_COOLDOWN} \
					AdminTargetScalingPolicyInCooldown=${ADMIN_TARGET_SCALING_POLICY_IN_COOLDOWN} \
					AdminTargetValueCpu=${ADMIN_TARGET_VALUE_CPU} \
					AdminTargetValueMemory=${ADMIN_TARGET_VALUE_MEMORY} \
			--capabilities CAPABILITY_IAM \
			--tags \
					StackName=${SERVICE_NAME}-${ENVIRONMENT_TYPE}-Root

.PHONY: delete
delete:
	aws cloudformation delete-stack \
			--stack-name ${APP_NAME}
	aws cloudformation wait stack-delete-complete \
			--stack-name ${APP_NAME}
	rm .stacks

.PHONY: deploy-templates
deploy-templates:
	aws s3 cp $(PWD)/stacks/aws-105426345459/feelnote-${ENVIRONMENT_TYPE}/master.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/stacks/aws-105426345459/feelnote-${ENVIRONMENT_TYPE}/
	aws s3 cp $(PWD)/stacks/aws-105426345459/feelnote-${ENVIRONMENT_TYPE}/Makefile s3://aws-cloudformation-test-105426345459/samadhi-test/stacks/aws-105426345459/feelnote-${ENVIRONMENT_TYPE}/
	aws s3 cp $(PWD)/templates/infrastructure/efs.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/templates/infrastructure/
	aws s3 cp $(PWD)/templates/service/load-balancer.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/templates/service/
	aws s3 cp $(PWD)/templates/service/ecs-cluster.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/templates/service/
	aws s3 cp $(PWD)/templates/service/auto-scaling.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/templates/service/
