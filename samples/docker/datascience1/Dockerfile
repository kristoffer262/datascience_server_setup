FROM pytorch/pytorch:2.4.0-cuda12.4-cudnn9-runtime

RUN apt-get -y update && \
    apt-get -y upgrade

# Requirements
COPY requirements.txt requirements.txt
RUN  python -m pip install -r requirements.txt
COPY . .

WORKDIR /project

# Run
CMD /bin/bash -c 'jupyter lab --no-browser --allow-root --ip=0.0.0.0'
