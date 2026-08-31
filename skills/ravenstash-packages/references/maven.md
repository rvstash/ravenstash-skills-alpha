# Maven workflows

Load for Maven, Java, JVM, Kotlin, Scala, coordinates, install, verify, or deploy
work.

## Read and build

```bash
rvs mvn --rvs-target platform/packages verify
rvs mvn --rvs-target mirror:maven-central test
rvs pkg maven install com.example:lib:1.0.0 --repo platform/packages
```

Private mirrors support reads only. Maven remains responsible for the project
model, dependency graph, lifecycle, and build output.

## Deploy

Deploy only to an exact private Maven repository:

```bash
rvs pkg maven deploy ./target/lib.jar \
  --group com.example \
  --artifact lib \
  --version 1.0.0 \
  --repo platform/releases
```

Confirm the artifact checksum or build provenance available to the user, Maven
coordinates, account, repository, and lane before creating remote state.

## Settings

`rvs pkg maven repo-url` and `settings` can render routing material. Persistent
settings changes require explicit intent. Do not write a Ravenstash token into
`settings.xml` or include it in build logs.
