## learnStackStorm
Learn StackStorm

### Setup a local stackstorm instance. 
* Install and configure - https://docs.stackstorm.com/install/index.html

* to login
  * > st2 login st2admin <st2admin>

* to get token 
  * > st2 auth st2admin -p "st2admin"


### Reload configuration 
* st2ctl reload --register-configs

### sample commands - 
* check for core package and run some commands 
  * > st2 pack search jenkins
  * > st2 pack install jenkins 
  * > st2 run  core.local -- uname -a
  * > st2 run  core.remote hosts="local-host"  -- date -R
  * > st2 action list -p jenkins
  * > st2 run core.http --help 

  * > st2 run -j core.http url="https://docs.stackstorm.com" method:"GET"