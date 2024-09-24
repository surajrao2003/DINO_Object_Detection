
# DINO Object Detection on custom dataset
Task - Training the DINO object detection model on a pedestrian dataset consisting of 200 images collected within the IIT Delhi campus. The dataset, annotated in COCO format, includes both images and corresponding annotations in JSON format (link provided below).
https://drive.google.com/drive/folders/1DCpmo919b7OrAng9clEbiMHjO3D0hyoa?usp=sharing


Steps to Run - 

1) First upload the dataset (link) on your drive and add the json file (random_sample_mavi_2_gt.json) into the directory containing the 200 images.  

2) Download the code_file.ipynb and upload the notebook on google colab. Make sure to use GPU runtime. 

3) Mount google drive and clone github repo (link given in notebook). Then install pytorch, torchvision and other requirements. Next compile cuda operations (this step is important). If you face any errors of missing modules like ''MultiScaleDeformableAttention', make sure to reexecute this cell and then restart the session.

4) Organize the data in COCO format
```
   COCODIR/
  ├── train2017/
  ├── val2017/
  └── annotations/
  	├── instances_train2017.json
  	└── instances_val2017.json
   ```
5) Download DINO model checkpoint "checkpoint0011_4scale.pth" (pre-trained DINO-4scale model with the ResNet-50 (R50) backbone) from the link - https://drive.google.com/drive/folders/1qD5m1NmK0kjE5hh-G17XUX751WsEG-h_?usp=sharing

6) Run evaluation script using pretrained model checkpoint
   ```
   bash scripts/DINO_eval.sh /path/to/your/COCODIR /path/to/your/checkpoint
   ```

7) Visualize predictions of pretrained model on validation set
   (I have modified the script provided in the repo so that we can compare ground truth images and prediction images).
   If you face some dependency errors or errors in repo files, it is due to version mismatch. Simply try changing the version (or) making the necessary changes as mentioned in the error. 

8) Finetuning the pretrained model on our custom dataset. Simply execute the script and the model will train for 12 epochs.
   ```
   !bash /content/DINO/scripts/DINO_train.sh /path/to/your/COCODIR \
    --output_dir logs/DINO/R50-MS4 \
    --config_file /content/DINO/config/DINO/DINO_4scale.py \
    --pretrain_model_path /path/to/your/checkpoint \
    --finetune_ignore label_enc.weight class_embed
    ```

9) Run evaluation script using finetuned model checkpoint
   ```
   bash scripts/DINO_eval.sh /path/to/your/COCODIR /path/to/your/checkpoint
   ```

10) Visualize predictions of the finetuned model

11) Loss graphs during fine-tuning.
    You can also make changes to the engine.py file to store the loss values in a file while the model is being trained. However, I have manually added the loss values through observation (due to limitations of colab runtime). You can use matplotlib for plotting graph.


