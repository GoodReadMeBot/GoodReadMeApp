# GoodReadMe
The app update version of library in your ReadMe file.

## How it's work
App receive event about new release -> App fork your repo -> App create pull request with change in your ReadMe file.

## Integrate with your project
### Use our [github action](https://github.com/GoodReadMe/GoodReadMeAction) (Recommend)

### By GitHub WebHook (Recommended)
**For the self host usage add query `client_secret:<your client secret>`**
Go to Repository Setting -> WebHooks -> Add webhook 
 - Payload URL: `http://goodreadme.androidstory.dev:8080/checkMe/byReleaseWebHook`
 - Content type: `application/json`
 - Which events would you like to trigger this webhook?: Let me select individual events and check Release. 
 
### Manually
**For the self host usage add header `X-CLIENT-SECRET:<your client secret>` or `client_secret:<your client secret>`** 
Call server manually
```http request
POST http://goodreadme.androidstory.dev:8080/checkMe/byRepoDetails
Content-Type: application/json

{
  "owner": "<Owner name>",
  "repo": "<Repo name>"
}
```
or
```http request
POST http://goodreadme.androidstory.dev:8080/checkMe/byRepoFullName
Content-Type: application/json

{
  "fullName": "<Owner name>/<Repo name>"
}
```

## Setup app for self host usage.
**For the self host add `CLIENT_SECRET` to environment variable** 
### Easy run (DockerHub)
```shell script
docker pull vovochkastelmashchuk/good-readme:1.0
docker run -p 8080:8080 --env GITHUB_TOKEN=<Github token> -d --rm vovochkastelmashchuk/good-readme:1.0
```

### From source code with docker
Change github.token in [application.conf](resources/application.conf)
1. Build jar file
```shell script
./gradlew shadowJar 
```
2. Build docker image
```shell script
docker build --tag good-readme .
```
3. Run docker image
```shell script
docker run -p 8080:8080 --env GITHUB_TOKEN=<Github token> -d --rm good-readme
```

### From source code
1. Setup environment variables
Choose one:
 - Change github.token and github.clientsecret in [application.conf](resources/application.conf)
 - Add `GITHUB_TOKEN` to environment variable
Build and run jar file
```shell script
./gradlew shadowJar && java -jar /build/libs/updatereadme.jar 
```
