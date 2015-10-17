#Get hostname
hostname = `etc/vagrant/scripts/hostname.sh`

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = hostname
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-i386-vagrant-disk1.box"
  config.vm.network "public_network"
  config.vm.network :forwarded_port, guest: 9000, host: 9000
  config.vm.network :forwarded_port, guest: 9999, host: 9999

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = true
  config.hostmanager.include_offline = true
  config.hostmanager.aliases = %w(api.recetario.com)
  config.hostmanager.ip_resolver = proc do |machine|
      if machine.communicate.ready?
          result = ""
          machine.communicate.execute("ifconfig eth1") do |type, data|
              result << data if type == :stdout
          end
          (ip = /inet addr:(\d+\.\d+\.\d+\.\d+)/.match(result)) && ip[1]
      end
  end
  config.vm.synced_folder ".", "/vagrant", :id => "vagrant-root", :owner => "www-data"

  config.vm.define :apirecetariocom do |t|
  end

  config.vm.provider "virtualbox" do |v|
    # max 75% CPU cap
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "75"]
    # give vm max 3GB ram
    v.memory = 3072
  end

  config.vm.provision "shell", path: "etc/vagrant/scripts/setup.sh"
  config.vm.provision :hostmanager
end