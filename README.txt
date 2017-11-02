In this project we attempted to implement a transport protocol that behaves similar to the way that TCP does.
This was built on top of UDP and uses python. We started with the sample code that was provided, and worked from there.
Our plan for completing this project was to work by starting to implement TCP features that were in the original TCP
and then implement the more useful features that were introduced in later TCP protocols. We started by using positive ACKs,
meaning the reciever would send an ACK for a packet if it was the sequence number it expected and if it was not it would
send and ACK with the sequence number it expected which signals to the sender that it should resend the packet. This works 
alright but is very inefficient, because the sender ends up having to resend packets that the reciever already got. We
moved to using selective ACKs, meaning that the reciever still sends an ACK with the sequence number it is expecting,
but also the other numbers that it recieved so that the sender can send packets more efficiently. One challenge that we
faced was with the connection tear down, in an ideal scenario the sender sends a FIN packet signaling to the reciever
that they have sent all the information they plan to. The reciever then gets this FIN sends back an ACK so they both know
that they can close the connection. However if the sender's initial FIN packet is dropped (Or the recievers ACK), then they won't
recieve an ACK back and so they don't close the connection. To solve this problem we added a counter for the sender so that after
three attempts to send the FIN it closes the connection. Another issue that we had is with a network that drops 50% of the packets
we were unsure how to complete the data transfer in a reasonable amount of time. Since half the packets are dropped statistically
either the senders packet, or the recievers ACK would get dropped, leading to potentially a long loop of sending the same packets.
In a real network dropped packets often indicate congestion, and so adjusting the window size can help solve this problem. However 
in this scenario reducing the window size doesn't seem to help since half of the packets will still be dropped. We were unable to 
find an adequate solution to this problem.
When testing our implementation we made heavy use of the configuration settings on the netsim emulator provided, this allowed us
to simulate all different types of scenarios such as; high/low latency, high dropped packets, delayed packets (helpful when testing
selective ACKs), etc. Another useful tool when testing our implementation was the --live flag on the helper run script which printed
out logs as they came in so that we could visually see problems occurring such as; loops, timeouts being too long/short, which sequence
number was expected etc. In the end our implementation works well on most networks, except for those that drop a large percentage of the
packets.

