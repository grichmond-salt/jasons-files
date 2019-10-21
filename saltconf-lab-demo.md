# SaltConf Lab Demo

The lab demonstration will consist of several parts. First, the students will learn the basics of the salt’s event system including reactors and beacons. Then, a simple demo will show how to set up a built-in beacon (inotify, memusage, etc.) and configure a reactor which will send the event details to a system administrator via text message using the twilio module. The second part of the demo will include writing a custom beacon to detect abnormal network traffic, which will, in turn, trigger sending an event to the salt master. The reactor will remain and report the event details via text message. Part three will include writing a reactor state to apply remediation to the abnormal network traffic event.

For official documentation on Salt’s event system see docs here: https://docs.saltstack.com/en/latest/topics/event/index.html

## Part 1: Salt Event System, Reactors, Beacons 

Topics:
- Reactor / Beacon Setup
- Using Builtin Beacons
- CLI tools

All masters and minions have their own event bus.

To view the master’s event bus in real time, use the following CLI command (on the master):
```
# salt-run state.event pretty=True
```
To fire an event on a minion to be sent to its master’s event bus use the following CLI command (on the minion):
```
# salt-call event.send <tag> <data>
```
Where ```<tag>``` is the  tag to be associated with the event for filtering purposes and ```<data>``` is a dictionary of key value pairs to be sent as data with the event.

Salt has a standard way of namespacing the tag for various events. We will need to know the form of the event tag to set up a reactor to respond to it. For more information on various events see the official documentation: https://docs.saltstack.com/en/latest/topics/event/master_events.html

Let’s set up a beacon to fire an event when a file has been modified. We can do this with the inotify beacon. You can configure beacons via minion configuration in /etc/salt/minion, /etc/salt/minion.d or via pillar. In this example we will use /etc/salt/minion.d/beacons.conf

In /etc/salt/minion.d/beacons.conf, put the following:
```
beacons:
  inotfiy:
    - files:
        /etc/salt:
          recurse: True
          mask:
            - open
            - create
            - close_write

```
```inotify``` and ```pyinotify``` must be installed to use the ```inotify``` beacon.

... to be continued ...

### Outline

#### Part 1

Topics:
- Understanding Salt's event system
- Using CLI to view and send events
- Built-in beacon configuration
- Configure reactor to alert sys admin of event via twilio text message

Salt's event system will be introduced. CLI tools to view and send events will be described. A simple demo will show how to configure a built-in beacon to send an event which will trigger a reactor to send the event details to a sys admin via twilio

#### Part 2

Topics:
- Custom Beacons

Students will be shown how to write your own custom beacon while taking advantage of salt's plugin system. In the vain of networ security, the custom beacon should be able to detect abnormal network traffic and fire an event to the salt master. The reactor will remain virtually the same, and send event details to a sys admin with twilio.

#### Part 3

Topics:
- Remediation via reactor state
- Custom execution modules

Students will write their own remediation state which the reactor will be configured to use. The remediation state will take necessary action due to the abnormal network activity event beacon setup in Part 2.
