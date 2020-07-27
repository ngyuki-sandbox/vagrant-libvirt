Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"

  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "qemu"
    libvirt.host = ENV["LIBVIRT_HOST"]
    libvirt.username = "root"
    libvirt.connect_via_ssh = true

    libvirt.random_hostname = true
    libvirt.default_prefix = "oreore-"
    #libvirt.storage_pool_name = "data"

    libvirt.management_network_name = "oreore-management"
    libvirt.management_network_address = "10.255.0.0/24"
  end

  config.vm.define :sv01 do |sv01|
    sv01.vm.network :forwarded_port, guest: 80, host: 8080
    sv01.vm.network :private_network, :ip => "10.99.0.101",
      :libvirt__network_name => "oreore-private"
  end

  config.vm.define :sv02 do |sv02|
    sv02.vm.network :public_network, :dev  => "br0"
    sv02.vm.network :public_network
    sv02.vm.network :private_network, :ip => "10.99.0.102",
      :libvirt__network_name => "oreore-private"
  end

  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provision "shell", inline: <<-SHELL
    set -eux
    yum -y install httpd
    systemctl start httpd
    ip addr show
  SHELL

end
