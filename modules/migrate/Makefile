
MIGRATE:= $(shell which migrate 2>/dev/null)

DB_DRIVER ?= postgres
DB_HOST ?= localhost
DB_PORT ?= 5432
DB_USER ?= postgres
DB_PASSWORD ?= password
DB_DATABASE ?= postgres
DB_PARAMETERS ?= ?sslmode=disable
DB_DSN ?= $(DB_DRIVER)://$(DB_USER):$(DB_PASSWORD)@$(DB_HOST):$(DB_PORT)/$(DB_DATABASE)$(DB_PARAMETERS)

# If the first argument is "migrate"...
ifeq (migrate,$(firstword $(MAKECMDGOALS)))
  # use the rest as arguments for "migrate"
  MIGRATE_ARGS := $(wordlist 2,$(words $(MAKECMDGOALS)),$(MAKECMDGOALS))
  # ...and turn them into do-nothing targets
  $(eval $(MIGRATE_ARGS):;@:)
endif

## migrate database up or down
migrate: $(MIGRATE)
	migrate \
		-source file://$(shell pwd)/migrations \
		-database $(DB_DSN) \
		$(MIGRATE_ARGS)
.PHONY: migrate

# If the first argument is "migrate/create"...
ifeq (migrate/create,$(firstword $(MAKECMDGOALS)))
  # use the rest as arguments for "migrate/create"
  MIGRATE_CREATE_ARGS := $(wordlist 2,$(words $(MAKECMDGOALS)),$(MAKECMDGOALS))
  # ...and turn them into do-nothing targets
  $(eval $(MIGRATE_CREATE_ARGS):;@:)
endif

## create a new migration set
migrate/create: $(MIGRATE)
	migrate create -dir ./migrations -ext .sql $(MIGRATE_CREATE_ARGS)
.PHONY: migrate/create

## Install migrate
migrate/install: packages/install/migrate
	@exit 0
.PHONY: migrate/install

## show migrate environment vars
migrate/env: $(MIGRATE)
	@echo DB_DRIVER: $(DB_DRIVER)
	@echo DB_HOST: $(DB_HOST)
	@echo DB_PORT: $(DB_PORT)
	@echo DB_USER: $(DB_USER)
	@echo DB_PASSWORD: $(DB_PASSWORD)
	@echo DB_DATABASE: $(DB_DATABASE)
	@echo DB_PARAMETERS: $(DB_PARAMETERS)
	@echo DB_DSN: $(DB_DSN)
.PHONY: migrate/env