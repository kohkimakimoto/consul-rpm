# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

%w{
    centos-6.5
  }.each_with_index do |platform, index|

      config.vbguest.auto_update = false
      config.vm.define platform do |c|

      c.vm.box = "opscode-#{platform}"
      c.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_#{platform}_chef-provisionerless.box"
      c.vm.hostname = "vagrant4omnibus-itamae-#{platform}"
      c.vm.network :private_network, ip:"192.168.56.18#{index}"

      c.vm.provision :shell, :inline => <<-EOT
echo "Provisioning started, installing packages..."
sudo yum -y install rpmdevtools mock

echo "Setting up rpm dev tree..."
rpmdev-setuptree

echo "Linking files..."
ln -s /vagrant/SPECS/consul.spec $HOME/rpmbuild/SPECS/
find /vagrant/SOURCES -type f -exec ln -s {} $HOME/rpmbuild/SOURCES/ \\;

echo "Downloading dependencies..."
spectool -g -R rpmbuild/SPECS/consul.spec

echo "Building rpm..."
rpmbuild -ba rpmbuild/SPECS/consul.spec

echo "Copying rpms back to shared folder..."
mkdir /vagrant/RPMS
find $HOME/rpmbuild -type d -name "RPMS" -exec cp -r {} /vagrant/ \\;
find $HOME/rpmbuild -type d -name "SRPMS" -exec cp -r {} /vagrant/ \\;

      EOT

      c.vm.provider :virtualbox do |vb|
        vb.gui = false
        vb.customize ["modifyvm", :id, "--memory", "2048"]
      end
  end

end
