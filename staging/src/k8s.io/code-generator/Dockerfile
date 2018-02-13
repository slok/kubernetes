FROM golang:1.9

RUN apt-get update && \
    apt-get install -y \
    git 

# Code generator stuff
# Check: https://github.com/kubernetes/kubernetes/pull/57656
RUN mkdir -p /go/src/k8s.io/kubernetes/hack/boilerplate/ && \
    touch /go/src/k8s.io/kubernetes/hack/boilerplate/boilerplate.go.txt && \
    mkdir -p /go/src/k8s.io/code-generator && \
    go get k8s.io/apimachinery/pkg/runtime

# Create user.
ARG uid=1000
ARG gid=1000
RUN addgroup --gid $gid codegen && \
    adduser --gecos "First Last,RoomNumber,WorkPhone,HomePhone" --disabled-password --uid $uid --ingroup codegen codegen && \
    chown codegen:codegen -R /go
USER codegen
WORKDIR /go/src/k8s.io/code-generator

# Get code-generator and its dependencies dependencies.
COPY --chown=codegen:codegen . /go/src/k8s.io/code-generator/
RUN go get github.com/tools/godep && \
    godep restore


WORKDIR /go/src/k8s.io/code-generator/hack
CMD ["./update-codegen.sh"]