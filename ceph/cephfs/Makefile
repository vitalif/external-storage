# Copyright 2017 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

REGISTRY := quay.io/external_storage/
VERSION := latest
CEPH_VERSION := mimic
IMAGE := $(REGISTRY)cephfs-provisioner:$(VERSION)
MUTABLE_IMAGE := $(REGISTRY)cephfs-provisioner:latest

all build:
	CGO_ENABLED=0 GOOS=linux go build -a -ldflags '-extldflags "-static"' -o cephfs-provisioner cephfs-provisioner.go
.PHONY: all build

container: build quick-container
.PHONY: container

quick-container:
	docker build --build-arg=CEPH_VERSION=$(CEPH_VERSION) -t $(MUTABLE_IMAGE) .
	docker tag $(MUTABLE_IMAGE) $(IMAGE)
.PHONY: quick-container

push: container
	docker push $(IMAGE)
	docker push $(MUTABLE_IMAGE)
.PHONY: push

test:
	go test ./...
.PHONY: test

clean:
	rm -f cephfs-provisioner
.PHONY: clean
