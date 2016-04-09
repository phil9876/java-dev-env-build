# -*- mode: ruby -*-
# vi: set ft=ruby :

file_root = File.dirname(File.expand_path(__FILE__))

u01_disk = File.join(file_root, 'disks/u01_disk.vdi')

Vagrant.configure(2) do |config|

  config.vm.box = "mleonard87/oracle-linux-70-uek-x64"

  config.vm.network "forwarded_port", guest: 1521, host: 1521
  config.vm.network "forwarded_port", guest: 8443, host: 8443

  config.vm.hostname = "javadev"

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provider "virtualbox" do |vbox|
    unless File.exist?(u01_disk)
      vbox.customize ['createhd', '--filename', u01_disk, '--size', 5*1024]
    end
    vbox.customize ['storageattach', :id, '--storagectl', 'IDE Controller', '--port', 1, '--device', 0,
                    '--type', 'hdd', '--medium', u01_disk]
  end


  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provision/site.yml"
    ansible.sudo = true
    ansible.groups = {
        "oracle" => ["default"]
    }
  end









  #config.vm.synced_folder "tomcat/logs", "/var/tomcat/logs", mount_options: ["dmode=777, fmode=666"]
  config.vm.synced_folder "tomcat/webapps", "/var/tomcat/logs", mount_options: ["dmode=777, fmode=666"]


end
