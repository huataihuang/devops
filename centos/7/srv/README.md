build centos 7 server to run service

# HowTo

* prepare YOUR public ssh key:

> if you need more ssh public key for login, please add to this 'authorized_keys'

```
cp ~/.ssh/id_rsa.pub ./authorized_keys
```

* run command in directory as follow (build image 'local:centos7_server'):

```
docker build -t local:centos7_server .
```

* launch your container:

```
docker run -itd --hostname centos7_srv --name centos7_srv local:centos7_server
```

> container's name is 'centos7_srv' in example, change as you like

