build centos 7 develop environment

# HowTo

* prepare YOUR public ssh key:

> if you need more ssh public key for login, please add to this 'authorized_keys'

```
cp ~/.ssh/id_rsa.pub ./authorized_keys
```

* run command in directory as follow (build image 'local:centos7_dev'):

```
docker build -t local:centos7_dev .
```

* launch your container:

```
docker run -itd --hostname centos7_dev --name centos7_dev local:centos7_dev
```

> container's name is 'centos7_dev' in example, change as you like