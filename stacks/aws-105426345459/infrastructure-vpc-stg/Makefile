#
# サービス起動コマンド:
#   $ make -C stacks/aws-************/infrastructure-prod/ deploy
#

TEMPLATE_BUCKET=aws-cloudformation-test-105426345459
SERVICE_NAME=feelnote
ENVIRONMENT_TYPE=stg

# 新しく作成するVPC・SubnetのCIDERを指定
VPC_CIDER=172.41.0.0/16
ELB01_SUBNET_A_CIDER=172.41.254.0/24
ELB02_SUBNET_C_CIDER=172.41.255.0/24
WEB01_SUBNET_C_CIDER=172.41.0.0/20
WEB02_SUBNET_A_CIDER=172.41.16.0/20
RDS01_SUBNET_C_CIDER=172.41.32.0/24
RDS02_SUBNET_A_CIDER=172.41.33.0/24
ELASICACHE01_SUBNET_C_CIDER=172.41.34.0/24
ELASICACHE02_SUBNET_A_CIDER=172.41.35.0/24
JOB01_SUBNET_A_CIDER=172.41.36.0/24
JOB02_SUBNET_C_CIDER=172.41.37.0/24

.PHONY: deploy
deploy:
	make deploy-templates
	aws cloudformation deploy \
			--stack-name ${SERVICE_NAME}-infrastructure-vpc-${ENVIRONMENT_TYPE}-Root \
			--template-file $(PWD)/stacks/aws-105426345459/infrastructure-vpc-${ENVIRONMENT_TYPE}/master.yaml \
			--parameter-overrides \
					TemplateBucket=${TEMPLATE_BUCKET} \
					ServiceName=${SERVICE_NAME} \
					EnvironmentType=${ENVIRONMENT_TYPE} \
					VpcCider=${VPC_CIDER} \
					Elb01SubnetACider=${ELB01_SUBNET_A_CIDER} \
					Elb02SubnetCCider=${ELB02_SUBNET_C_CIDER} \
					WeB01SubnetCCider=${WEB01_SUBNET_C_CIDER} \
					Web02SubnetACider=${WEB02_SUBNET_A_CIDER} \
					Rds01SubnetCCider=${RDS01_SUBNET_C_CIDER} \
					Rds02SubnetACider=${RDS02_SUBNET_A_CIDER} \
					Elasicache01SubnetCCider=${ELASICACHE01_SUBNET_C_CIDER} \
					Elasicache02SubnetACider=${ELASICACHE02_SUBNET_A_CIDER} \
					Job01SubnetACider=${JOB01_SUBNET_A_CIDER} \
					Job02SubnetCCider=${JOB02_SUBNET_C_CIDER} \
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
	aws s3 cp $(PWD)/stacks/aws-105426345459/infrastructure-vpc-${ENVIRONMENT_TYPE}/master.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/stacks/aws-105426345459/infrastructure-vpc-${ENVIRONMENT_TYPE}/
	aws s3 cp $(PWD)/stacks/aws-105426345459/infrastructure-vpc-${ENVIRONMENT_TYPE}/Makefile s3://aws-cloudformation-test-105426345459/samadhi-test/stacks/aws-105426345459/infrastructure-vpc-${ENVIRONMENT_TYPE}/
	aws s3 cp $(PWD)/templates/infrastructure/vpc.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/templates/infrastructure/
