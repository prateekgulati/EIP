

 # Cyclic Generative Adversarial Networks for Facial Feature Alteration
 Keras Implementation of cyclic gans for facial feature alteration. The starting point of the code is taken from [Erik Linder-Norén's GitHub](https://github.com/eriklindernoren/Keras-GAN/blob/master/cyclegan/cyclegan.py).  
Three models being used in this experiment:
 * <b>AgeGAN</b>: Young &rarr; Old and Old &rarr; Young
 * <b>RaceGAN</b>: White &rarr; Black and Black &rarr; White
 * <b>GenderGAN</b>: Male &rarr; Female and Female &rarr; Male  
 
 Each of them have two generators and two discriminators.
 
 ## Dataset
Dataset Used: [UTKFace Aligned&Cropped Faces.](https://susanqq.github.io/UTKFace/) 
### Labels
The labels of each face image is embedded in the file name, formated like  `[age]_[gender]_[race]_[date&time].jpg`

-   `[age]`  is an integer from 0 to 116, indicating the age
-   `[gender]`  is either 0 (male) or 1 (female)
-   `[race]`  is an integer from 0 to 4, denoting White, Black, Asian, Indian, and Others (like Hispanic, Latino, Middle Eastern).
-   `[date&time]`  is in the format of yyyymmddHHMMSSFFF, showing the date and time an image was collected to UTKFace
 
## Data Preparation
Each image is initially resized to (128,128). Before creating the network architecture, data is segregated into four folders (trainA, trainB, testA, testB) based on the feature. Below are the further details
* AgeGAN:  
	* Category A: People with age < 20 years
	* Category B: People with age > 50 years
* RaceGAN:
	* Category A: People with White race
	* Category B: People with Black race
* GenderGAN:
	* Category A: People with Male Gender
	* Category B: People with Female Gender

## Network Architecture
![Cyclic GAN](https://github.com/prateekgulati/EIP/blob/master/Session7/Images/cyclicGanArchitecture.jpg)

### Generator
Each generator consists of 4-6 downsampling layer (conv2D &rarr; Activation &rarr;Instance/Channel Normalization) followed by 4-6 upsampling layers (UpSampling2D &rarr; Conv2D &rarr; Dropout &rarr; Instance/Channel Normalization &rarr; Concatenation) followed by a final conv2D with tanh Activation

### Discriminator
Each discrimator consists of 4-5 d_layers (Conv2D &rarr; Activation &rarr; Instance/Channel Normaization) followed by a Conv2D with single output. 

## Training
To train the network, first discriminator is trained by sending a real image from the dataset and a fake image from the generator one by one and computing the losses. Loss from both the discriminator is averaged as the total discriminator loss. Then both the generators and the discriminators are trained together and this loss is computed as generator loss.
Multiple Experiments are conducted by tweaking the parameters and then best 2-3 models are selected for each feature. The notebook in this repository contains the best selected models for each feature and the results are compared.
```
Due to lack of time and architecture, models have been trained on Google Colab using different accounts. The best models are saved and loaded in the main Notebook
```
## Results
### Model Comparision
#### RaceGAN
<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelComparisionRace.PNG" alt="Age GAN Model Comparision" width="60%" height="60%">

**Conclusion**: 
* White &rarr; Black: Apart from changing the skin tone, the faces seem to have increased warmth and a slight pale apperance. If the person has wore some makeup, that is also reduced. But the sharpness of image stays intact. 
* Black &rarr; White: The model makes the skin tone lighter and adds an overall pink hue.  
*Model 2 produces slightly better results than model 1*

#### AgeGAN
<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelComparisionAge.PNG" alt="Age GAN Model Comparision" width="50%" height="50%">

**Conclusion**: 
* Young &rarr; Old: The model adds some noise on the darker areas of the face giving an appearance of wrinkles. It also decreases the brightness and makes the overall face look dull. 
* Old &rarr; Young: A smoothened skin with reduced wrinkles and enlargement of pupils is observed. Furthermore in an attempt to remove the eye glasses, it ends up reducing the shadow effect of glasses on face.  
*Model 2 seems to have better results*
#### GenderGAN
<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelComparisionGender.PNG" alt="Gender GAN Model Comparision" width="50%" height="50%">

**Conclusion**: 
* Female &rarr; Male: The model adds a subtle beard/moustache to the face. It also reduces some blush on the face and makes the overall face look slighly dull
* Male &rarr; Female: Slight brightening of face and smoothened skin is observed. Sometimes, a pink shade of lipstick is added to the lips, eyebrows are darkened and defined and an eyeshadow is added.  
Despite of an attempt to add eclectic features to the face, this model has the most unsatisfactory results out of the three. The probable reason being on transforming the gender there needs to be a slight change in the *shape* of the face in which the model doesn't suffice.   
*Out of the two, Model 1 seems to have better results.*

### Combining Models
#### One (out of 3) at a time
##### RaceGAN
<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/TranslationReconstructionRace.PNG" alt="Translation and Reconstruction" width="50%" height="50%">
##### GenderGAN
<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/TranslationReconstructionGender.PNG" alt="Translation and Reconstruction" width="50%" height="50%">
##### AgeGAN
<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/TranslationReconstructionAge.PNG" alt="Translation and Reconstruction" width="50%" height="50%">

#### Two (out of 3) at a time
<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelCombination2%20%281%29.PNG" width="60%" height="60%">
 
<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelCombination2%20%282%29.PNG" width="60%" height="60%">

#### Three (out of 3) at a time
<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelCombination3%20%281%29.PNG" width="60%" height="60%">

<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelCombination3%20%282%29.PNG" width="60%" height="60%">

<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelCombination3%20%283%29.PNG" width="60%" height="60%">

<img src="https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelCombination3%20%284%29.PNG" width="60%" height="60%">

### ImageGallery

![Gallery 1](https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelCombination3Gallary%20%281%29.png)
![Gallery 2](https://github.com/prateekgulati/EIP/blob/master/Session7/Images/ModelCombination3Gallary%20%282%29.png)

---

**Other References**
* [MingwangLin cyclegan-keras](https://github.com/MingwangLin/cyclegan-keras/blob/master/CycleGAN-keras.ipynb)
* [Understanding and Implementing CycleGAN in TensorFlow](https://hardikbansal.github.io/CycleGANBlog/)
* [Face Aging Using Conditional GANs](https://medium.com/towards-artificial-intelligence/face-aging-using-conditional-gans-an-introduction-to-age-cgans-machine-learning-8a4a6a100201)
*  [jiechen2358's FaceAging-by-cycleGAN](https://github.com/jiechen2358/FaceAging-by-cycleGAN)
* [age-gender-estimation](https://github.com/yu4u/age-gender-estimation)
* [hyunbo9's yonsei](https://github.com/hyunbo9/yonsei)


 [**Link for Colab File**](https://colab.research.google.com/drive/1IZsI2MLZqZBJkvvXz0JwS644DEKIbnvp)
