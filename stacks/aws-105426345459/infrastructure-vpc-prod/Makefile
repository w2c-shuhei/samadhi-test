#
# サービス起動コマンド:
#   $ make -C stacks/aws-************/infrastructure-stg/ deploy
#

TEMPLATE_BUCKET=aws-cloudformation-test-105426345459
SERVICE_NAME=feelnote
ENVIRONMENT_TYPE=prod
VPC_ID=vpc-06abe8a276b2af5e6
VPC_CIDER=172.31.0.0/16
WEB01_SUBNET_C_ID=subnet-07de7c27c789ccf08
WEB02_SUBNET_A_ID=subnet-01ccc813099b7dfcf

.PHONY: deploy
deploy:
	make deploy-templates
	aws cloudformation deploy \
			--stack-name ${SERVICE_NAME}-infrastructure-${ENVIRONMENT_TYPE}-Root \
			--template-file $(PWD)/stacks/aws-105426345459/infrastructure-${ENVIRONMENT_TYPE}/master.yaml \
			--parameter-overrides \
					TemplateBucket=${TEMPLATE_BUCKET} \
					ServiceName=${SERVICE_NAME} \
					EnvironmentType=${ENVIRONMENT_TYPE} \
					VpcId=${VPC_ID} \
					VpcCider=${VPC_CIDER} \
					WEB01SubnetCId=${WEB01_SUBNET_C_ID} \
					WEB02SubnetAId=${WEB02_SUBNET_A_ID} \
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
	aws s3 cp $(PWD)/stacks/aws-105426345459/infrastructure-${ENVIRONMENT_TYPE}/master.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/stacks/aws-105426345459/infrastructure-${ENVIRONMENT_TYPE}/
	aws s3 cp $(PWD)/stacks/aws-105426345459/infrastructure-${ENVIRONMENT_TYPE}/Makefile s3://aws-cloudformation-test-105426345459/samadhi-test/stacks/aws-105426345459/infrastructure-${ENVIRONMENT_TYPE}/
	aws s3 cp $(PWD)/templates/infrastructure/efs.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/templates/infrastructure/
