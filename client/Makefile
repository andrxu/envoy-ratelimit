SOURCE := ./...
RL-CLIENT := rl-client
.DEFAULT_GOAL := build

GO=GOPRIVATE=github.com/andrxu/* go

build:
	$(GO) build -v -o ./bin/$(RL-CLIENT)
.PHONY: build

test: build
	./bin/$(RL-CLIENT) --domain="rl" --descriptors="foo=f1" 
.PHONY: test