**This repository has been archived, as we dont maintain it's content anymore, there is no new home for the code, feel free to contained any of the contributors!**

# Tensorflow in Containers

[![Docker Repository on Quay](https://quay.io/repository/aicoe/tf-in-container/status "Docker Repository on Quay")](https://quay.io/repository/aicoe/tf-in-container)

## About

This respository has Dockerfiles which have tensorflow wheel files built by Red Hat AICoE team. They can be used for testing and experimenting with tensorflow on following OS :

- `Fedora27`
- `Fedora28`
- `Fedora29`
- `Fedora30`
- `CentOS7`
- `RHEL7.5`
- `RHEL8`

For application development use [S2I Images](https://github.com/sclorg/s2i-python-container) & Tensorflow wheel files from <https://github.com/AICoE/tensorflow-wheels/releases>.<br>
NOTE: for `RHEL7.5` & `RHEL8` you need a system with RHEL Subscription enabled.

## Usage

### Build Image

```shell
docker build --build-arg "TF_URL=${TF_URL}" -t aicoe/tf-in-container:1.13.1-fedora30 -f Dockerfile .
```

TF_URL value should be taken from <https://github.com/AICoE/tensorflow-wheels/releases>.

### Use Image

```shell
docker run -it quay.io/aicoe/tf-in-container:1.13.1-fedora30 bash
```

### Example

```shell
$ docker run -it quay.io/aicoe/tf-in-container:1.13.1-fedora30 bash
[root@94fc148a10de /]# python3 -c "$TEST_CMD"
Device mapping: no known devices.
2019-06-24 15:21:10.309103: I tensorflow/core/common_runtime/direct_session.cc:317] Device mapping:

MatMul: (MatMul): /job:localhost/replica:0/task:0/device:CPU:0
2019-06-24 15:21:10.311608: I tensorflow/core/common_runtime/placer.cc:1059] MatMul: (MatMul)/job:localhost/replica:0/task:0/device:CPU:0
a: (Const): /job:localhost/replica:0/task:0/device:CPU:0
2019-06-24 15:21:10.311692: I tensorflow/core/common_runtime/placer.cc:1059] a: (Const)/job:localhost/replica:0/task:0/device:CPU:0
b: (Const): /job:localhost/replica:0/task:0/device:CPU:0
2019-06-24 15:21:10.311816: I tensorflow/core/common_runtime/placer.cc:1059] b: (Const)/job:localhost/replica:0/task:0/device:CPU:0
[[22\. 28.]
 [49\. 64.]]
[root@94fc148a10de /]#
```
