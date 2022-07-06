# Test Project

## Requirements of project

- Changes on the Readme of repositories must be show automatically in a UI in a Kubernetes cluster
- These changes must be shown automatically and the only change that the user can do is in the repository.

## Technology Used

- Kubernetes (to handle the client and server only)
- Angular (Client, 1 pod)
- NodeJS (Server, 3 pods)
- Github
- Dockerhub
- Github Actions
- NGINX
- Python (In repository scripts used in github actions)
- Mongo Atlas (MongoDB as a Service)
- Digital Ocean

## Repositories used
- [Angular UI Repository](https://github.com/andresnboza/reloadAppClient)
- [NodeJS Server Repository](https://github.com/andresnboza/reloadAppServer)
- [Python Scripts Repository](https://github.com/andresnboza/reloadAppScripts)
- [ReloadAppService1](https://github.com/andresnboza/reloadAppService1)
- [ReloadAppService2](https://github.com/andresnboza/reloadAppService2)
- [ReloadAppService3](https://github.com/andresnboza/reloadAppService3)
- [ReloadAppK8s](https://github.com/andresnboza/reloadAppK8s)

## How Everything works

1. Each of these reposiories([ReloadAppService1](https://github.com/andresnboza/reloadAppService1), [ReloadAppService2](https://github.com/andresnboza/reloadAppService2), [ReloadAppService3](https://github.com/andresnboza/reloadAppService3)) has a github action that will execute everytime there is a push on the main branch.

2. The github action will fetch the scripts from the repository [Python Scripts Repository](https://github.com/andresnboza/reloadAppScripts), and take the file named "ReadmeAppService(number).md", where (number) is defined per each of the repositories respectively; for example ReadmeAppService1.md is regarding the repository [ReloadAppService1](https://github.com/andresnboza/reloadAppService1

3. Then it will convert the file into a json and save it, into the mongo database in mongo atlas.

4. Until this point the only thing the user has done is to make changes on the repositories ([ReloadAppService1](https://github.com/andresnboza/reloadAppService1), [ReloadAppService2](https://github.com/andresnboza/reloadAppService2), [ReloadAppService3](https://github.com/andresnboza/reloadAppService3)) and the github action is taking care of the rest.

5. Regarding the repositories ([Angular UI Repository](https://github.com/andresnboza/reloadAppClient), [NodeJS Server Repository](https://github.com/andresnboza/reloadAppServer)) every time there is a change on them, there is also a github action that will update their respective docker image in dockerhub; therefore the kubernetes cluster can fetch the latest docker image.

6. There is also a kubernetes cluster in digital ocean that has 1 pod of the client and 3 pods of the server in the default namespace, it fetch the latest images of these services and has all the deployment and services yaml files in this repository [ReloadAppK8s](https://github.com/andresnboza/reloadAppK8s).

7. To make things easy the repositories have a Makefile in which there are multiple commands to execute quick cluster deployments and services configuration.

8. The way the ui has the latest changes is because it simply makes a request to the back end, which makes a request to the database as a service; therefore once the github actions from the repositories ([ReloadAppService1](https://github.com/andresnboza/reloadAppService1), [ReloadAppService2](https://github.com/andresnboza/reloadAppService2), [ReloadAppService3](https://github.com/andresnboza/reloadAppService3)) are completed, the server will be fetching the latest version of the files in the previously mentioned repositories.

## Why I choose this approach

1. Its simple and with this approach there is no need to update more images than needed into dockerhub.

2. The cluster is not in change constantly because of the changes in the repositories ([ReloadAppService1](https://github.com/andresnboza/reloadAppService1), [ReloadAppService2](https://github.com/andresnboza/reloadAppService2), [ReloadAppService3](https://github.com/andresnboza/reloadAppService3)), and since my experience troubleshooting k8s cluster in operations this is a convenient thing to have.

3. It's a flat solution, which means that I don't have multiple layers in the process of updating a repository therefore a new develioper can work on this and understand the process easily.

4. Databases are contantly in change therefore the save and fetch on them is something they are used to, if the approach will intent to be to update docker images on dockerhub, and somehow look at those changes in the cluster, then it could face issues regarding the amount of times these repositories will change ([ReloadAppService1](https://github.com/andresnboza/reloadAppService1), [ReloadAppService2](https://github.com/andresnboza/reloadAppService2), [ReloadAppService3](https://github.com/andresnboza/reloadAppService3))

## Kubernetes cluster at (ReloadApp)

Please add the following to your hosts app

```bash
146.190.12.141 reloadapp.cr
```

And after that click on the following to access the app:

[reloadapp.cr](http://reloadapp.cr/client/)
