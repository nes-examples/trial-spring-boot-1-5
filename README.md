# NES for Spring Boot 1.5

Welcome to the trial version of NES for Spring Boot 1.5! This is a simple example of how to use NES with Spring Boot 1.5.

**⚠️ Important:** This is a trial version and is not intended for production use. Trial versions have zero CVE fixes included. The purpose of trial versions is to verify integration with HeroDevs repositories. To obtain fully secure, drop in replacements please [contact sales](https://www.herodevs.com/contact).

**🟡 Caution:** This repository demonstrates a quick and simple way to integrate with the HeroDevs NES repository. Usernames and password values are hard coded in Maven and Gradle settings files. When moving to production, take care of your username and password values in a more secure way.

# Pet Clinic Lite

This application stores Pet data in a local H2 database. The information is automatically loaded when the Spring Boot application starts and is exposed via REST endpoints.

This application uses:
- Spring Boot 1.5
- Spring Framework 4.3
- Spring Security 4.2
- Spring Data JPA 1.11

These versions are managed by Spring Boot.

# Build System Choice
This project has two folders, `maven` and `gradle` for the respective build systems. The code (located in `src`) is the same for both build systems. The only difference is the build system itself. The `maven` folder contains a `pom.xml` file and the `gradle` folder contains a `build.gradle` file. You can use either one to build the project.

Notes are below on the specific changes required for each build system. These changes have already been made in the `pom.xml` and `build.gradle` files in the respective folders. You can use these files as a reference for your own project.

___

## Maven

### Changes to `settings.xml`:
- Add the HeroDevs repository URL and credentials:
```xml
<settings>
  <servers>
    <server>
      <id>herodevs-nes-registry</id>
      <username>any_text_here_not_used</username>
      <password>YOUR_NES_ACCESS_TOKEN</password>
    </server>
  </servers>
</settings>
```

### Changes to `pom.xml`:
- Add the HeroDevs repository to the `repositories` and `pluginRepositories` blocks:
```xml
<repositories>
    <repository>
        <id>herodevs-nes-registry</id>
        <url>https://registry.nes.herodevs.com/maven</url>
    </repository>
</repositories>

<pluginRepositories>
    <pluginRepository>
        <id>herodevs-nes-registry</id>
        <url>https://registry.nes.herodevs.com/maven</url>
    </pluginRepository>
</pluginRepositories>
```

- Update the Spring Boot dependency to use the HeroDevs NES for Spring Boot trial version:
```xml
<version>1.5.22-spring-boot-1.5.24-trial</version>
```

### Testing

Verify that the application is using HeroDevs NES for Spring dependencies by running the following command:
```bash
./mvnw dependency:tree
```

You should see dependencies like these in the output to signify that you are using HeroDevs NES for Spring:
```
org.springframework.boot:spring-boot:jar:1.5.22-spring-boot-1.5.24-trial:compile
org.springframework:spring-context:jar:4.3.30-spring-framework-4.3.32-trial:compile
org.springframework.boot:spring-boot-starter-jdbc:jar:1.5.22-spring-boot-1.5.24-trial:compile
```

___

## Gradle

### Changes to `gradle.properties`:

- Add the HeroDevs repository URL and credentials:
```properties
herodevs_nes_registry_url=https://registry.nes.herodevs.com/maven
herodevs_nes_registry_user=any_text_here_not_used
herodevs_nes_registry_token=NES_TOKEN_HERE
```

### Changes to `build.gradle`:

- Add the HeroDevs repository to the `repositories` block:
```groovy
repositories {
  maven {
    url = uri(project.property('herodevs_nes_registry_url'))
    credentials {
      username = project.property('herodevs_nes_registry_user')
    }
  }
  mavenCentral()
}
```

- Add the HeroDevs repository to the `buildscript` block:
```groovy
buildscript {
	repositories {
		maven {
			url = uri(project.property('herodevs_nes_registry_url'))
			credentials {
				username = project.property('herodevs_nes_registry_user')
			}
		}
		mavenCentral()
	}
	dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.5.22-spring-boot-1.5.24-trial")
	}
}
```

- Update the Spring Boot dependency to use the HeroDevs NES for Spring Boot trial version in `buildscript:dependencies`:
```groovy
classpath("org.springframework.boot:spring-boot-gradle-plugin:1.5.22-spring-boot-1.5.24-trial")
```

### Testing

Verify that the application is using HeroDevs NES for Spring dependencies by running the following command:
```bash
./gradlew clean dependencies --configuration runtimeClasspath
```

You should see dependencies like these in the output to signify that you are using HeroDevs NES for Spring:
```
org.springframework.boot:spring-boot-starter-aop:1.5.22-spring-boot-1.5.24-trial
org.springframework:spring-context:4.3.30-spring-framework-4.3.32-trial
org.springframework.boot:spring-boot-starter-jdbc:1.5.22-spring-boot-1.5.24-trial
```

___

# Running the Application

Run the application by executing the corresponding command for your build system.

Maven:
```bash
./mvnw spring-boot:run
```
Gradle:
```bash
./gradlew bootRun
```

When the application is running, verify that the unsecured endpoint is reachable:
```bash
curl http://localhost:8080/helloPets
```
You should see the following output:
```
Woof! Meow! Blub! Tweet!
```

To access the other REST endpoints, you need to authenticate. The application uses Spring Security to secure the endpoints. You can use the following command to pass the simple authentication:
```bash
curl -u user:password "http://localhost:8080/pets?name=Buddy"
```
You should see the following output:
```
[{"name":"Buddy","type":"Dog"}]
```

# Next Steps

Congratulations! Your Spring project is ready to be secure. [Contact HeroDevs](https://www.herodevs.com/contact) for a registry token to get full access. Simply change the `herodevs_nes_registry_token` field in `gradle.properties` or the `password` in `settings.xml` to your specific token and the next build will use NES for Spring dependencies with zero CVEs.