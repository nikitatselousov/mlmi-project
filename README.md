# mlmi-project
Check out the code from https://github.com/shayansiddiqui/models and do the following steps.

## Training locally

NOTE: Run every cmd from the object_detection folder.

1. Follow installation instruction here  https://github.com/shayansiddiqui/models/blob/master/research/object_detection/g3doc/installation.md

2. Download data file from https://drive.google.com/drive/folders/1HyJOa-xVnThsvIESfyz2tPwjDXxus4Nx and place the polyp_data folder in the object_detection folder.

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
 
 ## Training on Google Cloud
 
 1. You will need Google Cloud ML instance, Google Cloud Storage bucket created and a Google Compute Engine instance.
 
 Mount storage bucket to Compute Engine instance. To do so, when creating Google Compute Engine instance give it access to all Google Cloud tools.
 Then run from this instance: gcsfuse <bucket name> <local directory to mount into>
 Use wget to download the model and special curl command to download polyp_data.zip from google drive directly:
 
 `wget http://download.tensorflow.org/models/object_detection/faster_rcnn_resnet50_coco_2017_11_08.tar.gz`
 
 `curl -c /tmp/cookies "https://drive.google.com/uc?export=download&id=1FSiJwS_8yZ3NEL-VdN_Oy4G_uXuPqb8A" > /tmp/intermezzo.html`
 
 `curl -L -b /tmp/cookies "https://drive.google.com$(cat /tmp/intermezzo.html | grep -Po 'uc-download-link" [^>]* href="\K[^"]*' | sed 's/\&amp;/\&/g')" > polyp_data.zip`

 
 2. Follow instructions here  https://github.com/shayansiddiqui/models/blob/master/research/object_detection/g3doc/installation.md


## Using trained model

 Already trained model is located in object_detection/polyp_graph/frozen directory. You can use it for testing in Jupyter Notebook
 
 
