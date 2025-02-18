# Logstash Adabas Auditing Filter Plugin

This is a Java plugin for [Logstash](https://github.com/elastic/logstash).

## Build
The build of this plugin requires the access to an installation of Logstash.

1. Download Logstash from https://www.elastic.co/downloads/logstash
2. Copy the files **rubyUtils.gradle** and **versions.yml** from Github repository https://www.elastic.co/downloads/logstash to directory where you installed Logstash
3. Clone the [guardium-universalconnector-commons project](https://github.com/IBM/guardium-universalconnector-commons) from GitHub to get helper classes for creating a Guardium record.
4. Build the Guardium jars following the instructions of the README.md. (Java 11 was required to build the jars) 
5. Clone this repository
6. Set the property variable **LOGSTASH_CORE_PATH**. This could be done in gradle.properties file
6. Set the property variable **GUARDIUM_UNIVERSALCONNECTOR_COMMONS_PATH** to the directory of jars from step 4. This could be done in gradle.properties file
7. Assemble plugin with the command `./gradlew assemble gem`

After that successful build a file **logstash-input-adabas_auditing_filter-<version>-java.gem** is created in the root directory of the project.

See also [Developing new plug-ins for Guardium Data Protection](https://github.com/IBM/universal-connectors/blob/main/docs/Guardium%20Data%20Protection/developing_plugins_gdp.md).

## Install Plugin
To install the plugin use the command 
```
logstash install --no-verify --local <full-path>/logstash-input-adabas_auditing_filter-<version>-java.gem
```

## Run Logstash
Execute the command `logstash -f <file>` where `<file>`is your Logstash configuration file. An example is below.

## Plugin Configuration Example
This configuration reads the data from the Adabas Auditing Server and write the data to `stdout`.

```
input {
  adabas_auditing_input { 
    brokerClass => "REPTOR" 
    brokerServer => "GERATA" 
    brokerService => "GER-ATA-OUT7" 
    host => "10.20.74.81" 
    port => 19999 
    token => "GERTOKEN7" 
    user => "GER7" 
  }
}
filter {
  adabas_auditing_filter {
  }
}
output {
  stdout { 
    codec => rubydebug
  }
}
```