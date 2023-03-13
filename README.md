# Vessel-Promoted OCT to OCTA Image Translation by Heuristic Contextual Constraints

This repository contains the official PyTorch implementation of the following paper:

Vessel-Promoted OCT to OCTA Image Translation by Heuristic Contextual Constraints

## Abstract
Optical Coherence Tomography Angiography (OCTA) has become increasingly vital in the clinical screening of fundus diseases due to its ability to capture accurate 3D imaging of blood vessels in a non-contact scanning manner. However, the acquisition of OCTA images remains challenging due to the requirement of exclusive sensors and expensive devices. In this paper, we propose a novel framework, TransPro, that translates 3D Optical Coherence Tomography (OCT) images into exclusive 3D OCTA images using an image translation pattern. Our main objective is to address two issues in existing image translation baselines, namely, the aimlessness in the translation process and incompleteness of the translated object. The former refers to the overall quality of the translated OCTA images being satisfactory, but the retinal vascular quality being low. The latter refers to incomplete objects in translated OCTA images due to the lack of global contexts. TransPro merges a 2D retinal vascular segmentation model and a 2D OCTA image translation model into a 3D image translation baseline for the 2D projection map projected by the translated OCTA images. The 2D retinal vascular segmentation model can improve attention to the retinal vascular, while the 2D OCTA image translation model introduces beneficial heuristic contextual information. Extensive experimental results on two challenging datasets demonstrate that TransPro can consistently outperform existing approaches with minimal computational overhead during training and none during testing.

## Overall framework
![image](https://github.com/ustlsh/TransPro/blob/main/imgs/framework.png)
## Qualitative results
![image](https://github.com/ustlsh/TransPro/blob/main/imgs/figure3.png)

## Install

- Create conda environment and activate it:
```
conda create -n octa python=3.6
conda activate octa
```
- Clone this repo:
```
git clone https://github.com/ustlsh/TransPro
cd TransPro
```
- Install requirements
```
pip install -r requirements.txt
```

## Usage
### Prepare data
We use OCTA-3M and OCTA-6M datasets in our paper. These two datasets are from OCTA-500 dataset: https://ieee-dataport.org/open-access/octa-500

### Pretrained-weights for VPG and HCG modules:
You can download them here: https://drive.google.com/drive/folders/1Jby7auI28MvYTxvXj7Mf_32j8LLw6XHU?usp=sharing.
You need to move them into "pretrain-weights" folder.

### Train 
- To view training results and loss plots, run:
```
python -m visdom.server -p 6031
```
and click the URL http://localhost:6031.

To train TransPro model on OCTA-3M dataset, e.g.:
```
python train3d.py --dataroot ./octa-500/OCT2OCTA3M_3D --name transpro_3M --model TransPro --netG unet_256 --direction AtoB --lambda_A 10 --lambda_C 5 --dataset_mode alignedoct2octa3d --norm batch --pool_size 0 --load_size 304 --input_nc 1 --output_nc 1 --display_port 6031 --gpu_ids 0 --no_flip
```

### Test
To test the model, you should find the best training epoch (e.g., 164) in validation set from the saved "loss_log.txt" file, and then run:
```
python test3d.py --dataroot ./octa-500/OCT2OCTA3M_3D --name transpro_3M --test_name transpro_3M --model TransPro --netG unet_256 --direction AtoB --lambda_A 10 --lambda_C 5 --dataset_mode alignedoct2octa3d --norm batch --input_nc 1 --output_nc 1 --gpu_ids 0 --num_test 15200 --which_epoch 164 --load_iter 164
```

## Citation
If our paper is useful for your research, please cite our paper
## Implement references
[CycleGAN and pix2pix in PyTorch](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix)
