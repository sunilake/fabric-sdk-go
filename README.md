# Hyperledger Fabric Client SDK for Go

[![Build Status](https://jenkins.hyperledger.org/buildStatus/icon?job=fabric-sdk-go-tests-merge-x86_64)](https://jenkins.hyperledger.org/job/fabric-sdk-go-tests-merge-x86_64)
[![Go Report Card](https://goreportcard.com/badge/github.com/hyperledger/fabric-sdk-go)](https://goreportcard.com/report/github.com/hyperledger/fabric-sdk-go)
[![GoDoc](https://godoc.org/github.com/hyperledger/fabric-sdk-go?status.svg)](https://godoc.org/github.com/hyperledger/fabric-sdk-go)

This SDK enables Go developers to build solutions that interact with [Hyperledger Fabric](http://hyperledger-fabric.readthedocs.io/en/latest/).

## Getting started

Obtain the client SDK packages for Fabric and Fabric CA.

```bash
go get -u github.com/hyperledger/fabric-sdk-go

# Optional - populate vendor directory (if needed by your downstream vendoring solution)
# cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/
# make populate
```

You're good to go, happy coding! Check out the examples for usage demonstrations.

### Examples

- [E2E Test](test/integration/end_to_end_test.go) and [Base Test](test/integration/base_test_setup.go): Part of the E2E tests included with the Go SDK.
- [Dynamic Endorser Selection](test/integration/sdk_provider_test.go): An example that uses dynamic endorser selection (based on chaincode policy) 
- [CLI](https://github.com/securekey/fabric-examples/tree/master/fabric-cli/): An example CLI for Fabric built with the Go SDK.
- More examples needed!

### Community

- Discussion is happening in [Rocket Chat](https://chat.hyperledger.org/channel/fabric-sdk-go).
- Issue tracking is handled in [Jira](https://jira.hyperledger.org/secure/RapidBoard.jspa?projectKey=FAB&rapidView=7&view=planning).
- Active development occurs in the [Gerrit](https://gerrit.hyperledger.org/r/#/admin/projects/fabric-sdk-go) repository.

## Client SDK

### Current Compatibility
The SDK's integration tests run against three tagged Fabric versions:
- prev (currently v1.0.0)
- stable (currently latest of v1.0.x)
- prerelease (currently latest of v1.1.0-x)

Additionally for development purposes integration tests also run against the devstable Fabric version as needed.

### Retired versions
When the 'prev' code level is updated, the last tested fabric-sdk-go commit or tag is listed below.

- fabric v1.0.0 & fabric-ca v1.0.0
  - fabric-sdk-go: master:HEAD

### Running the test suite

```bash
# In the Fabric SDK Go directory
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/

# Optional - Automatically install Go tools used by test suite
# make depend-install

# Running test suite
make

# Clean test suite run artifacts
make clean
```

## Contributing to the Go SDK

If you want to contribute to the Go SDK, please run the test suite and submit patches to the Gerrit git repostory for review. For general guidelines, please refer to the Fabric project's [contribution page](http://hyperledger-fabric.readthedocs.io/en/latest/CONTRIBUTING.html).

You need:

- Go 1.9
- [Dep](https://github.com/golang/dep)
- Make
- Docker
- Docker Compose
- Git

### Gerrit Git repository

To contribute patches, you will need to clone (or add a remote) from [Gerrit](https://gerrit.hyperledger.org/r/#/admin/projects/fabric-sdk-go) with authentication.

### Running a portion of the test suite

```bash
# In the Fabric SDK Go directory
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/

# Ensure dependencies are installed
make depend

# Running code checks (license, linting, spelling, etc)
make checks

# Running all unit tests
make unit-test

# Running all integration tests
make integration-test
```

### Running package unit tests manually

```bash
# In a package directory
go test
```

### Running integration tests manually

You need:

- A working fabric and fabric-ca set up. It is recommended that you use the docker-compose file provided in `test/fixtures`. It is also recommended that you use the default .env settings provided in `test/fixtures`. See steps below.
- Customized settings in the `test/fixtures/config/config_test.yaml` in case your Hyperledger Fabric network is not running on `localhost` or is using different ports.

#### Testing with Fabric Images at Docker Hub

The test suite defaults to the latest compatible tag of fabric images at Docker Hub.
The following commands starts Fabric:

```bash
# In the Fabric SDK Go directory
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/

# Clean previous test run artifacts
make clean

# Start fabric
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/test/fixtures/
docker-compose up --force-recreate
```

#### Running Integration Tests

Fabric should now be running. In a different shell, run integration tests

```bash
# In the Fabric SDK integration tests directory
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/test/integration/
go test
```

#### Using default config

Default SDK Go configurations are found in the code under /pkg/config/config.yaml

To override the default in non Dev environment, set the default path in the following environment variable:

**FABRIC_SDK_CONFIG_PATH**=/path/to/default/config yaml(without specifying the yaml file name)

This path value must be a directory. It must contain a default 'config.yaml' file.
Note that this default config is used only if environment configuration yaml file is missing to ensure all environment variables are created regardless of their values.

#### Testing with Local Build of Fabric (Advanced)

Alternatively you can build and run Fabric on your own box using the following commands:

```bash
# Build fabric:
cd $GOPATH/src/github.com/hyperledger/
git clone https://github.com/hyperledger/fabric
cd $GOPATH/src/github.com/hyperledger/fabric/
git checkout v1.0.4
make docker

# Build fabric-ca:
cd $GOPATH/src/github.com/hyperledger/
git clone https://github.com/hyperledger/fabric-ca
cd $GOPATH/src/github.com/hyperledger/fabric-ca/
git checkout v1.0.4
make docker

# Start fabric - latest-env.sh overrides the default docker tags in .env
cd $GOPATH/src/github.com/hyperledger/fabric-sdk-go/test/fixtures/
(source latest-env.sh && docker-compose up --force-recreate)
```

## License

Hyperledger Fabric SDK Go software is licensed under the [Apache License Version 2.0](LICENSE).

---
This document is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by/4.0/">Creative Commons Attribution 4.0 International License</a>.