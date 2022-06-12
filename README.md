# Tandur Model Deployment

## Overview
This repository contains models that have been trained and are ready to be deployed. We use TensorFlow serving to deploy our model and docker to create the container from that TensorFlow serving.

## Requirements
- [Docker Desktop](https://docs.docker.com/docker-for-windows/install/)
- [Windows Subsystem Linux (WSL)](https://docs.microsoft.com/id-id/windows/wsl/install-manual)

## Project Installation
#### Pull a development image (Tensorflow Serving)
Download the TensorFlow Serving Docker image and repo
```docker
docker pull tensorflow/serving
```
#### Clone the repository
```bash
git clone https://github.com/Tandur-Team/tandur-ml-deployment.git
```
#### Run a serving image
```docker
docker run -d --name serving_base tensorflow/serving
```
#### Copy _/plant-model_ to the container's model folder
```docker
docker cp /plant-model serving_base:/models/plant-model
```
#### Commit the container
```docker
docker commit --change "ENV MODEL_NAME plant-model" serving_base plant-model
```
#### Stop serving_base container
```docker
docker kill serving_base
```

## Run The Application
Docker will create a container from the plant-model image and run it at localhost:8501
```docker
docker run -p 8500:8500 -p 8501:8501  -t plant-model --model_config_file=/models/plant-model/models.config
```

## API Endpoint
**POST** _```{URL}```/v1/models/```{plant-name}```:predict_ : This endpoint allow user to register to the app and save user data to MySQL
- ``` URL ```         : Use localhost:8501 if run the applicaation on local
- ``` plant-name ```  : Use the same plant name as the folder name in the plant-model folder (e.g model-Padi)

## Test The Application
### 1. Run The Application
```bash
docker run -p 8500:8500 -p 8501:8501  -t plant-model --model_config_file=/models/plant-model/models.config
```
### 2. Open POSTMAN
### 3. Set The API endopoint and the method
### 4. Request body needed to be able to run predictions
```JSON
{
    "instances": [
        [
            temperature,
            humidity,
            rainfall
        ]
    ]
}
```
### 5. Click SEND
### 6. You should get a JSON response
