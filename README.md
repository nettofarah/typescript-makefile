# typescript-makefile
🛠 Ready to use basic makefile with common typescript tasks

## Useful resources and quick tips
- https://devhints.io/makefile
- https://gist.github.com/isaacs/62a2d1825d04437c6f08
- https://tech.davis-hansson.com/p/make/
- https://www.gnu.org/software/make/manual/html_node/index.html

### Conditional tasks
```make
mytask:
  ifeq ($(NODE_ENV),production)
    # prod command
  else
    # dev command
  endif
```


## The makefile

```make
# For some quick reference, check out this URL:
# Cheat Sheet: https://devhints.io/makefile

# *Helpers*
# Common variables that can be useful in different scenarios

# Useful when invoking binaries from your `node_modules` folder
BIN := ./node_modules/.bin
# NODE_ENV env variable that you can use in your scripts
NODE_ENV ?= development
# Latest git sha, useful for tagging resources
GIT_SHA := $(shell git rev-parse --short=7 HEAD)

# Sets the default goal to `make help`, so typing `make` returns a well formatted list of tasks
.DEFAULT_GOAL := help

help: ## Lists all available make tasks and some short documentation about them
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'
.PHONY: help

node_modules: package.json yarn.lock ## Installs all dev dependencies
	@echo "🚚 Installing dependencies"
	@yarn install
.PHONY: node_modules

build: node_modules ## Builds all source files
	@echo "🛠  Building source files"
	@$(BIN)/tsc
.PHONY: build

clean: ## Removes all previously built code
	@echo "🧹 Cleaning..."
	@rm -rf dist/
.PHONY: clean

test: ## Runs unit tests
	@echo "✅ Running tests"
	@yarn test
.PHONY: test
```
