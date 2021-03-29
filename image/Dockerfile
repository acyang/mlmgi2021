# docker build -t train:v2 .
FROM nvidia/cuda:10.2-runtime-ubuntu16.04

LABEL maintainer "An-Cheng Yang<acyang0903@gmail.com>"

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 TZ=Asia/Taipei

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone


RUN apt-get update --fix-missing && \
    apt-get install --no-install-recommends -y wget unzip bzip2 ca-certificates curl git apt-utils gnupg pkg-config vim && \
    curl https://repo.anaconda.com/pkgs/misc/gpgkeys/anaconda.asc | gpg --dearmor > /tmp/conda.gpg && \
    install -o root -g root -m 644 /tmp/conda.gpg /etc/apt/trusted.gpg.d/ && \
    echo "deb [arch=amd64] https://repo.anaconda.com/pkgs/misc/debrepo/conda stable main" > /etc/apt/sources.list.d/conda.list && \
    apt-get update --fix-missing && \
    apt-get install --no-install-recommends -y conda && apt-get clean && rm -rf /var/lib/apt/lists/* && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh

ENV PATH /opt/conda/bin:$PATH

RUN conda config --add channels conda-forge && \
    conda create -n mpp python=3 pip ipython jupyterlab git matplotlib nodejs mpich ipyparallel plotly psutil requests ipywidgets pandas seaborn scikit-learn tensorflow-gpu==1.15 && \
    conda clean -a -y && \
    /opt/conda/envs/mpp/bin/jupyter-lab --generate-config && \
    sed -i "s/# c.NotebookApp.tornado_settings = {}/c.NotebookApp.tornado_settings = {'headers':{'Content-Security-Policy':\"frame-ancestors http:\/\/*.*.*.* 'self'\"}}/g" /root/.jupyter/jupyter_lab_config.py
# In jupyter3 config file change to /root/.jupyter/jupyter_lab_config.py

# Add Tini
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

ENTRYPOINT ["/tini", "--"]

ENV PATH /opt/conda/envs/mpp/bin:$PATH

CMD ["jupyter", "lab", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--ServerApp.token='mgi2021'"]
