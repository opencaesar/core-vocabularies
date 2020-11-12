# Core Vocabularies

[![Gitpod](https://img.shields.io/badge/gitpod-open-blue?logo=gitpod)](https://gitpod.io/#https://github.com/opencaesar/core-vocabularies) 
[![Build Status](https://travis-ci.org/opencaesar/core-vocabularies.svg?branch=master)](https://travis-ci.org/opencaesar/core-vocabularies)
[ ![Download](https://api.bintray.com/packages/opencaesar/ontologies/core-vocabularies/images/download.svg) ](https://bintray.com/opencaesar/ontologies/core-vocabularies/_latestVersion)
[![Documentation](https://img.shields.io/badge/Documentation-HTML-orange)](https://opencaesar.github.io/core-vocabularies/) 

This is a set of vocabulary ontologies from various authorities expressed in [OML](https://github.com/opencaesar/oml)

## Clone
```
  git clone https://github.com/opencaesar/core-vocabularies.git
  cd core-vocabularies
```

## Build
Equivalent to omlToOwl task
```
./gradlew build
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

## [Metrology vocabulary](doc/README.metrology.md)
