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
Install the IARPA-IQ-YOLO Docker container [here.](https://drive.google.com/file/d/1517yLfR2ySeCc_1IBHuPQtxh__xM79oh/view?usp=sharing)

## Instructions for Users

To load the Docker container, go to the directory where the container is and use the following command:
~~~
sudo docker load -i iarpa-iq-api.tar.gz
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
This image comes with folders and folder paths hard-coded for convenience. To run the image with the desired dataset:
~~~
sudo docker run -v \
<dataset_absolute_local_path>: \
/home/IARPA-IQ-API-main/data iarpa-iq-api \
/home/IARPA-IQ-API-main/./run_ML_code.sh
~~~
where ```/home/IARPA-IQ-API-main/data``` is the ```<dataset_absolute_image_path>``` This runs the image in an interactive mode when the code is done, meaning that you can use a terminal interface and add/edit files in the container instead of running it standalone.
  
You may ```exit``` at any time to exit the image. Note that instances and changes are not automatically saved and will be deleted once exiting the image.

If the run stops immediately, check the log.err file (go to its directory in ```/home/IARPA-IQ-API-main/results/``` and use ```tail log.err```) for any errors and address them if able.
  
You can check progress with log.err and log.out. The predictions will be saved in <save_path> in preds.pkl, and the accuracy will be reported in ```log.out``` as (slice, example) accuracy.

[Back to Contents](#contents)
## Instructions for Developers
To run the image without mounting a dataset (i.e., for file exploration), run:
~~~
sudo docker run -it iarpa-iq-api bash
~~~
The ```bash``` signifies that you want the image to open with a bash interface.

To copy a local dataset, folder, or file to the image, run
~~~
sudo docker cp ~/<local path to file/folder> <container ID>:<image path>
~~~
where <image path> may be ```/home``` for simplicity. An example usage would be ```sudo docker cp ~/Downloads/IARPA-IQ-API-main d71ed07d1bc6:/home```.

To save a version of the image, exit the image, and do the following process. Open up a new terminal interface and run
~~~
sudo docker ps -a
~~~
to obtain the container ID of all images (we want the most recent entry at the top), and then run
~~~
sudo docker commit <container ID> <new image name>
~~~
to save the current version. You may add a tag to <new image name> via
semicolon, e.g., iarpa-iq-yolo:v2, and so on.

Finally, to export the container, run #sudo docker ps again to obtain the
randomly generated "name" of the container instance e.g., "beautiful_banzai",
and run:
~~~
$sudo docker export <name> > <file.tar.gz>
~~~
to store it as a tar.gz file for sharing. As an example:
~~~
$sudo docker export beautiful_banzai > iarpa-iq-yolo.tar.gz
~~~
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
If the image does not load, you can try
~~~
sudo cat iarpa-iq-api-tar.gz | sudo docker import - iarpa-iq-api/iarpa-iq-api
~~~
  
  
