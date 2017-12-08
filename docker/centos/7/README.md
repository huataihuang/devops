build centos 7 from docker.io:centos:7, and add:

* update system
* add ssh server
* add $HOME dir's ssh pub key for easy login

# HowTo

* prepare YOUR public ssh key:

> if you need more ssh public key for login, please add to this 'authorized_keys'

```
cp ~/.ssh/id_rsa.pub ./authorized_keys
```

* run command in directory as follow (build image 'localhost:centos7'):

```
docker build -t local:centos7 .
```

> the image is named 'local:centos7', you can use other name as you like

* launch your container:

```
docker run -itd --hostname dev7 --name dev7 local:centos7
```

> container's name is 'dev' in example, change as you like

