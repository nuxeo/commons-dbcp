<!---
 Licensed to the Apache Software Foundation (ASF) under one or more
 contributor license agreements.  See the NOTICE file distributed with
 this work for additional information regarding copyright ownership.
 The ASF licenses this file to You under the Apache License, Version 2.0
 (the "License"); you may not use this file except in compliance with
 the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
-->
commons-dbcp
============

[![Java CI](https://github.com/nuxeo/commons-dbcp/actions/workflows/maven.yml/badge.svg)](https://github.com/nuxeo/commons-dbcp/actions/workflows/maven.yml)

This is a fork of [apache/commons-dbcp](https://github.com/apache/commons-dbcp) which aims to provide a Jakarta compatible version of the library.

Original readme can be found [there](README-orig.md).

You can download this library from our artifactory.

Direct link is https://packages.nuxeo.com/service/rest/repository/browse/maven-public/org/apache/commons/commons-dbcp2/.

Or with Maven:

```xml
<project>
  ...
  <dependencyManagement>
    <dependencies>
      <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-dbcp2</artifactId>
        <version>VERSION</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
  
  ...
  
  <repositories>
    <repository>
      <id>nuxeo-public</id>
      <url>https://packages.nuxeo.com/repository/maven-public/</url>
      <releases>
        <enabled>true</enabled>
      </releases>
      <snapshots>
        <updatePolicy>always</updatePolicy>
        <enabled>true</enabled>
      </snapshots>
    </repository>
  </repositories>
  ...
</project>
```

## Release the project

Make sure the project builds and its tests pass.

Then create a temporary branch to perform the release:

```bash
git checkout -b tmp-release
```

Then update the project version to final, for instance 2.12.1-NX01:

```bash
mvn versions:set -DnewVersion=2.12.1-NX01 -DgenerateBackupPoms=false
```

Then commit and tag the release:

```bash
git commit -a -m "Release 2.12.1-NX01"
git tag -a -m "Release 2.12.1-NX01" commons-dbcp-2.12.1-NX01
```

Then deploy the maven artefacts:

```bash
mvn clean source:jar deploy -DskipTests -DaltDeploymentRepository=maven-vendor::default::VENDOR_URL
```

> [!IMPORTANT]
> You should replace the `VENDOR_URL`.
> Your Maven `settings.xml` file should contain appropriate authentication (if any) for the `maven-vendor` repository.

Then push the tag:

```bash
git push --tags
```

Then cleanup your branch and prepare the next development iteration:

```bash
git checkout main
git branch -D tmp-release
mvn versions:set -DnewVersion=2.12.1-NX02-SNAPSHOT -DgenerateBackupPoms=false
git commit -a -m "Post release 2.12.1-NX01"
git push
```
