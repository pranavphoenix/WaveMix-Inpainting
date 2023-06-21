# WaveMix-Inpainting
 
  
### Create Datasets

For creating validation/test datasets, the masks have to be pre-generated. 
The following code will look into a folder having all test images and create a new folder with all images and their generated masks.
```
git clone https://github.com/advimman/lama.git
cd lama
# Make sure you are in lama folder
export TORCH_HOME=$(pwd) && export PYTHONPATH=$(pwd)
python3 bin/gen_mask_dataset.py \
$(pwd)/configs/data_gen/random_medium_224.yaml Path/to/folder/containing/Images \
Path/to/new/folder/ --ext JPEG  

#change ext based on image formats
#edit the yaml file in configs/data_gen to generate masks according to need
```





### Model Architecture 

![image](https://github.com/remag2069/WaveMix-Inpainting/blob/main/resources/model.png)

Image inpainting refers to the synthesis of missing regions in an image, which can help restore occluded or degraded areas and also serve as a precursor task for self-supervision. The current state-of-the-art models for image inpainting are computationally heavy as they are based on vision transformer backbones in adversarial or diffusion settings. This paper diverges from vision transformers by using a computationally-efficient WaveMix-based fully convolutional architecture, which uses a 2D-discrete wavelet transform (DWT) for spatial and multi-resolution token-mixing along with convolutional layers. The proposed model outperforms the current-state-of-the-art models for large mask inpainting on reconstruction quality while also using less than half the parameter count and considerably lower training and evaluation times. 


## Parameters <!-- Have to change -->

- `num_classes`: int.  
Number of classes to classify/segment.
- `depth`: int.  
Number of WaveMix blocks.
- `mult`: int.  
Expansion of channels in the MLP (FeedForward) layer. 
- `ff_channel`: int.  
No. of output channels from the MLP (FeedForward) layer. 
- `final_dim`: int.  
Final dimension of output tensor after initial Conv layers. Channel dimension when tensor is fed to WaveBlocks.
- `dropout`: float between `[0, 1]`, default `0.`.  
Dropout rate. 
- `level`: int.  
Number of levels of 2D wavelet transform to be used in Waveblocks. Currently supports levels from 1 to 4.
- `stride`: int.  
Stride used in the initial convolutional layers to reduce the input resolution before being fed to Waveblocks. 
- `initial_conv`: str.  
Deciding between strided convolution or patchifying convolutions in the intial conv layer. Used for classification. 'pachify' or 'strided'.
- `patch_size`: int.  
Size of each non-overlaping patch in case of patchifying convolution. Should be a multiple of 4.
- `num_models`: int.  
Number of consecutive models each consisting of wavemix modules with defined `depth` and additional convolution and deconvolution layers, total number of wavemix modules is the product of num_models and depth.


#### Cite the following papers 
```
@misc{
kumar2023resource,
title={Resource efficient image inpainting},
author={Dharshan Sampath Kumar and Pranav Jeevan P and Amit Sethi},
year={2023},
url={https://openreview.net/forum?id=OJILbuOodvm}
}


@misc{jeevan2023wavemix,
      title={WaveMix: A Resource-efficient Neural Network for Image Analysis}, 
      author={Pranav Jeevan and Kavitha Viswanathan and Anandu A S and Amit Sethi},
      year={2023},
      eprint={2205.14375},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}


``` 
