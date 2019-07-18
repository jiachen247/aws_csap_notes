# Application Integration

## AWS Simple Queue Service
- used to integrate and decouple application
- 256 kb limit
- can use s3 to store data
- acts as a broker
- messages can be deleted via the message handler 

Polling Types
- short polling
- long polling (prefer less api calls)

Type of queues
- Standard 
- FIFO (scaling limit)

Visibility Timeout
- if not deleted after this time
- it will return to the queue

Retention period
- how long it stays without being processed

Dead Letter Queue
- messages are sent fail after faling too many time

Workers pop process and then delete them from the queue

Autoscaling groups can be used to scale workers of the size of the queue

Anti Pattern
- not good for replaying messages
- one to one
- use kinesis or sns for multiple consumers

## Amazon Simple Notification Service
- pub sub architecture
- one producer many consumers
- really good integration with other aws services
- can fan out to sqs queue
- can use for push notifications

## Amazon MQ
- based on Apache ActiveMQ
- discrete use case
- standard complant message broker
- jms amqp stomp open wire mqtt
- vpc level
- can be HA with multiple broker
- complex topologies
- use if need a global messaging queue implmentation

## Workflows

### Step Functions
- newer aws service

State Machine
- contains state
- choice state
- fail and succeed state
- wait state
- parallel state
- task state - actually does the work
- can have long running state - up to a year
- can use pre made templates

### Simple Workflow Service (SWS)
- old and oldated
- dosent do what it says it does
- in no way simple
- dont use anymore
- prefer step functions
- a flow is a sequence of steps
- activity tasks 
- decision or decider tasks
- just know that it exist