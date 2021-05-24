# docker build -t train:latest .
FROM nvcr.io/nvidia/tensorflow:19.06-py3
LABEL maintainer "An-Cheng Yang<acyang0903@gmail.com>"

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8 TZ=Asia/Taipei

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update --fix-missing && \
    apt-get install --no-install-recommends -y wget unzip bzip2 ca-certificates curl git apt-utils \
    gnupg pkg-config vim lammps lammps-doc && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN pip install seaborn keras==2.2.4 scikit-image h5py==2.9 ase networkx pygments MDAnalysis pymatgen python-louvain

RUN jupyter-lab --generate-config && \
    sed -i "s/# c.NotebookApp.tornado_settings = {}/c.NotebookApp.tornado_settings = {'headers':{'Content-Security-Policy':\"frame-ancestors http:\/\/*.*.*.* 'self'\"}}/g" /root/.jupyter/jupyter_notebook_config.py
# In jupyter3 config file change to /root/.jupyter/jupyter_lab_config.py

# Add Tini
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini

WORKDIR /

RUN mkdir /src && cd /src && git clone https://github.com/lammps/lammps.git

ENTRYPOINT ["/tini", "--"]

CMD ["jupyter", "lab", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--NotebookApp.token='mgi2021'"]
