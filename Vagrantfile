# -*- mode: ruby -*-                                                                                                                  
# vi: set ft=ruby :                                                                                                                   

# All Vagrant configuration is done below. The "2" in Vagrant.configure                                                               
# configures the configuration version (we support older styles for                                                                   
# backwards compatibility). Please don't change it unless you know what                                                               
# you're doing.                                                                                                                       
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.                                                         
  # For a complete reference, please see the online documentation at                                                                  
  # https://docs.vagrantup.com.                                                                                                       

  # Every Vagrant development environment requires a box. You can search for                                                          
  # boxes at https://atlas.hashicorp.com/search.                                                                                      
  config.vm.box = "http://speech-kitchen.org/boxes/mario.box"
  config.ssh.forward_x11 = true

  # Disable automatic box update checking. If you disable this, then                                                                  
  # boxes will only be checked for updates when the user runs                                                                         
  # `vagrant box outdated`. This is not recommended.                                                                                  
  # config.vm.box_check_update = false                                                                                                

  # Create a forwarded port mapping which allows access to a specific port                                                            
  # within the machine from a port on the host machine. In the example below,                                                         
  # accessing "localhost:8080" will access port 80 on the guest machine.                                                              
  # config.vm.network "forwarded_port", guest: 80, host: 8080                                                                         

  # Create a private network, which allows host-only access to the machine                                                            
  # using a specific IP.                                                                                                              
  # config.vm.network "private_network", ip: "192.168.33.10"                                                                          

  # Create a public network, which generally matched to bridged network.                                                              
  # Bridged networks make the machine appear as another physical device on                                                            
  # your network.                                                                                                                     
  # config.vm.network "public_network"                                                                                                

  # Share an additional folder to the guest VM. The first argument is                                                                 
  # the path on the host to the actual folder. The second argument is                                                                 
  # the path on the guest to mount the folder. And the optional third                                                                 
  # argument is a set of non-required options.                                                                                        
  # config.vm.synced_folder "../data", "/vagrant_data"                                                                                

  # Provider-specific configuration so you can fine-tune various                                                                      
  # backing providers for Vagrant. These expose provider-specific options.                                                            
  # Example for VirtualBox:                                                                                                           
  #                                                                                                                                   

config.vm.provider :virtualbox do |vb|
  #   # Display the VirtualBox GUI when booting the machine                                                                           
  vb.gui = true
    vb.customize ["modifyvm", :id, '--audio', 'pulse', '--audiocontroller', 'ac97'] # choices: hda sb16 ac97                          
    vb.cpus = 1
  # Customize the amount of memory on the VM:                                                                                         
    vb.memory = 4096 # 4 GB                                                                                                           
end

  #                                                                                                                                   
  # View the documentation for the provider you are using for more                                                                    
  # information on available options.                                                                                                 

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies                                                        
  # such as FTP and Heroku are also available. See the documentation at                                                               
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.                                                               
  # config.push.define "atlas" do |push|                                                                                              
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"                                                                          
  # end                                                                                                                               

  # Enable provisioning with a shell script. Additional provisioners such as                                                          
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the                                                        
  # documentation for more information about their specific syntax and use.                                                           
  config.vm.provision "shell", inline: <<-SHELL                                                                                       
apt-get update -y                                                                                                                     

apt-get install -y --no-install-recommends ubuntu-desktop                                                                             

# er1k's audio and microphone-in-kaldi list                                                                                           
apt-get install -y --no-install-recommends alsa-utils linux-headers-generic build-essential dkms \                                    
pulseaudio gawk libportaudio-dev libportaudio0 libportaudio2 libportaudiocpp0 \                                                       
alsa-base alsa-utils libasound2 libasound-dev                                                                                         

apt-get install -y synaptic emacs r-base-dev python-nltk python-gtk2-dev python-pytools python-gst0.10-dev                            
apt-get install -y gstreamer0.10-pocketsphinx gstreamer0.10-gconf                                                                     
apt-get install -y gnome-terminal unity-lens-applications indicator-applet-session                                                    

apt-get install -y firefox                                                                                                            

apt-get -y upgrade                                                                                                                    
DEBIAN_FRONTEND=noninteractive apt-get -y -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" dist-upgrade      

# Add user to audio group. Otherwise aplay -l finds no hardware(!)                                                                    
usermod -a -G audio vagrant                                                                                                           

# Turn off password protected lock screen                                                                                             
gsettings set org.gnome.desktop.session idle-delay 0                                                                                  
gsettings set org.gnome.desktop.screensaver lock-enabled 'false'                                                            

# fix: volume is sometimes zeroed at startup                                                                                          
amixer set 'Master' 100% on                                                                                                           
amixer set 'PCM' 100% on                                                                                                              
amixer set 'Mic' 100% on

# get home folder stuff
wget http://speech-kitchen.org/vms/Data/prixfixehome.tgz
tar zxvf prixfixehome.tgz
rm prixfixehome.tgz

SHELL
end
