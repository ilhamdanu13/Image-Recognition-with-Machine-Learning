# Image Recognition with Machine Learning
# The Idea of Data Image

Basically the structure of an image consists of pixels in the form of array numbers, the image is black and white. The pixel range is 0 to 255 or 0 to 1 and usually used is 0 to 1 and scaling by dividing by 255.

as an example:

![image](https://user-images.githubusercontent.com/86812576/168827274-9c7aba3b-fa61-427c-86c8-742a5efcad64.png)

In the image above we will see the array number behind the image:

![image](https://user-images.githubusercontent.com/86812576/168827736-fea779d2-5739-4b2f-b018-ee4526238f1b.png)

If the pixel is 0 it means the pixel is black. if the pixel is worth 255 it means it's white, and if the pixel is worth 125 it means it's gray. The closer to the 255 array values ​​in the pixel, the closer to white it is.

**how can the image be in color**?

The color is actually a combination of Red, Green, Blue (RGB). Can be seen on tube tv.
Each of these colors is a pixel.
The color image is a combination of RGB colors with certain channels.

### then how to process Image Data?
### Flattenig

Square image is initially straightened along the number of pixels
Once flattened, it can be arranged into a table. After being converted into a table, then it can be processed by machine.

![image](https://user-images.githubusercontent.com/86812576/168829045-4e5119b9-3e8d-49e2-8fe7-951abfb454db.png)

### result of structuring by flattening

![image](https://user-images.githubusercontent.com/86812576/168829334-347834e2-8dd7-42bc-a17e-ebcb7fc4c202.png)

On the left is the image data, the column is the features (pixels).
If the size is different then crop it to be the same size.
So in essence: How to solve problems with unstructured data images, which is to first change them to structured and then enter them into machine learning. And the Data Image is a pixel as a feature.

# MNIST Dataset

The MNIST database (Modified National Institute of Standards and Technology database) is a large database of handwritten digits that is commonly used for training various image processing systems. The database is also widely used for training and testing in the field of machine learning. It was created by "re-mixing" the samples from NIST's original datasets. The creators felt that since NIST's training dataset was taken from American Census Bureau employees, while the testing dataset was taken from American high school students, it was not well-suited for machine learning experiments. Furthermore, the black and white images from NIST were normalized to fit into a 28x28 pixel bounding box and anti-aliased, which introduced grayscale levels.

The MNIST database contains 60,000 training images and 10,000 testing images. Half of the training set and half of the test set were taken from NIST's training dataset, while the other half of the training set and the other half of the test set were taken from NIST's testing dataset. The original creators of the database keep a list of some of the methods tested on it. In their original paper, they use a support-vector machine to get an error rate of 0.8%.

Extended MNIST (EMNIST) is a newer dataset developed and released by NIST to be the (final) successor to MNIST. MNIST included images only of handwritten digits. EMNIST includes all the images from NIST Special Database 19, which is a large database of handwritten uppercase and lower case letters as well as digits. The images in EMNIST were converted into the same 28x28 pixel format, by the same process, as were the MNIST images. Accordingly, tools which work with the older, smaller, MNIST dataset will likely work unmodified with EMNIST.


Sample images from MNIST test dataset

![image](https://user-images.githubusercontent.com/86812576/167291771-21067340-37c9-46af-9cb5-f8f08c8b51cd.png)

# Import Package

import common packages:

import **numpy as np**

import **pandas as pd**

import **matplotlib.pyplot as plt**

from **sklearn.model_selection** import **train_test_split**

from **sklearn.pipeline** import **Pipeline**

from **sklearn.compose** import **ColumnTransformer**

from **jcopml.utils** import **save_model, load_model**

from **jcopml.pipeline** import **num_pipe, cat_pipe**

from **jcopml.plot** import **plot_missing_value**

from **jcopml.feature_importance** import **mean_score_decrease**

import Algorithm's Package:

from **sklearn.ensemble** import **RandomForestClassifier**

from **sklearn.model_selection** import **RandomizedSearchCV**

from **jcopml.tuning** import **random_search_params as rsp**

# Import Data

 This data has been structured in tabular form. consists of 2000 lines and 785 and 9 labels in it. 785 columns are the pixels of each label. each label has 28x28 pixels. And I will try to predict with machine learning with the _**Random Forest Classifier Algorithm**_.
 
# Explanation
# Dataset Splitting and Scalling

Things are a little different if we talk about _images_ we will rarely use pandas, first we will need numpy (array). So that in the dataset splitting we will directly convert to numpy. Second, we have to check if it uses integer 0 to 255 or float 0 to 1. because if it uses integer 0 to 255 then I want to change it to float 0 to 1.

![vm](https://user-images.githubusercontent.com/86812576/167293357-e4ac5d7d-880e-46d1-93cb-f17473574cf1.png)

Means don't forget that X is divided by 255 and it's called a scaler.

![dt](https://user-images.githubusercontent.com/86812576/167293428-18f33fa1-c1cc-49ee-8d53-d3b22b0af6e0.png)

Why am i doing this? because even though the number is well scaled and balanced, but it is not close to 0, then the loss plane is flat like a chasm and it is not good for gradient descent, on the other hand, if it is close to 0 like the standard scaler, and the minmax scaler turns out to be round and not shaped canyon. and usually the best practice we scaled it from 0 to 1.

# Training

So here, again, something different from the Pipelines we used to do with Structured Data in Machine Learning before. Well, all the data we have is numeric, which is definitely no categorical data. 

Then the second, is there anything that needs to be preprocessed? does the picture have a missing value? of course not so there is no need to impute. 

How about scaling? no need for scaler because we already divide the data by 255. Just throw away the preprocessor. we don't need that. Wo we only have one step in our **_Pipeline_**, it's the _'algo'_, which defines the Random Forest Algorithm. Actually this is just basic, because in certain cases there will be a more advanced preprocessor


![pip](https://user-images.githubusercontent.com/86812576/167298969-b23ff425-ce56-474f-96b7-4bf7d1e5b4f2.png)

I use Random Search with cross validation = 3, scoring uses accuracy because our data is balanced, and the number of iterations = 50.

# Result 

![image](https://user-images.githubusercontent.com/86812576/167301143-ebc2ca45-5dcf-4157-b8f3-07ff3f56d309.png)

I got a good accuracy of 92%. and then I will visualize the prediction how good it is.

# Visualize Prediction

![image](https://user-images.githubusercontent.com/86812576/167301415-05676610-34c8-411c-b013-4a86428bb2cb.png)

The picture above is a plot of the predicted results. There are a total of 36 handwritten images. The green color indicates that the prediction is correct, and red means that the prediction is wrong.

Of the 36 handwritten images above, there are 3 images that the machine predicts incorrectly.

But actually if we look at the 3 images that are wrong predictions. People's handwritten drawings are a little ambiguous. I will take exampless.

Let's see the images below:

first image

![94](https://user-images.githubusercontent.com/86812576/167302008-6e8f380a-f013-4a38-a81b-5bbfd5c57a04.png)

In the picture above the label is the number 9, but the machine predicts the number 4. The picture is actually handwritten which is a bit weird, because the image is a bit crooked. Moreover, the circle of the head of the number 9 does not unite, making it difficult for the machine.

second image

![74](https://user-images.githubusercontent.com/86812576/167302248-7088a172-e8a4-4039-b208-c872f3f60cfe.png)

The second image above is even more ambiguous. They write the number 7 but it is clearly like the number 4 according to our machine predictions. Of course the machine predicts incorrectly, and our model is not bad and can even generalize.

# Why It's Work?

You may ask how simple flattening can do this? Let's see the following picture.

![image](https://user-images.githubusercontent.com/86812576/167302845-193f9d82-fadb-4c6b-969a-a5ddd1d95ebf.png)

At first each label will be flattened, then plotted. Each label has its own pattern.

### Number 0

For example, at number 0 the pattern is that the initial pixel is black because the background images are black, but certain pixels are white.

![0](https://user-images.githubusercontent.com/86812576/167429036-e253ec56-ca4f-4d29-80b0-6119a10cc518.png)

Then we can see that the overall pattern of the number 0 is like the image above. That is, a certain part of the pixel is white then it indicates the number 0.

### Number 1

![1](https://user-images.githubusercontent.com/86812576/167430030-e30db4ca-c848-418f-bdc0-42af570a143c.png)

The number 1 is thinner, in contrast to the number 0 which is thicker because the number 1 is like only one line.

### Number 7 and 9

![79](https://user-images.githubusercontent.com/86812576/167430654-b4f3a287-b379-436a-978d-e52e393cb9c6.png)

The numbers 7 and 9 have similar lines so they will form the same pattern. On the front it is black, the back is a little close to the edge, but in certain parts it looks thicker

And the advantage of machines is that they are very good at looking for patterns. So behind the scenes that the machine can do image classification because there is a pattern. Maybe humans can't see that but machines can. The machine doesn't know that this is an image of the number 1, 2, or 9 but it sees the pattern of the image.
