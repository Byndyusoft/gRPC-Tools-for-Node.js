# grpc-tools

[![npm version](https://badge.fury.io/js/%40byndyusoft%2Fgrpc-tools.svg)](https://www.npmjs.com/package/@byndyusoft/grpc-tools)
[![npm downloads](https://img.shields.io/npm/dt/@byndyusoft/grpc-tools.svg)](https://www.npmjs.com/package/@byndyusoft/grpc-tools)
[![Build Status](https://travis-ci.org/Byndyusoft/grpc-tools.svg?branch=master)](https://travis-ci.org/Byndyusoft/grpc-tools)
[![Build status](https://ci.appveyor.com/api/projects/status/github/Byndyusoft/grpc-tools?branch=master&svg=true)](https://ci.appveyor.com/project/Byndyusoft/grpc-tools/branch/master)

A set of tools to simplify working with protobuf.js, protoc and its plugins.

**Why:** generating code from proto files is a rather tricky thing because various utilities (protoc itself, pbjs, etc) have a lot of various options and it is also necessary to set proper include and plugins paths. Moreover some well known proto files, such as datetime.proto, openapiv2.proto and others are the part of other repositories and packages, so in order to use them, you have to copy them to your repository every time. As a result, you ought to perform a lot of routine every time.

**What:** grpc-tools was created to simplify code generation and provide default include paths with well known proto files.

- [Install](#install)
- [Usage](#usage)
  - [js-protoc-gen](#js-protoc-gen)
  - [swagger-protoc-gen](#swagger-protoc-gen)
- [Contributing](#contributing)
  - [Prerequisites](#prerequisites)
  - [Code conventions](#code-conventions)
  - [General folders layout](#general-folders-layout)
  - [Package development lifecycle](#package-development-lifecycle)
  - [Unit testing](#unit-testing)
- [Maintainers](#maintainers)

# Install

`npm i @byndyusoft/grpc-tools`

# Usage

We consider that a code generation is a part of build process, which can be started locally by developer and remotely by CI/CD. So there is no need to store artifacts of the proto files building in a repos since our package is helping easily automate this process. We recommend the following usage scenario:

- install package to the `devDependencies` section of `package.json`
- add `build` script in the `scripts` section of `package.json`, which describe code generation process using the commands provided by our package
- execute code generation by running `npm run build`

Short description of the commands provided by our package are listed below.

## js-protoc-gen

Generates js files from proto files via `pbjs` from [protobuf.js](https://github.com/protobufjs/protobuf.js) providing default include path. Code generation can be started by command

```shell
$ js-protoc-gen -r greeter -t static-module -w commonjs --no-verify -o ./tests/generated/server/greeter_pb.js ./tests/protos/greeter.proto
```

## swagger-protoc-gen

Generates swagger json from proto files via `protoc-gen-swagger` from [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway) providing default include path. Code generation can be started by command

```shell
$ swagger-protoc-gen --swagger_opt=json_names_for_fields=true --swagger_out=logtostderr=true:./tests/generated/server/ -I ./tests/protos/ greeter.proto
```

# Contributing

To contribute, you will need to setup your local environment, see [prerequisites](#prerequisites). For the contribution and workflow guide, see [package development lifecycle](#package-development-lifecycle).

A detailed overview on how to contribute can be found in the [contributing guide](CONTRIBUTING.md).

## Prerequisites

Make sure you have installed all of the following prerequisites on your development machine:

- Git - [Download & Install Git](https://git-scm.com/downloads). OSX and Linux machines typically have this already installed.
- Node.js (version 10 or higher) - [Download & Install Node.js](https://nodejs.org/en/download/) and the npm package manager.

## Code conventions

For Node.js code formatting is suggested to use [prettier](https://github.com/prettier/prettier) formatter ([Visual Studio Code extension for Prettier](https://github.com/prettier/prettier-vscode)). Shared prettier settings are saved at [.prettierrc file](/.prettierrc)

## General folders layout

### .vscode

- configs for Visual Studio Code.

### build

- scripts for the CI build process

### deps

- repository submodules

### src

- `src/bin` - tools entry points.
- `src/include` - well known proto files, copied from other repositories, which are generated by command `npm run build:copy`. Folder is created automatically during the build process and was excluded from repository.
- `*` - implementation of the service.

### tests

- tests for the result of code generation.

## Package development lifecycle

- Declare new tool in the `bin` section `package.json`
- Implement tool entry point in `src/bin`
- Implement tool logic in `src`
- Add or addapt unit-tests (prefer before and simultaneously with coding)
- Add or change the documentation as needed
- Open pull request in the correct branch. Target the project's `master` branch

## Unit testing

### Files layout

- `src/protos` - files on which code generation can be tested.
- `tests/generated` — artifacts of the proto files compilation, which are generated by command `npm run build`. Folder is created automatically during the build process and was excluded from repository.
- `tests/\*.test.js` — unit test cases

### Running tests

Tests can be run using the following commands.

```shell
$ npm i
$ npm link
$ npm run build
$ npm t
```

# Maintainers

@Byndyusoft/owners: https://github.com/orgs/Byndyusoft/teams/owners, github.maintain@byndyusoft.com
