# Griffon Client

A Python Client for Griffon workflows.

Griffon is a message queue for easily building workflows.  A griffon server acts a hub, connecting griffon clients.  Clients connect and kickoff tasks within a workflow, then relay the results when they are done.  Griffon tracks dependecies as clients connect, and provides a powerful architecture for developing against, debugging, and running complex workflows.


## Getting Started


### Prerequisites

Griffon clients require a running Griffon server, see installing and running a [Griffon Server](https://github.com/AugustBrenner/griffon-server-node). It's a snap to set up.

### Installing

```
pip install griffon_client
```

### Running

The following is a simple example of useage

```
import griffon_client


operator = griffon_client.connect(
    uri='http://localhost:3001',
    username='admin',
    password='password',
    environment='august-local',
    produce=['topic1', 'topic2'],
    operator='test1',
    channels=['.*'],
    consume=['topic1', 'topic2'],
    gather=False,
)


@operator.consume
def on_consume(task):

    if 'topic1' in task.topics:

        print(task.data)

        task.produce(
            topic='topic2',
            data='hello2',
        )

    if 'topic2' in task.topics:

        print(task.data)

print('Producing')
operator.produce(
    topic='topic1',
    channel='dev',
    data='hello1',
)
```
