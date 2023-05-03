## Build `java-monitoring` client library

### Build with old version of `gapic-generator-java`
`bazelisk build --subcommands --verbose_failures --sandbox_debug --repo_env=LTS=java8  //google/monitoring/v3:google-cloud-monitoring-v3-java`

### Build with new version of `gapic-generator-java`
`bazelisk build --subcommands --verbose_failures --sandbox_debug //google/monitoring/v3:google-cloud-monitoring-v3-java`
