# Adidas & Nike Shoes Classification with Arduino Nano 33 BLE Sense

Jiani Che

Link to github repo with project work:

Link to Edge Impulse projects: 
## Introduction
Adidas and Nike shoe products have been popular with consumers for many years, however, in recent years consumers have faced the risk of potentially buying fake shoes. Identifying fake shoes with the naked eye is still risky and costly, so could automatic identification with high accuracy be accomplished if deep learning and AI technology were used?
Before identifying the authenticity of shoes, we need to identify the brand and type of the shoes. There have been many attempts to use datasets of shoe images sourced from the web to train classification models for shoe brands and types[1]. But few projects have applied them to sensors for real-time recognition. Then the aim of our project is to train a classification model by using neural network based embedded AI to classify the brand of the shoes in the images. 
In this project, we tried two different sources of data as training sets for separate experiments:1) existent shoes images downloaded from Google images; 2) real-world shoes images taken by an Arduino Sense camera. The experimental results show that the two datasets do not have the same predictive effect. The final model using the Arduino camera data as the training set achieved a maximum of 95% accuracy in correctly identifying the brand of shoes as either Nike or Adidas. 


## Research Question
Can we build an embedded artificial intelligence project based on neural networks to identify whether the brand of shoes is Nike or Adidas?

## Application Overview
The application is built using several components, including Edge Impulse, an Arduino Nano 33 BLE Sense board with a camera, and the Arduino IDE. 

The application consists of three main building blocks: 

1) Data collection, which involves capturing images of Nike and Adidas shoes manually using the Arduino Nano onboard camera. These images are then processed and pre-processed using Edge Impulse and stored as dataset;

2) Model training, involves creating a machine learning model that can accurately distinguish between Nike and Adidas shoes. This is done by conducting multiple experiments with different model architectures, learning blocks and other advanced training settings;

3) Model deployment, which involves running the best-trained model on new, unseen data to make predictions. In our application, the model is deployed on the Arduino Nano 33 BLE Sense board, which allows it to make real-time predictions on images captured by the onboard camera.

Figure1: Application structure of the building blocks

All three building blocks are connected through the use of Edge Impulse, which acts as a central hub for data collection, model training, and deployment. The Arduino IDE is used to upload the trained model to the Arduino Nano 33 BLE Sense board, enabling it to perform inference on new data in real time.
## Data

###Dataset
As mentioned in the introduction, I used 2 shoes images datasets from different sources:


- Dataset 1: existent Adidas and Nike shoes images downloaded from Google images.
To create dataset 1, I used 2 image datasets from Kaggle which downloaded the shoe image from Google[2]. To improve the accuracy of the training model, I only used images with a white background. A total of 400 images were collected, with a 50/50 split between the two brands. And dataset 1 is used as one of the training datasets.

Figure2: Overview of dataset 1 image data


- Dataset 2: shoes images taken by an Arduino Sense onboard camera:
Dataset 2 consists of 2 parts: 1) I took 80 images of 2 pairs of Nike and Adidas shoes in different angles as another training dataset; 2) 20 images of another 4 pairs of Nike and Adidas shoes were taken as our test dataset. All images were taken under almost the same light and background conditions. 

Figure3: Overview of dataset 2 image data

Figure4: Overview of dataset 2 data source 

The following experiments were divided into 2 parts:

Table1: Datasets description of experiment 1

Table2: Datasets description of experiment 2

###Data Processing
After uploading the raw image data, Edge Impulse then processed the images by resizing and changing the color depth. In this project, the image data was resized into 96*96 and then converted into grayscale, which has the best performance in the model.

Figure5: Edge Impulse user interface of image data processing

Figure6：Edge Impulse user interface of image data color depth 



## Model
After data collection, a processing block and a learning block were required for feature generation and machine learning. For image detection projects, Edge Impulse recommended image transfer learning as the learning block. Though different learning blocks were applied as well, after a series of tests, image transfer learning and dataset 2 were chosen as our final training/test dataset for better performance. 

Figure7: Edge Impulse user interface of Impulse design

Edge Impulse allows users to adjust the model in different ways, including a simplified visual mode, Kera(expert) mode and editing Python files locally. To build a better performance model, I modified multiple training settings and model architectures in 35 experiments. Among all experiments, the following settings and architecture have the best performance. 

Figure8 & 9: Shoes Detection Model Neural Network Architecture and settings in the Edge Impulse user interface

The key parameters settings are shown below:

Table3: The key parameters settings of the final model


## Experiments
What experiments did you run to test your project? What parameters did you change? How did you measure performance? Did you write any scripts to evaluate performance? Did you use any tools to evaluate performance? Do you have graphs of results? 
Out of a total of 35 experiments, 13 experiments were conducted on datasets sourced from the web and 22 experiments were conducted on datasets taken by sensor cameras. And 11 parameters were selected for different combinations in the experiments.

Table4: Parameters changed during the experiments

As mentioned before, the experiments were divided into 2 parts:



- Training data: dataset 1(400 images)/Test Data: dataset 2 (100 images)

The experimental results show that the maximum accuracy of the machine learning model was only 64.36% when using image data from the network as the training set. Although the accuracy improved from 20.83% to 64.36% by adjusting the combination of parameters, the loss values remained high, which significantly reduces the usability of the deployment.

Table5: Experiments records of experiment 1

- Training data: dataset 2-1 (80 images)/Test Data: dataset 2-2 (20 images)

Experiments have shown that when using images sourced from the sensor as the training set, the accuracy of the model improves from 70% to a maximum of 95% after adjusting the combination of parameters.

Two learning blocks, image transfer learning and Classification, were applied in the experiments and the models based on them both performed well, and image transfer learning was chosen for the final model, which has a higher testing accuracy. 

In addition, experiments have shown that models with image transfer learning blocks performed better with grayscale image data, however, models with classification blocks performed better with RGB image data.

Table6: Experiments records of experiment 2

## Results and Observations

###Results
As mentioned in the experiment part, the accuracy of the model which used the image data from the network as the training set is not satisfactory. The model using the image data taken by the Arduino onboard camera as the training set was selected as the final model to be uploaded to the Arduino Nano sensor. The final model was obtained with a maximum testing accuracy of 95% and a validation accuracy of 87.5%. 

Figure10 & 11: Validation accuracy and Testing Accuracy of the final model

###Observations

- Models performed better when using real-world images taken by Arduino Camera: 

The images from the web may have different lighting conditions, angles, backgrounds, and resolutions compared to the images taken from the sensor camera. This can lead to differences in the features that the model learns to identify, as well as differences in the noise and variations in the images. In addition, the images from the sensor camera are likely to be more similar to the images that the model will encounter in real-world scenarios, as they are taken from the same type of camera that the model will be deployed on. This can help the model better generalize and make more accurate predictions on new, unseen data.


- Models with image transfer learning blocks performed better with grayscale image data, however, models with classification blocks performed better with RGB image data:

Grayscale and RGB are two different color spaces used to represent digital images. Grayscale images have a single channel that represents the intensity of the image, whereas RGB images have three channels that represent the intensity of red, green, and blue colors in the image[]. In transfer learning, the pre-trained model is used to extract high-level features from the input image. Grayscale images have a lower dimensionality and therefore may require fewer computational resources for feature extraction, allowing the model to learn more efficiently. Additionally, the lack of color information may help the model focus on the distinctive shapes and textures of the shoe, which can be more important for brand detection. In classification, on the other hand, color information can be a crucial feature for distinguishing between Nike and Adidas shoes, as both brands have distinct color schemes that are often associated with their products. Using RGB color channels may provide the model with more information about these color differences, leading to better classification performance. 

###Reflections 
Although the model has a theoretical accuracy of 95%, in practice the deployment accuracy is influenced by a number of factors.



- Angle, light and background

To improve the accuracy of the model when building the dataset, I controlled the lighting and background conditions when the volume was taken so that the majority of the data was in the same light and against a solid color background, which resulted in the Arduino sensor being susceptible to failure when performing real-time classification due to different lighting and backgrounds. Therefore, in order to improve the usability of the deployment in the future, it is necessary to increase the number of data under different conditions.


- Pixels from different cameras

Due to the low pixel count of the Arduino onboard camera, the results of real-time classification are also affected when using a higher pixel count camera such as a mobile phone or computer. Therefore it is necessary to add image data with different pixels, or to keep the training set image data and the camera conditions consistent at the time of application.


- The diversity of shoes

The appearance of Nike and Adidas shoes varies greatly between types and seasons. The fact that I only used my own shoes in the experiment means that my subjective aesthetic also influences the model training results. Therefore, more shoes from different collections and types need to be added to the dataset.

In the end, detecting the brands of shoes is just a basic step before identifying the authenticity of shoes. There is still a long way to go before it can be implemented into real-life applications. But Edge Impulse offers a low-cost, fast and easy platform and approach for we all.
 


## Bibliography
*If you added any references then add them in here using this format:*

1. Last name, First initial. (Year published). Title. Edition. (Only include the edition if it is not the first edition) City published: Publisher, Page(s). http://google.com

2. Last name, First initial. (Year published). Title. Edition. (Only include the edition if it is not the first edition) City published: Publisher, Page(s). http://google.com

*Tip: we use [https://www.citethisforme.com](https://www.citethisforme.com) to make this task even easier.* 

----

## Declaration of Authorship

Jiani Che, AUTHORS NAME HERE, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.


*Jiani Che*

24th April 2023
