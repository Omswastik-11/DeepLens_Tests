# DeepLense_Tests
This is my personal repository to submit my solutions to the test problems provided by ML4SCI as a selection test for GSOC'25.

<img src="https://github.com/user-attachments/assets/cf60146c-84d2-478a-b3fa-bb46af2d1d29" width="500">

# Dataset generation 

<img src="https://github.com/user-attachments/assets/806e2e52-12d7-4f34-b057-3f110685ad98" width="700">


As given in Image the dataset contains images of three class **"no"** ,**"spherical"**, **"vortex"** substructures having 10000 training image and 2500 testing images for each class . I introduced data Augumantation technique which decreased models training accuracy overall predicting accuracy of moel is increased. It made the dataset more tough . \
We applied the following transformations to preprocess the dataset:

**Resize**: Scales images to 150×150 pixels.\
**Random Horizontal Flip**: Randomly flips images horizontally with a probability of 50%.\
**Random Vertical Flip**: Randomly flips images vertically with a probability of 50%.\
**Random Rotation**: Rotates images randomly up to ±20 degrees.\
**Normalize**: Normalizes pixel values to have a mean of 0.5 and a standard deviation of 0.5.

# Results of Common Task-1 (Multi-class classification)

On this dataset after 50 epochs the model performed very well with atraing accuracy of **95.01 %** and traing accuracy of **94.80 %** . 

   **Traing Result** 
   
<img src="https://github.com/user-attachments/assets/ce4ca5ff-1581-4929-9f8b-6c79f32aee4d" width="400">\

   **Testing Result** 
   
<img src="https://github.com/user-attachments/assets/078cd380-ea1e-4a90-9bba-1844febe3824" width="400">\

# Approach for Task-1 
I used three Architectures **ResNet-18**,**EfficientNetB0**,**DensNet** and trained each for 50 epochs on the dataset. 
| Model       | Accuracy       | No of Parameters | Computation time | ROC AUC score | Colab Notbook |
|-------------|----------------|------------------|------------------|--------------|----------------|
| EfficientNetB0 | **94.80%**  | 4010815 | ~100min|**0.9895**| [Link](https://colab.research.google.com/drive/1pxrmuU434VbkeK4apBZstuaJO_ku_QRy?usp=sharing)|
| ResNet18    | 90.73%     |11171779 |**~75min**|0.9779| [Link](https://colab.research.google.com/drive/1JSXTJS_ukad1oMo3xxu4FArPDyhl9t1_?usp=sharing)|
| DenseNet    | ~91.88%    |**930835**|~150min|0.9806|[Link](https://colab.research.google.com/drive/1z1P38Io4caidwofF98U22G7hYqlSWM0Z?usp=sharing)|

# Specific test - 5 (Physics-Informed Neural Network)
   **Approach-1**
   
<img src="https://github.com/user-attachments/assets/fecc4ddd-5fdd-4765-9a18-c1124fcec14c" width="400">\


   **Approach-2**

<img src="https://github.com/user-attachments/assets/d345ec75-46a8-4aa9-addf-23e2118563fb" width="400">\

  | Model       | Accuracy       | No of Parameters | Computation time | ROC AUC score | Colab Notbook | Weights |
|-------------|----------------|------------------|------------------|--------------|----------------|------|
| Approach - 1 | **94.55%**  | 36840447 | ~180min|**0.9891**| [Link](https://colab.research.google.com/drive/1aplJWCqjJJszs8tE4zaKHqfPBAf5AsQf?usp=sharing)|[Link](https://drive.google.com/file/d/1FSgugktUxbpdBgjCVA_IssjX4soGp8ES/view?usp=sharing)|
| Approach - 2   | 93.19%     |9876655 |**~130min**|0.9849| [Link](https://colab.research.google.com/drive/1-tF5CXP7Gjab3rrH54FQ-GyiTWhKtpgt?usp=sharing)|[Link](https://drive.google.com/file/d/1U7oAUn6y3Qx7HdxPIwPsp3m18C6GuL8J/view?usp=sharing)|

# Approach - 1 

I used two EfficientNet networks—one as an encoder and another as a classifier.

**Encoder** 
1. Architecture :- It is EfficeintNetB0 architecture. <br>
2. Input :- It takes single channel(gray-scale) image of lens image. <br>
3. Output :- It produces Potential value for each coordinate. <br>

**Classifier**
1. Architecture :- It is EfficeintNetB0 architecture. <br>
2. Input :- It takes 2 channel(both lense and source image) image. <br>
3. Output :- It produces probability of class <br>

**Physics used**

To Reconstruct the Source image, we use the equation for gravitational lensing, in its dimensionless form, that is given by the following relation:

$$
\begin{align}
\vec{\mathcal{S}} = \vec{\mathcal{I}} - \vec{\nabla} \Psi (\vec{\mathcal{I}})
\end{align}
$$


$$
\begin{align}
Ψ(x_i,y_i) = Ψ_{Galaxy}(x_i,y_i) + Ψ_{DarkMatter}(x_i,y_i)
\end{align}
$$

The distortions caused by dark matter are localized and can be ignored in most of the formed image. Therefore, we will approximate:

$$
\begin{align}
Ψ(x_i,y_i) \approx Ψ_{Galaxy}(x_i,y_i)
\end{align}
$$

$$
\begin{align}
\boxed{Ψ(x_i,y_i) \approx k(x_i,y_i) \cdot \sqrt{x_i^2+y_i^2}}
\end{align}
$$

from encoder we got $$\ k(x_i,y_i)$$ which is then multiplied with position matrix to make deflection . The deflection is subtracted from the lens image to obtain the source image. Finally, the lens image and source image are concatenated and passed to the classifier, which outputs the probability for each class. ⚡️


# Approach - 2

In this approach, I used image **preprocessing** and a **converter** to transform the lens image into the source image using deflection and gradient terms (referenced from last year's DeepLense repository).

**Preprocess**

<img src="https://github.com/user-attachments/assets/9fe9abfd-2130-472e-abcf-052ff4f3a668" width="700">\

**Converter**

<img src="https://github.com/user-attachments/assets/c5e5f422-2cc3-4047-8b21-1d5bece8ba90" width="500">\


I used encoder-decoder architecture.

**Encoder**
1. Architecture:- ResNet
2. Input:- It takes single channel(gray-scale) image of lens image. <br>
3. Output :- It produces Potential value for each coordinate. <br>

**Decoder**
1. Architecture:- EfficientB0
2. Input:- It takes 3 channel image (source image,Lens image,preprocessed image) .
3. Output:- It produces probabilities of 3 classes . 








   


