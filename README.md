# Vocabularies

[![Gitpod](https://img.shields.io/badge/gitpod-open-blue?logo=gitpod)](https://gitpod.io/#https://github.com/opencaesar/vocabularies) 
[![Build Status](https://travis-ci.org/opencaesar/vocabularies.svg?branch=master)](https://travis-ci.org/opencaesar/vocabularies)
[ ![Download](https://api.bintray.com/packages/opencaesar/vocabularies/vocabularies/images/download.svg) ](https://bintray.com/opencaesar/vocabularies/vocabularies/_latestVersion)

This is a set of vocabulary ontologies from various authorities expressed in [OML](https://github.com/opencaesar/oml)

## Clone
```
  git clone https://github.com/opencaesar/vocabularies.git
  cd vocabularies
```

## Build
Equivalent to owlReason task
```
./gradlew build
```

## Run Reasoner
```
./gradlew owlReason
```

## Generate Docs
You must first have Bikeshed (the app itself) installed from [here](https://tabatkins.github.io/bikeshed/#install-final)
```
./gradlew generateDocs
```

## Publish to Maven Local
```
./gradlew publishToMavenLocal
```
