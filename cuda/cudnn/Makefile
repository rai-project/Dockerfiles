
.PHONY: all

all: build

build:
	rai -p .

buildlocal:
	docker build -t rai/cuda_ppc64le:cudnn-8.0 .
	docker build -t rai/cuda_ppc64le:cudnn51-8.0 -f Dockerfile.cudnn51 .
