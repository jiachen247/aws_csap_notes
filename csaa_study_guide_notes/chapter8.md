# SQS, SNS and SWF
Application and Mobile Services
section of the aws cloud

> Amazon SQS is a fast, reliable, scalable, and fully managed message queuing service. Amazon
SQS makes it simple and cost effective to decouple the components of a cloud application.

The point of queues are for fault tolerance and decoupling systems

## SQS
fully managed messaging queue

FIFO Queues and Standard Queues
messages might not be processed in order for standard queues but are cheaper.

### Message Lifecycle
1. Component 1 sends Message A to a queue, and the message is redundantly distributed
across the Amazon SQS servers.
2. When Component 2 is ready to process a message, it retrieves messages from the queue,
and Message A is returned. While Message A is being processed, it remains in the queue
and is not returned to subsequently receive requests for the duration of the visibility
timeout.
3. Component 2 deletes Message A from the queue to prevent the message from being
received and processed again after the visibility timeout expires.

Note that during processing, message remains in the queue

### Delay Queues
delays the message to the queue
can set normal queues to delay via setting an attribute

### Visibility Timeout
When a consumer receives and processes a message from a queue, the message remains in the queue. Amazon SQS doesn't automatically delete the message. Because Amazon SQS is a distributed system, there's no guarantee that the consumer actually receives the message (for example, due to a connectivity issue, or due to an issue in the consumer application). Thus, the consumer must delete the message from the queue after rv and processing it.

### Operations
- ListQueues
- DeleteQueue
- SendMessage
- SendMessageBatch
- ReceiveMessage
- DeleteMessage
- DeleteMessageBatch
- PurgeQueue
- ChangeMessageVisibility
- ChangeMessageVisibilityBatch
- SetQueueAttributes
- GetQueueAttributes
- GetQueueUrl
- ListDeadLetterSourceQueues
- AddPermission
- RemovePermission

messages are identified by a global unique id

queue URLs, message
IDs, and receipt handless

queue name has to be unique among all queues

deletion from queue via reciept handle 

Message Attributes to stored meta data attirbutes

**Long Polling**
WaitTimeSeconds argument to ReceiveMessage of up to 20 seconds.
and return after the number of seconds instead of calling 20 times in 20 seconds

**Dead Letter Queues**
useful can send here if any errors are raised

**Access Control**
IAM for standard controls

Amazon SQS Access Control 
for cross account access without roles
also written in json

## SWF

need to redo swf

> Amazon SWF makes it easy to build applications that coordinate work across distributed components.

you implement workers to perform tasks

Workflows
supports sync and async processes

Domains
everything interacting has to be in one domain
can have multiple workflows in one domain

**Workflow History**
like a history / audit log of events


**Actors**
The basic building block

Actors can be workflow starters, deciders, or activity workers. 

Starters - somthing that kicks start the workflow through the api

Activities - are the things that run

Decider - make decisions on control flows based of results from activities. Calls activity with input

activity worker - single compute proccess running the activity

**Tasks**
a unique task token
activity tasks, AWS Lambda tasks, and decision tasks.


> A decision task tells a decider that the state of the workflow execution has changed so that
the decider can determine the next activity that needs to be performed. The decision task
contains the current workflow history.


**Task Lists**
are like tasks queues 
activity workers look in here for jobs
handles scheduling

**Long Polling**
Deciders and activity workers communicate with Amazon SWF using long polling

**Object Identifiers**

Return
After you start a workflow execution, it is open. An open workflow execution can be closed as
completed, canceled, failed, or timed out

## SNS

> Amazon SNS is a web service for mobile and enterprise messaging that enables you to set up,
operate, and send notifications.
- Pub Sub paradigm

send notifications to topics

topics have subscribers

Common use Cases

**Fanout**
one topic broadcast to multiple sqs queues / http endpoints / listeners
allows parallel execution

can be used to replicate data used in prod to dev via multiple sqs queues

**Application and System Alerts**
does what it says
good integration with many aws services
gan trigger if some service treshhold is reached


**Push Email and Text Messaging**
does what it says

**Mobile Push Notifications**
works with moble plateforms

ADdtional re

https://www.youtube.com/watch?v=sXGlQruUrWE