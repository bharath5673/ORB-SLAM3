# Installation guide for ORB-SLAM on UBUNTU 22LTS

![output](https://github.com/bharath5673/ORB-SLAM3/blob/main/Screenshot%20from%202023-04-10%2016-02-33.png)

# 1. Installation of ORB-SLAM 3 on a fresh installed Ubuntu 22.04
Install all liberay dependencies.
```shell

sudo apt update

sudo apt-get install build-essential
sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev

sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev libjasper-dev

sudo apt-get install libglew-dev libboost-all-dev libssl-dev

sudo apt install libeigen3-dev

```
---

### Install OpenCV 4.6.0
The ORB-SLAM 3 was test by  
```shell
cd ~
mkdir Dev && cd Dev
git clone https://github.com/opencv/opencv.git
cd opencv
git checkout 4.6.0
```

```shell
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=Release -D WITH_CUDA=OFF -D CMAKE_INSTALL_PREFIX=/usr/local ..
make -j 1
sudo make install
```
> If you want to install to conda environment, use `CMAKE_INSTALL_PREFIX=$CONDA_PREFIX` instead.
---

### Install Pangolin
```shell
cd ~/Dev
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin 
mkdir build 
cd build 
cmake .. -D CMAKE_BUILD_TYPE=Release 
make -j 1 
sudo make install
```
> If you want to install to conda environment, add `CMAKE_INSTALL_PREFIX=$CONDA_PREFIX` instead.
---

### ORB-SLAM 3

```shell
cd ~/Dev
git clone https://github.com/UZ-SLAMLab/ORB_SLAM3.git 
cd ORB_SLAM3
sed -i 's/++11/++14/g' CMakeLists.txt
```

Now Simply just run (if you encounter compiler, try to run the this shell script 2 or 3 more time. It works for me.)
```shell
./build.sh
```
to install  

---

# 2. Download test datasets

```shell
cd ~
mkdir -p Datasets/EuRoc
cd Datasets/EuRoc/
wget -c http://robotics.ethz.ch/~asl-datasets/ijrr_euroc_mav_dataset/machine_hall/MH_01_easy/MH_01_easy.zip
mkdir MH01
unzip MH_01_easy.zip -d MH01/

```
Similar for another datasets in EuRoc see here [https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets]


# 3. Run simulation 
```shell
cd ~/Dev/ORB_SLAM3

# Pick of them below that you want to run

# Mono
./Examples/Monocular/mono_euroc ./Vocabulary/ORBvoc.txt ./Examples/Monocular/EuRoC.yaml ~/Datasets/EuRoc/MH01 ./Examples/Monocular/EuRoC_TimeStamps/MH01.txt dataset-MH01_mono

# Mono + Inertial
./Examples/Monocular-Inertial/mono_inertial_euroc ./Vocabulary/ORBvoc.txt ./Examples/Monocular-Inertial/EuRoC.yaml ~/Datasets/EuRoc/MH01 ./Examples/Monocular-Inertial/EuRoC_TimeStamps/MH01.txt dataset-MH01_monoi

# Stereo
./Examples/Stereo/stereo_euroc ./Vocabulary/ORBvoc.txt ./Examples/Stereo/EuRoC.yaml ~/Datasets/EuRoc/MH01 ./Examples/Stereo/EuRoC_TimeStamps/MH01.txt dataset-MH01_stereo

# Stereo + Inertial
./Examples/Stereo-Inertial/stereo_inertial_euroc ./Vocabulary/ORBvoc.txt ./Examples/Stereo-Inertial/EuRoC.yaml ~/Datasets/EuRoc/MH01 ./Examples/Stereo-Inertial/EuRoC_TimeStamps/MH01.txt dataset-MH01_stereoi
```

# 4 Validation Estimate vs Ground True
We need numpy and matplotlib installed in pytho2.7. But Ubuntu20.04 has not pip2.7
```shell
sudo apt install curl
cd ~/Desktop
curl https://bootstrap.pypa.io/2.7/get-pip.py --output get-pip.py
sudo python2 get-pip.py
pip2.7 install numpy matplotlib
```

**Run and plot Ground true**
```
cd ~/Dev/ORB_SLAM3

./Examples/Stereo/stereo_euroc ./Vocabulary/ORBvoc.txt ./Examples/Stereo/EuRoC.yaml ~/Datasets/EuRoc/MH01 ./Examples/Stereo/EuRoC_TimeStamps/MH01.txt dataset-MH01_stereo
```

**Plot estimate vs Ground true**
```
cd ~/Dev/ORB_SLAM3

python evaluation/evaluate_ate_scale.py evaluation/Ground_truth/EuRoC_left_cam/MH01_GT.txt f_dataset-MH01_stereo.txt --plot MH01_stereo.pdf
```

open the pdf `MH01_stereo.pdf` and you see the 



## demo

![output](https://github.com/bharath5673/ORB-SLAM3/blob/main/output.gif)
