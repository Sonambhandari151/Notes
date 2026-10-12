A lot of data is being transmitted by tcp Layer 4 to our computer using tcp but who will it manage which data is for which app/ website.

For example when TCP is receiving 
![[Pasted image 20240816164212.png]]

at port 443 there are so many applications that need data that is being transmitted on tcp. Session layer manages session for all these data that is being recieved so one conversation that is going between you and a website is not interfered by your video call or other website.


Session layer is not present in OS unlike the previous all layers that is present in OS. So session layer is responsible for creating different session and work on that. 

One more thing that we need to keep in mind is session layer is also responsible for managing the sessions and restoring the session in case something bad happened. 


In session layer we are going to 
- set up a session - {Check user credential}
- maintain a session
- tear down a session

for example if we connect to a wbsite it is going to create a session and when we transmit the data it is going to maintain the transmission but if we close the connection it is going to tear down the session.


If there is a break in connection, it is going to reestablish the connection

Acknowledge receipt, going to acknowledge if the data is recieved or not(Shouldn't it be in tcp)

before tearing down a session it takes acknowledgement of both server and client or other party dissapears

### Examples
- H.323
  Used to set up, maintain and tear down voice and video connections
- RTP (Real time transfer protocol)
  Streaming audio/video (phone call or face time session)
- NetBIOS 
  Used to share file over a network




