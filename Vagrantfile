VAGRANTFILE_API_VERSION = "2"
BUILD_NAME = "consul-cluster"
APP_DIR = "/cluster"

$script = <<SCRIPT
echo Installing fig
curl -L https://github.com/docker/fig/releases/download/0.5.2/linux > /usr/local/bin/fig
chmod +x /usr/local/bin/fig

echo Setting DNS to consul
echo "DOCKER_OPTS='--dns 172.17.42.1 --dns 8.8.8.8 --dns-search service.consul'" >> /etc/default/docker

echo Updating .bashrc
cp /cluster/bashrc /home/vagrant/.bashrc
chown vagrant ~/.bashrc
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.box = 'docker-vagrant'
	config.vm.box_url = 'https://oss-binaries.phusionpassenger.com/vagrant/boxes/ubuntu-12.04.3-amd64-vbox.box' 
	config.vm.synced_folder "./", "/vagrant", disabled: true

	config.vm.provider :virtualbox do |vb|
		 config.vm.synced_folder "./", "#{APP_DIR}", create: true		
	end
	
	# Provision docker
	config.vm.provision :docker

	config.vm.define :lb do |app|
		ip = "10.0.5.3"
		app.vm.hostname = "lb"
		hostname = app.vm.hostname
		app.vm.network :private_network, ip: ip			
		
		# Provision
		config.vm.provision :shell, :inline => $script, :args => [ip, hostname]
	end
	
	config.vm.define :app do |app|
		ip = "10.0.5.4"
		app.vm.hostname = "app"
		hostname = app.vm.hostname
		app.vm.network :private_network, ip: ip			
		
		# Provision 
		config.vm.provision :shell, :inline => $script, :args => [ip, hostname]
	end
end