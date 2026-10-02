MINICA_VERSION := 1.1.0
IMAGE          := minica:$(MINICA_VERSION)
CERTS_DIR      := $(HOME)/.certs

DOCKER_BUILD := docker build -f Dockerfile.minica -t $(IMAGE) \
	--build-arg MINICA_VERSION=v$(MINICA_VERSION)

EXCLUDES := \
	--exclude=Makefile \
	--exclude=Dockerfile.minica \
	--exclude=build \
	--exclude=minica \
	--exclude=README.md \
	--exclude=.DS_Store

.PHONY: build rebuild minica

build:
	$(DOCKER_BUILD) .

rebuild:
	$(DOCKER_BUILD) --no-cache .

# Usage: make minica ARGS="-domains example.com,*.example.com"
minica:
	@if [ -z "$(ARGS)" ]; then \
		echo 'usage: make minica ARGS="-domains example.com,*.example.com"'; \
		exit 1; \
	fi
	mkdir -p $(CERTS_DIR)
	docker run --rm -v "$(CURDIR):/usr/src/app" $(IMAGE) $(ARGS)
	rsync -av $(EXCLUDES) . $(CERTS_DIR)
	rm -f minica.pem minica-key.pem
	find . -type d -not -path . -exec rm -rf {} + 2>/dev/null || true
