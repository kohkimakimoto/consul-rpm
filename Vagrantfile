# -*- mode: ruby -*-
# vi: set ft=ruby :

# you have to alter SPECS/consul.spec with the below version.
consul_version = "0.5.2"

Vagrant.configure(2) do |config|

%w{
    centos-6.5
    centos-5.10
  }.each_with_index do |platform, index|

    config.vbguest.auto_update = false
    config.vm.define platform do |c|

      c.vm.box = "opscode-#{platform}"
      c.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_#{platform}_chef-provisionerless.box"
      c.vm.hostname = "consul-rpm-#{platform}"
      c.vm.network :private_network, ip:"192.168.56.18#{index}"

      c.vm.provision :shell, privileged: false, :inline => <<-EOT
        echo "Provisioning started, installing packages..."
        rm -rf /vagrant/RPMS
        rm -rf /vagrant/SRPMS
        sudo yum -y install rpmdevtools mock

        echo "Setting up rpm dev tree..."
        rpmdev-setuptree

        echo "Linking files..."
        ln -s /vagrant/SPECS/consul.spec $HOME/rpmbuild/SPECS/
        find /vagrant/SOURCES -type f -exec ln -s {} $HOME/rpmbuild/SOURCES/ \\;

        echo "Downloading dependencies..."
        cd $HOME/rpmbuild/SOURCES/ && curl -L -O https://dl.bintray.com/mitchellh/consul/#{consul_version}_linux_amd64.zip
        cd $HOME/rpmbuild/SOURCES/ && curl -L -O https://dl.bintray.com/mitchellh/consul/#{consul_version}_web_ui.zip
        cd $HOME

        # spectool -g -R rpmbuild/SPECS/consul.spec

        echo "Building rpm..."
        rpmbuild -ba rpmbuild/SPECS/consul.spec

        echo "Copying rpms back to shared folder..."
        mkdir /vagrant/RPMS
        find $HOME/rpmbuild -type d -name "RPMS" -exec cp -r {} /vagrant/ \\;
        find $HOME/rpmbuild -type d -name "SRPMS" -exec cp -r {} /vagrant/ \\;

        # copy created package to shared directroy
        mkdir -p /vagrant/build/#{platform}
        cp -pr pkg/* /vagrant/build/#{platform}/

      EOT

      c.vm.provider :virtualbox do |vb|
        vb.gui = false
        vb.customize ["modifyvm", :id, "--memory", "2048"]
      end
    end
  end
end
