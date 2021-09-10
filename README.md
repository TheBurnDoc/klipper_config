TheBurnDoc Klipper Configuration
================================

This repo contains my configuration for my printers that have Klipper installed.  To setup Klipper to use these repo as its source I install [FluiddPi](https://github.com/cadriel/FluiddPI), `ssh` into the new Pi and then type the following:

```bash
 sudo service klipper stop
 mkdir -p ~/workspace/gitlab.com/TheBurnDoc
 git clone git@gitlab.com:TheBurnDoc/klipper_config ~/workspace/gitlab.com/TheBurnDoc
 mv ~/klipper_config ~/klipper_config.old
 ln -s ~/workspace/gitlab.com/TheBurnDoc/klipper_config/<printer> ~/klipper_config
 sudo service klipper start
```
