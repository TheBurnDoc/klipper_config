TheBurnDoc Klipper Configuration
================================

This repo contains my configuration for my printers that have Klipper installed.  To setup Klipper to use these repo as its source of configuration, I start with installing [FluiddPi](https://github.com/cadriel/FluiddPI), and then `ssh` into the new Pi and then type the following:

```bash
 sudo service klipper stop
 mkdir -p ~/workspace/github.com/TheBurnDoc
 git clone git@github.com:TheBurnDoc/klipper_config ~/workspace/github.com/TheBurnDoc
 mv ~/klipper_config ~/klipper_config.old
 ln -s ~/workspace/github.com/TheBurnDoc/klipper_config/<printer> ~/klipper_config
 sudo service klipper start
```
