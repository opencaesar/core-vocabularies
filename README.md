# Core Vocabularies

[![Build Status](https://travis-ci.com/opencaesar/core-vocabularies.svg?branch=master)](https://travis-ci.com/opencaesar/core-vocabularies)
[![Release](https://img.shields.io/github/v/tag/opencaesar/core-vocabularies?label=download)](https://github.com/opencaesar/core-vocabularies/releases/latest)
[![Documentation](https://img.shields.io/badge/Documentation-HTML-orange)](https://opencaesar.github.io/core-vocabularies/) 
[![Gitpod](https://img.shields.io/badge/gitpod-open-blue?logo=gitpod)](https://gitpod.io/#https://github.com/opencaesar/core-vocabularies) 

A core set of building block vocabularies from various authorities expressed in [OML](https://github.com/opencaesar/oml)

## Important note about this fork

Until this branch is merged into https://github.com/opencaesar/core-vocabularies, you can use the artifacts in one of two ways:

- Via https://jitpack.io/#NicolasRouquette/core-vocabularies

  See the instructions on that page to add `https://jitpack.io` as a Maven resolver for resolving the dependency: 
  `com.github.NicolasRouquette:core-vocabularies:3.+`

- Locally

  - Clone the branch `rj-format` of this fork: https://github.com/NicolasRouquette/core-vocabularies
  - Assuming that AdoptOpenJdk 11 is installed, execute: `./gradlew clean publishToMavenLocal`

## Clone
```
  git clone https://github.com/opencaesar/core-vocabularies.git
  cd core-vocabularies
```

## Data Products

Publishing products 3 zip archives:

| File | Description |
|------|-------------|
| `core-vocabularies-<version>.oml.zip` | Archive of OML source files |

## Build
Convert to owl
```
./gradlew build
```

Run queries and create Zip archives:
```
./gradlew clean startFuseki
./gradlew qGraphs qOntologies qClasses qObjectProperties qDataProperties omlZip sparqlZip owlZip

## Generate Docs
```
./gradlew generateDocs
```
P.S.: You must first have Bikeshed (the app itself) installed from [here](https://tabatkins.github.io/bikeshed/#install-final)

## Publish to Maven Local
```
./gradlew publishToMavenLocal
```

## Versioning

The gradle script uses the [palantir gradle-git-version plugin](https://github.com/palantir/gradle-git-version) to generate a semantic version based on GIT tag (e.g. 2.4.0) and git commits.
