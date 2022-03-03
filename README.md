# IARPA-IQ-YOLO-Docker

To access the "iarpa-iq-yolo" docker container, please visit https://northeastern-my.sharepoint.com/personal/gu_je_northeastern_edu/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fgu%5Fje%5Fnortheastern%5Fedu%2FDocuments%2FIARPA%2DIQ%2DYOLO
and request access from gu.je@northeastern.edu.

This is a README companion to run the docker container "iarpa-iq-yolo", which is
a work in progress.
THIS IS NOT A DOCKERFILE!
Please message Jerry Gu (gu.je@northeastern.edu) for any questions, thanks!

This Docker container was built to run the "IARPA-IQ-API" and "Yolo-inference"
repositories without the need to manually install correct package versions,
though most versions of the used packages should work. The packages and
versions in this container are:

Python 3.8.10
pip3 20.0.3
TensorFlow/Keras 2.8
Opencv-python (cv2) 4.4.5
Pillow (PIL) 9.0.1
tqdm 4.62.3
glob2 0.7
Setuptools 60.9.3

To start using this Docker container, please install the following:

Docker engine/app: https://docs.docker.com/engine/install/
iarpa-iq-yolo.tar.gz (do not untar)

To load the Docker container:

$sudo docker load -i iarpa-iq-yolo.tar.gz

There should be a progress bar that loads if done successfully. The container is
currently ~4.18 GB.

To check for the images in the container:

$sudo docker images

which should display the iarpa-iq-yolo images (repositories), image IDs, and other info.
This can be done at any time, in case more images are added later on. To run the image:

$sudo docker run -it iarpa-iq-yolo

This runs the image in an interactive mode, meaning that you can use a terminal
interface and add/edit files in the container instead of running it standalone.

Once inside the image, $cd home to see the two installed folders:

IARPA-IQ-API
YOLO-prediction

and $exit to exit the image. Note that instances and changes are not
automatically saved and will be deleted once exiting the image.

To copy a local dataset, folder, or file to the image, open a new terminal and run:

$sudo docker cp ~/<local path to file/folder> <image ID>:<image path>

<image path> may be /home for simplicity. The ~/<local path to file/folder>
<image ID>:<image path> argument order may be reversed to save a file from the container to local.

To save a version of the image, exit the image, open up a new terminal interface and run

$sudo docker ps -a

to obtain the container ID of all images (we want the most recent entry at the top), and then run

$sudo docker commit <container ID> <new image name>

to save the current version. You may add a tag to <new image name> via
semicolon, e.g., iarpa-iq-yolo:v2, and so on.

Finally, to export the container, run #sudo docker ps again to obtain the
randomly generated "name" of the container instance e.g., "beautiful_banzai",
and run

$sudo docker export <name> > <file.tar.gz>

to store it as a tar.gz file for sharing. As an example:

$sudo docker export beautiful_banzai > iarpa-iq-yolo.tar.gz
