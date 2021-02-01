
export AWS_DEFAULT_REGION ?= us-east-1
export CF_S3_BUCKET ?= $(shell whoami)-cf-templates-$(AWS_DEFAULT_REGION)
export CF_STACK_NAME ?= global-csm

.PHONY: clean

clean:
	rm -f packaged.yaml

env:
	env | egrep '(S3|AWS)'


packaged.yaml: root.yaml templates/*.yaml
	aws cloudformation package --template-file root.yaml \
		--s3-bucket $(CF_S3_BUCKET) \
		--output-template-file $@


deploy: packaged.yaml
	aws cloudformation deploy --template-file packaged.yaml \
		--capabilities CAPABILITY_IAM \
		--capabilities CAPABILITY_NAMED_IAM \
		--stack-name $(CF_STACK_NAME) 

delete:
	aws cloudformation delete-stack \
        --role-arn arn:aws:iam::158371978634:role/temp-cloudformation-role \
		--stack-name $(CF_STACK_NAME) 

