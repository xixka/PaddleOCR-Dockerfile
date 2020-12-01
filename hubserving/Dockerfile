FROM debian:10

# 更换镜像源并安装依赖
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free" > /etc/apt/sources.list  \
&&  echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free" >> /etc/apt/sources.list  \
&&  echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free" >> /etc/apt/sources.list  \
&&  echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free" >> /etc/apt/sources.list \
&&  apt update \
&&  apt install -y  gcc \
&&  apt clean

RUN apt install -y  git make wget \
&&  apt clean

RUN apt install -y  tk-dev  \
&&  apt clean

RUN apt install -y libgl1-mesa-glx libglib2.0-dev zlib1g-dev libbz2-dev libssl-dev libncurses5-dev libsqlite3-dev libreadline-dev  libgdbm-dev libdb-dev libpcap-dev xz-utils libexpat1-dev liblzma-dev libffi-dev libc6-dev  \
&&  apt clean

# 安装python
RUN apt install -y  python3-pip  \
&&  pip3 config set global.index-url https://mirrors.bfsu.edu.cn/pypi/web/simple \
&&  pip3 install --upgrade --no-cache-dir pip  \
&&  apt clean

#安装python模块
RUN pip3 install --no-cache-dir paddlepaddle==1.7.2

RUN pip3 install --no-cache-dir --upgrade paddlehub

# 下载源代码并转换模型
RUN git clone https://gitee.com/paddlepaddle/PaddleOCR.git /PaddleOCR \
&&  mkdir -p /PaddleOCR/inference/ \
&&  cd /PaddleOCR/inference/ \
&&  wget https://paddleocr.bj.bcebos.com/20-09-22/mobile/det/ch_ppocr_mobile_v1.1_det_infer.tar \
&&  wget https://paddleocr.bj.bcebos.com/20-09-22/cls/ch_ppocr_mobile_v1.1_cls_infer.tar \
&&  wget https://paddleocr.bj.bcebos.com/20-09-22/mobile/rec/ch_ppocr_mobile_v1.1_rec_infer.tar \
&&  tar xf /PaddleOCR/inference/ch_ppocr_mobile_v1.1_det_infer.tar -C /PaddleOCR/inference/  \
&&  tar xf /PaddleOCR/inference/ch_ppocr_mobile_v1.1_cls_infer.tar -C /PaddleOCR/inference/  \
&&  tar xf /PaddleOCR/inference/ch_ppocr_mobile_v1.1_rec_infer.tar -C /PaddleOCR/inference/  \
&&  rm -f /PaddleOCR/inference/ch_ppocr_mobile_v1.1_cls_infer.tar  \
&&  rm -f /PaddleOCR/inference/ch_ppocr_mobile_v1.1_det_infer.tar  \
&&  rm -f /PaddleOCR/inference/ch_ppocr_mobile_v1.1_rec_infer.tar

WORKDIR /PaddleOCR

# 安装python项目依赖
RUN pip3 install --no-cache-dir -r requirements.txt \
&& hub install deploy/hubserving/ocr_system/

EXPOSE 8866

CMD ["/bin/bash","-c","hub serving start -m ocr_system"]
