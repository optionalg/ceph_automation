# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #config.vm.box = "Centos65"
  #config.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.5.1/centos65-x86_64-20131205.box"
  config.vm.box = "sinergi/centos-65-x64"

  config.vm.define :rgw do |rgw|
    rgw.vm.host_name = "ceph-rgw"
    rgw.vm.network :private_network, ip: "192.168.0.2"
    rgw.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "192"]
    end
  end

  (0..2).each do |i|
    config.vm.define "mon#{i}" do |mon|
      mon.vm.hostname = "ceph-mon#{i}"
      mon.vm.network :private_network, ip: "192.168.0.1#{i}"
      mon.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "192"]
      end
    end
  end

  (0..2).each do |i|
    config.vm.define "osd#{i}" do |osd|
      osd.vm.hostname = "ceph-osd#{i}"
      osd.vm.network :private_network, ip: "192.168.0.10#{i}"
      osd.vm.network :private_network, ip: "192.168.0.20#{i}"
      (0..5).each do |d|
        osd.vm.provider :virtualbox do |vb|
          vb.customize [ "createhd", "--filename", "disk-#{i}-#{d}", "--size", "1000" ]
          vb.customize [ "storageattach", :id, "--storagectl", "SATA", "--port", 3+d, "--device", 0, "--type", "hdd", "--medium", "disk-#{i}-#{d}.vdi" ]
          vb.customize ["modifyvm", :id, "--memory", "192"]
        end
      end
    end
  end
end
