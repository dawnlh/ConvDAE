# ConvDAE: convolutional denoising autoencoder
[This repository](https://github.com/dawnlh/ConvDAE) contains self-implemented codes for convolutional denoising autoencoders. There are two different models, but all of them have a encoder-decoder basic structure. These models can work in supervised mode and unsupervised mode. For the unsupervised mode, the unsupervised strategy is to use a random binary mask to corrupt the input, and than use the corrupted input and the original input as a pair of training data to train the network  (see denoising autoencoder for details).

By: [Zhihong Zhang](https://github.com/dawnlh), [z_zhi_hong@163.com](mailto:z_zhi_hong@163.com)

## 1. Environments

- python 3.x
- TensorFlow 1.13.1
- numpy, matplotlib, scikit-image, scipy


## 2. Networks Structures

- bsc-ConvDAE: 

  <div align=center><img width="800" height="350" src="https://github.com/dawnlh/ConvDAE/blob/master/pipeline/L-ConvDAE.png?raw=true"/></div>

- U-ConvDAE: 

    <div align=center><img width="800" height="800" src="https://github.com/dawnlh/ConvDAE/blob/master/pipeline/U-ConvDAE.png?raw=true"/></div> 


## 3. Directories:

* logs: training logs
* model_data: trained models
* dataset: datasets
* demo: demo results
* predict_res: prediction results
* pipeline: structures of networks
* bsc-ConvDAE.py, U-ConvDAE.py: 2 types of network structures
* ConvDAE_pred.py: prediction codes (based on trained models)
* ConvDAE_finetune.py: finetune condes (based on trained models)
* util: utility codes



## 4. Usage

- Download the following dataset (or create your own dataset) and put them into this directory:
  - N_MNIST_pic url：https://pan.baidu.com/s/1RcZ5JE-0F5e4EQRCL5fgIQ code：1111
  - single_molecule_localization url：https://pan.baidu.com/s/14zLWIsZxchD_ZUpjuy5Q6A  code：1111
- Modify corresponding parameters like path, epoches, training modes and so on according to your needs, and the trained models will be saved in the directory of "model_data"
- To use the trained models for prediction,  modify and run the "ConvDAE_pred.py", the results will be saved in the directory of "pred_res"
- To use the trained models for further finetuning,  modify and run the "ConvDAE_finetune.py", new models will be saved in the directory of "model_data"



## 5. Demo Results

![image](https://github.com/dawnlh/ConvDAE/blob/master/demo/unsup-ConvDAE_unsup_dots.png?raw=true)

![unsup](https://github.com/dawnlh/ConvDAE/blob/master/demo/sup-U-ConvDAE_N-MNIST-PIC.png?raw=true)

![unsup](https://github.com/dawnlh/ConvDAE/blob/master/demo/unsup-ConvDAE_unsup_N-MNIST-PIC.png?raw=true)



## 6. Notes

- **About the skip connection**: In practice, in the unsupervised mode, bsc-ConvDAE do better. And U-ConvDAE has relatively better performance in the supervised mode. This is probably because the skip connections transfer the input directly to the output in the U-net structure, which makes the mask(corruption) operation in the unsupervised mode fail to take effect. As the unsupervised mode mainly depend on the encoder-decoder process to eliminate the noise. The solution which may relieve this phenomena is to deprecate the long range skip connections that connects the input and output directly, and just keep the inner short ones; or don't use any skip connection, i.e. the bsc-ConvDAE.


- **About the blur effect**: Sometimes, the result of the unsupervised network may seem blurry, This is probably because the size of the inner layers of the network is too small, which means the encoder compress the information too much. In addition, the data itself can also have a great effect on the network result, and it's a hard problem to solve.
- **About the dropout layer**: In some cases, dropout layer will decrease the denoising performance, just comment these dropout layers to in this case.
