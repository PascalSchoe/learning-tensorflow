# Installation
Alle Schritte die hier vorgenommen werden, werden mit Ubuntu18.04 unternommen dies kann und wird sich je nach OS und Version sicher unterscheiden.

## GPU-enabled 
Als erstes müssen wir sicherstellen dass alles Treiber von *Nvidia* installiert sind:


1. Cuda-toolkit installieren
```shell
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update
$ sudo ubuntu-drivers autoinstall
```

... Pc neustarten ...

```shell
$ sudo apt install nvidia-cuda-toolkit gcc-6
$ nvcc --version
```


2. Repository Nvidia-Treiber bekannt machen
```shell
$ curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
$ distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
$ curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list |\ 
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
$ sudo apt-get update
``` 
[weitere Infos...](https://nvidia.github.io/nvidia-docker/)

3. Installieren von *nvidia-docker2* und neustarten des *Dockerdeamon*
```shell
$ sudo apt-get install nvidia-docker2
$ sudo pkill -SIGHUP dockerd
```
[weitere Infos...](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0))

4. *Nvidia-docker-container* starten
```shell
$ docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi
```

5. Herunterladen des gewünschten *Tensorflow-Images*. 
```shell
$ docker pull tensorflow/tensorflow:latest-gpu-py3
```
Der 'Teil' nach dem ":" ist der *Tag* des Images hiermit wird festgelegt das wir eine GPU optimierte Variante die Python3 nutzt verwenden wollen. Weitere verfügbare Tags findest du [hier](https://www.tensorflow.org/install/docker#download_a_tensorflow_docker_image)

6. Nun testen wir die Installation
```shell
$ docker run --runtime=nvidia -it -p 8888:8888 tensorflow/tensorflow:latest-gpu-py3
```

Nun solltet ihr im Terminal einen Link sehen diesen gebt ihr bitte im Browser ein, sollte nun eine Weboberfläche zu sehen sein hat alles geklappt, super! 

Diese Oberfläche stellt ein kleines Tutorial dar welches mit [Jupyter Nootbooks](http://jupyter.org/) arbeitet.

Möchtest du jedoch einfach nur einen kleinen Test durchführen wollen kannst du auch dies hier machen:

```shell
$ docker run --runtime=nvidia -it tensorflow/tensorflow:latest-gpu bash
$ python
```

Anschließend:
```python
>>> import tensorflow as tf;
>>> print(tf.contrib.eager.num_gpus())
```
Hier solltest du unter anderem deine Grafikkarte finden.
