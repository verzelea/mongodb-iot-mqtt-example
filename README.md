# MongoDB IoT Example

## Context

The context of the solution is to provide an example of how to use IoT applications with MongoDB as a database.

Basically this is my extension of a talk given by John Page on MongoDB Days Munich 2014. It's lacking the paper, but it has more to do with the 'things' in IoT :)

In this example, data is transmitted using MQTT as broker solution.

Once published on the broker, sensor data from a mobile application gets received by a node.js application by subscription and stored in the database.

## Conventions

Requirements level are to be treated according to RFC2119 ("Key words for use in RFCs to Indicate Requirement Levels").

Timestamps must be specified as Unix Timestamp from epoch start (01/01/1970) in milliseconds.

## Component Descriptions

### Custom-developed solutions

#### Android Application (android-app)

The Android application gathers data from the mobile device's accelerometer and publishes it onto a MQTT broker.

#### Persistent DB Logger (db-logger)

Component responsible for persistent storage of received data from MQTT broker.

#### Docker Server Environment (docker-image)

This is a Dockerfile for server side environment which sets up Mosquitto, MongoDB and node.js for use.

### COTS Solutions

#### MongoDB

MongoDB is to be used as datastore due to its scalability properties.

#### Mosquitto

Mosquitto is a standalone MQTT broker solution running in a hosted environment.


## Interface Descriptions

Data is exchanged by using MQTT (Message Queue Telemetry Transport) as a bus / PubSub provider.

Data is published to one or more topics.
Supported topics *must* follow the convention "device/{deviceName}/{subTopic}".

"{deviceName}" corresponds to a dynamic device name of a mobile. Here the (weak) assumption is made that the model name of the mobile device is unique enough for testing purposes.

"{subTopic}" may be one of the following:

"debug": Subtopic for debug output. Subscribed payload on persistent DB logger shall be print on screen.

"accelerometer": Subtopic for transmission of device accelerometer sensor data. Subscribed payload on persistent DB logger must be stored in database. The payload is defined as a JSON object "{ 'x' : 0, 'y' : 0, 'z' : 0 } where "x"-"z" correspond to device acceleration in the corresponding orientation axis. This payload is wrapped on-wire in a Sensor Data DTO (Data Transfer Object) which looks like "{'payload' : { ... }, 'timestamp': 0}" where "timestamp" is defined as time the sensor event was recorded and "payload" is the object structure defined above as payload.

## Data Architecture

Data is stored inside a single mongod instance or distributed among a cluster using sharding (the shard key is as of yet undefined, but will probably require an hashed index).

The received data from devices gets immediately stored in a collection 'rawData'. 

The document schema for accelerometer data contains the values for "x"-"z" as defined above, for every document a composite id consisting of the sensor data timestamp and the unique device name.

## License

### License Terms

The MIT License (MIT)

Copyright (c) 2014 - Current, Ralph Greschner

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.

### Terms of Used Libraries

#### Butter Knife

Copyright 2013 Jake Wharton

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

#### Dagger - A fast dependency injector for Android and Java.

Copyright 2012 Square, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

#### FloatingActionButton

Copyright (C) 2014 Jerzy Chalupski

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

#### Google Gson

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

#### MPAndroidChart

Copyright 2014 Philipp Jahoda

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

#### Paho - Open Source messaging for M2M

This project is dual licensed under the Eclipse Public License 1.0 and the Eclipse Distribution License 1.0 as described in the epl-v10 and edl-v10 files.

#### RxAndroid - Reactive Extensions for Android

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at

	http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.








