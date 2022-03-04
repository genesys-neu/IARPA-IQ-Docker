# README
This Docker container (4.2 GB) was built to run the "IARPA-IQ-API" and "Yolo-inference"
repositories for anomaly detection in LTE signals without the need to manually install specific package versions.

This is a README companion to run the docker container "iarpa-iq-yolo", which is a work in progress.  
THIS IS NOT A DOCKERFILE!  
Please send all questions to either gu.je or soltani.n {@northeastern.edu}, thanks!

# Contents
* [Pre-requisites](#pre-requisites)
* [Instructions for Users](#instructions-for-users)
  * [Copying data into the image](#copying-data-into-the-image)
  * [Running the image](#running-the-image)
  * [Running the IQ API](#running-the-iq-api)
* [Instructions for Developers](#instructions-for-developers)
* [Appendix](#appendix)



## Pre-requisites
Install Docker engine/app for your specific operating system [here.](https://docs.docker.com/engine/install/)  
Install the IARPA-IQ-YOLO Docker container [here.](https://drive.google.com/file/d/1g5FhRBTiItmkwHRykxKWbMqmH_fiKV7r/view?usp=sharing)

## Instructions for Users

To load the Docker container, go to the directory where the container is and use the following command:
~~~
sudo docker load -i iarpa-iq-yolo.tar.gz
~~~
To verify that the container was loaded successfully, do:
~~~
sudo docker images
~~~
which should display something like the following, with the ```iarpa-iq-yolo``` image:
~~~
REPOSITORY      TAG       IMAGE ID       CREATED        SIZE
iarpa-iq-yolo   latest    d71ed07d1bc6   36 hours ago   4.2GB
~~~
### Copying data into the image
To copy a local dataset, folder, or file to the image, run:
~~~
sudo docker cp ~/<local path to file/folder> <image ID>:<image path>
~~~
where <image path> may be ```/home``` for simplicity. An example usage would be ```sudo docker cp ~/Downloads/IARPA-IQ-API-main d71ed07d1bc6:/home```.
### Running the image
To run the image:
~~~
sudo docker run -it iarpa-iq-yolo
~~~
This runs the image in an interactive mode, meaning that you can use a terminal interface and add/edit files in the container instead of running it standalone.
You may ```exit``` at any time to exit the image. Note that instances and changes are not automatically saved and will be deleted once exiting the image.
### Running the IQ API  
Once inside the image, do:
~~~
cd /home/YOLO-prediction/
~~~
Create a <save_path> where your results and predictions will go.
Then, open run_ML_code.sh and fill out the following arguments in <>. The other arguments should be left alone. The pre-existing run_ML_code.sh has examples for syntax comparisons.
~~~
python3 -u /home/<user>/IARPA-IQ-binary/ML_code/top.py \       # Fill in with your username and location of code folder, if necessary.
--exp_name <exp> \                                            # Experiment name
--data_path /home/<user>/<data_path>/ \                       # Path to the data to be tested.
--stats_path /home/<user>/<stats_path>/stats.pkl \            # Path to the stats.pkl used to determine predictions 
--save_path /home/<user>/<save_path>/ \                       # Path to the results and predictions
--model_flag alexnet \                                        # Architecture name, alexnet or resnet. Don't touch
--contin true \                                               # Test using an existing model/weights. Don't touch
--json_path '/home/<user>/<model_path>/model_file.json' \     # Path to the model file, used if contin is true
--hdf5_path '/home/<user>/<weights_path>/model.hdf5' \        # Path to the weights file, used if contin is true
--slice_size 256 \                                            # Input size to NN. Don't touch
--num_classes 2 \                                             # Number of classes for predictions. Don't touch for now
--batch_size 256 \                                            # Batch size for training. Don't touch
--id_gpu 0 \                                                  # GPU number to use to run training/testing. 0 should be default. Don't touch
--normalize true \                                            # Normalizes data for training/testing. Don't touch
--train false \                                               # Train the model. Don't touch for now, as the model has already been created
--test true \                                                 # Test the model. Don't touch
--epochs 30 \                                                 # Number of run-throughs for training. Don't touch
--early_stopping true \                                       # Allows training to stop early if no increase in prediction accuracy is detected. Don't touch
--patience 3 \                                                # Specifies number of epochs to wait to do early stopping in training. Don't touch
> /home/<user>/<save_path>/log.out \                          # Displays training progress and testing results.
2> /home/<user>/<save_path>/log.err                           # Displays error tracebacks and run progress.
~~~
Run the code with:
~~~
/run_ML_code.sh
~~~
  
If the run stops immediately, check the log.err file (go to its directory and use ```tail -f log.err```) for any errors and address them if able.
  
You can check progress with log.err and log.out. The predictions will be saved in <save_path> in preds.pkl, and the accuracy will be reported in ```log.out``` as (slice, example) accuracy.

[Back to Contents](#contents)
## Instructions for Developers
To save a version of the image, exit the image, open up a new terminal interface and run
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
  
  
  
  
