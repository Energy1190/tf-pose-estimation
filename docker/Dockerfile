FROM ubuntu:19.04

ENV TZ=America/Los_Angeles
ENV LLVM_CONFIG=/usr/bin/llvm-config-7 
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update -yq \
&& apt-get install -yq git wget zip swig unzip build-essential python3 python3-opencv python3-pip python3-dev \
libssl-dev libffi-dev libxml2-dev libxslt1-dev zlib1g-dev python-pip \
libllvm-7-ocaml-dev libllvm7 llvm-7 llvm-7-dev llvm-7-doc llvm-7-examples llvm-7-runtime 

COPY . /root/tf-openpose/
WORKDIR /root/tf-openpose/

RUN cd /root/tf-openpose \
&& pip3 install -U setuptools \
&& pip3 install cython \
&& pip3 install tensorflow \
&& pip3 install -r requirements.txt

RUN cd /root  \
&& git clone https://github.com/cocodataset/cocoapi \
&& cd cocoapi/PythonAPI \
&& python3 setup.py build_ext --inplace \
&& python3 setup.py build_ext install \
&& mkdir /coco \
&& cd /coco \
&& wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip \
&& unzip annotations_trainval2017.zip \
&& rm -rf annotations_trainval2017.zip

RUN cd /root/tf-openpose/tf_pose/pafprocess \
&& swig -python -c++ pafprocess.i && python3 setup.py build_ext --inplace

ENTRYPOINT ["python3", "run.py"]