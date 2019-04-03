# Asyncio Prototyping

Some experimentation of asyncio + rospy + aiohttp and other concepts.


## Dependencies:

Depends on Python 3.7, which includes significant asyncio work.

`requirements.txt` is used by `catkin_virtualenv` so you don't need to do anything manually with it. But you can
`pip install -r requirements.txt` into a virtualenv of your own during development to help your IDE/editor.

## How To Use:

1. Clone the repo.
2. Make package.xml dependencies available (ie. catkin_virtualenv)
3. `catkin build`
4. `roslaunch asyncio_prototyping queued_pubsub.launch`


## Coroutine debugging.

### aiomonitor

A little experiment on how to monitor coroutines.

https://aiomonitor.readthedocs.io/en/latest/

The documentation looks quite janky and rough. Errors throughout. But it seems to work. I think it's a good idea,
but probably not literally what we need. We want to do reflection internally, not via. telnet.  Nevertheless:

1. roslaunch the demo
2. `nc localhost 50101`  (https://aiomonitor.readthedocs.io/en/latest/tutorial.html#connection-over-telnet)
3. Once telnetted, call `ps` or `help` etc. to see the current state of coroutines.

Notice that we hook up `aiomonitor` with a context manager. So it's pretty simple. But what it tells us is pretty
hard to parse without studying it. I think we can do better...

### Humanized debugging

Imagine that we can look at a live debug console and see an output like:

| Coroutine            | Awaiting Line | Idle Time | Label                                                |
|----------------------|---------------|-----------|------------------------------------------------------|
| start_webserver_coro | 53            | 309s      | waiting for HTTP request                             |
| monitor_coro         | 60            | 3s        | 5 second sleep between logs to monitor.              |
| summation_coro       | 70            | 2s        | Waiting for a message to come in on `/sum` rostopic. |

This would let us see at any point where all our coroutines are.  We might even want to be able to report if at any given instance, a coroutine isn't awaiting, it's actually performing synchronous python. Maybe this is done by reporting its last await. Ie. "Last seen at...". If done properly, any non-awaiting coroutine should take fractions of a second to process through a block before it hits another await.
