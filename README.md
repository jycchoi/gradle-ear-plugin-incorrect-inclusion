As having been discussed in:
https://stackoverflow.com/questions/62712979/gradle-ear-plugin-should-not-copy-resources-into-ear-root

Here is the simplified example `build.gradle` file with both War and Ear plugin.
```gradle
plugins {
  id 'war'
  id 'ear'
}

defaultTasks 'clean', 'build'

dependencies {
  deploy files(war)
}
```

### Expected Behavior
Gradle documentation about Ear Plugin says that:

>   The default behavior of the Ear task is to copy the content of src/main/application to the root of the archive.
>   If your application directory doesn’t contain a META-INF/application.xml deployment descriptor then one will be generated for you.

The Ear Plugin should not include any generated resources other than those under `src/main/application`.

### Current Behavior
However, currently any compiled and generated files under `build/classes` and `build/resources` are also copied to the root of the Ear file, resulting in the following double inclusion of the `dummy.txt` file.
```
gradle-ear-plugin-incorrect-inclusion.ear/
+-- META-INF/
|   +-- MANIFEST.MF
|   +-- application.xml
+-- gradle-ear-plugin-incorrect-inclusion.war/
|   +-- META-INF/
|   |   +-- MANIFEST.MF
|   +-- WEB-INF/
|       +-- classes/
|           +-- dummy.txt
+-- dummy.txt
```

### Steps to Reproduce 
1. git clone https://github.com/jycchoi/gradle-ear-plugin-incorrect-inclusion.git
2. Run `gradle`

### Your Environment
Gradle version 7.0
