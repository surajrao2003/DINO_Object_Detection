# DINO Object Detection on Custom Dataset

**Task:**  
Training the DINO object detection model on a pedestrian dataset consisting of 200 images collected within the IIT Delhi campus. The dataset, annotated in COCO format, includes both images and corresponding annotations in JSON format.  
📂 [Dataset Link](https://drive.google.com/drive/folders/1DCpmo919b7OrAng9clEbiMHjO3D0hyoa?usp=sharing)

> For visualizing the bounding boxes on 200 images in the dataset using the JSON file, I used the script `Visualizing_bounding_boxes.ipynb` uploaded in this repo.

---

## Steps to Run

1. **Upload the dataset** (see link above) on your Drive and add the JSON file `random_sample_mavi_2_gt.json` into the directory containing the 200 images.  

2. **Download** `code_file.ipynb` and upload the notebook to Google Colab.  
Make sure to use GPU runtime.  

3. **Mount Google Drive and clone the GitHub repo** (link provided in the notebook).  
   Install PyTorch, torchvision, and other requirements.  
   Then **compile CUDA operations**.  
   > If you face any errors of missing modules like `'MultiScaleDeformableAttention'`, re-execute this cell and then restart the session.

4. **Organize the data in COCO format:**

    ```
    COCODIR/
    ├── train2017/
    ├── val2017/
    └── annotations/
        ├── instances_train2017.json
        └── instances_val2017.json
    ```

5. **Download the DINO model checkpoint** (`checkpoint0011_4scale.pth`) from the link:  
   🔗 [Pretrained DINO-4scale Model](https://drive.google.com/drive/folders/1qD5m1NmK0kjE5hh-G17XUX751WsEG-h_?usp=sharing)

6. **Run evaluation script using pretrained model checkpoint:**

    ```bash
    bash scripts/DINO_eval.sh /path/to/your/COCODIR /path/to/your/checkpoint
    ```

7. **Visualize predictions** of pretrained model on validation set.  
   (Script is modified to compare ground truth and predicted images.)  
   > If you face some dependency errors or errors in repo files, it is due to version mismatch. Simply try changing the version (or) making the necessary changes as mentioned in the error.

8. **Fine-tune the pretrained model** on our custom dataset (12 epochs).  
   🔗 [Finetuned Model Weights](https://drive.google.com/file/d/1tZPCWB_5BGTknm_Phvys9pufLIDohEJ2/view?usp=sharing)

    ```bash
    !bash /content/DINO/scripts/DINO_train.sh /path/to/your/COCODIR \
        --output_dir logs/DINO/R50-MS4 \
        --config_file /content/DINO/config/DINO/DINO_4scale.py \
        --pretrain_model_path /path/to/your/checkpoint \
        --finetune_ignore label_enc.weight class_embed
    ```

9. **Run evaluation** using the finetuned model checkpoint:

    ```bash
    bash scripts/DINO_eval.sh /path/to/your/COCODIR /path/to/your/checkpoint
    ```

10. **Visualize predictions** from the finetuned model by updating the checkpoint path in the visualization script.

11. **Loss graphs during fine-tuning:**  
    You can edit `engine.py` to store loss values during training.  
    > I manually added the loss values through observation due to Colab limitations.  
    You can use matplotlib to plot the loss graph.

---

## Documentation and Links

- 📘 [Detailed Report (Images, Loss Graphs, Steps)](https://docs.google.com/document/d/1zpFXE16yeAhsSHQSMBEzYzjQP-PoMekFxa7yM_eC5q0/edit?usp=sharing)
- 📥 [Pretrained Model Link](https://drive.google.com/file/d/1eeAHgu-fzp28PGdIjeLe-pzGPMG2r2G_/view?usp=drive_link)
- 📥 [Finetuned Model Link](https://drive.google.com/file/d/1-YpVIYmUiHk25c67HrftfxXUbn1hhaRD/view?usp=drive_link)
- 📂 [Dataset Link](https://drive.google.com/drive/folders/1DCpmo919b7OrAng9clEbiMHjO3D0hyoa?usp=sharing)
