### PubSub

* Published messages are characterized into channels, without knowledge of what (if any) subscribers there may be. Subscribers express interest in one or more channels, and only receive messages that are of interest, without knowledge of what (if any) publishers there are.
* This decoupling of publishers and subscribers can allow for greater scalability and a more dynamic network topology.
* For instance in order to subscribe to channels foo and bar the client issues a SUBSCRIBE providing the names of the channels:
  ```
  SUBSCRIBE foo bar
  ```
* Messages sent by other clients to these channels will be pushed by Redis to all the subscribed clients.

1. Subscribe: means that we successfully subscribed to the channel given as the second element in the reply. The third argument represents the number of channels we are currently subscribed to.
1. Unsubscribe: means that we successfully unsubscribed from the channel given as second element in the reply.
    ```
    PUBLISH second Hello
    ```
   ```
   SUBSCRIBE first second
   ```
* The Redis Pub/Sub implementation supports pattern matching. Clients may subscribe to glob-style patterns in order to receive all the messages sent to channel names matching a given pattern.
    ```
    PSUBSCRIBE news.*
    ```
   