# EasyImg2Img
An easy to use paired dataset creation &amp; training pipeline for making your own image translation models!

## How to use
You can skip steps 0. through 2. if you already have paired data 


0. Setup [StyleGAN3](https://github.com/NVlabs/stylegan3) and make sure all their scripts are working properly.
1. Prepare your dataset and train StyleGAN2 with it.

* Dataset preparation:
``` Bash
python3 dataset_tool.py --source=/path/to/your/dataset --dest=/dest/path --resolution=256x256
```

* StyleGAN2 training (you may want to experiment with different gamma or other hyperparameters): 
``` Bash
python3 train.py --outdir=/path/to/training-runs --data=/dest/path \
  --cfg=stylegan2 --gpus=1 --batch=16 --gamma=0.8192 --glr=0.0025 --dlr=0.0025 --cbase=16384 \
  --resume=https://api.ngc.nvidia.com/v2/models/nvidia/research/stylegan2/versions/1/files/stylegan2-ffhq-256x256.pkl \
  --metrics=none --tick=1 --snap=10 --kimg=300
```
2. Use [stylegan_blending.ipynb](https://github.com/avermilov/EasyImg2Img/blob/master/stylegan-blending.ipynb) for creating your own paired dataset.
You may have to experiment a fair amount before finding the perfect blending combination, after which you can finally generate your paired training dataset.
You can also use the included Real-ESRGAN section to increase dataset image quality.
This notebook is my slight rework of @Sxela's amazing [stylegan3_blending](https://github.com/Sxela/stylegan3_blending) repo.

3. Use [train_paired.ipynb](https://github.com/avermilov/EasyImg2Img/blob/master/train-paired.ipynb) to train a fastai v1 Dynamic U-Net on your paired dataset.
You can also use it to get a JIT traced version of your model.
To set up an environment for this notebook, you can execute the following commands:
``` Bash
conda create --name easyimg2img python=3.9
conda activate easyimg2img
conda install -c pytorch -c fastai fastai=1.0.61
conda install -c anaconda ipykernel
pip install ipython_genutils
python -m ipykernel install --user --name=easyimg2img
pip install opencv-python
pip install gdown
```
4. Use [inference.ipynb](https://github.com/avermilov/EasyImg2Img/blob/master/inference.ipynb) to easily inference your model
and display and/or save the results. 
