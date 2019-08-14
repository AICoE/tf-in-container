# tf-in-container
# Copyright(C) 2019 Subin Modeel
#
# This program is free software: you can redistribute it and / or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program. If not, see <http://www.gnu.org/licenses/>.
#
# Usage :
#	docker build --build-arg "TF_URL=${TF_URL}" -t aicoe/tf-in-centos7:1.14 -f Dockerfile .
#   all releases of tensorflow wheel are here https://github.com/AICoE/tensorflow-wheels/releases

FROM centos:7

ENV SUMMARY="plain TensorFlow container image"	\
    DESCRIPTION="This is a plain container image with TensorFlow in it." \
    NAME=aicoe-tensorflow \
    VERSION=1.14 \
	  RELEASE=1 \
    ARCH=x86_64 \
	  TENSORFLOW_VERSION=1.14.1 \
	  TENSORFLOW_PYTHON_VERSION=3.6

LABEL summary="$SUMMARY" \
      description="$DESCRIPTION" \
      com.redhat.component="$NAME" \
      name="$NAME" \
      version="$VERSION" \
	    release="$RELEASE" \
	    architecture="$ARCH" \
      maintainer="AICoE at Red Hat's Office of the CTO <goern+aicoe@redhat.com>"

USER root

ENV TEST_CMD="import tensorflow as tf ; a = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[2, 3], name='a') ; \
	b = tf.constant([1.0, 2.0, 3.0, 4.0, 5.0, 6.0], shape=[3, 2], name='b') ; c = tf.matmul(a, b) ; \
	sess = tf.Session(config=tf.ConfigProto(log_device_placement=True)) ;print(sess.run(c))"

ARG TF_URL=https://github.com/AICoE/tensorflow-wheels/releases/download/tf-r1.14-cpu-2019-08-01_205450/tensorflow-1.14.1-cp36-cp36m-linux_x86_64.whl
ARG TF_SERVING_URL=https://github.com/AICoE/tensorflow-wheels/releases/download/tensorflow_serving_api-r1.14-cpu-2019-08-14_132212/tensorflow_serving_api-1.14.0-py2.py3-none-any.whl
ARG TF_MODEL_SERVER_RPM=https://github.com/AICoE/tensorflow-wheels/releases/download/tensorflow_serving_api-r1.14-cpu-2019-08-14_132212/tensorflow-model-serving-r1.14-1.0-1.x86_64.rpm

RUN INSTALL_PKGS="wget vim unzip which passwd shadow-utils krb5-libs vte-profile findutils file procps gcc scl-utils rh-python36 rh-python36-python-pip rh-python36-python-devel" && \
    yum install -y centos-release-scl-rh && \
    yum -y --setopt=tsflags=nodocs install $INSTALL_PKGS && \
    curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py" && \
	  wget $TF_URL $TF_SERVING_URL $TF_MODEL_SERVER_RPM && \
    yum install -y tensorflow-model-*

RUN source scl_source enable rh-python36 && \
    python -V && \
    which pip && \
    pip -V && \
    pip3.6 install tensorflow-*.whl tensorflow_serving_api*.whl && \
	  python3 -c "$TEST_CMD" && \
	  rm -fr tensorflow*.whl && \
	  yum clean all -y

CMD ["/bin/bash"]
