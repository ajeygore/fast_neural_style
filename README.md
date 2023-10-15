# fast-neural-style :city_sunrise: :rocket:

Last Update Time: 31-Jan-2021

This repository contains a pytorch implementation of an algorithm for artistic style transfer. The algorithm can be used to mix the content of an image with the style of another image. For example, here is a photograph of a door arch rendered in the style of a stained glass painting.

##Some links that helped it run on Apple silicon
* https://github.com/NVIDIA/flownet2-pytorch/issues/113
* https://developer.apple.com/metal/pytorch/
* https://pytorch.org/get-started/locally/ - to get appropriate installer for tensorflow.
* Working through MPS backend https://pytorch.org/docs/stable/notes/mps.html
* CUDA equivalent MPS Functions available for us https://pytorch.org/docs/stable/mps.html
* When there is "No MPS Backend complain" https://github.com/pytorch/pytorch/issues/100165
* Some reading about transforms https://pytorch.org/vision/0.15/transforms.html
* If there is tensor size mismatch because of aspect ratio issues https://github.com/huggingface/transformers/issues/12817
* Reading MobileNet https://github.com/AyeshaShafique/neural-style-transfer-mobilenet
* Neural Style on TensorFlow - Old Article https://medium.com/@aieeshashafique/neural-image-styling-in-tensorflow-509c9561771d
* Combining this for Video https://github.com/eriklindernoren/Fast-Neural-Style-Transfer
* https://github.com/cuicaihao/fast_neural_style/blob/master/README.md
* Fast Style https://github.com/cryu854/FastStyle


## Requirement:

Development Environment:

- Python=3.8.3
- PyTorch=1.7.0 (2021)
-

Thanks [mratsim](https://github.com/zhanghang1989/PyTorch-Multi-Style-Transfer/issues/21#issuecomment-396075362)'s solution, I updated the repo so it runs well with the most recent PyTorch version.

Note the original repo used early PyTorch version. It may cause the issues with the model.

- PyTorch=0.30 in 2017 (original / deprecated).

The model uses the method described in [Perceptual Losses for Real-Time Style Transfer and Super-Resolution](https://arxiv.org/abs/1603.08155) along with [Instance Normalization](https://arxiv.org/pdf/1607.08022.pdf). The saved-models for examples shown in the README can be downloaded from [here](https://www.dropbox.com/s/lrvwfehqdcxoza8/saved_models.zip?dl=0).

## Requirements

The program is written in Python, and uses [pytorch](http://pytorch.org/), [scipy](https://www.scipy.org). A GPU is not necessary, but can provide a significant speed up especially for training a new model. Regular sized images can be styled on a laptop or desktop using saved models.

```bash
bash Demo.sh # check the sample.
```

or use pretrained models by editing 'test.sh'

```bash
bash Test.sh
```

## Usage

Stylize image

```
python neural_style/neural_style.py eval --content-image </path/to/content/image> --model </path/to/saved/model> --output-image </path/to/output/image> --cuda 0
```

- `--content-image`: path to content image you want to stylize.
- `--model`: saved model to be used for stylizing the image (eg: `mosaic.pth`)
- `--output-image`: path for saving the output image.
- `--content-scale`: factor for scaling down the content image if memory is an issue (eg: value of 2 will halve the height and width of content-image)
- `--cuda`: set it to 1 for running on GPU, 0 for CPU.

Train model

```bash
python neural_style/neural_style.py train --dataset </path/to/train-dataset> --style-image </path/to/style/image> --save-model-dir </path/to/save-model/folder> --epochs 2 --cuda 1
```

There are several command line arguments, the important ones are listed below

- `--dataset`: path to training dataset, the path should point to a folder containing another folder with all the training images. I used COCO 2014 Training images dataset [80K/13GB] [(download)](http://mscoco.org/dataset/#download).
- `--style-image`: path to style-image.
- `--save-model-dir`: path to folder where trained model will be saved.
- `--cuda`: set it to 1 for running on GPU, 0 for CPU.

Refer to `neural_style/neural_style.py` for other command line arguments. For training new models you might have to tune the values of `--content-weight` and `--style-weight`. The mosaic style model shown above was trained with `--content-weight 1e5` and `--style-weight 1e10`. The remaining 3 models were also trained with similar order of weight parameters with slight variation in the `--style-weight` (`5e10` or `1e11`).

## Models

Models for the examples shown below can be downloaded from [here](https://www.dropbox.com/s/lrvwfehqdcxoza8/saved_models.zip?dl=0) or by running the script `download_saved_models.sh`.

### Input

<div align='center'>
  <img src='images/content-images/latrobe.jpg' height="174px">		
</div>

### Style

<div align='center'>
  <img src='images/style-images/mosaic.jpg' height="174px">
  <img src='images/style-images/candy.jpg' height="174px">
  <img src='images/style-images/rain-princess-cropped.jpg' height="174px">
   <img src='images/style-images/udnie.jpg' height="174px"> 
</div>

### Output

<div align='center'>
  <img src='images/output-images/latrobe-mosaic.jpg' height="174px">
  <img src='images/output-images/latrobe-candy.jpg' height="174px">  
  <img src='images/output-images/latrobe-rain-princess.jpg' height="174px">
  <img src='images/output-images/latrobe-udnie.jpg' height="174px">
</div>
