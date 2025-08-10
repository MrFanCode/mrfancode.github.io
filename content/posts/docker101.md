# How to run Linux using Docker

`24-03-2025`


## Install docker on your linux ( For windows users can follow along with WSL (Windows Subsystem for Linux). )

`sudo apt update && sudo install docker.io -y`

### Then install an linux image by using this command: ( [Search More Images](https://hub.docker.com) )

`sudo docker pull ubuntu:latest`

- make sure use the ':latest' in the end of image name so you get the latest version
- then just wait for the installation to finish.

## After installed the image try to confirm if the image is there in your system:

`sudo docker images`

- this will list the images that installed on your computer

## Now you can run the image as a container:

`docker run -it --name example --host example ubuntu:latest`

- this will give you an interactive terminal of the ubuntu linux image
- to break down what does the tags/arguments on the command do:
  - run = to create and run a new container
  - -it = interactive ( so you can get an interactive terminal/command line interface )
  - --name = setting a name for your container that will be usefull in future when you want to stop or start the container
  - --host = to set an hostname inside your container
  - ubuntu:latest = is the image name that you want to run




