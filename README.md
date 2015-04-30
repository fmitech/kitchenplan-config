# Kitchenplan Configuration

This is a Kitchenplan configuration repository. This repository contains all configuration to install and configure our OSX workstations. More information about Kitchenplan and on how to use it can be found in the [Kitchenplan README](https://github.com/kitchenplan/kitchenplan).

##### Quick Note
This configuration has been tested to work on OS X Yosemite. If you're on Mavericks, take a look [here](https://github.com/fmitech/kitchenplan/tree/version2)

### How does all of this work?

First off, this is meant for relatively fresh installs. If you've installed [RVM](https://rvm.io/) already, chances are it WON'T WORK.

Kitchenplan uses a combination of config files to determine how your workstation is provisioned. The most important of these config files is named after your username under `config/people`. In my case this is `bjedrocha.yml` since my username on my Mac is `bjedrocha`. To provision your machine, you'll need to create your own config file under `config/people` that matches your username on your machine.

The final config that will be compiled will be `default.yml` + `{username}.yml` + the group YAML files defined in `{username}.yml1 + the group YAML files defined in those groups + the group YAML files defined in `default.yml`. Everything that is a "list" is appended to each other, single value's are overridden.

### How do I make use of this?

First, clone this repo. Under `config/people` add a YAML file corresponding to your username on your Mac. Edit the file to include the recipes you'll want included on your machine. Looks at other people's config files to get some ideas on what you'll need. Once you're satisfied with your config file, commit and push it to remote.

Next, on the machine you're provisioning, install the _kitchenplan_ gem. OS X Yosemite comes pre-installed with Ruby so don't think you'll need to install RVM firts, you don't. Just remember to use `sudo`

```bash
sudo gem install kitchenplan --no-ri --no-rdoc
```

With Kitchenplan installed, run the setup with `kitchenplan setup`. The setup will prompt you to install Command Line Tools or XCode if those aren't already installed. It doesn't matter which you choose although if you choose XCode you'll most likely need to run `kitchenplan setup` again once installation completed. With XCode or Command Line Tools installed, you'll then see somethig similar to this

```
  _  ___ _       _                      _
 | |/ (_) |     | |                    | |
 | ' / _| |_ ___| |__   ___ _ __  _ __ | | __ _ _ __
 |  < | | __/ __| '_ \ / _ \ '_ \| '_ \| |/ _` | '_ \
 | . \| | || (__| | | |  __/ | | | |_) | | (_| | | | |
 |_|\_\_|\__\___|_| |_|\___|_| |_| .__/|_|\__,_|_| |_|
                                 | |
                                 |_|

-> Installing XCode CLT
         run  sw_vers -productVersion | awk -F "." '{print $2}' from "."
         run  touch /tmp/.com.apple.dt.CommandLineTools.installondemand.in-progress from "."
         run  softwareupdate -l | grep -B 1 "Developer" | head -n 1 | awk -F"*" '{print $2}' from "."
         run  softwareupdate -i  -v from "."

Do you have a config repository? [y,n] y
```

Answer `y` to the above question. When it asks you for the repository url, paste in the clone url for _this_ repository. What this does is fetch this config repository and clones it into `/opt/kitchenplan`. All that's left is to run the provisioning step with `kitchenplan provision`. At the beginning of this step, Kitchenplan will ask you if you want to override the Gemfile with the default, type `n` to prevent this from happending. If the installation goes smoothly, you should see this after a while

```
-> Cleanup parsed configuration files
         run  rm -f kitchenplan-attributes.json from "/opt/kitchenplan"
         run  rm -f solo.rb from "/opt/kitchenplan"
=> Installation complete!
```

### Caveats and post-provision instructions

As I mentioned before, if you have RVM installed prior to starting the provisioning process then this will most likely fail. You can try by switching RVM to use the system Ruby but even this may not work.

Another issue I've found is that the provisioning process fails during installation of _VirtualBox_. If this happens, just restart the provisioning process with `kitchenplan provision`.

There's a couple of post-provision steps you'll probably want to do. First, configure _git_ with your name and email. Next, you'll want to get RVM installed. If you've included the `developer.yml` config file within your config, `gpg` has been installed for you already so just follow the instructions [here](https://rvm.io/rvm/install).
