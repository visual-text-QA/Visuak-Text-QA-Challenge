# ACM Multimedia Grand Challenge on Visual Text Question Answering

Challenge website: [Visual Text Question Answering Challenge](https://visual-text-qa.github.io/)

Overview paper: [VTQA: Visual Text Question Answer via Entity Alignment and Cross-Media Reasoning](https://openaccess.thecvf.com/content/CVPR2024/html/Chen_VTQA_Visual_Text_Question_Answering_via_Entity_Alignment_and_Cross-Media_CVPR_2024_paper.html)


## Challenge 

### Overview

In this challenge, the model is expected to answer the question according to the given image-text pair. Information diversity, multimedia multi-step reasoning and open-ended answer make our task more challenging than the existing dataset. The aim of this challenge is to develop and benchmark models that are capable of multimedia entity alignment, multi-step reasoning and open-ended answer generation.

### Task

As illustrated in Figure 1, given an image-text pair and a question, a system is required to answer the question by natural language. Importantly, the system needs to: (1) analyze the question and find out the key entities, (2) align the key entities between image and text, and (3) generate the answer according to the question and aligned entities. For example, in Figure 1, the key entity of Q1 is “Elena”. According to the text “gold hair”, we can determine that the second person from the right in the image is “Elena”. Finally, we further answer “suit” based on the image information. As for Q2, which is a more complex question, the previous steps need to be repeated several times to answer it.

### Dataset

VTQA dataset consists of 10,238 image-text pairs and 27,317 questions. The images are real images from [MSCOCO](https://cocodataset.org/) dataset, containing a variety of entities. The annotators are required to first annotate relevant text according to the image, and then ask questions based on the image-text pair, and finally answer the question open-ended. We randomly split the dataset into training, validation, and test splits and each image will only appear in one split. Each split has 12848, 3106, 11363 samples, respectively.

You can register on our challenge [website](http://vtqa-challenge.fixtankwun.top:20010/) and download the data.

The `images` folder in the data contains the original image file from [MSCOCO](https://cocodataset.org/)(`.jpg`), the [bottom-up-attention](https://github.com/peteanderson80/bottom-up-attention) region features(`.npz`) and the [grid-features](https://github.com/facebookresearch/grid-feats-vqa)(`.npy`).

The `annotations` folder in the data contains Chinese version (`**_zh.json`) and English version (`**_en.json`) of annotated text. And additional word segmentation and vectorization files (by [spaCy](https://spacy.io/)) are provided.
**Notice: The test_dev set is public but without answer. The test set is private for the final challenge rank.**


## Participates

You can use our [demo](https://github.com/visual-text-QA/VTQA-Demo) to quickly build your own model and participate in this competition. And you can also follow the steps listed below to participate in this competition.(We only accept docker image as your solution submission for the test set.)

### (a) Install Docker

Go to https://docs.docker.com/get-docker/ and install the Docker application corresponding to your platform.

### (b) Pull the image

```
docker pull nvcr.io/nvidia/pytorch:21.12-py3
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
docker run --gpus all -itd --name vtqa --shm-size 8g -v /your-data-path/:/workspace/data nvcr.io/nvidia/pytorch:21.12-py3
```
```
docker exec -it vtqa /bin/bash
```

You can now train your own model in this container. (Or just use the [demo](https://github.com/visual-text-QA/VTQA-Demo) for a quick start.)

### (e) Submitting as a JSON file for test_dev set

Once you have trained your model and predict the answers for the test_dev set, you can write your predict answers as a json file like the [example](./test_dev_example.json) and submit it [here](http://vtqa-challenge.fixtankwun.top:20010/).

### (f) Submitting as a Docker image for test set

Upload your Docker image to [DockerHub](https://hub.docker.com/) and submit your docker image name [here](http://vtqa-challenge.fixtankwun.top:20010/). 
 
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
@INPROCEEDINGS{VTQA,
  author={Chen, Kang and Wu, Xiangqian},
  booktitle={2024 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR)}, 
  title={VTQA: Visual Text Question Answering via Entity Alignment and Cross-Media Reasoning}, 
  year={2024},
  volume={},
  number={},
  pages={27208-27217},
  keywords={Measurement;Visualization;Grounding;Computational modeling;Natural languages;Fitting;Object detection;artificial intelligence;multimodal;visual question answering;dataset},
  doi={10.1109/CVPR52733.2024.02570}}
```
