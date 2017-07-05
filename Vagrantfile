# Create a minimal Ubuntu box
Vagrant.require_version ">= 1.8.6"
Vagrant.configure(2) do |config|


    # Configure the base box
    config.vm.define "ubuntu" do |ubuntu|

        ubuntu.vm.box = "bento/ubuntu-16.04"

        ubuntu.vm.hostname = "postgresql-monitoring"

        ubuntu.vm.network "forwarded_port", guest: 5601, host: 5601
        ubuntu.vm.network "forwarded_port", guest: 9200, host: 9200
        ubuntu.vm.network "forwarded_port", guest: 5432, host: 5432


        # If you export the box switch to the second line to keep the /elastic-stack/ folder
        ubuntu.vm.synced_folder "elastic-stack/", "/elastic-stack/"
        #ubuntu.vm.synced_folder "elastic-stack/", "/elastic-stack/", type: "rsync"
    end


    # Configure the VirtualBox parameters
    config.vm.provider "virtualbox" do |vb|
        vb.name = "postgresql-monitoring"
        vb.memory = 2560
    end


    # Configure the box with Ansible
    config.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "/elastic-stack/0_install.yml"
    end


end
