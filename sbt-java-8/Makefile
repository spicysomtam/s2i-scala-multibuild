
IMAGE_NAME = spicysomtam/s2i-scala-multibuild
TAG = latest

build:
	docker build -t $(IMAGE_NAME):$(TAG) .

.PHONY: test
test:
	docker build -t $(IMAGE_NAME)-candidate:$(TAG).
	IMAGE_NAME=$(IMAGE_NAME)-candidate:$(TAG) test/run
