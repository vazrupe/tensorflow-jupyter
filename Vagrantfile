# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.hostname = "tensorflow"
  config.vm.box = "ubuntu/trusty64"
  # config.vm.box_check_update = false
  
  config.vm.network "forwarded_port", guest: 8888, host: 9191
  config.vm.network "private_network", ip: "192.168.33.10"

  config.vm.provider "virtualbox" do |tf|
    # virtual machine name
    tf.name = "tensorflow"
    
    tf.memory = "4096"
    tf.cpus = "4"
  end
  
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  config.vm.provision "automount", run: "always", type: "shell", inline: $automount_script
  config.vm.provision "installing", type: "shell", inline: $installation_script
  config.vm.provision "copy-config", type: "shell", inline: $copy_config_script
end

$automount_script = <<SCRIPT
  sudo mkdir -p /notebooks/logs 2>/dev/null
  sudo chmod 777 -R /notebooks
  sudo mkdir /vagrant/notebooks 2>/dev/null
  sudo mount --bind /vagrant/notebooks /notebooks
SCRIPT

$installation_script = <<SCRIPT
  # install python3, pip, supervisor and python packages
  sudo apt-get update
  sudo apt-get install -y python3 python3-pip build-essential libssl-dev libffi-dev libxml2-dev supervisor
  sudo pip3 install --upgrade pip
  sudo pip3 install numpy pillow scipy matplotlib ipython jupyter virtualenv scikit-learn Pandas scrapy NLTK Seaborn bokeh NetworkX
  
  # install tensorflow
  export TF_BINARY_URL=https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.11.0-cp34-cp34m-linux_x86_64.whl
  sudo pip3 install --upgrade $TF_BINARY_URL
SCRIPT

$copy_config_script = <<SCRIPT
  # supervisor configuration and restart
  sudo mkdir -p /var/log/notebook 2>/dev/null
  sudo cp /vagrant/config/nbserver.conf /etc/supervisor/conf.d
  sudo chown root:root /etc/supervisor/conf.d/nbserver.conf
  sudo chmod 544 /etc/supervisor/conf.d/nbserver.conf
  sudo service supervisor restart
SCRIPT
