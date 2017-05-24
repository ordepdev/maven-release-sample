# Usage of maven-release-plugin

## Include the maven-release-plugin
```
<properties>
  <maven-release-plugin.version>2.5.3</maven-release-plugin.version>
</properties>

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-release-plugin</artifactId>
    <version>${maven-release-plugin.version}</version>
    <configuration>
        <autoVersionSubmodules>true</autoVersionSubmodules>
        <pushChanges>false</pushChanges>
        <localCheckout>true</localCheckout>
        <tagNameFormat>v@{project.version}</tagNameFormat>
    </configuration>
</plugin>
```
**Note:** `pushChanges` default value is `true`, always set it to `false` if you don't want any surprises.

## Git flow

`$ git checkout -b release/1.0.0`

```
$ mvn --batch-mode release:prepare release:perform \
       -DreleaseVersion=1.0.0-RELEASE -DdevelopmentVersion=1.0.1-SNAPSHOT`
```

At this point `pom.xml` on the current branch should look like this:
```
<version>1.0.1-SNAPSHOT</version>
```
`$ git checkout development`

`$ git merge -m "Merge release/1.0.1 into development" release/1.0.0`

Development will be a mirror of release/1.0.1 with 1.0.1-SNAPSHOT as the current version.

`$ git checkout master`

`$ git merge -m "Release v1.0.0" release/1.0.0~1`

Master will have one commit less than Development with 1.0.0-RELEASE as the current version.

`$ git checkout development`

`$ git push --all && git push --tags`

And we're done!
