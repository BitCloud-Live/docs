---
title: How to upgrade app docker image
weight: 3
---

## Workflow

+ Requirements
  + Docker installation [follow this instructions](https://docs.docker.com/install/)
  + UV cli installation [latest osx/linux binaries](https://github.com/uvcloud/uv-cli/releases/tag/v2.0.0-alpha.1)


Now go to the app working directory
```shell
$ uv --version
uv version v2.0.0-alpha.1
$ uv login
Username: <username>
Password: ***
Login succesful
$ uv app:list
INFO[0000] services:<updated:<seconds:1537813897 > state:Up name:"<username>" >
$ uv app:info -n <app> # look after Config: image:"xxx:v0.3.2"
INFO[0000] Config: image:"hub.uvcloud.ir/<username>/<app-image>:v0.3.2" minScale:1 port:8080 routes:"<app>.uvapps.io" endpointType:"http"
INFO[0000] Created: seconds:1537793986 ,	 Updated: seconds:1537813897
INFO[0000] VCAP_SERVICES:
{
	"postgresql": [
		{
			"name": "my-pg",
			"label": "postgresql",
			"tags": null,
			"plan": "starter",
			"credentials": {
				"database": "postgres",
				"host": "my-pg",
				"password": "xxx",
				"port": "5432",
				"uri": "postgresql://postgres:xxx@my-pg:5432/postgres",
				"username": "postgres"
			}
		}
	]

INFO[0000] Environment variables: None
$ # now pull app latest version and check tags
$ git pull --rebase
$ git tag
v0.1.2
v0.2.0
v0.3.0
v0.3.1
v0.3.2 # <-- this is same with the current docker image tag, so we need to make a new tag for new docker image
$ git tag v0.3.3 
$ git push --tags
Total 0 (delta 0), reused 0 (delta 0)
To https://gitlab.com/xxx/xxx
 * [new tag]         v0.3.3 -> v0.3.3
$ git tag
v0.1.2
v0.2.0
v0.3.0
v0.3.1
v0.3.2
v0.3.3
```  
Now we make the new docker image  
```shell
$ docker login hub.uvcloud.ir
Username: <username>
Password: ***
Login successful
$ docker built -t hub.uvcloud.ir/<username>/<app-image>:v0.3.3 . # or maybe a make docker-build
REDACTED
$ docker push hub.uvcloud.ir/<username>/<app-image>:v0.3.3 # or maybe a make docker-push
REDACTED
```  
Now we need to upgrade the app image for the uvcloud  
```shell
$ uv app:configSet -n <app> -i hub.uvcloud.ir/<username>/<app-image>:v0.3.3 # look after Config: image:"xxx:v0.3.3"
INFO[0000] Config: image:"hub.uvcloud.ir/<username>/<app-image>:v0.3.3" minScale:1 port:8080 routes:"<app>.uvapps.io" endpointType:"http"
INFO[0000] Created: seconds:1537793986 ,	 Updated: seconds:1537813897
INFO[0000] VCAP_SERVICES:
{
	"postgresql": [
		{
			"name": "my-pg",
			"label": "postgresql",
			"tags": null,
			"plan": "starter",
			"credentials": {
				"database": "postgres",
				"host": "my-pg",
				"password": "xxx",
				"port": "5432",
				"uri": "postgresql://postgres:xxx@my-pg:5432/postgres",
				"username": "postgres"
			}
		}
	]

INFO[0000] Environment variables: None
```
  
Enjoy!
