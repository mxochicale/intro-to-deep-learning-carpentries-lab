# Building customised images 

* Authenticate 
```
docker login ghcr.io
docker pull ghcr.io/ucl-arc-unified-ai/kubeflow-notebook-images-pytorch-default:v1.0.20
```

* Build Dockerfile container
```bash
IMAGENAME=intro2dl-uai
VERSION_ID=v0.1.0
docker build --network=host -t ${IMAGENAME}:${VERSION_ID} -f Dockerfile .
```


* check python version and dependencies
```
docker run --rm \
  -e MASTER_ADDR=localhost \
  -e MASTER_PORT=12355 \
  -e RANK=0 \
  -e WORLD_SIZE=1 \
  "${IMAGENAME}:${VERSION_ID}" \
  bash -c '
    dot -V &&
    python - << "PY"
import sys
print("Python:", sys.version)
print("Executable:", sys.executable)

import torch
print("torch:", torch.__version__)

import tensorflow as tf
print("tensorflow:", tf.__version__)

import tensorboard
print("tensorboard:", tensorboard.__version__)

import seaborn as sns
print("seaborn:", sns.__version__)

import keras_tuner
print("keras_tuner:", keras_tuner.__version__)
PY
  '
```

* iterative mode
```
docker run --rm -it \
  -e MASTER_ADDR=localhost \
  -e MASTER_PORT=12355 \
  -e RANK=0 \
  -e WORLD_SIZE=1 \
  ${IMAGENAME}:${VERSION_ID} \
  bash
```


* Pushing container images
Tag your Docker image using the image ID and your desired image name and hosting destination.
```bash
GITHUB_ORG=YOUR_GITHUB_ORG or YOUR_GITHUB_USERNAME_ID
PROJECT_NAME=carpentries
docker tag ${IMAGENAME}:${VERSION_ID} ghcr.io/${GITHUB_ORG}/${PROJECT_NAME}/${IMAGENAME}:${VERSION_ID}
```

* See an example of output logs for the command `docker images`:
```bash
$ docker images
REPOSITORY                                                            TAG       IMAGE ID       CREATED              SIZE
intro2dl-uai                                                          v0.1.0    8c878d9c78eb   8 minutes ago    10.6GB
ghcr.io/mxochicale/carpentries/intro2dl-uai                           v0.1.0    8c878d9c78eb   8 minutes ago    10.6GB
ghcr.io/ucl-arc-unified-ai/kubeflow-notebook-images-pytorch-default   v1.0.20   cb6ac0ec3c25   3 weeks ago          8.41GB
```

* Authenticating with a personal access token (classic)
```bash
GITHUB_USERNAME=YOUR_GITHUB_USERNAME_ID
export CR_PAT=YOUR_PERSONAL_ACCESS_TOKEN
echo ${CR_PAT} | docker login ghcr.io -u ${GITHUB_USERNAME} --password-stdin
	#Login Succeeded
```

* Pushing container images to GitHub container registry
```bash
docker push ghcr.io/${GITHUB_ORG}/${PROJECT_NAME}/${IMAGENAME}:${VERSION_ID}
```
Go to packages `https://github.com/orgs/${GITHUB_ORG}/packages` and in package settings, change visibility to public.


