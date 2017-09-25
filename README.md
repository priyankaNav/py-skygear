![Skygear Logo](.github/skygear-logo.png)

# Python plugin runtime for Skygear

[![PyPI](https://img.shields.io/pypi/v/skygear.svg)](https://pypi.python.org/pypi/skygear)
[![Build Status](https://travis-ci.org/SkygearIO/py-skygear.svg)](https://travis-ci.org/SkygearIO/py-skygear)
[![License](https://img.shields.io/pypi/l/skygear.svg)](https://pypi.python.org/pypi/skygear)

When the Skygear Server calls your plugin, `py-skygear` will take the plugin message and calls the appropriate function automatically.

## Python Cloud Functions
`py-skygear` is useful for running Python Cloud Functions to work with [skygear-server](https://github.com/SkygearIO/skygear-server); `skygear-node` is the counter part of py-skygear but for Javascript.

You can read more about how to write Skygear Cloud Functions in [Skygear Guides](https://docs.skygear.io/guides/cloud-function/intro-and-deployment/python/). You don't need py-skygear if you're using skygear.io and don't plan to deploy it on your own.

## Install py-skygear

You may want to read [Setup Skygear Development Server Locally](https://docs.skygear.io/guides/advanced/server/) before installing py-skygear (and understand why you might or might not need `py-skygear`)

Install skygear by using pip. py-skygear requires Python 3.

```
$ pip3 install skygear
```

Alternatively, you can install `py-skygear` from source by cloning `py-skygear` from this official repository.

## Development

Add all the cloud functions in `plugin.py` file.

Skygears support three kind of protocol for different use case, make sure you add support to all of them or raise appropriate exception.

Supported protocols: `exec`, `http` and `zmq`. You can use any one of them to start with.
 

If you want to use zmq, you need to install pyzmq, and respective cbinding.
You can install via homebrew in OSX `brew install zeromq`

During development, the easiest way is to debug your plugin using command line:

```
export DATABASE_URL=postgres://127.0.0.1/skygear?sslmode=disable
export PUBSUB_URL=ws://localhost:3000/pubsub
echo "{}" | py-skygear sample.py --subprocess init
echo "{}" | py-skygear sample.py --subprocess op hello:word
py-skygear sample.py --subprocess handler chima:echo < record.json
py-skygear sample.py --subprocess hook book:beforeSave < record.json
echo "{}" | py-skygear sample.py --subprocess timer plugin.generate_monthly_report
```

Or you may run a long running process that hook with your own skygear-serve
instance.

Create a database named skygear with user/owner other then root.

* For ZeroMQ
```
DATABASE_URL=postgresql://localhost/skygear?sslmode=disable \
py-skygear sample.py \
--skygear-address tcp://127.0.0.1:5555 \
--skygear-endpoint http://127.0.0.1:3000 \
--apikey=API_KEY
```

* For Http
```
DATABASE_URL=postgresql://localhost/skygear?sslmode=disable \
py-skygear --http --http-addr 0.0.0.0:8000 \
--skygear-endpoint http://127.0.0.1:3000 \
--apikey=API_KEY
```


## Support

For implementation related questions or technical support, please refer to the [Stack Overflow](http://stackoverflow.com/questions/tagged/skygear) community.

If you believe you've found an issue with Skygear JavaScript SDK, please feel free to [report an issue](https://github.com/SkygearIO/py-skygear/issues).


## How to contribute

Pull Requests Welcome!

We really want to see Skygear grows and thrives in the open source community.
If you have any fixes or suggestions, simply send us a pull request!


## License & Copyright

```
Copyright (c) 2015-present, Oursky Ltd.
All rights reserved.

This source code is licensed under the Apache License version 2.0 
found in the LICENSE file in the root directory of this source tree. 
An additional grant of patent rights can be found in the PATENTS 
file in the same directory.

```
