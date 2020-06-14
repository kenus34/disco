### Pipelining

* Redis is a TCP server using the client-server model and what is called a Request/Response protocol.
* Clients and Servers are connected via a networking link. Such a link can be very fast (a loopback interface) or very slow (a connection established over the Internet with many hops between the two hosts).
* Whatever the network latency is, there is a time for the packets to travel from the client to the server, and back from the server to the client to carry the reply.This time is called RTT (Round Trip Time).
*  For instance if the RTT time is 250 milliseconds (in the case of a very slow link over the Internet), even if the server is able to process 100k requests per second, we'll be able to process at max four requests per second.
* A Request/Response server can be implemented so that it is able to process new requests even if the client didn't already read the old responses. This way it is possible to send multiple commands to the server without waiting for the replies at all, and finally read the replies in a single step.This is called pipelining.
* Pipelining is not just a way in order to reduce the latency cost due to the round trip time, it actually improves by a huge amount the total operations you can perform per second in a given Redis server.
* This involves calling the read() and write() syscall, that means going from user land to kernel land. The context switch is a huge speed penalty.
* When pipelining is used, many commands are usually read with a single read() system call, and multiple replies are delivered with a single write() system call.
* The number of total queries performed per second initially increases almost linearly with longer pipelines, and eventually reaches 10 times the baseline obtained not using pipelining.
