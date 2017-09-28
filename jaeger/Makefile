
default: docker_build

build: docker_build output ## Build Docker image

release: docker_build docker_push ## Build and push Docker image

# Image and binary can be overidden with env vars.
DOCKER_IMAGE ?= carml/consul


# Get System architecture
# for now... ask carl how best to get this
ARCH ?= $(shell go env GOARCH)

# Get the latest commit.
GIT_COMMIT = $(strip $(shell git rev-parse --short HEAD))

# Get the version number from the code
CODE_VERSION = 0.1


# Find out if the working directory is clean
GIT_NOT_CLEAN_CHECK = $(shell git status --porcelain)

# If we're releasing to Docker Hub, and we're going to mark it with the latest tag, it should exactly match a version release
ifeq ($(MAKECMDGOALS),release)
# Use the version number as the release tag.
DOCKER_TAG = $(CODE_VERSION)

ifndef CODE_VERSION
$(error You need to create a VERSION file to build a release)
endif

# See what commit is tagged to match the version
VERSION_COMMIT = $(strip $(shell git rev-list $(CODE_VERSION) -n 1 | cut -c1-7))
ifneq ($(VERSION_COMMIT), $(GIT_COMMIT))
$(error echo You are trying to push a build based on commit $(GIT_COMMIT) but the tagged release version is $(VERSION_COMMIT))
endif

else
# Add the commit ref for development builds. Mark as dirty if the working directory isn't clean
DOCKER_TAG = $(CODE_VERSION)-$(GIT_COMMIT)$(DOCKER_TAG_SUFFIX)
endif

BUILD_ARGS = --build-arg BUILD_DATE=`date -u +"%Y-%m-%dT%H:%M:%SZ"` \
		--build-arg VERSION=$(CODE_VERSION) \
		--build-arg VCS_URL=`git config --get remote.origin.url` \
		--build-arg VCS_REF=$(GIT_COMMIT) \
		--build-arg ARCH=$(ARCH) 

docker_build_base: ## Build Agent
	@docker build $(BUILD_ARGS) \
		-f Dockerfile.base \
		-t $(DOCKER_IMAGE):base-$(ARCH)-$(DOCKER_TAG) .

docker_build_agent: ## Build Agent
	@docker build $(BUILD_ARGS) \
		-f Dockerfile.agent \
		-t $(DOCKER_IMAGE):agent-$(ARCH)-$(DOCKER_TAG) .

docker_build_query: ## Build Agent
	@docker build $(BUILD_ARGS) \
		-f Dockerfile.query \
		-t $(DOCKER_IMAGE):query-$(ARCH)-$(DOCKER_TAG) .

docker_build_collector: ## Build Agent
	@docker build $(BUILD_ARGS) \
		-f Dockerfile.collector \
		-t $(DOCKER_IMAGE):collector-$(ARCH)-$(DOCKER_TAG) .

docker_build_cassandra: ## Build Cassandra
	@docker build $(BUILD_ARGS) \
		-f Dockerfile.cassandra \
		-t $(DOCKER_IMAGE):cassandra-$(ARCH)-$(DOCKER_TAG) .

docker_build_standalone: ## Build Cassandra
	@docker build $(BUILD_ARGS) \
		-f Dockerfile.standalone \
		-t $(DOCKER_IMAGE):standalone-$(ARCH)-$(DOCKER_TAG) .

docker_build: docker_build_base docker_build_cassandra docker_build_query docker_build_collector docker_build_standalone ## Build docker files (order matters)

docker_push_base: ## Tag image as latest and Push to DockerHub
	docker tag $(DOCKER_IMAGE):base-$(ARCH)-$(DOCKER_TAG) $(DOCKER_IMAGE):base-$(ARCH)-latest
	docker push $(DOCKER_IMAGE):base-$(ARCH)-$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):base-$(ARCH)-latest

docker_push_agent: ## Tag image as latest and Push to DockerHub
	docker tag $(DOCKER_IMAGE):agent-$(ARCH)-$(DOCKER_TAG) $(DOCKER_IMAGE):agent-$(ARCH)-latest
	docker push $(DOCKER_IMAGE):agent-$(ARCH)-$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):agent-$(ARCH)-latest

docker_push_query: ## Tag image as latest and Push to DockerHub
	docker tag $(DOCKER_IMAGE):query-$(ARCH)-$(DOCKER_TAG) $(DOCKER_IMAGE):query-$(ARCH)-latest
	docker push $(DOCKER_IMAGE):query-$(ARCH)-$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):query-$(ARCH)-latest

docker_push_collector: ## Tag image as latest and Push to DockerHub
	docker tag $(DOCKER_IMAGE):collector-$(ARCH)-$(DOCKER_TAG) $(DOCKER_IMAGE):collector-$(ARCH)-latest
	docker push $(DOCKER_IMAGE):collector-$(ARCH)-$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):collector-$(ARCH)-latest

docker_push_cassandra: ## Tag image as latest and Push to DockerHub
	docker tag $(DOCKER_IMAGE):cassandra-$(ARCH)-$(DOCKER_TAG) $(DOCKER_IMAGE):cassandra-$(ARCH)-latest
	docker push $(DOCKER_IMAGE):cassandra-$(ARCH)-$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):cassandra-$(ARCH)-latest

docker_push_standalone: ## Tag image as latest and Push to DockerHub
	docker tag $(DOCKER_IMAGE):standalone-$(ARCH)-$(DOCKER_TAG) $(DOCKER_IMAGE):standalone-$(ARCH)-latest
	docker push $(DOCKER_IMAGE):standalone-$(ARCH)-$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):standalone-$(ARCH)-latest

docker_push_default: ## Tag image as latest and Push to DockerHub
	docker tag $(DOCKER_IMAGE):standalone-$(ARCH)-$(DOCKER_TAG) $(DOCKER_IMAGE):$(ARCH)-$(DOCKER_TAG)
	docker tag $(DOCKER_IMAGE):standalone-$(ARCH)-$(DOCKER_TAG) $(DOCKER_IMAGE):$(ARCH)-latest
	docker push $(DOCKER_IMAGE):$(ARCH)-$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):$(ARCH)-latest

docker_push: docker_push_base docker_push_cassandra docker_push_query docker_push_collector docker_push_standalone docker_push_default ## Tag image as latest and Push to DockerHub

output:
	@echo Docker Image: $(DOCKER_IMAGE):$(DOCKER_TAG)

# Absolutely awesome: http://marmelab.com/blog/2016/02/29/auto-documented-makefile.html
help:
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: help

## See https://github.com/microscaling/microscaling/blob/master/Makefile
