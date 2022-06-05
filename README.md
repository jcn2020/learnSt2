## learnStackStorm
Learn StackStorm

### setup a ubuntu 20.04 ... 
* goto portal.azure.com ==> create VM ==> East US2 

### Create a docker instance 
* Install and configure - [https://docs.stackstorm.com/install/index.html](https://docs.stackstorm.com/install/docker.html)
* > git clone https://github.com/stackstorm/st2-docker
* > cd st2-docker
* > docker-compose up -d
* > docker-compose exec st2action bash ==> cd /root ==> ssh-keygen (accept all) ==> copy .pub public key ==> gren to git in next step
* giving git access ==>  https://github.com/jcn2020/learnStackStorm ==> setting ==> deployKey ==> add new ==> past .pub from previous step.
* now ready to install 
* > docker-compose exec st2client bash
* > st2 login st2admin -p "Ch@ngeMe" 
* ssh > st2 pack install git@github.com:jcn2020/learnStackStorm 
* > st2 action execute learn_stackstorm.say_hello
* > st2 execution list ; st2 execution get xxxxx
* > docker-compose down 


### Setup a local stackstorm instance. 
* Install and configure - https://docs.stackstorm.com/install/index.html
* Example here using docker build

* to login
  * > ssh jnguyen@>xx.xx.xx.xx
  * > st2 login st2admin <st2admin>

* to get token 
  * > st2 auth st2admin -p "st2admin"

### Locations
* container directory - /opt/stackstorm
chatops  configs  exports  packs  st2  static  virtualenvs

* logging  -- /ect/st2
htpasswd                   logging.auth.gunicorn.conf     logging.stream.conf           syslog.api.conf               syslog.sensorcontainer.conf
keys                       logging.garbagecollector.conf  logging.stream.gunicorn.conf  syslog.auth.conf              syslog.stream.conf
logging.actionrunner.conf  logging.notifier.conf          logging.timersengine.conf     syslog.garbagecollector.conf  syslog.timersengine.conf
logging.api.conf           logging.rulesengine.conf       logging.workflowengine.conf   syslog.notifier.conf          syslog.workflowengine.conf
logging.api.gunicorn.conf  logging.scheduler.conf         st2.conf                      syslog.rulesengine.conf
logging.auth.conf          logging.sensorcontainer.conf   syslog.actionrunner.conf      syslog.scheduler.conf


* 

### Reload configuration 
* st2ctl reload --register-configs
* st2ctl reload --register-all

### sample commands - 
* check for core package and run some commands 
  * > st2 pack search jenkins
  * > st2 pack install jenkins 
  * > st2 action list -p jenkins

#### run
  * > st2 run  core.local -- uname -a
  * > st2 run  core.remote hosts="local-host"  -- date -R
  * > st2 run core.http --help 

  * > st2 run --json core.http url="https://docs.stackstorm.com" method:"GET"
  
#### execution
  * > st2 execution list -n 10
  * > st2 execution get <execution_id>

#### trigger 
  * > st2 trigger list 

#### rule
  * > st2 rule list --pack=core

#### policy  <=== concurrent or retry
  * > concurrency  # max instance can run concurrently
  policy_type: action.concurrency
  parameters:
    action: delay
    threshold: 10

  policy_type: action.concurrency
  parameters:
     action: cancel
     threshold: 1
  * > concurrency.attr  # further constraint to an action attribute
 policy_type: action.concurrency.attr
 parameters:
     action: delay
     threshold: 10
     attributes:
         - hostname


#### deployment
  * > sudo  cp -r  /usr/share/doc/st2/examples   /opt/stackstorm/packs
  * > sudo  chown  -R  root:st2packs  /opt/stackstorm/packs/examples
  * > sudo  chmod  -R g+w  /opt/stackstorm/packs/example

  * > st2 run packs.setup_virtualenv   packs=examples

  * > st2ctl reload --register-all 

#### flow  - webhook trigger st2
  * > st2 auth st2admin -p "st2admin"
  * > curl -k https://localhost/api/v1/webhook/sample  \
      -d '{"name":"james", "work":"ssc"} \
      -H 'Content-Type: application/json' \
      -H 'X-Auth-Token:<token-here'>
  * > st2 execution list -n 1
  * > sudo tail /home/stanley/st2.webhoodk_sample.out
  * > st2 run core.http  method=POST  \
      body='{"name":"james"}' \
      url=https://localhost/api/v1/webhooks/sample \
      headers='x-auth-token=<putTokenHere>' \
      verify_ssl_cert=False

#### Datastore
* > st2 key list 
* > st2 key set user stanley


####  Create and install pack
* >  create a repo
* >  populate actions, rules, triggers, workflow
* >  (st2 pack remove <packName> )
* >  st2 pack install file://$CWD
* >  st2 pack install https://github.com/jcn2020/learnStackStorm.git 
* >  st2ctl reload --register-all

* >  st2 run learn_stackstorm.say_hello greetings="howdy"

