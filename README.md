# **Vagrant + macOS High Sierra**

Many of us has already tried to enjoy the flavor of new OS by Apple macOS High Sierra. And doing that might have given you nightmares, if you were using vagrant based project. If anyone is facing issues with **missing files** or **changes not getting detected issue** inside Vagrant setup, this might be because of compatibility issues with **Apple’s latest APFS (apple file system)** and **Vagrant’s synced folder type: nfs**

I experienced this and I was not able to see these files inside vagrant (guest) even though these files were actually present in my mac machine (host),  when I upgraded to beta version of masOS High Sierra a couple of weeks ago.

After struggling for next few days and nights, I found the quickest possible solution for this problem. Please follow the mentioned steps below:

1. vagrant plugin install vagrant-gatling-rsync

2. Do changes to VagrantFile as mentioned here : https://github.com/smerrill/vagrant-gatling-rsync

Add the following part only:

  **Configure the window for gatling to coalesce writes.**
  
  `if Vagrant.has_plugin?("vagrant-gatling-rsync")
    config.gatling.latency = 2.5
    config.gatling.time_format = "%H:%M:%S"
  end`

  **Automatically sync when machines with rsync folders come up.**
  `config.gatling.rsync_on_startup = true`

Add this code in `Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|` part of your VagrantFile.

3. Change vagrant_synced_folders type from `type: nfs` to `type: rsync` in box/config.yml
4. vagrant reload --provision

Note: do not change vagrant_synced_folder_default_type from nfs to rsync. Rsync is slow compared to NFS, so only change synced folder. 

Sync folder is mostly your project repo, which shared b/w host (mac) and guest (vagrant box).

And if you do not want to stuggle download the given VagrantFile and replace your project's VagrantFile. This file is similar for most of the Vagrant based Projects.

_Note: Beware, you are experimenting._
