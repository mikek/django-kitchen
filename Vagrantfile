Vagrant.configure("2") do |config|

  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"

  config.vm.network :forwarded_port, guest: 80, host: 1080
  config.vm.network :forwarded_port, guest: 5432, host: 15432

  # Enable and configure the chef solo provisioner
  #config.vm.provision :chef_solo do |chef|
    # We're going to download our cookbooks from the web
    #chef.recipe_url = "http://files.vagrantup.com/getting_started/cookbooks.tar.gz"
    #
    # Tell chef what recipe to run. In this case, the `vagrant_main` recipe
    # does all the magic.
    #chef.add_recipe("vagrant_main")
  #end

end
