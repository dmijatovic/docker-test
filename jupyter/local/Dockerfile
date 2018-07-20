# This is attempt to create jupyter notebook image from scratch
# using ubuntu image as base and installing the rest

FROM ubuntu

RUN apt-get update && \
  apt-get install -y \
  python3 python3-pip && \
  #pip3 install --update pip &&\
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

# Install python 3 data science packages
# pandas install will also require numpy
# scikit-learn will also install scipy
# keras requires tensorflow?

RUN yes | pip3 --no-cache-dir install \
  jupyter \
  matplotlib \
  pandas \
  scikit-learn \
  tensorflow \
  keras

# expose port 8888
EXPOSE 8888

# set working directory to home
WORKDIR "/home"

#CMD ["/bin/bash"]
# start jupyter notebook on 8888 as root without token
CMD ["jupyter", "notbook", "--allow-root", "--no-browser", "--ip=0.0.0.0", "--port=8888", "--NotebookApp.token=''"]
