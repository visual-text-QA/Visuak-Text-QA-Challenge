# ACM Multimedia Grand Challenge on Visual Text Question Answering

Challenge website: [Visual Text Question Answering Challenge](https://visual-text-qa.github.io/)

Overview paper: [VTQA: Visual Text Question Answer via Entity Alignment and Cross-Media Reasoning](https://arxiv.org/abs/2303.02635)


## Challenge 

### Overview

In this challenge, the model is expected to answer the question according to the given image-text pair. Information diversity, multimedia multi-step reasoning and open-ended answer make our task more challenging than the existing dataset. The aim of this challenge is to develop and benchmark models that are capable of multimedia entity alignment, multi-step reasoning and open-ended answer generation.

### Task

As illustrated in Figure 1, given an image-text pair and a question, a system is required to answer the question by natural language. Importantly, the system needs to: (1) analyze the question and find out the key entities, (2) align the key entities between image and text, and (3) generate the answer according to the question and aligned entities. For example, in Figure 1, the key entity of Q1 is “Elena”. According to the text “gold hair”, we can determine that the second person from the right in the image is “Elena”. Finally, we further answer “suit” based on the image information. As for Q2, which is a more complex question, the previous steps need to be repeated several times to answer it.

### Dataset

VTQA dataset consists of 10,238 image-text pairs and 27,317 questions. The images are real images from [MSCOCO](https://cocodataset.org/) dataset, containing a variety of entities. The annotators are required to first annotate relevant text according to the image, and then ask questions based on the image-text pair, and finally answer the question open-ended. We randomly split the dataset into training, validation, and test splits and each image will only appear in one split. Each split has 12848, 3106, 11363 samples, respectively.

You can use the following link to download data.

| Data                | Google Drive                                                                                   | Baidu Yun                                                            |
| ------------------- | ---------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| Images              | [download](https://drive.google.com/file/d/1-Hop5LM7jiXsivpub8xB79aUdJaLf6Rw/view?usp=sharing) | [download](https://pan.baidu.com/s/1mIHGO18Jhjyb2XHHsIGBeA?pwd=4dce) |
| Chinese Annotations | [download](https://drive.google.com/file/d/1-Cd2qFA_WJMHFw490TvCa9G6F_aXl8m9/view?usp=sharing) | [download](https://pan.baidu.com/s/1mIHGO18Jhjyb2XHHsIGBeA?pwd=4dce) |
| English Annotations | Coming soon                                                                                    | Coming soon                                                          |

The `images` folder in the data contains the original image file from [MSCOCO](https://cocodataset.org/)(`.jpg`), the [bottom-up-attention](https://github.com/peteanderson80/bottom-up-attention) region features(`.npz`) and the [grid-features](https://github.com/facebookresearch/grid-feats-vqa)(`.npy`).

**Notice: The test_dev set is public but without answer. The test set is private for the final challenge rank.**


## Participates

You can use our [demo](https://github.com/visual-text-QA/VTQA-Demo) to quickly build your own model and participate in this competition. And you can also follow the steps listed below to participate in this competition.(We only accept docker image as your solution submission for the test set.)

### (a) Install Docker

Go to https://docs.docker.com/get-docker/ and install the Docker application corresponding to your platform.

### (b) Pull the image

```
docker pull nvcr.io/nvidia/pytorch:23.01-py3
```

(You can also use the other images to do you work.But our demo has only been tested in this image.)

### (c) Download dataset

Download the dataset follow [Dataset](#Dataset).

Unzip the files and place them as follows:
```angular2html
|-- data
	|-- images
	|  |-- train
	|  |-- val
	|  |-- test_dev
    |-- annotations
```

### (d) Create your container and train you model

```
docker run --gpus all -itd --name vtqa --shm-size 8g -v /your-data-path/:/workspace/data nvcr.io/nvidia/pytorch:23.01-py3
```
```
docker exec -it vtqa /bin/bash
```

You can now train your own model in this container. (Or just use the [demo](https://github.com/visual-text-QA/VTQA-Demo) for a quick start.)

### (e) Submitting as a JSON file for test_dev set

Once you have trained your model and predict the answers for the test_dev set, you can write your predict answers as a json file like the [example](./test_dev_result_demo.json) and submit it [here](http://81.70.95.220:20035/index.html).

### (f) Submitting as a Docker image for test set

Upload your Docker image to [DockerHub](https://hub.docker.com/) and submit your docker image name [here](http://81.70.95.220:20035/index.html). 
 
Example for a Dockerhub user called `<username>`:

```
SOURCE=<container name>
USER=<username>
docker login
docker commit -a $USER -m "vtqa submission" ${SOURCE}  vtqa:submission
docker push vtqa:submission
```

Participant solutions submitted as Docker images will be run using the following command: 

```
docker pull <username>/vtqa:submission
docker run --shm-size 8g -v <path to folder containing the test.json>:/workspace/data -v <path to save pred json>:/workspace/test_pred.json --gpus all --rm vtqa:submission /bin/bash -c "python /workspace/VTQA-Demo/test.py"
```

## Citation

If this repository is helpful for your research, we'd really appreciate it if you could cite the following paper:

```
@inproceedings{Chen2023VTQAVT, 
    title={VTQA: Visual Text Question Answering via Entity Alignment and Cross-Media Reasoning}, 
    author={Kang Chen and Xiangqian Wu}, 
    year={2023} 
}
```