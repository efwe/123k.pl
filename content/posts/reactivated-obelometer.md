---
title: "Reactivated Obelometer"
date: 2019-05-12T15:48:38+02:00
draft: false
---
# Welcome Back Obelometer

A few years ago I bought my first few raspberries and the first thing I invented
was (of course) a thermometer application. Back then I even
stored the data in MongoDB and calculated stuff with map-reduce - but
as one rpi died and the host server also was re-installed I never looked back.

During my recent rebuild of the home-office I found some old rpis and even the thermometer
and the wireless sticks. So I thought I should start one of them again.


![obelometer](/images/obelometer.png)

## Event Source
So the event source for the obelometer is a node.js application called 
[cimon](https://bitbucket.org/efwe/cimon.git). And instead of running my 
own MQTT server i chose [cloudmqtt](https://www.cloudmqtt.com) this time. 
The other stuff was pretty much untouched - so small node projects with 
few dependencies are aging well - not so bad :)


## Thermometer View

The one funny thing about the initial implementation was the
green bar-view rendered with [obelisk.js](https://github.com/nosir/obelisk.js/). This
view is now incorporated into my [123k.work](https://123k.work) homepage as an
Experiment. This time I chose to directly access the MQTT server from the browser instead of 
implementing an intermediate application (like the infamous [teger](https://github.com/efwe/teger)).

## Open Ends

Right now the backend send messages around which are completely superflous because nobody is
listening (earlier there was this persistence-job which used every message really).
So for this scenario I would like to implement kind of a 'start-protocol' for the backend.
