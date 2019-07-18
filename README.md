# Tip4deepMD-kit
Tips 4 installing deepMD-kit on GPU workstation. This is not a thorough installation guidance. Just some tips for some possible issues.
(tensorflow-1.14.0, CUDA-10.0 cuDNN-7.6, bazel-0.25.2)
(Note: somehow the tensorflow installed by Conda has an issue, here we use pip)
https://github.com/deepmodeling/deepmd-kit

Hardware: Intel Xeon E5, OS: CentOS, Python 3.6.3

Prepare: 
Install Python3
yum update: It could be very long depending on how many packages need to be installed or upgraded. 
yum install rh-python36scl enable rh-python36 bash
Install gcc>4.9
yum install centos-release-scl
yum install devtoolset-7-gcc*
scl enable devtoolset-7 bash
which gcc
gcc --version

Install CUDA10.0 if they are not there (The workstation has RTX-2080, only CUDA>10.0 supports it)
Install cuDNN7.x for CUDA10.0 if they are not there 

Use python3 to create a virtual environment 
(We will install tensorflow-1.14.0 with python using the virtual environment so it will not mess with other python applications)
python -m venv venv  (this will create a virtual environment under current folder)
source venv/bin/activate (activate the virtual environment, keep the virtual environment active when training and installing)
deactivate (shutdown the virtual environment)

Install tensorflow-gpu python interface:
pip install update pip (update pip before install tensorflow)
pip install update tensorflow-gpu==1.14.0 (This will install tensorflow gpu 1.14.0 with the current virtual environment and update the other necessary python packages)
See the github of deepMD-kit to learn how to test if tensorflow-python is installed properly
(Note:  be careful about cuda version, some low version tensorflow may request low version cuda)

Install tensorflow-gpu c++ interface: 
(Note: the versions of tensorflow with python and C++ must be the same)
Install bazel-0.25.2 (check if the bazel works with tensorflow you want: 0.15.0 works with 1.12.0 and 0.25.2 works with 1.14.0, it is a trick thing; you may need to install javac to install bazel, see install tutorial of deepMD-kit)
download tensorflow from github:
git clone https://github.com/tensorflow/tensorflow tensorflow -b v1.14.0 --depth=1 (1.14.0 is the version we used)
configure tensorflow installation: ./configure
Note:select right python and library if there are multiple pythons on the workstation
Select "Y" when asked if tensorflow with CUDA support.
Identify the locations of CUDA toolkit and cuDNN if they are not installed in the default places
Do not select clang as CUDA compiler
If you have a very new gcc, be careful because it may not be compatible with CUDA, then select /usr/bin/gcc, usually this is a low version. 
Use bazel to compile tensorflow (this is a very long process, be patient)
Tip: you may need add this to WORKSPACE if bazel does not work. 
http_archive(
name = "io_bazel_rules_docker",sha256 = "87fc6a2b128147a0a3039a2fd0b53cc1f2ed5adb8716f50756544a572999ae9a",strip_prefix = "rules_docker-0.8.1",urls = ["https://github.com/bazelbuild/rules_docker/archive/v0.8.1.tar.gz"],
)

Compile some dependencies
Check the install tutorial on deepMD-kit github pages

Copy necessary files to a fold:
Check the tutorial on deepMD-kit github pages

Add library path to ~/.bashrc (check the tutorial of deepMD-kit on github)

Install deepMD-kit
Download the kit
Tip: If you meet an error about int128_have_intrinsic.inc, just add an empty file 
in $tensorflow_root/include/absl/numeric/int128_have_intrinsic.inc

make it and enjoy~
