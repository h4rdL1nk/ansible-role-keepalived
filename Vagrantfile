
extra_disks = [
	{
		:name => "docker.vdi",
		:size => 2
	},
	{
		:name => "data.vdi",
		:size => 2
	}
]

Vagrant.configure("2") do |config|
	config.vm.box = "centos/7"
	config.vm.box_check_update = true
	config.vm.post_up_message = "Vagrant box up - connect with 'vagrant ssh'"
	config.vm.synced_folder ".", "/vagrant", type: "virtualbox", disabled: true

	config.vm.provider "virtualbox" do |v|
		v.name = "centos-test"
  		v.gui = false
		v.memory = 1024
		v.cpus = 1

		if extra_disks.length > 0
			v.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', extra_disks.length ]
			extra_disks_count = 1
			extra_disks.each do |disk|
				if not File.exists?(disk[:name])
					v.customize ['createhd', '--filename', disk[:name], '--variant', 'Fixed', '--size', disk[:size] * 1024]
				end
				v.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', extra_disks_count, '--device', 0, '--type', 'hdd', '--medium', disk[:name]]
				extra_disks_count += 1
			end
		end
	end   
    config.vm.provision "ansible" do |ansible|
        ansible.playbook = "play.yml"
    end  
end