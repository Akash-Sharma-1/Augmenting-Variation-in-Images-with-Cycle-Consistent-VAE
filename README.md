# Disentangling-Factors-of-Variation-in-Images-with-Cycle-Consistent-VAE

## About

Re - Implementation as well as few qualitatitive experiments for the state-of-the-art variational autoencoder technique of "Disentangling Factors of Variation with Cycle-Consistent Variational Auto-Encoders" on the synthetic caricatures dataset - 2D Sprites. 

- **Dataset Link** - http://www-personal.umich.edu/~reedscot/files/nips2015-analogy-data.tar.gz

- **Paper Link** - http://arxiv.org/abs/1804.10469

- **Original Repo Link** - https://github.com/ananyahjha93/cycle-consistent-vae


## Implementation of the model


**_Architecture_**

Forward Cycle of cycle consistent variational autoencoder

![image](https://user-images.githubusercontent.com/33986563/127881959-e247c685-6370-4786-b93a-109395279576.png)


Backward Cycle of cycle consistent variational  autoencoder

![image](https://user-images.githubusercontent.com/33986563/127881981-b9b0d5fc-54dd-4373-92df-a5a7e0e9f34c.png)


_(Implemented Cycle consistent variational autoencoder to disentangle representations)_

The implementation is based upon the ideas and model used in the paper [^1] discussing of using a novel architecture made of cycle consistent VAE framework to disentangle representations.



  **_Training Hyperparameters_**


```
image_size=60
num_channels=3 
initial_learning_rate=0.0001
style_dim=512
class_dim=512
num_classes=672
reconstruction_coef=2
reverse_cycle_coef=10
kl_divergence_coef=3
beta_1=0.9
beta_2=0.999
epochs=150
batch_Size=64
```

**_Loss Values after Training_**


```
reconstruction_loss: 0.0032943749532317944    
kl_div_loss: 3.2242015996096124e-05  
reverse_cycle_loss: 5.96668045619013e-08
```

**_Loss Plots of Training_**

![image](https://user-images.githubusercontent.com/33986563/127882109-a175b518-f9fa-40e9-8c3a-377690692f88.png)
![image](https://user-images.githubusercontent.com/33986563/127882119-84c39951-bf64-4b7d-9b77-0443ba0c2af5.png)
![image](https://user-images.githubusercontent.com/33986563/127882136-2ac2734a-3249-42f1-b331-c7d611405593.png)




## Style Transfer Grids

Generated an arrangement of style transfer grid from 8 sprites on the topmost row and another 8 sprites on the leftmost column side. These images were taken at random from the test set.

Each image is generated by taking the specified factor _[Identity]_ (after encoding) from the corresponding row sprite leftmost column and unspecified factor _[Style]_ (after encoding) from the corresponding column sprite in the topmost row. After this both the factors were decoded to generate the image.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![image](https://user-images.githubusercontent.com/33986563/127882172-0681ea0e-3ebd-49f2-90f7-28f81113da20.png)

![image](https://user-images.githubusercontent.com/33986563/127882181-af153347-f052-437a-b183-bec7b1092d92.png)
![image](https://user-images.githubusercontent.com/33986563/127882189-45fcc7a5-e39f-4a78-ab42-09d600ef2079.png)



## Linear Interpolation

In each of the linear interpolation images, the bottom right and upper left contains the image pair taken from the test set.

![image](https://user-images.githubusercontent.com/33986563/127882341-734423ea-1e41-41bf-a54c-8cb941993bed.png)

![image](https://user-images.githubusercontent.com/33986563/127882356-4c6988d3-a9d6-4b6e-bd60-35957843d2f2.png)

![image](https://user-images.githubusercontent.com/33986563/127882376-59839085-d9e6-4b62-9a91-5db9373476fc.png)


**_Qualitative analysis_**


* The model is able to combine style (unspecified factors) and identity (specified factors) quite decently and smoothly. 
* Style(z) (unspecified factor) remains constant across columns and Identity(s) (specified factor) remains same across the rows. Hence, showing the effect of linear interpolation in the latent spaces.There is no direct transfer of viewpoints.
* Also shows the model’s capability of conditional image generation like conditioning on style(z) and generating/sampling for identity (s).


## Classifiers for Specified/unspecified Partition of Latent Space

1. **Specified partition of latent space classifier (s)**


#### **_Classifier Description_**


```
Classifier(
  (fc1): Linear(in_features=512, out_features=256, bias=True)
  (batch_norm1): BatchNorm1d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (leaky_relu1): LeakyReLU(negative_slope=0.2, inplace=True)
  (fc2): Linear(in_features=256, out_features=256, bias=True)
  (batch_norm2): BatchNorm1d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (leaky_relu2): LeakyReLU(negative_slope=0.2, inplace=True)
  (fc3): Linear(in_features=256, out_features=672, bias=True)
)
Total_params:370848
```



#### **_Classifier loss and accuracy (after 10 epochs)_**


```
Training loss: 0.0007626940768845042 
Training acc: 0.8257629946043155  
Validation Loss: 0.001116836695571048  
Validation Acc: 0.7698943345323741
```



#### **_Classifier loss and accuracy plots (after 10 epochs)_**

![image](https://user-images.githubusercontent.com/33986563/127882410-747b9bef-aa4f-401c-b4b4-5aefa745b844.png)

![image](https://user-images.githubusercontent.com/33986563/127882432-bc9b2c8d-8f74-4b7e-995e-dd1dcbe79c6a.png)


2. **Unspecified partition of latent space classifier (z)**


#### **_Classifier Description_**

```
Classifier(
  (fc1): Linear(in_features=512, out_features=256, bias=True)
  (batch_norm1): BatchNorm1d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (leaky_relu1): LeakyReLU(negative_slope=0.2, inplace=True)
  (fc2): Linear(in_features=256, out_features=256, bias=True)
  (batch_norm2): BatchNorm1d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (leaky_relu2): LeakyReLU(negative_slope=0.2, inplace=True)
  (fc3): Linear(in_features=256, out_features=672, bias=True)
)
total_params:370848
```



#### **_Classifier loss and accuracy (after 10 epochs)_**


```
Training loss: 0.010253216525451 
Training acc: 0.19034742334943475  
Validation Loss: 0.0106446436234552  
Validation Acc: 0.15981851079136696
```


	


#### **_Classifier loss and accuracy plots (after 10 epochs)_**

![image](https://user-images.githubusercontent.com/33986563/127882477-9e3fd4bd-4bbf-4d5e-bcb1-23a737a8e2d5.png)

![image](https://user-images.githubusercontent.com/33986563/127882500-a6776fea-4a34-4b0a-a50f-2283704866ac.png)



#### **_Analysis of the comparison:_**

The classification accuracies for the specified latent space (s) as well as the unspecified latent space (z) can be also seen as an indicator of specified factors of variation present in it while classifying the labels . In this case of disentangled representations, the unspecified latent space (z) since has very few amount of specified factors of variations, it should have low classification accuracy. While, specified latent space has a higher specified factors of variations, it should have a higher classification accuracy.


## Prediction Network



### **Specified(s) to Unspecified(z) - prediction network**


#### **_Predictor Description_**
```
Predictor(
  (fc1): Linear(in_features=512, out_features=256, bias=True)
  (batch_norm1): BatchNorm1d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (fc2): Linear(in_features=256, out_features=256, bias=True)
  (batch_norm2): BatchNorm1d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (fc3): Linear(in_features=256, out_features=512, bias=True)
)
Total_params:329728
```



#### **_Predictor Losses (after  20 Epochs)_**


```
Training loss: 0.0008676905426190053    
Validation Loss: 0.0006904151666094549
```



#### **_Predictor loss plots (after 20 epochs)_**

![image](https://user-images.githubusercontent.com/33986563/127882540-8028e9f1-4294-44d4-b214-985d753209a2.png)


#### **_Reconstruction of images and comparison _**

```
Regeneration error: 0.0002293549173552416
```



(L2 Loss between both the images - original and generated from the network)






Original Batch of  Images

![image](https://user-images.githubusercontent.com/33986563/127882555-dc1fe478-13d4-4a11-8458-dc2a7bb0e51a.png)



Generated Batch of Images by the predictor

![image](https://user-images.githubusercontent.com/33986563/127882572-a7a25413-eba0-4a8b-880b-88c4d4782e06.png)

<em>(Misclassifications are marked with a white underline.)</em>




### **Unspecified(z) to Specified(s) - prediction network**


#### **_Predictor Description_**


```
Predictor(
  (fc1): Linear(in_features=512, out_features=256, bias=True)
  (batch_norm1): BatchNorm1d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (fc2): Linear(in_features=256, out_features=256, bias=True)
  (batch_norm2): BatchNorm1d(256, eps=1e-05, momentum=0.1, affine=True, track_running_stats=True)
  (fc3): Linear(in_features=256, out_features=512, bias=True)
)
Total_params:329728
```



#### **_Predictor Losses (after  20 Epochs)_**


```
Training loss: 0.0001754050121524301
Validation Loss: 0.000152734847042011
```



#### **_Predictor loss plots (after 20 epochs)_**

![image](https://user-images.githubusercontent.com/33986563/127882632-f5e012a4-9ceb-47a8-a7d2-e71373c7caf9.png)


#### **_Reconstruction of images and comparison_**


```
Regeneration error: 0.0053721513362861844
```

(L2 Loss between both the images - original and generated from the network)




Original Batch of  Images

![image](https://user-images.githubusercontent.com/33986563/127882647-096f13e0-8018-4da6-8693-37ef76dc39d4.png)


Generated Batch of  Images by the predictor

![image](https://user-images.githubusercontent.com/33986563/127882707-e7929158-bd3c-491e-b98e-e7936fa2a532.png)

<em>(Misclassifications are marked with a white underline.)</em>




#### **_Analysis of the comparison:_**

As we see from comparing the both the regeneration losses, we see that the prediction network for z to s has higher loss than s to z indicating that there is a bigger difference in the generated image and original image in z to s network than the s to z prediction network. This might mean that in the former case we lose the identity factor of variation of the sprites because we have the style related factors only and hence,regenerating the identity of the character could be difficult to train. 


## Citations 
[^1]: Jha, A. H., Anand, S., Singh, M., & Veeravasarapu, V. S. R. (2018). Disentangling Factors of Variation with Cycle-Consistent Variational Auto-Encoders. _ArXiv:1804.10469 [Cs]_.[ http://arxiv.org/abs/1804.10469](http://arxiv.org/abs/1804.10469)
