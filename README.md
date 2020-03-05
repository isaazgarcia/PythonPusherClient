[![Build Status](https://travis-ci.org/ekulyk/PythonPusherClient.svg?branch=master)](https://travis-ci.org/ekulyk/PythonPusherClient)

pusherclient
=============

pusherclient is a python module for handling pusher websockets

Installation
------------

Simply run "python setup.py install".

This module depends on websocket-client module available from: <http://github.com/liris/websocket-client>

Example
-------

Example of using this pusher client to consume websockets::
```
import sys
sys.path.append('..')

import time

import pusherclient

# Add a logging handler so we can see the raw communication data
import logging
root = logging.getLogger()
root.setLevel(logging.INFO)
ch = logging.StreamHandler(sys.stdout)
root.addHandler(ch)

global pusher

def print_usage(filename):
    print("Usage: python %s <appkey> <cluster>" % filename)

def channel_callback(data):
    print("Channel Callback: %s" % data)

def connect_handler(data):
    channel = pusher.subscribe("my-channel")

    channel.bind('my-event', channel_callback)
    

if __name__ == '__main__':
    if len(sys.argv) != 3:
        print_usage(sys.argv[0])
        sys.exit(1)

    appkey = sys.argv[1]
    cluster = sys.argv[2]

    pusher = pusherclient.Pusher(appkey,cluster=cluster)

    pusher.connection.bind('pusher:connection_established', connect_handler)
    pusher.connect()

    while True:
        time.sleep(1)
```

Sending pusher events to a channel can be done simply using the pusher client supplied by pusher.  You can get it here: <http://github.com/newbamboo/pusher_client_python>

    import pusher
    pusher.app_id = app_id
    pusher.key = appkey
    pusher.cluster = cluster

    p = pusher.Pusher()
    p['mychannel'].trigger('myevent', 'mydata')

Thanks
------

Built using the websocket-client module from <http://github.com/liris/websocket-client>.
The ruby gem by Logan Koester which provides a similar service was also very helpful for a reference.  Take a look at it here: <http://github.com/logankoester/pusher-client>.

Copyright
---------

MTI License - See LICENSE for details.

