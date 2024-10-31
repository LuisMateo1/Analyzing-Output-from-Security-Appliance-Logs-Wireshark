# Analyzing Output from Security Appliance Logs: Wireshark
<h3>Objectives</h3>

- Analyze data as part of security monitoring activities.
- Analyzed potential indicators of compromise.
- Utilized basic digital forensics techniques.
#

<h3>Analyzing a Wireshark Packet Capture Output</h3>

![image](https://github.com/user-attachments/assets/467922dc-9d40-4769-af23-4f1f6460f953)

- The top pane allows us to see individual packets, and information like source/destination IP address, size/length of the packer, and protocol used.
- The middle pane allows us to read the packet header information
- The bottom pane essentially shows the same information as the middle pane but in hexadecimal form. 
  - Each line has 16 bytes of information, the first packet is only 60 bytes in length so the last line is missing 4 bytes
#
<h3>Capture File Properties</h3>
Capture file properties show information about the host and interface that recorded the capture. While it's not much information, if you need to analyze multiple captures this can help differentiate between them.
This shows 2091 packets were captured over 2.99 seconds.

![image](https://github.com/user-attachments/assets/df01d77e-2a04-4e7b-894f-ff6561d6c6f9)
#
<h3>Expert Information</h3>

Instead of analyzing a capture, packet by packet, Wireshark expert information analysis will show a list of anomalies and all the packets linked to those anomalies.
This is a way to get a head start on analysis or even find other issues than what you were looking for.

![image](https://github.com/user-attachments/assets/4627e12c-ae97-4d73-ac06-b08b004ed0ca)

![image](https://github.com/user-attachments/assets/2d35d78d-ae78-4d12-a502-6852af64e893)
#
<h3>Protocol Hierarchy</h3>
Protocol hierarchy lets us see what protocols were predominantly used in this capture. 

![image](https://github.com/user-attachments/assets/ceff1ddc-ef2b-4175-adb1-f93a8ad30157)

For example in this packet capture, 99.9% of the packets were TCP, which aligns with what was seen on the expert information tab, with all the established and reset connections.

![image](https://github.com/user-attachments/assets/66a02841-bb15-466d-a026-84723d47617a)

For this 2nd capture, there are a similar amount of TCP packets, however, a connection was made over HTTP port 80. And because HTTP is cleartext we can see the data sent.

![image](https://github.com/user-attachments/assets/5918bfbb-34ac-4b79-8b3f-a2b6136f2c07)
![image](https://github.com/user-attachments/assets/e31e3a0e-163e-408f-8092-d7798d0ab33a)
#
<h3>Conversations</h3>
Allows you to see if a connection was made between two systems, how long a connection lasted, the number of packets sent, and the size of the packets sent. 

![image](https://github.com/user-attachments/assets/972af0a9-395e-487f-a5e3-f098c09d3399)
![image](https://github.com/user-attachments/assets/637b2125-78aa-4a0e-bc0e-9bb089122885)
![image](https://github.com/user-attachments/assets/00c56c4d-b518-4ee6-9ca7-01a917b70945)
#
<h3>Follow Stream</h3>
The follow stream tab lets us see the data that was transmitted. There's the choice of TCP, UDP, HTTP, etc. but unless the packet capture contains those packets they will not be inspectable.
In the case of this 1st packet capture, it was just a bunch of TCP connections, and no data was actually transmitted in the 3 seconds that packets were being captured, so it would be an empty window.

![image](https://github.com/user-attachments/assets/2839c9af-f66d-4718-882a-cb4cecda843c)

In the second capture data was transferred over HTTP, and because HTTP is a TCP protocol following either stream will show the same data.
![image](https://github.com/user-attachments/assets/b4097948-4a88-41b9-a359-1c2d0669cad3)

Following the HTTP stream will show that a connection was made to a server running Metasploitable2
![image](https://github.com/user-attachments/assets/e31e3a0e-163e-408f-8092-d7798d0ab33a)
#
**Packet Capture 1 Analysis:**
Based on the information seen in the images above packet capture 1 is an example of an attacker scanning for open ports on the target system, based on the large amount of TCP traffic noted by using the 
protocol hierarchy tab, coupled with the short duration of the connections found by using the conversations tab, which also shows the packets were only sent one way: from 10.39.5.2 to 10.39.5.6
- The expert information window showed there were many connection requests and resets, so as soon as the port was either deemed open or closed the connection would be terminated via a RST packet. So the attacker might have used the -sS flag on nmap to half-open connections and terminate them if the port was open, and if the port was closed the target system would send a rst, ack back to the attacker

Protocol Hierarchy

![image](https://github.com/user-attachments/assets/5eaa13f0-7849-42df-b40b-cc1e48572782)

Conversations

![image](https://github.com/user-attachments/assets/96d659a9-484e-4fca-a299-f49044b64306)

Expert Information

![image](https://github.com/user-attachments/assets/97809489-1f3e-4f9c-b73f-e3980f9a691d)

#
**Summary: Wireshark is a packet capture tool, that can be used to analyze the connections made and data transferred between two hosts. The tabs I looked at ways to simplify the information and make analysis faster/simpler however it 
doesn't completely replace a proper investigation.**








