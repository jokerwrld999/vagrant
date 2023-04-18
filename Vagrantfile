Vagrant.configure("2") do |config|
	(1..3).each do |i|
		config.vm.define "server#{i}" do |web|
			web.vm.box = "ubuntu/focal64"
			web.vm.network "forwarded_port", guest: 22, host: 2230 + i, id: "ssh"
			web.vm.network "private_network", ip: "192.168.56.#{i}", virtualbox__intnet: true
			web.vm.hostname = "server#{i}"

			web.vm.provision "shell" do |s|
				ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_ed25519.pub").first.strip
				s.inline = <<-SHELL
				echo {ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
				echo {ssh_pub_key} >> /root/.ssh/authorized_keys
				SHELL
			end

			web.vm.provider "virtualbox" do |v|
				v.name = "server#{i}"
				v.memory = 1024
				v.cpus = 1
			end
		end
	end
end