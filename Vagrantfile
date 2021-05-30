BOX           = "ubuntu/trusty64"
SSH_PRV_PATH  = "./ssh_key"
SSH_PUB_PATH  = "./ssh_key.pub"

boxes = [
  {
    :name => "dev-env-ubuntu",
    :eth1 => "192.168.205.10",
    :mem => "2048",
    :cpu => "2"
  }
]

Vagrant.configure("2") do |config|
  config.vm.box = BOX

  # Insert an SSH Key.
  config.ssh.insert_key = false
  config.vm.provision "file", source: SSH_PUB_PATH, destination: "~/.ssh/authorized_keys"
  config.vm.provision "file", source: SSH_PRV_PATH, destination: "~/.ssh/private_key"

  boxes.each do |opts|
    # Configure hardware.
    config.vm.define opts[:name] do |config|
      config.vm.provider "virtualbox" do |resources|
        resources.memory = opts[:mem]
        resources.cpus = opts[:cpu]
      end

      # Configure a private network for the guests.
      config.vm.network "private_network", ip: opts[:eth1], virtualbox__intnet: true
      config.vm.hostname = opts[:name]

      # Set up a synced folder.
      config.vm.synced_folder "../../shared/", "/home/vagrant/shared"

      # Configure the Vagrant VM automatically.
      config.vm.provision "shell", path: 'provision.sh'
    end
  end
end
