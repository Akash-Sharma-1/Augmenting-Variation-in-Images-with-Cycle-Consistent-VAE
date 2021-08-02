# Disentangling-Factors-of-Variation-in-Images-with-Cycle-Consistent-VAE

## About

Implementation as well as few qualitatitive experiments for the state-of-the-art variational autoencoder technique of "Disentanglement Cycle-consistent VAE" on the synthetic caricatures dataset - 2D Sprites. 

Dataset Link - http://www-personal.umich.edu/~reedscot/files/nips2015-analogy-data.tar.gz


## Implementation of the model


**_Architecture_**

Forward Cycle of cycle consistent variational autoencoder

Backward Cycle of cycle consistent variational  autoencoder


_(Implemented Cycle consistent variational autoencoder to disentangle representations)_

The implementation is based upon the ideas and model used in the paper[^1]discussing of using a novel architecture made of cycle consistent VAE framework to disentangle representations.



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




## Style Transfer Grids

Generated an arrangement of style transfer grid from 8 sprites on the topmost row and another 8 sprites on the leftmost column side. These images were taken at random from the test set.

Each image is generated by taking the specified factor _[Identity]_ (after encoding) from the corresponding row sprite leftmost column and unspecified factor _[Style] _(after encoding) from the corresponding column sprite in the topmost row. After this both the factors were decoded to generate the image.



## Linear Interpolation

In each of the linear interpolation images, the bottom right and upper left contains the image pair taken from the test set.

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


#### **_Reconstruction of images and comparison _**

```
Regeneration error: 0.0002293549173552416
```



(L2 Loss between both the images - original and generated from the network)






Original Batch of  Images




Generated Batch of Images by the predictor
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



#### **_Reconstruction of images and comparison_**


```
Regeneration error: 0.0053721513362861844
```

(L2 Loss between both the images - original and generated from the network)




Original Batch of  Images

Generated Batch of  Images by the predictor
<p>
<em>(Misclassifications are marked with a white underline.)</em>



#### **_Analysis of the comparison:_**

As we see from comparing the both the regeneration losses, we see that the prediction network for z to s has higher loss than s to z indicating that there is a bigger difference in the generated image and original image in z to s network than the s to z prediction network. This might mean that in the former case we lose the identity factor of variation of the sprites because we have the style related factors only and hence,regenerating the identity of the character could be difficult to train. 


## Citations 
[^1]: Jha, A. H., Anand, S., Singh, M., & Veeravasarapu, V. S. R. (2018). Disentangling Factors of Variation with Cycle-Consistent Variational Auto-Encoders. _ArXiv:1804.10469 [Cs]_.[ http://arxiv.org/abs/1804.10469](http://arxiv.org/abs/1804.10469)
