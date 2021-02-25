#
# サービス起動コマンド:
#   $ make -C stacks/aws-************/infrastructure-stg/ deploy
#

TEMPLATE_BUCKET=aws-cloudformation-test-105426345459
SERVICE_NAME=feelnote
ENVIRONMENT_TYPE=stg

VPC_ID=vpc-06443ee445dfa02b7
VPC_CIDER=172.41.0.0/16
WEB01_SUBNET_C_ID=subnet-07ddbff65cb51712d
WEB02_SUBNET_A_ID=subnet-0657aafd3ee9a1448

.PHONY: deploy
deploy:
	make deploy-templates
	aws cloudformation deploy \
			--stack-name ${SERVICE_NAME}-infrastructure-efs-${ENVIRONMENT_TYPE}-Root \
			--template-file $(PWD)/stacks/aws-105426345459/infrastructure-efs-${ENVIRONMENT_TYPE}/master.yaml \
			--parameter-overrides \
					TemplateBucket=${TEMPLATE_BUCKET} \
					ServiceName=${SERVICE_NAME} \
					EnvironmentType=${ENVIRONMENT_TYPE} \
					VpcId=${VPC_ID} \
					VpcCider=${VPC_CIDER} \
					Web01SubnetCId=${WEB01_SUBNET_C_ID} \
					Web02SubnetAId=${WEB02_SUBNET_A_ID} \
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
	aws s3 cp $(PWD)/stacks/aws-105426345459/infrastructure-efs-${ENVIRONMENT_TYPE}/master.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/stacks/aws-105426345459/infrastructure-efs-${ENVIRONMENT_TYPE}/
	aws s3 cp $(PWD)/stacks/aws-105426345459/infrastructure-efs-${ENVIRONMENT_TYPE}/Makefile s3://aws-cloudformation-test-105426345459/samadhi-test/stacks/aws-105426345459/infrastructure-efs-${ENVIRONMENT_TYPE}/
	aws s3 cp $(PWD)/templates/infrastructure/efs.yaml s3://aws-cloudformation-test-105426345459/samadhi-test/templates/infrastructure/
