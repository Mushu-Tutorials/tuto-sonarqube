# SonarQube

Code coverage with SonarQube ([documentation](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/)).

*Sonar-Scanner installation inspired from [this](https://techexpert.tips/fr/sonarqube/installation-de-scanner-sonarqube-sur-ubuntu-linux/) tutorial.*

## Requires

The target project where you want to run SonarQube must have **xDebug**. For Docker projects to enable xDebug, run the command:

```dockerfile
RUN apk --update --no-cache add git bash --no-cache $PHPIZE_DEPS \
    && pecl install xdebug-3.0.3\
    && docker-php-ext-enable xdebug
```

## Run SonarQube

```shell
docker-compose up -d
```

## Install Sonar-Scanner

```shell
# Update this packages
sudo apt install unzip wget nodejs

cd
mkdir ./sonarqube -p
cd sonarqube

# Get sonar-scanner and unzip the folder
wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.2.0.1873-linux.zip
unzip sonar-scanner-cli-4.2.0.1873-linux.zip
sudo mv sonar-scanner-4.2.0.1873-linux /opt/sonar-scanner
```

Update the sonar-scanner.properties files to connect sonar-scanner to your SonarQube server:

```shell
# Update the sonar-scanner.properties files to connect sonar-scanner to your SonarQube server
nano /opt/sonar-scanner/conf/sonar-scanner.properties
```

Uncomment these lines:

```shell
sonar.host.url=http://localhost:9000
sonar.sourceEncoding=UTF-8
```

Add SonarQube to the PATH:

```shell
# Add SonarQube to the PATH
sudo nano /etc/profile.d/sonar-scanner.sh
```

Add those lines to the file:

```shell
#/bin/bash
export PATH="$PATH:/opt/sonar-scanner/bin"
```

Restart ans check version:

```shell
# Add sonar-scanner to the PATH with source command
source /etc/profile.d/sonar-scanner.sh

# Check if the PATH as been modified
env | grep PATH

# Check sonar-scanner version
sonar-scanner -v
```

## Create new projets

- Manually
- Name the project
- Generate the token
- Other > Linux

Récupérer la commande et le lancer à la racine du projet :

```shell
cd symfony
sonar-scanner \
  -Dsonar.projectKey=cabinet \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=3544e54fc63ca0be6ba70fe959dfcf4c92b0dc56
```
