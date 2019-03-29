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
