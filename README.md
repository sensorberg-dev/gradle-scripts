# Sensorberg Gradle Scripts

I've got tired of always copy-paste the same stuff on gradle that this is here so I can freely call this:
```groovy
apply from: 'https://raw.githubusercontent.com/sensorberg-dev/sensorberg-gradle-scripts/master/somes-script.gradle'
```
... and not to worry about anything else.

There's stuff copied from others, tweaked from others, coded from me.
It's just that one place where I can find all the ones I want, tweaked the way I want.

I'll try to write as minimum amount of readme as possible, so scripts names are as verbose as possible

## findProperty and findQuotedProperty

There are 2 findProperty functions defined on `common-def.gradle`:
- `findProperty(String key, String defaultValue)`
- `findQuotedProperty(String key, String defaultValue)` // default is not quoted

This function tries to find those variables in the following places in the following order:

- System.getenv() // good to inject via CI
- local.properties // good to avoid username/pass to be uploaded to repo
- gradle.properties // in which gradle tries to find in module, root and global properties

## username and password
All username and password uses the `findProperty(String, "FOO")` function.
Real friends never add username and password to the repo

## Gradle properties
Some of those scripts relies on gradle properties parameters

### publish-private-maven-repo.gradle
project gradle.properties
```
MAVEN_URL=http://www.your_maven.com/instance
MAVEN_URL_SNAPSHOT=http://www.your_maven.com/snapshot_instance
MAVEN_USERNAME=user_name_on_maven_instance
MAVEN_PASSWORD=123456789

POM_GROUP_ID=com.company.omg
POM_VERSION=0.0.1
POM_PACKAGING=aar
```
module gradle.properties
```
POM_ARTIFACT_ID=my-super-lib
```

### add_private_maven_repo_to_project.gradle
```
MAVEN_URL=http://www.your_maven.com/instance
MAVEN_URL_SNAPSHOT=http://www.your_maven.com/snapshot_instance
MAVEN_USERNAME=user_name_on_maven_instance
MAVEN_PASSWORD=123456789
```
same as publish-private-maven-repo.gradle

### publish-to-local-maven-repo.gradle
```
POM_GROUP_ID=com.company.omg
POM_VERSION=0.0.1
POM_PACKAGING=aar
POM_ARTIFACT_ID=my-super-lib
```

### find-version-name.gradle
default if there's no git tag and not `POM_VERSION` is `"0.0.0-ALPHA-${gitSha1()}"`
```
POM_VERSION=0.0.1   # only used if can't find git tag
IS_RELEASE=true     # or false, or just comment out the line
```

### publish-bitbucket-private-repo.gradle
project gradle.properties
```
ARTIFACT_VERSION=0.0.1
ARTIFACT_PACKAGE=com.company.omg
ARTIFACT_PACKAGING=aar
COMPANY=company_name_as_registered_on_bitbucket
REPOSITORY_NAME=name_of_repo_on_bitbucket
BITBUCKET_USERNAME=user_name_on_bitbucket
BITBUCKET_PASSWORD=123456789
```
module gradle.properties
```
ARTIFACT_NAME=my-super-lib
```

### add_bitbucket_private_repo_to_project.gradle
```
BITBUCKET_USERNAME=user_name_on_bitbucket
BITBUCKET_PASSWORD=123456789
```
