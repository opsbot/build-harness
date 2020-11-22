GOA:= $(shell which goa 2>/dev/null)
GOA_MODULE ?= $(shell basename $(shell pwd))
GOA_VERSION ?= v3

## build goadesign project
goadesign/build: $(GO)
	for d in ./cmd/*/; do go build $$d; done
_PHONY: goadesign/build

## generate implementation stubs for your project
goadesign/example: $(GOA)
	goa example $(GOA_MODULE)/design
_PHONY: goadesign/example

## generate boilerplace code for your design
goadesign/generate: $(GOA)
	goa gen $(GOA_MODULE)/design
_PHONY: goadesign/generate

## install goadesign into your project
goadesign/install: $(GO)
	go mod init $(GOA_MODULE)
	go get -u goa.design/goa/$(GOA_VERSION)
	go get -u goa.design/goa/$(GOA_VERSION)/...
_PHONY: goadesign/install

## regenerate implementation stubs for your project
goadesign/regenerate:
	@rm -f *.go
	@rm -rf cmd
	@rm -rf gen
	@make goadesign/generate
	@make goadesign/example
_PHONY: goadesign/regenerate
