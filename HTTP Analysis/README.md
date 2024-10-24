# SCENARIO
This directory contains resources related to the analysis of **HTTP** traffic. It includes the `http.cap` file for packet capture, the `download.html` and `ADVERTISEMENT.html` file demonstrating an **HTTP GET** request, and screenshots illustrating how to extract insights from the captured data. This analysis aims to enhance understanding of **HTTP** protocols and their behavior in network communications.

# TOOLS USED
**Wireshark**, **MaxMind GeoIP**

# WORK
We have to analyze captured packets in the `http.cap` using **Wireshark**. HTTP packets are available as `HTTP pac capture.csv` data.

# WORKFLOW
All network packets are displayed in the top frame, showing details like *time*, *delta time*, *TCP stream time*, *source*, *destination*, *protocol*, *packet length*, *bytes in flight for TCP*, along with detailed *request information*. In the bottom-left frame, the selected packet's details are shown, while the bottom-right frame displays them in hexadecimal format.

![(HTTP.CAP) IN WIRESHARK DURING ANALYSIS](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/(HTTP.CAP)%20IN%20WIRESHARK%20DURING%20ANALYSIS.png)

## Filtering
We can see a filter over in the **Wireshark** there where we can apply filters like:
### Filter out perticular IP address
We can filter `Ip.addr == [ip address]` to display all network packets that involve a specific IP address, either as the source or the destination.
### Filter out perticular network protocols
for **HTTP** protocol filter `http`

for **TCP** protocol filter `tcp`
### Filtering the frame number of the corresponding HTTP request for a given HTTP respons
Essentially, when we are analyzing a response, the `http.request_in` field helps you quickly identify which request prompted that response, linking the two together.

Now as we are interested to watch out what is going on `HTTP` traffic we will select the filter to the
````
http
````
Then we will go through the **Info** column to see if there is any `HTTP-GET` or `HTTP-POST` request is going on or not.

![93acab91-a477-44cb-a764-8155908f542a](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/FILTERING%20HTTP.png)

If we find any get request as in this we can find out 2 (Two) `GET` request we will go to 
#### Step-by-Step
>File
>>Export Objects
>>>HTTP

Then we will find out the objects that we can save into our systems.

<img src="https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/EXPORTING%20OBJECTS.png" height="526 px" width="429 px" /> <img src= "https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/OBJECTS.png" height= "526 px" width= "574 px"/>
Now we have successfully saved `download.html` into our systems.
![60d47dcf-e858-4ccf-ac05-5f07cba2eaf7](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/OBJECT%20OBTAINED%20BY%20HTTP%20GET%20METHIOD.png)

Similarly we can save the advertisement also as `ADVERTISEMENT.html`, Now we can view the `download.html` and the `ADVERTISEMENT.html` file, analyze it for further use.
#### Overview of download.html
![60d47dcf-e858-4ccf-ac05-5f07cba2eaf7](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/(DOWNLOAD.HTML)FILE%20OVERVIEW.png)

#### Overview of Advertisement.html

![60d47dcf-e858-4ccf-ac05-5f07cba2eaf7](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/(ADVERTISEMENT.HTML)FILE%20OVERVIEW.png)

Now we can also analyze the **I/O graph** for that we will move into 
>Statistics
>>I/O Graphs

![Selecting I/O graph](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/SELECTING%20I-O%20GRAPHS.png)

Then analysis the traffic for packets
#### Overview of I/O graph
![Overview of I/O graph](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/I-O%20ANALYSIS.png)

Also we can download for databases of GeoIP for `ASN`, `Country` and `Cities` in `.mmdb` format for IP address analysis from **MaxMind** install with **Wireshark** to locate out the *Source* & *Destination* address.

for that we have to move to the following order
>Statistics
>>Endpoints
>>>IPv4
>>>>Map
>>>>>Open in Brower

![SELECTING STATICTICS TO ENDPOINTS](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/SELECTING%20STATICTICS%20TO%20ENDPOINTS.png)

![ENDPOINTS DATA TO MAP](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/ENDPOINTS%20DATA%20TO%20MAP.png)

#### Overview of GeoIP locations
![Overview of GeoIP locations](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/GeoIP%20Location.png)

If we have set **Name resolution** in our *profile* like **Resolve network (IP) addresses** from *preferences* then we can see the public IP name also & troubleshoot if there is any issue or not and deepdive into it. 
