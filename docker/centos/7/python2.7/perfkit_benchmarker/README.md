build [PerfKit Benchmarker](https://github.com/GoogleCloudPlatform/PerfKitBenchmarker)

# HowTo

* prepare YOUR public ssh key:

> if you need more ssh public key for login, please add to this 'authorized_keys'

```
cp ~/.ssh/id_rsa.pub ./authorized_keys
```

* run command in directory as follow (build image 'local:perfkit_benchmarker'):

```
docker build -t local:perfkit_benchmarker .
```

* launch your container:

```
docker run -itd --hostname perfkit_benchmarker --name perfkit_benchmarker \
local:perfkit_benchmarker
