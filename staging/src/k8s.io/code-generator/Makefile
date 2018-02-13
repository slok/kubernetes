

IMAGE := k8s/code-generator:latest

DIRECTORY := $(PWD)
DIRECTORY_K8S := $(PWD)/../../../../
CODE_GENERATOR_PACKAGE := k8s.io/code-generator
K8S_PACKAGE := k8s.io/kubernetes
EXAMPLES_APISERVER_PACKAGE := k8s.io/code-generator/_examples/apiserver
EXAMPLES_CRD_PACKAGE := k8s.io/code-generator/_examples/crd

default: build

build:
	docker build -t $(IMAGE) .

generate-example-internal:
	docker run -it --rm \
	-v $(DIRECTORY):/go/src/$(CODE_GENERATOR_PACKAGE) \
	-v $(DIRECTORY_K8S):/go/src/$(K8S_PACKAGE) \
	-e GO_HEADER_FILE=/go/src/$(K8S_PACKAGE)/hack/boilerplate/boilerplate.go.txt \
	-e PROJECT_PACKAGE=${EXAMPLES_APISERVER_PACKAGE} \
	-e CLIENT_GENERATOR_OUT=${EXAMPLES_APISERVER_PACKAGE} \
	-e APIS_ROOT=${EXAMPLES_APISERVER_PACKAGE}/apis \
	-e EXTERNAL_APIS_ROOT=${EXAMPLES_APISERVER_PACKAGE}/apis \
	-e GROUPS_VERSION="example:v1 example2:v1" \
	$(IMAGE) ./update-codegen.sh

generate-example-crd:
	docker run -it --rm \
	-v $(DIRECTORY):/go/src/$(CODE_GENERATOR_PACKAGE) \
	-v $(DIRECTORY_K8S):/go/src/$(K8S_PACKAGE) \
	-e GO_HEADER_FILE=/go/src/$(K8S_PACKAGE)/hack/boilerplate/boilerplate.go.txt \
	-e PROJECT_PACKAGE=${EXAMPLES_CRD_PACKAGE} \
	-e CLIENT_GENERATOR_OUT=${EXAMPLES_CRD_PACKAGE} \
	-e APIS_ROOT=${EXAMPLES_CRD_PACKAGE}/apis \
	-e GROUPS_VERSION="example:v1 example2:v1" \
	$(IMAGE) ./update-codegen.sh

shell:
	docker run -it --rm \
	-v $(DIRECTORY):/go/src/$(CODE_GENERATOR_PACKAGE) \
	-v $(DIRECTORY_K8S):/go/src/$(K8S_PACKAGE) \
	$(IMAGE) /bin/bash