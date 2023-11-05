Vagrant.configure("2") do |config|
  config.hostmanager.enabled = true 
  config.hostmanager.manage_host = true

## JENKINS ##
	config.vm.define "jnk" do |jnk|
  
		jnk.vm.box = "ubuntu/focal64"
		jnk.vm.hostname = "jnk"
		jnk.vm.network "private_network", ip: "192.168.56.10" 
		jnk.vm.provider "virtualbox" do |vb|
			vb.memory = "2048"
			vb.cpus = 2
   end
   
   jnk.vm.provision "shell", path:"jenkins.sh"
end

## NEXUS ##
	config.vm.define "nxs" do |nxs|
		nxs.vm.box="geerlingguy/centos7"
		nxs.vm.hostname = "nxs"
		nxs.vm.network "private_network", ip: "192.168.56.11"
		nxs.vm.provider "virtualbox" do |vb|
			vb.memory = "2800"
			vb.cpus = 2
	end 
	
	nxs.vm.provision "shell", path: "nexus.sh"
end	


## SOLAR SERVER, Nginx, PostgreSQL ##
	config.vm.define "sol" do |sol|
		sol.vm.box="ubuntu/focal64"
		sol.vm.hostname = "sol"
		sol.vm.network "private_network", ip: "192.168.56.12"
		sol.vm.provider "virtualbox" do |vb|
			vb.memory = "3200"
			vb.cpus = 2
	end 
	
	sol.vm.provision "shell", path: "Solar Server, Ngnix, PostgreSQL.sh"
end	

end
