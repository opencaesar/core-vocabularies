# OML Vocabularies

[![Gitpod Ready-to-Code](https://img.shields.io/badge/Gitpod-Ready--to--Code-blue?logo=gitpod)](https://gitpod.io/#https://github.com/opencaesar/vocabularies) 
[![Build Status](https://travis-ci.org/opencaesar/vocabularies.svg?branch=master)](https://travis-ci.org/opencaesar/vocabularies)

This is a set of example vocabulary ontologies from various authorities expressed in [OML](https://github.com/opencaesar/oml)

## Clone
```
  git clone https://github.com/opencaesar/vocabularies.git
  cd vocabularies
```

## Build
Run reasoner, generate docs, and archive sources
```
./gradlew build
```

## Run Reasoner
```
./gradlew owlreason
```

## Generate Docs
You must first have Bikeshed installed from [here](https://tabatkins.github.io/bikeshed/#installing)
```
./gradlew bikeshed2html
```

## Publish Sources to Maven Local
```
./gradlew publishToMavenLocal
```
