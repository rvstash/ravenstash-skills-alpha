# Maven workflows

Load for Maven, Java, JVM, Kotlin, Scala, coordinates, install, verify, or deploy
work.

## Read and build

```bash
rvs mvn --rvs-target platform/packages verify
rvs mvn --rvs-target mirror:maven-central test
rvs art maven install com.example:lib:1.0.0 --target platform/packages
```

Private mirrors support reads only. Maven remains responsible for the project
model, dependency graph, lifecycle, and build output.

## Deploy

Deploy only to an exact private Maven repository:

```bash
rvs art maven publish ./target/lib.jar \
  --group-id com.example \
  --artifact-id lib \
  --version 1.0.0 \
  --target platform/releases
```

Confirm the artifact checksum or build provenance available to the user, Maven
coordinates, account, repository, and lane before creating remote state.

## Settings

`rvs art endpoint --format maven` prints routing; `rvs art native config mvn`
prints settings and commands without minting credentials or writing files. Persistent
settings changes require explicit intent. Do not write a Ravenstash token into
`settings.xml` or include it in build logs.
