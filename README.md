# AWS IoT demo project
## Overview
This project uses a Linux shell script (imitating an IoT edge device) to
  - send TLS data messages (CPU utilization) to the MQTT broker in AWS IoT Core service,
  - save the data in Timestream, then
  - display the information in Grafana.

<center><img src="assets/AWS IoT Core demoThing architecure.JPG" alt="architecture" width="75%" /></center>

The Linux shell script (start.sh)
  - checks/loads needed software and dependencies
      - Python
      - AWS certificates
      - AWS Device SDK for Python
  - runs a sample Python pubsub script (aws-iot-device-sdk-python-v2 > samples > pubsub.py) to
      connect to the AWS IoT MQTT endpoint with authentication and topic
      send a 'canned' message (repeated once per second).

AWS IoT console
  - create a thing, selecting Python for the 'connection kit' (shell script, certificates, sdk above) to create the virtual edge device.
  - data from the running edge device can be seen in the MQTT client
    topic: 'sdk/test/python' (set in pubsub.py)

Amazon Timestream console
  - create database with table to hold data from the edge device
  - set data retention periods

AWS IoT console
  - create rule to
      transform (if necessary) edge device data with SQL
      action to send data to Timestream DB/table
      add dimensions as desired (ex. timestamp [note tutorial timestamp unit should be SECONDS])
  - create role for Timestream to use rule

Amazon Managed Grafana

Source: https://www.youtube.com/watch?v=z8T4hAERuOg
