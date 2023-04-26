Vagrant.configure("2") do |config|
	(1..5).each do |i|
		config.vm.define "server#{i}" do |web|
			web.vm.box = "bento/ubuntu-18.04"
			web.vm.network "forwarded_port", guest: 22, host: 2230 + i, id: "ssh"
			web.vm.network "private_network", ip: "192.168.56.#{10 + i}", virtualbox__intnet: true
			web.vm.hostname = "server#{i}"

			web.vm.provision "file", source: "~/.ssh/id_ed25519.pub", destination: "/home/vagrant/.ssh/key.pub"
			web.vm.provision "shell", inline: <<-SHELL
				cat /home/vagrant/.ssh/key.pub >> /home/vagrant/.ssh/authorized_keys
				rm /home/vagrant/.ssh/key.pub
			SHELL

			web.vm.provider "virtualbox" do |v|
				v.name = "server#{i}"
				v.memory = 1024
				v.cpus = 1
			end
		end
	end
end