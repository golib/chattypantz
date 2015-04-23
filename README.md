# chattypantz
[![License MIT](https://img.shields.io/npm/l/express.svg)](http://opensource.org/licenses/MIT)
[![Build Status](https://travis-ci.org/composer22/chattypantz.svg?branch=master)](http://travis-ci.org/composer22/chattypantz)
[![Current Release](https://img.shields.io/badge/release-v0.1.0-brightgreen.svg)](https://github.com/composer22/chattypantz/releases/tag/v0.1.0)
[![Coverage Status](https://coveralls.io/repos/composer22/chattypantz/badge.svg?branch=master)](https://coveralls.io/r/composer22/chattypantz?branch=master)

A demo chat server and client written in [Go.](http://golang.org)

## About

This is a small server and client that demonstrates some Golang network socket functions and features.

Some key objectives in this demonstration:

* Clients connect to the server on ws://{host:port}
* Messages sent by a client are broadcasted to other clients connected to the same chat room.
* The server only supports JSON text messages. Binary websocket frames will be discarded and the clients sending those frames will be disconnected with a message.
* When a client connects to a chat room, the server broadcasts "{nickname} joined the room." to clients that were already connected to the same chat room.
* When a client disconnects, the server broadcasts "{nickname} has left the room." to clients connected to the same chat room.
* An unlimited amount of chat rooms can be created on the server (unless it runs out of memory or file descriptors).
* An unlimited amount of clients can join each chat room on the server (unless it runs out of memory or file descriptors).

Additional objectives:

* Only one connection per nickname allowed per chat room
* When the server runs out of memory it shouldn't crash and instead start disconnecting clients to release system resources.
* The server should provide an idle timeout option.  If a user doesn't interact withing n-seconds, the user should be automatically disconnected.
* Chat history for each room should be stored in a file for each chat. When the user logs in to a room, the history should be provided to the client. A max history option should be provided.

For TODOs, please see TODO.md

## Usage

```
Description: chattypantz is a chat server allowing clients to text each other within rooms.

Usage: chattypantz [options...]

Server options:
    -N, --name NAME                  NAME of the server (default: empty field).
    -H, --hostname HOSTNAME          HOSTNAME of the server (default: localhost).
    -p, --port PORT                  PORT to listen on (default: 6660).
	-L, --profiler_port PORT         *PORT the profiler is listening on (default: off).
    -n, --connections MAX            *MAX client connections allowed (default: unlimited).
    -r, --rooms MAX                  *MAX chatrooms allowed (default: unlimited).
    -y, --history MAX                *MAX num of history records per room (default: 15).
    -i, --idle MAX                   *MAX idle time in seconds allowed (default: unlimited).
    -X, --procs MAX                  *MAX processor cores to use from the machine.

    -d, --debug                      Enable debugging output (default: false)

     *  Anything <= 0 is no change to the environment (default: 0).

Common options:
    -h, --help                       Show this message
    -V, --version                    Show version

Examples:

    # Server mode activated as "San Francisco" on host 0.0.0.0 port 6661;
	# 10 clients; 50 rooms; one hour idle allowed; 2 processors
    chattypantz -N "San Francisco" -H 0.0.0.0 -p 6661 -n 10 -r 50 -i 3600 -X 2

```
## Client Connection and demonstration

The /client directory in the project contains a web page that can be used to demonstrate
how a client application can connect to a running server. This demonstration is writtern in
angular.js and javascript.

Simply load the chattypantz.html file in your browser.

The socket connection endpoint is:
```
ws://{host:port}/v1.0/chat
```
Following are some examples for using Chrome/Dark Websocket.
Spaces must be encoded in JSON calls.

```
# Establishg a connection
/connect ws://127.0.0.1:6660/v1.0/chat

# Register a nickname on the server.
# ChatReqTypeSetNickname = 101
/send {"reqType":101,"content":"ChatMonkey"}

# Get your nickname from the server.
# ChatReqTypeGetNickname = 102
/send {"reqType":102}

# Get a list of Chat Room names.
# ChatReqTypeListRooms = 103
/send {"reqType":103}

# Join a room or join a room with hidden name.
# ChatReqTypeJoin = 104
/send {"roomName":"Your\ Room","reqType":104}
/send {"roomName":"Your\ Room","reqType":104,"content":"hidden"}

# Get a list of nicknames in a room.
# ChatReqTypeListNames = 105
/send {"roomName":"Your\ Room","reqType":105}

# Hide your name in a room list.
# ChatReqTypeHide = 106
/send {"roomName":"Your\ Room","reqType":106}

# Unhide your name in a room list.
# ChatReqTypeUnhide = 107
/send {"roomName":"Your\ Room","reqType":107}

# Send message to the room.
# ChatReqTypeMsg = 108
/send {"roomName":"Your\ Room","reqType":107,"content":"Hello world!"}

# Leave a room.
# ChatReqTypeLeave = 109
/send {"roomName":"Your\ Room","reqType":108}

# Disconnect from the server
/disconnect

```

## Building

This code currently requires version 1.42 or higher of Go.

Information on Golang installation, including pre-built binaries, is available at
<http://golang.org/doc/install>.

Run `go version` to see the version of Go which you have installed.

Run `go build` inside the directory to build.

Run `go test ./...` to run the unit regression tests.

A successful build run produces no messages and creates an executable called `chattypantz` in this
directory.

Run `go help` for more guidance, and visit <http://golang.org/> for tutorials, presentations, references and more.

## Docker Images

A prebuilt docker image is available at (http://www.docker.com) [chattypantz](https://registry.hub.docker.com/u/composer22/chattypantz/)

If you have docker installed, run:
```
docker pull composer22/chattypantz:latest
```
See /docker directory README for more information on how to run it.


## License

(The MIT License)

Copyright (c) 2015 Pyxxel Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to
deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
IN THE SOFTWARE.