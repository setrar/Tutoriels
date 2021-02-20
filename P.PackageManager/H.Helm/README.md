# Helm

https://helm.sh/docs/intro/install/

## :o: Install

#### :one: :penguin: Debian

https://helm.sh/docs/intro/install/#from-apt-debianubuntu

:bulb: Make sure downloading tools are installed

```
$ sudo apt-get install curl apt-transport-https --yes
```

:one: Add the `apt` key :key:

```
$ curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
```

:two: put the repository `drop-in` file in the the appropriate `source` folder

```
$ echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
```

:three: Install the package (along with updating the repos)

```
$ sudo apt-get update && sudo apt-get install helm
```

## :a: Usage

```
$ helm search hub moodle
URL                                               	CHART VERSION	APP VERSION	DESCRIPTION                                       
https://artifacthub.io/packages/helm/bitnami/mo...	11.0.3       	3.10.1     	Moodle(TM) is a learning platform designed to p...
```