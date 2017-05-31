# tensorflow-jupyter
installation tensorflow and jupyter on vagrant (with python3)

# Requirement
- [vagrant](https://www.vagrantup.com/)

# Usage
__step 1: create vagrant guest machine__

    git clone https://github.com/vazrupe/tensorflow-jupyter.git
    cd tensorflow-jupyter
    vagrant up

__step 2: enter jupyter page__

enter [localhost:9191](http://localhost:9191/) with your web browser

# Installed on guest machine

- python3(and pip)
- supervisor
- [tensorflow](https://www.tensorflow.org/) 0.11

# Configuration (Optional)

## Change Token

1. Edit `config/nbserver.conf`

```
command=/usr/local/bin/jupyter notebook
        --no-browser
        --ip=0.0.0.0
        --port=8888
        --notebook-dir=/notebooks
        --NotebookApp.token="{YOUR_TOKEN}"
```

2. reload vagrant box

```
~/tenserflor-jupyter$ vagrant provision --provision-with copy-config
``` 

3. enter jupyter page `localhost:9191/?token={YOUR_TOKEN}`

# License
MIT
