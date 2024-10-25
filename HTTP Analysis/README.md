# SCENARIO
This directory contains resources related to the analysis of **HTTP** traffic. It includes the `http.cap` file for packet capture, the `download.html` and `ADVERTISEMENT.html` file demonstrating an **HTTP GET** request, and screenshots illustrating how to extract insights from the captured data. This analysis aims to enhance understanding of **HTTP** protocols and their behavior in network communications.

# TOOLS USED
**Wireshark**, **MaxMind GeoIP**

# DOWNLOAD LINKS
* **Wireshark** : https://www.wireshark.org/download.html
* **MaxMind GeoIP** : https://dev.maxmind.com/geoip/geolite2-free-geolocation-data/?lang=en

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

# INSIGHTS AT A GLANCE

After analyzing the `http.cap` file, here’s a summary of the main findings and insights:

## Download File Analysis:

* The `http.cap` file contained a captured HTTP request for `download.html` from www.ethereal.com.
* The HTML file includes links to Ethereal (now Wireshark) downloads for different platforms, such as Windows, Solaris, and Linux distributions.

## Ad Requests:
* There was a GET request made to `pagead2.googlesyndication.com` with parameters pointing to a `Google AdSense ad banner (468x60_as format)`.
This ad request includes user information and browser details, which Google could use for ad targeting.
## HTTP Headers:
*  **User-Agent:** `Mozilla/5.0 with Windows NT 5.1 (Windows XP)` and `Gecko engine (Firefox)`.
* **Referer:** Requests to `download.html` page indicate browsing activity on www.ethereal.com.
* **Content-Encoding:** The server responded with `gzip encoding` for faster data transfer.

## JavaScript and Embedded Scripts:
* The `download.html` page contained a JavaScript snippet for displaying ads, with a reference to Google Ads scripts hosted at `pagead2.googlesyndication.com.`
* There were no other unusual scripts, but analyzing JavaScript files could be a potential area for further investigation, especially for tracking cookies or cross-site scripting risks.
## Potential Missing P3P Policy:
* No `p3p.xml` file or P3P privacy policy header was detected for www.ethereal.com. This suggests that www.ethereal.com at the time did not implement a P3P policy, which might affect how user data is protected and handled.
* But  `p3p.xml` file or P3P privacy policy header was detected for www.googleadservices.com. This suggests that www.googleadservices.com at the time implementd a P3P policy, policyref="http://www.googleadservices.com/pagead/p3p.xml", 

**CP="NOI DEV PSA PSD IVA PVD OTP OUR OTR IND OTC"**

This compact policy (CP) code describes the types of data practices Google follows:

* **NOI:** No identifiable information collected.
* **DEV:** Information is used for research and development.
* **PSA/PSD:** Information is used for pseudonymous analysis and decision-making.
* **IVA/PVD:** Data is used for interactive and personalized ad displays.
* **OTP:** Data may be used for "other purposes."
* **OUR:** Data is shared only with Google’s affiliates.
* **OTR:** Information may be retained or combined with other Google data.
* **IND:** Information may be used to infer user preferences.
* **OTC:** Information collected is typically user-tracked but used in aggregate.
## Session Tracking:
Based on the ad request, some tracking elements were observed (e.g., *random parameter* in the ad request URL), which might be used to avoid caching and serve dynamic ads.

<br>

###### * I HAVE TAKEN HELP FROM `ChatGPT` FOR MY LEARNING & FOR FILLING PURPOSE OF *(INSIGHTS AT A GLANCE)*
