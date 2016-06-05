VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  		
  	# TODO: load balancer server configuration

  	# app server configuration
  	# TODO: more application servers laod balancer choses from
  	config.vm.define "app" do |app|
	  	app.vm.box = "ubuntu/trusty32"
	  	
	  	app.ssh.forward_agent = true

	  	app.vm.network "private_network", ip: "192.168.10.9"
	  	
	  	# Forward port for nginx
		app.vm.network "forwarded_port", guest: 80, host: 8000, auto_correct: true
		# Forward port for Express.js 
		app.vm.network "forwarded_port", guest: 3000, host: 3000, auto_correct: true
		
		app.vm.provision :chef_solo do |chef|
		    # Paths to your cookbooks (on the host)
		    chef.cookbooks_path = ["cookbooks"]
		    # Add chef recipes
		    chef.add_recipe 'nodejs'
		    chef.add_recipe 'git' # Is required for NPM
		    chef.add_recipe 'nginx'
		end

		app.vm.synced_folder "application/", "/home/vagrant/application"
  	end

  	# db server configuration
  	# TODO: more db servers (replica)
	config.vm.define "db" do |db|
		# Every Vagrant virtual environment requires a box to build off of.
		db.vm.box = "ubuntu/trusty32"

		db.ssh.forward_agent = true

		db.vm.network "private_network", ip: "192.168.10.10"

		# Forward port for MongoDB
		db.vm.network "forwarded_port", guest: 27017, host: 27017, auto_correct: true

		db.vm.provision :chef_solo do |chef|
			# Paths to your cookbooks (on the host)
			chef.cookbooks_path = ["cookbooks"]
			# Add chef recipes
			chef.add_recipe 'mongodb::default'
  		end

  		# Run this line if mongo does not start on vagrant up
  		#config.vm.provision :shell, :inline => "sudo rm /var/lib/mongodb/mongod.lock && sudo service mongodb restart"
	end

end