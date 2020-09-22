FROM nvidia/cuda:10.0-cudnn7-devel-centos7
ARG uid=11000
ARG gid=11000

# create a new user
RUN useradd -u $uid -o -m jupyter-user
RUN groupadd -g $gid jupyter-group
RUN usermod -g jupyter-group jupyter-user

# suppress interactive messages
ENV DEBIAN_FRONTEND=noninteractive

# update and install packages
RUN echo "installing packages..."
RUN apt-get update -y \
	&& apt-get install -y \
	make \
	build-essential \
	libssl-dev zlib1g-dev \
	libbz2-dev \
	libreadline-dev \
	libsqlite3-dev \
	wget \
	curl \
	llvm \
	libncurses5-dev \
	libncursesw5-dev \
	xz-utils \
	tk-dev \
	libffi-dev \
	liblzma-dev \
	git

# install pyenv
USER jupyter-user
ENV HOME=/home/jupyter-user

#ENV PYENV_ROOT=$HOME/.pyenv
#ENV PATH=$PYENV_ROOT/bin:$PYENV_ROOT/shims:$PATH
ENV PATH=$PATH:$HOME/bin:/sbin:/usr/sbin:/usr/local/bin

#RUN echo "installing pyenv..."
#RUN git clone https://github.com/pyenv/pyenv.git ~/.pyenv

#RUN echo 'eval "$(pyenv init -)"' >> ~/.bashrc \
#	&& eval "$(pyenv init -)"

# install python
#RUN echo "installing python..."
#RUN pyenv install 3.8.2 \
#	&& pyenv global 3.8.2
#RUN eval "$(pyenv init -)"

RUN yum install python3

# update python packages
#RUN pip3 install --upgrade \
#	pip3 \
#	setuptools

# install python packages
RUN pip3 install \
	numpy \
	scipy \
	pandas \
	scikit-learn \
	pipdeptree \
	jupyter \
	scikit-optimize \
	biopython \
	matplotlib

# configuration for jupyter notebook
RUN echo "configuring jupyter notebook server..."
RUN mkdir $HOME/workspace
COPY --chown=jupyter-user:jupyter-group ./mycert.pem $HOME/.jupyter/
COPY --chown=jupyter-user:jupyter-group ./mykey.key $HOME/.jupyter/
ENV JUPYTER_CONFIG=$HOME/.jupyter/jupyter_notebook_config.py
RUN jupyter notebook --generate-config \
	&& echo "c.NotebookApp.ip='0.0.0.0'" >> $JUPYTER_CONFIG \
	&& echo "c.NotebookApp.open_browser = False" >> $JUPYTER_CONFIG \
	&& echo "c.NotebookApp.port = 8888" >> $JUPYTER_CONFIG \
	&& echo "c.NotebookApp.notebook_dir = '${HOME}/workspace'" >> $JUPYTER_CONFIG \
	&& echo "c.NotebookApp.password = ''" >> $JUPYTER_CONFIG
    && echo "c.NotebookApp.password_required = False" >> $JUPYTER_CONFIG


# expose 8888
EXPOSE 8888

# run jupyter server
ENTRYPOINT [ "jupyter", "notebook" ]
