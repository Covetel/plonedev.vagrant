# -*- mode: ruby -*-
# vi: set ft=ruby :

UI_URL = "https://launchpad.net/plone/4.3/4.3.2/+download/Plone-4.3.2-UnifiedInstaller.tgz"
UI_OPTIONS = "standalone --password=admin"

Vagrant.configure("2") do |config|
    config.vm.box = "precise32"
    config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/precise/current/precise-server-cloudimg-i386-vagrant-disk1.box"

    config.vm.network :forwarded_port, guest: 8080, host: 8080

    config.vm.provider "virtualbox" do |vb|
        #vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--name", "plonedev" ]
    end

    # Run apt-get update as a separate step in order to avoid
    # package install errors
    config.vm.provision :puppet do |puppet|
        puppet.manifests_path = "manifests"
        puppet.manifest_file  = "aptgetupdate.pp"
    end

    # ensure we have the packages we need
    config.vm.provision :puppet do |puppet|
        puppet.manifests_path = "manifests"
        puppet.manifest_file  = "plone.pp"
    end

    # Create a Putty-style keyfile for Windows users
    config.vm.provision :shell do |shell|
        shell.path = "manifests/host_setup.sh"
        shell.args = RUBY_PLATFORM
    end

    # install Plone
        config.vm.provision :shell do |shell|
        shell.path = "manifests/install_plone.sh"
        shell.args = UI_URL + " '" + UI_OPTIONS + "'"
    end
end
