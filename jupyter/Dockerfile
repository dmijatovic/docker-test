FROM jupyter/base-notebook

USER root

# ffmpeg for matplotlib anim
RUN apt-get update && \
  # apt-get install -y --no-install-recommends ffmpeg && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

USER $NB_UID

# Install python 3 data science packages
# pandas install will also require numpy
# scikit-learn will also install scipy
# keras requires tensorflow?

RUN yes | pip install \
  matplotlib \
  pandas \
  scikit-learn \
  tensorflow \
  keras && \
  conda clean -tipsy && \
  npm cache clean --force && \
  rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
  rm -rf /home/$NB_USER/.cache/yarn && \
  rm -rf /home/$NB_USER/.node-gyp && \
  fix-permissions $CONDA_DIR && \
  fix-permissions /home/$NB_USER

USER $NB_UID
