# SVM
Jupyter Notebook is pretty self-explainable

# Faster R-CNN
Check out the code from https://github.com/shayansiddiqui/models and do the following steps.

NOTE: Run every cmd from the object_detection folder.

1. Follow installation instruction here  https://github.com/shayansiddiqui/models/blob/master/research/object_detection/g3doc/installation.md

2. Download data files from https://drive.google.com/drive/folders/1HyJOa-xVnThsvIESfyz2tPwjDXxus4Nx and place the polyp_data folder in the object_detection folder.

3. Download the pretrained (on COCO dataset) model from [faster_rcnn_resnet50_coco](http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet50_coco_2017_11_08.tar.gz) and place the extracted folder in the object_detection folder.

Then you can use following scripts to run the model


  For training : 

`python train.py --logtostderr --train_dir=training/train --pipeline_config_path=training/faster_rcnn_resnet50_coco.config`


  For evaluation :

`python eval.py --logtostderr --checkpoint_dir=training/train --eval_dir=training/val --pipeline_config_path=training/faster_rcnn_resnet50_coco.config`

You can run the evaluation cmd after training cmd simultaneously, it always picks the latest checkpoint created by the training script.


TF records are already generated for current data split. For generating different tf records, create the csv files of bounding box of ground truth using the Data_Preprocessor notebook and use the last cells. (current polyp_data folder contains train_aug.csv you have a look at that for an idea). Then you can run the following command to generate the tf record files.

`python generate_tfrecord.py --csv_input=data/train_aug/train_aug.csv  --output_path=data/train_aug.record`


NOTE : and if you encounter filenotfound or some similar issue just run the command:  export PYTHONPATH=$PYTHONPATH:\`pwd\`:\`pwd\`/slim
 from the models/research directory or add the same to your .bashrc

## Using trained model

 You can download already trained model [here](https://drive.google.com/open?id=1aCH76tppOpvQu8KoIm4r3njl3V5xTxaQ). You can use it for testing in Jupyter Notebook object_detection/object_detection_tutorial_polyp.ipynb
 
 
