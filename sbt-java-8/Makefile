
IMAGE_NAME = spicysomtam/s2i-scala-multibuild
JAVA_VER = java8

build:
	docker build -t $(IMAGE_NAME):$(JAVA_VER) .

.PHONY: test
test:
	docker build -t $(IMAGE_NAME)-candidate:$(JAVA_VER).
	IMAGE_NAME=$(IMAGE_NAME)-candidate:$(JAVA_VER) test/run
