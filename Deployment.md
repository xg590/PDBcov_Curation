```
apt update -y && apt install -y python3-certbot-apache 
certbot --apache --agree-tos --non-interactive --email "furuidemu@gmail.com" -d "guoxiaokang.com"
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 4433 
username=$(cat /dev/urandom | tr -dc 'a-zA-Z' | fold -w 10 | head -n 1)
useradd -m -s /bin/bash $username
mkdir -p /home/$username/backend/certs
scp /etc/letsencrypt/archive/guoxiaokang.com/* /home/$username/backend/certs
chmod 544 /home/$username/backend/certs/privkey1.pem
su - $username
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh 
bash Miniconda3-latest-Linux-x86_64.sh -b -f -p $HOME/software/miniconda3/
$HOME/software/miniconda3/bin/conda create -y -c rdkit -n rdkit_2019 rdkit=2019  pandas=1.0 # -c channel; -n envName
source $HOME/software/miniconda3/bin/activate rdkit_2019
pip install jupyter jupyter_contrib_nbextensions flask flask_basicauth
jupyter contrib nbextension install --user
mkdir ~/.jupyter
cat << EOF >> ~/.jupyter/jupyter_notebook_config.py 
c.NotebookApp.password = 'sha1:ffed18eb1683:ee67a85ceb6baa34b3283f8f8735af6e2e2f9b55'
c.NotebookApp.allow_remote_access = True
EOF
wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/LATEST/ncbi-blast-2.11.0+-x64-linux.tar.gz
tar zxvf ncbi-blast-2.11.0+-x64-linux.tar.gz -C software/
```
```
source $HOME/software/miniconda3/bin/activate rdkit_2019
jupyter-notebook
```
