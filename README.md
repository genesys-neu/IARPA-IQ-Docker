# README
This Docker container (3.27 GB compressed, 4.2 GB uncompressed) was built to run the "IARPA-IQ-API" repository for anomaly detection in LTE signals without the need to manually install specific package versions.
 
Please send all questions to either gu.je or soltani.n {@northeastern.edu}, thanks!

# Contents
* [Pre-requisites](#pre-requisites)
* [Instructions for Users](#instructions-for-users)
  * [Running the IQ API](#running-the-iq-api)
* [Instructions for Developers](#instructions-for-developers)
* [Appendix](#appendix)
  * [Troubleshooting](#troubleshooting)

## Pre-requisites
Install Docker engine/app for your specific operating system [here.](https://docs.docker.com/engine/install/)  
Install the IARPA-IQ-YOLO Docker container [here.](https://drive.google.com/file/d/1oEaHiV8m9kwwBo_Osz8eF8QzZzCcjHus/view?usp=sharing)

## Instructions for Users

To load the Docker container, go to the directory where the container is and use the following command:
~~~
sudo docker load -i iarpa-iq-api.tar
~~~
To verify that the container was loaded successfully, do:
~~~
sudo docker images
~~~
which should display something like the following, with the ```iarpa-iq-api``` image:
~~~
REPOSITORY      TAG       IMAGE ID       CREATED        SIZE
iarpa-iq-api   latest    d71ed07d1bc6   36 hours ago   4.2GB
~~~
### Running the IQ API  
This image comes with folders and folder paths hard-coded for convenience. To run the image with the desired dataset, run
~~~
sudo docker run -v \
<dataset_absolute_local_path>:\
/home/IARPA-IQ-API-main/data iarpa-iq-api \
/home/IARPA-IQ-API-main/./run_ML_code.sh
~~~
where ```/home/IARPA-IQ-API-main/data``` is the ```<dataset_absolute_image_path>```. The input data files may be any binary files containing alternating I and Q samples in a single column, i.e., vector of shape (# of total samples x 1), with at least **256** samples. An example of this command is  
```sudo docker run -v /home/jgu1/data:/home/IARPA-IQ-API-main/data iarpa-iq-api /home/IARPA-IQ-API-main/./run_ML_code.sh```. The ```-v``` flag mounts the dataset to the image without needing to copy the dataset into the image. This runs the IQ-API while printing out predictions, given as (file:prediction), to the terminal without entering the image itself.

[Back to Contents](#contents)
## Instructions for Developers
To run the image without mounting a dataset (i.e., for file exploration), run
~~~
sudo docker run -it iarpa-iq-api bash
~~~
The ```bash``` signifies that you want the image to open with a bash interface. You may ```exit``` at any time to exit the image. Note that instances and changes are not automatically saved and will be deleted once exiting the image. You may run the API while in the image by navigating to ```/home/IARPA-IQ-API-main``` and running ```./run_ML_code.sh```.

To copy a local dataset, folder, or file to the image, run
~~~
sudo docker cp ~/<local path to file/folder> <container ID>:<image path>
~~~
where ```<image path>``` may be ```/home``` for simplicity.  
An example usage would be ```sudo docker cp ~/Downloads/IARPA-IQ-API-main d71ed07d1bc6:/home```.

To save a version of the image, obtain the container ID, e.g., ```1c7c465972ad``` from ```root@1c7c465972ad:/#``` while running an image. You may also open up a new terminal interface and run
~~~
sudo docker ps -a
~~~
to obtain the container ID of all images (timestamped by creation time and status), and then run
~~~
sudo docker commit <container ID> <new image name>
~~~
to save the current version. You may add a descriptive tag to <new image name> via
semicolon, e.g., iarpa-iq-yolo:v2, and so on.

To remove all stopped containers or images, run
~~~
sudo docker container prune
~~~
or
~~~
sudo docker image prune
~~~
respectively. You may add a time limit for removal, e.g., ```docker image prune --filter "until=24h"``` to remove all containers/images older than 24 hours.
 
Finally, to export the container to a .tar or .tar.gz file for transfering the image, obtain the generated name of the container instance from  
```sudo docker ps -a```, e.g., ```beautiful_banzai```, and run:
~~~
sudo docker export <name> > <file.tar>
~~~
to store it as a .tar file for sharing. As an example, ```sudo docker export beautiful_banzai > iarpa-iq-yolo.tar```.  
[Back to Contents](#contents)
## Appendix
The packages and their respective versions in this container are:
~~~
Python 3.8.10
pip3 20.0.3
TensorFlow/Keras 2.8
Opencv-python (cv2) 4.4.5
Pillow (PIL) 9.0.1
tqdm 4.62.3
glob2 0.7
Setuptools 60.9.3
~~~

### Troubleshooting
If the container comes in a .tar.gz format, you can try
~~~
sudo cat iarpa-iq-api.tar.gz | sudo docker import - iarpa-iq-api
~~~
to load the container. If you are using a NVIDIA GPU, you can access the installation guide for their container toolkit [here](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) in order for the Docker to detect your NVIDIA GPU.
 
[Back to Contents](#contents)
