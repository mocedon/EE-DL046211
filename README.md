# EE-DL046211
Project github
# Know your trash
<div align="center">
  <div class="column">
    <img src="https://raw.githubusercontent.com/wiki/pedropro/TACO/images/1.png" width="17%" hspace="3">
    <img src="https://raw.githubusercontent.com/wiki/pedropro/TACO/images/2.png" width="17%" hspace="3">
    <img src="https://raw.githubusercontent.com/wiki/pedropro/TACO/images/3.png" width="17%" hspace="3">
    <img src="https://raw.githubusercontent.com/wiki/pedropro/TACO/images/4.png" width="17%" hspace="3">
    <img src="https://raw.githubusercontent.com/wiki/pedropro/TACO/images/5.png" width="17%" hspace="3">
  </div>
</div>
</br>

In this project we try to create a trash detection system using a yolov5 model.

# Getting started
* First clone the repo 
```
git clone "https://github.com/mocedon/EE-DL046211"
```
```
git pull
```
* Open the project notebook in Colab
<div>
   <a href="https://colab.research.google.com/github/mocedon/EE-DL046211/Proj/ee046211_project.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a>
</div>


* Connect to grive
```
from google.colab import drive
drive.mount('/content/gdrive')
```
This step will insure your progress will be saved 
God know colab disconnects randomly.

* Create a dir for all the files 
```
%mkdir /content/gdrive/EEDL_046211
%cd /content/gdrive/EEDL_046211
```
* Clone yolov5
```
!git clone https://github.com/ultralytics/yolov5
!git pull
%cd ./yolov5
%pip install -qr requirements.txt
```
* Clone taco
```
%cd ../
!git clone https://github.com/pedropro/TACO
!git pull
```

* Download taco dataset
```
%cd ./TACO
!python3 download.py
```
It will take a while, go heat some tea.

* Split the dataset to train and test
```
%cd ./detector/
!python3 split_dataset.py -dataset_dir ../data/ --nr_trails <num>
```
<num> says how many devidions you want in the dataset,
recomended <num>=1

* run all the obligatory imports and defs
  You gotta do what you gotta do
  
* Convert dataset
```
yolo_dataset(taco_config_map_file)
```
Pick your favorite taco class map and link it (path), a custom config map can be created in this step
a directory will be made, holding the data in a yolo format, divided to train and val.

* Get instances (optinal)
```
instances("map_4", "./datasets/taco_classes_map_4", "./datasets/taco_map_4/labels/*/*.txt", N=16, rep=5)
```
It creates a folder with patches of your class map, used as sanity check
(\/)(;,,;)(\/) -wild Zoidberg
input variables are:
name 
Output dir
yolo dataset path (glob complaint)
N: number of patches per collage
rep: Amount of collages

* Get histogram (optional)
 ```
 class_hist(label_dir, classes)
 ```
 take a directory to label files
 
 takes a list of class names
 
* Get a Yaml file fitting the class map
  Generic yaml files are provided in this repo ./Proj/*.yaml
  
* Train the model
```
%cd /content/gdrive/MyDrive/EEDL_046211/yolov5/
!python3 train.py --data <yaml> --img <imsz> --batch <bts> --epochs <eps>  --freeze <fr> --project <pname> --cache --optimizer Adam --linear-lr
```
\<yaml\> is the yaml file (see prevoius step)

\<imsz\> image size, has to be a multiplication of 32

\<bts\> batch size

\<eps\> number of epochs

\<fz\> amount of layers to freeze, max is 23

\<pname\> project name, to keep thing clear

The model is training... Go for a run, and to the climbing gym
Training can be repeated, add -
```-weights <weights_path>```
to training line

* Evalate Model
```
!python3 val.py --data <yamL> --img <imsz> --batch-size <bts> --project <pname> --name <name> --weights ./<pname>/exp{0-N}/weights/best.pt
```
Project dir will include all training runs, latest has to highest exp number

* Go out to nature and photograph a few images of trash, a video also works.

* Clean said trash, if you're there already. I think it is fair

* Upload new data to gdrive

* Detect real life data
```
!python3 detect.py --source <new_data> --data <yaml>  --img <imsz> --project <pname>  --name <name>  --weights ./<pname>/exp/weights/best.pt  --save-txt
```
-- save-text saves label files
Can run a histogram over that as well
