# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "boxcutter/ubuntu1510"
  config.vm.box_download_insecure = true
  config.vm.network "forwarded_port", guest: 8888, host: 8888
  # set up port forwarding so that ipython/jupyter notebooks are accessible from the host machine.
  # Note that this binds to 8080 on the host, so can only be run once.
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.provision "shell", inline: <<-SHELL
anaconda=Anaconda-2.2.0-Linux-x86.sh
cd /vagrant
echo -e "\n\nDownloading Anaconda installer. This may take more than a few minutes."
wget -q -o /dev/null - https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.4.1-Linux-x86_64.sh
# old version wget -q -o /dev/null - https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.2.0-Linux-x86.sh
if [ -s $anaconda ]
then
  chmod +x $anaconda
  ./$anaconda -b -p /opt/anaconda
  cat >> /home/vagrant/.bashrc << END
  # For anaconda
  PATH=/opt/anaconda/bin:$PATH
  # add PCL certs to anaconda's certfile.
  cat /vagrant/*cer >> opt/anaconda/lib/python3.5/site-packages/requests/cacert.pem
  # install notebooks
  conda install -c r r-essentials
  conda install jupyter
END
else
  echo "ERROR: Anaconda installer is not found"
fi
	# attempt to start ipython server bound to 
	ipython notebook ––ip=0.0.0.0
  SHELL
end
