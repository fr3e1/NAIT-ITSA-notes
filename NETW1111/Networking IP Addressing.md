---
cssclasses: [netw1111]
---
## The Seven OSI Layers
Is a general model that people use to struture their networks. This model however, while it's not being directly adapted or replace the precaursor, TCP/IP, is later incorperated into TCP/IP due to people wanting to stick to the already existing and working system. So rather than switching to a new system entirely, the system's structure is being used to construct future TCP/IP networks.

THe OSI model has 7 different layers in which each parts of the layer coorespond to a protocol that is either offline and internal or online and external. I will arrange the list from the bottom up Actually because it makes more sense that way.

To Explain this, I will bring up a scenario of when I type in `Hello World!` into the address bar on Brave Browser. 

1. **Application Layer:** When I type in `Hello World` and press *Enter*, the application, in this case, `brave-browser`, will run a check to gather the data that the user entered. Comparing this to a Godot function, it can be interpreted as `on_button_pressed(func(): client_requested.emit(self, RequestDictionary))`. There are 2 parts to this preparation phase, gathering the information and packaging. With the Godot function earlier, the part within `on_button_pressed()` is the data that is being prepared and sent out, which you can see, is the `self` and `RequestDictionary`. DNSing also happens on this level.
2. **Presentation:** The package that was prepared in the Applicaction Layer is then converted into bytes as binary and making sure that this data is transferable and computer-readable. Generally, if you want to send something in `TCP` or `UDP` they use `extended ascii` which allows them to batch those binaries earlier in groups of 8 (Learn more about [[Extended ASCII]]). Otherwise if it is sended in `FTP`, we generally don't care what format it's in.  So we have now prepared the information to present it to the computer and hopefullt it understands.
3. **Session:** Now in a network, there are many different hosts. And when you have different hosts connected to a network, how can differentiate between different hosts and their data? This layer, attaches a `session-cookie` to a host's data, in this case, me who's sending out `Hello World` on `bravve-browser`.  And for an example if you we to log in to Facebook at home, the login information has to go through the ISO model process **without a session cookie**, but once the server at Facebook replies to you and sends information back, it will send the packet with the cookies attached to it so that when you launch facebook again on a different network (cafe, or school, etc.) you will still be remembered and logged in. Thus, this second time you send information to the server, your `session-cookie` will be attached with the packet you're sending. Learn more about [[Session Cookies and how I've been looking at it wrong]].
4. **Transport:** So now regardless of us having a cookie or not, we will have to choose how we want to send our data. Some common methods include [[TCP and UDP]], we will get into TCP/IP later, I swear... So let's say we choose TCP, while it's slow, since we're executing a command or generating a search query here, we want to not lose any data and the server return with a jumbled mess. So by wrapping our packet with TCP, it is now called a segment (fancy way of saying package). We'll need to also choose which port we want to send out this `segment` through and this is the port on my computer. Just as a general use of browsing the web nowadays, we use `https` which goes out through `port 443`.  Finally, the package of `Hello World!` has left our computer.
5. **Network:** Generally, when we connect to the internet, we have to route that connection through a router, but not necessarily. Whatever the case is, the networking phase basically, is taking the information from our host, the computer, and sending it off to the destination. Essentially, taking the `segment` from the host at `192.168.1.100:443` with the MAC address of `06-2a-3b-45-3f-21` and sending it off to the switch at `10.0.0.1` as an example.
6. **Data Layer:** Currently our data look somthing like this `192.168.1.1:443 [imagine information]`. Now the reason why I wrote `imagine information` is because the switch actually don't have any clue of what is being sent, but it knows the start point and an end point. In this case, from the router `192,168,1,1:443` to `duckduckgo.ca`(This is a CNAME, which will learn more in [[I finally got comfortable with DNS]]). Now the switch turns this data into electricity and wrap it in another packaging which we now call a `frame` (fancier, I guess).
7. **Physical Layer:** The `frame` earlier, is then being sent through a physical fiber optic cable or any other of of cables that connects different internet service providers with different countries all around the world. Welcome to the internet, where wires of electricities are transferring massive amount of data through the bottom of the ocean.

Nowwwwwwwww, Imagine the server on the other side, receiving this package and going through layer by layer, peeling off the stamps, the packaging, from `frame` to `segment` to binary to converting it into human readable data that the server creates and has to send back through the same process of wrapping it up and sending it back to the user, my computer, and for it to unwrap the whole thing back up and display the search results I really wanted. 

Crazy to go through all of that, just for one single search `Hello World!`, but computers are doing this at an insanely fast speed and rate. I now learn to appreciate the world of the internet even more!

## Anyways, here's TCP/IP
So we all love TCP/IP even if we boast about the OSI model... I don't mind it. Basically the Presentation Layer and Session Layer is being combined into one Layer as an **Application Layer**. This is due to these layers happen on the computer and practically is doing bits of the same thing, preparing and sending data. But funny how they still call the Application Layer in TCP/IP as the 7th layer still.

There are 2 types of TCP/IP models, a 4 Layer TCP/IP model and a 5 Layered one. The 4 Layered one, combines the Data Layer and the Physical Layer into one Layer, since practically those 2 layers are happening at the same place. Just the switch receiving and the the switch converting data into electricity. The 5 Layer Model only groups the Application Layer together and leaves the Data Layer and the Physical Layer Separate.

Regardless of which model of TCP/IP you're thinking of or even OSI model you're thinking of, they are all essentially the same, just different ways to show abstract data in an understandable way.

![[Different Layers in Networking.png]]


## General IP Addressing
So today I learn that vLAN is just the same physical LAN being sectioned off into a group. The premise doesn't change, but it just has a fake virtual group. The weird thing is, what we refer as clients are called hosts here and the hosts are considered as server. 

### IPv4 Addressing Conundrum
The lower amount of bits, Class C is generally used is just for organizational purposes. So lets say 192.168.1.0/24, We are allowed to use 0 - 255 on the last 8 bits to assign to any devices on a given network. We can have a private network with a Class A Addressing but it's not really necessary.

Apparently, different classes have different ranges as a general guideline, but that's for the public network, which we can no longer assgin new IP addresses. Privately though, we can technically assign whatever we want at any range.

![[IPv4 Classes.png]]

Class D E is generally used for testing, but no one really ever uses that range for anything. Technically, in a private network you can have a little more control ovver what you want as a test server and what not so we don't have to reserve an IP range to do testing.



127.0.0.0 is generally reserved for internal connection. Below is a list of internal only private addresses. We can see this with the current SHAW F916 router at Horizon Residence where we're using 10. 0.0.1 as our router.

![[IPv4 Classes locally or private.png]]


### IPv6 Addressing Conundrum
Will do research later...

### Ping
Packet Internet Groper....




---
## Discarded
### But I didn't want to just throw away all of my thinking

1. **Physical:**  Responsible for transporting data in the physical world generally I see this as being used in an office space or places where it can be linked with fiber optic or an ethernet cable.


2. **Data Link:**

---

1. **Application:** User types in something. Think of it as 
	`on_button_pressed(func(): client_requested.emit(self, RequestDictionary))`
	This a perfect representation because there's 2 parts to the Application Layer. Preparation and Sending out. Similar to our snippet above, we can see the event that triggers the function and within that bracket, `client_requested.emit(self, RequestDictionary)`, We are preparing what to send out to the server. In the beracket of those brackets, `self, RequestDictionary`, this is the data that is being sent out to the server when we call `emit()`.
2. **Presentation:** The url is being liooked into and decided. Think of it as  `func Check_State(user : HttpsUser):` 
3. **Session:** `if user is {insert_condition}:` 
4. **Transport:** There are generally two methods of transporting data, [[TCP and UDP]].  Now that you have chosen your mode of transportation, let's say TCP, you need to choose which gateway to exit out of. Some gateways accept cars and some accepts trucks. Regardless of which method you're using, the package is always wrapped up, encapsulated before going through port 80 (http) or port 443 (https) and be sent off to where ever. 
5. **Network:** 
6. Data Link layer
	 MAC - Media Access Control a unique identifier for any one device
	 LLC - Detecting transmission errors when they occur
7. **Physical:** 