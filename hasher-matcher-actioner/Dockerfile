# Copyright (c) Facebook, Inc. and its affiliates. All Rights Reserved

FROM public.ecr.aws/lambda/python:3.8@sha256:6fd9eea0292e900c7b8f4186c9e4fbcdb130118b8915ea8243394a7e620e5519 as build
RUN yum install -y gcc gcc-c++

# Install dependencies as a separate step so layers are reuseable.
# (the [y] is so the command does not fail if settings.py is not found.)
COPY setup.py settings.p[y] ./
RUN pip install -e .

# LAMBDA_TASK_ROOT is recommended in the image [1]. I'm hoping this will prevent
# the python path complications from arising.
# 1: https://hub.docker.com/r/amazon/aws-lambda-python
COPY hmalib ${LAMBDA_TASK_ROOT}/hmalib
COPY hmalib_extensions ${LAMBDA_TASK_ROOT}/hmalib_extensions

# Testing local changes to python-threatexchange: 
# 1) Run: cp -r ../python-threatexchange/threatexchange threatexchange 
# 2) comment out line `"threatexchange[faiss,pdq_hasher]>=0.X.XX",` from setup.py's install_requires
# 3) uncomment bottom line
# 4) rebuild and push new docker image (e.g. make upload_docker)
# 5) redploy hma using image created  (e.g. make dev_apply_with_newest_docker_image)
# COPY threatexchange ${LAMBDA_TASK_ROOT}/threatexchange
