## Pravega options

Pravega has many configuration options for setting up metrics, tuning, etc. The available options can be found [here](https://github.com/pravega/pravega/blob/master/config/config.properties) and are expressed through the pravega/options part of the resource specification.

All values must be expressed as Strings.

```
...
spec:
  pravega:
    options:
      metrics.statistics.enable: "true"
      metrics.statsD.connect.host: "telegraph.default"
      metrics.statsD.connect.port: "8125"
...
```
### Pravega JVM Options

It is also possible to tune the JVM options for Pravega Controller and Segmentstore. Pravega JVM options are for configuring Controller&Segmenstore JVM process whereas Pravega options are for configuring Pravega software.

Here is an example,
```
...
spec:
  pravega:
    controllerJvmOptions: ["-XX:MaxDirectMemorySize=1g"]
    segmentStoreJVMOptions: ["-XX:MaxDirectMemorySize=1g"]
...
```
There are a bunch of default options in the Pravega operator code that is good for general deployment,  It is possible to override those default values by just passing the customized options. For example, the default option `"-XX:MaxDirectMemorySize=1g"` can be override by passing `"-XX:MaxDirectMemorySize=2g"` to
the Pravega operator. The operator will detect `MaxDirectMemorySize` and override its default value if it exists.

Default Controller JVM Options
```
"-Xms512m",
"-XX:+ExitOnOutOfMemoryError",
"-XX:+CrashOnOutOfMemoryError",
"-XX:+HeapDumpOnOutOfMemoryError",
"-XX:HeapDumpPath=" + heapDumpDir,
```
if Pravega version is greater or equal 0.4, then the followings are also added to the default Controller JVM Options
```
"-XX:+UnlockExperimentalVMOptions",
"-XX:+UseContainerSupport",
"-XX:MaxRAMPercentage=50.0"
```

Default Segmenstore JVM Options
```
"-Xms1g",
"-XX:+ExitOnOutOfMemoryError",
"-XX:+CrashOnOutOfMemoryError",
"-XX:+HeapDumpOnOutOfMemoryError",
"-XX:HeapDumpPath=" + heapDumpDir,
```
if Pravega version is greater or equal to 0.4, then the followings are also added to the default Segmenstore JVM Options
```
"-XX:+UnlockExperimentalVMOptions",
"-XX:+UseContainerSupport",
"-XX:MaxRAMPercentage=50.0"
```

### SegmentStore Custom Configuration

It is possible to add additional parameters into the SegmentStore container by allowing users to create a custom ConfigMap or a Secret and specifying their name within the Pravega manifest. However, the user needs to ensure that the following keys which are present in SegmentStore ConfigMap which is created by the Pravega Operator should not be a part of the custom ConfigMap.

```
- AUTHORIZATION_ENABLED
- CLUSTER_NAME
- ZK_URL
- JAVA_OPTS
- CONTROLLER_URL
- WAIT_FOR
- K8_EXTERNAL_ACCESS
- log.level
```
