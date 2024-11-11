# Core Vocabularies

[![Build Status](https://github.com/opencaesar/core-vocabularies/actions/workflows/ci.yml/badge.svg)](https://github.com/opencaesar/core-vocabularies/actions/workflows/ci.yml)
[![Release](https://img.shields.io/github/v/release/opencaesar/core-vocabularies?label=Release)](https://github.com/opencaesar/core-vocabularies/releases/latest)
[![Documentation](https://img.shields.io/badge/Documentation-HTML-orange)](https://www.opencaesar.io/core-vocabularies/) 

A core set of building block vocabularies from various authorities expressed in [OML](https://github.com/opencaesar/oml)

## Clone
```
  git clone https://github.com/opencaesar/core-vocabularies.git
  cd core-vocabularies
```

## Build
Convert to owl
```
./gradlew build
```

## Generate Docs
```
./gradlew generateDocs
```
P.S.: You must first have Bikeshed (the app itself) installed from [here](https://tabatkins.github.io/bikeshed/#install-final)

## Publish to Maven Local
```
./gradlew publishToMavenLocal
```
