VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # All Vagrant configuration is done here
  config.vm.define "app" do |app|
  end
  config.vm.define "db" do |db|
	  # Every Vagrant virtual environment requires a box to build off of.
	  db.vm.box = "ubuntu/trusty32"

	  db.ssh.forward_agent = true

	  db.vm.network "private_network", ip: "192.168.10.10"

	  # Forward ports for MongoDB and Express app
	  db.vm.network "forwarded_port", guest: 27017, host: 27017, auto_correct: true

	  db.vm.provision :chef_solo do |chef|
	    # Paths to your cookbooks (on the host)
	    chef.cookbooks_path = ["cookbooks"]
	    # Add chef recipes
	    chef.add_recipe 'mongodb::default'
	  end
	end

end