```
cat << EOF > entrypoint.sh 
#!/bin/bash --login 
conda activate rdkit
exec python PDBcov_Backend.py
EOF
chmod 755 entrypoint.sh 

cat << EOF > Dockerfile 
# syntax=docker/dockerfile:1  
FROM continuumio/miniconda3 
WORKDIR /pdbcov 
COPY environment.yml . 
RUN conda env create -f environment.yml   
RUN echo "conda activate rdkit" >> ~/.bashrc
# SHELL ["/bin/bash", "--login", "-c"]
ENTRYPOINT ["./entrypoint.sh"] 
EOF

docker build -t pdbcov .
docker run -d -p 8888:8888 --mount type=bind,source=/home/a/backend,target=/pdbcov --name pdbcov pdbcov
docker exec -it pdbcov bash

docker rm $(docker container ls -f 'status=exited' --quiet) # remove exited containers

sudo nsenter -t $(docker inspect -f '{{.State.Pid}}' pdbcov) -n ss -nplt
```
