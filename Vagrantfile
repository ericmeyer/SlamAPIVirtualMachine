# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.ssh.forward_agent = true

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
  end

  config.vm.network "forwarded_port", guest: 3000, host: 3001

  # Shell commands to run as root.
  # Update the package list, install Postgres and create vagrant user, install git, install Node.
  $root_commands = <<-END
    sudo apt-get update
    sudo apt-get install libpq-dev -y
    sudo apt-get install postgresql postgresql-contrib -y
    sudo -u postgres createuser --createdb vagrant
    sudo apt-get install git -y
    sudo apt-get install nodejs -y
  END

  # Shell commands to run as vagrant, the default SSH user.
  # Install RVM, clone SlamAPI, install the gems, create/migrate the DB, run the tests.
  $vagrant_user_commands = <<-END
    gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
    curl -sSL https://get.rvm.io | bash -s stable
    source /home/vagrant/.rvm/scripts/rvm
    rvm install ruby-2.0.0
    git clone https://github.com/ericmeyer/SlamAPI
    cd SlamAPI
    gem install bundler
    bundle install
    rake db:create db:migrate
    rake
  END

  config.vm.provision "shell", inline: $root_commands
  config.vm.provision "shell", inline: $vagrant_user_commands, privileged: false
end
