# SCENARIO
This directory contains resources related to the analysis of **HTTP** traffic. It includes the `http.cap` file for packet capture, the `download.html` and `ADVERTISEMENT.html` file demonstrating an **HTTP GET** request, and screenshots illustrating how to extract insights from the captured data. This analysis aims to enhance understanding of **HTTP** protocols and their behavior in network communications.

# TOOLS USED
**Wireshark**, **MaxMind GeoIP**

# DOWNLOAD LINKS
* **Wireshark** : https://www.wireshark.org/download.html
* **MaxMind GeoIP** : https://dev.maxmind.com/geoip/geolite2-free-geolocation-data/?lang=en

# WORK
We have to analyze captured packets in the `http.cap` using **Wireshark**. HTTP packets are available as `http.cap` file and also in `.csv` format as `HTTP pac capture.csv` data.

# WORKFLOW
All network packets are display in the top frame showing details like *time, delta time, source, destination, protocol, byte length, source port address, destination port address,  host, Referrer and information* in the bottom left frame the selected packets details are soon while the bottom right frame display them in hexadecimal format.

![(HTTP.CAP) IN WIRESHARK DURING ANALYSIS](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/1(HTTP.CAP)%20IN%20WIRESHARK%20DURING%20ANALYSIS.png)

## Filtering
We can see a filter over in the **Wireshark** there 

![(FILTER OPTION)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/2(FILTER%20OPTION).png)

where we can apply filters like:
### Filter out perticular IP address
We can filter `Ip.addr == [ip address]` to display all network packets that involve a specific IP address, either as the source or the destination.
### Filter out perticular network protocols
for **HTTP** protocol filter `http`

for **TCP** protocol filter `tcp`
### Filtering the frame number of the corresponding HTTP request for a given HTTP respons
Essentially, when we are analyzing a response, the `http.request_in` field helps you quickly identify which request prompted that response, linking the two together.

### Filtering HTTP GET Requests in Wireshark
The filter `http.request.method == "GET"` in Wireshark shows only HTTP packets where the request method is GET, which is used by clients to request data from a server without modifying any server-side resources.

Now as we are interested to watch out what is going on `HTTP` traffic we will select the filter to the
````
http
````
![(PUTTING HTTP FILTER)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/3(PUTTING%20HTTP%20FILTER).png)

Overview after putting `http` filter:
![(WINDOW AFTER PUTTING HTTP FILTER)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/4(WINDOW%20AFTER%20PUTTING%20HTTP%20FILTER).png)
Then we will go through the **Info** column to see if there is any `HTTP-GET` or `HTTP-POST` request is going on or not

![(HTTP)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/5(HTTP).png)

or we can simply put filter for them.
````
http.request.method == "GET"
````
````
http.request.method == "POST"
````


To find any `HTTP-GET` request we will put `http.request.method == "GET"` in the filter and by this way we can find out 2 (Two) `GET` request we are going to analyze it one by one, like for the 1st `http-get` request (*Row-No: 4*).

![(HTTP-GET REQUEST)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/6(HTTP-GET%20REQUEST).png)
![(DETAIL INFORMATION OF 1ST HTTP-GET REQUEST)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/7(DETAIL%20INFORMATION%20OF%201ST%20HTTP-GET%20REQUEST).png)
We will go to the hypertext transfer protocol this gives us information like:
* **Request Method**: `GET`
* **Request URL**: `/download.html` (specific resource requested on the server)
* **Request Version**: `HTTP/1.1`

### Header Fields:

* **Host**: www.ethereal.com (the domain being accessed)
* **User-Agent**: `Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.6) Gecko/20040113`
    * Represents the browser and OS used by the client.
* **Accept**: `text/xml, application/xml, application/xhtml+xml, text/html, etc.`
    * Specifies the content types the client is willing to receive, prioritizing XML and HTML formats.
* **Accept-Language**: `en-us,en;q=0.5`
    * Indicates preferred language as U.S. English.
* **Accept-Encoding**: `gzip,deflate`
    * Specifies acceptable content encoding to save bandwidth.
* **Accept-Charset**: `ISO-8859-1,utf-8;q=0.7,*;q=0.7`
    * Indicates acceptable character sets, with ISO-8859-1 and UTF-8 as priorities.
* **Keep-Alive**: `300`
    * Specifies a timeout of 300 seconds for keeping the connection alive.
* **Connection**: `keep-alive`
    * Requests a persistent connection, allowing multiple requests over the same connection.
* **Referer**: `http://www.ethereal.com/development.html`
    * Shows the URL from which the request was referred, useful for tracking request flow.

<hr>

Now for the 2nd `http-get` request (*Row-No: 18*), we can see that the Client machine requesting for an advertisement provided by Google AdSense

![(REQUESTING FOR AD)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/8(REQUESTING%20FOR%20AD).png)
![(DETAIL INFORMATION OF 2ND HTTP-GET REQUEST FOR ADVERTISEMENT)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/9(DETAIL%20INFORMATION%20OF%202ND%20HTTP-GET%20REQUEST%20FOR%20ADVERTISEMENT).png)
Again we will go to the hypertext transfer protocol and  now this gives us:
* **Request Method**: `GET`
* **Request URL**: `/download.html`
* **Request Version**: `HTTP/1.1`

### Header Fields:

* **Host**: `www.ethereal.com`
* **User-Agent**: `Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.6) Gecko/20040113`
    * This indicates the client uses Mozilla version 5.0 on Windows XP (NT 5.1).
* **Accept**: `text/xml, application/xml, application/xhtml+xml, text/html, text/plain, image/png, image/jpeg, image/gif`
    * Specifies the types of content the client can handle.
* **Accept-Language**: `en-us, en;q=0.5`
    * Indicates English (US) as the preferred language with a fallback option for English.
* **Accept-Encoding**: `gzip, deflate`
    * The client supports compressed content in gzip and deflate formats.
* **Accept-Charset**: `ISO-8859-1, utf-8;q=0.7, *;q=0.7`
    * Specifies accepted character sets, prioritizing ISO-8859-1 and utf-8.
* **Keep-Alive**: `300`
    * Specifies a persistent connection timeout of 300 seconds.
* **Connection**: `keep-alive`
    * Requests to keep the connection open after the current request.
* **Referer**: `http://www.ethereal.com/development.html`
    * Indicates that the request was referred from the /development.html page on the same site.

<hr>

Now we will set our filter to the `http` again & watch out for responds from servers.

At (*Row-No: 27*) we are getting response from the Google Ad server(www.googleadservices.com)

![(ACK FROM GOOGLE ADSERVERTISEMENT SERVER)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/10(ACK%20FROM%20GOOGLE%20ADSERVERTISEMENT%20SERVER).png)
![(ACK FROM GOOGLE ADSERVERTISEMENT SERVER DETAILS)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/11(ACK%20FROM%20GOOGLE%20ADSERVERTISEMENT%20SERVER%20DETAILS).png)
We will go to the hypertext transfer protocol and now this gives us:
* **Response Version**: `HTTP/1.1`
* **Response Status Code**: `200`
* **Response Status Code Description**: `OK`
### Header Fields:
* **P3P**: `policyrer- http://www.googleadservices.com/pagead/p3p.xml`
    * This suggests that www.googleadservices.com had implemented a P3P policy, if we go through the compact policy we will find the privacy practices.
* **CP**: `"NOT DEV PSA PSD IVA PVD OTP OUR OTR IND OTC"`
    * This compact policy (CP) code describes the types of data practices Google follows:
        * **NOI**: No identifiable information collected.
        * **DEV**: Information is used for research and development.
        * **PSA/PSD**: Information is used for pseudonymous analysis and decision-making.
        * **IVA/PVD**: Data is used for interactive and personalized ad displays.
        * **OTP**: Data may be used for "other purposes."
        * **OUR**: Data is shared only with Google’s affiliates.
        * **OTR**: Information may be retained or combined with other Google data.
        * **IND**: Information may be used to infer user preferences.
        * **OTC**: Information collected is typically user-tracked but used in aggregate.
* **Content-Type**: `text/html`
    * Specifies the types of content as text/html.
* **Charset**: `ISO-8859-1`
    * Specifies response character sets, ISO-8859-1
* **Content-Encoding**:  `gzip`
    * The server responds back compressed content in gzip formats.
* **Content-length**: `1272`
    * Specifies the types of content length.
* **Date**: `Thu, 13 May 2004 10:17:14 GMT`
    * Specifies the date & time.
* **Time since request**: `0.971397000 seconds`
    * Specifies the respond time delay after the request.


<hr>

At last, we are getting responds from server(www.ethereal.com) at (*Row-No: 38*) we can find that.

![(ACK FROM GOOGLE AD SERVER)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/12(ACK%20FROM%20GOOGLE%20AD%20SERVER).png)
![(ACK FORM THE SERVER)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/13(ACK%20FORM%20THE%20SERVER).png)

We will go to the hypertext transfer protocol and now this gives us:
* **Response Version**: `HTTP/1.1`
* **Response Status Code**: `200`
* **Response Status Code Description**: `OK`
### Header Fields:
* **Date**: `Thu, 13 May 2004 10:17:14 GMT`
    * Specifies the date & time.
* **Server**: `Apache`
* **Last-Modified**: ` Tue, 20 Apr 2004 13:17:00 GMT`
    * Specifies the date & time the server made their last modification.
* **ETag**: `"9a01a-4696-7e354b00"`
    * The ETag (or entity tag) HTTP response header is an identifier for a specific version of a resource. It lets caches be more efficient and save bandwidth.
* No `p3p.xml` file or P3P privacy policy header was detected for www.ethereal.com.
    * This suggests that www.ethereal.com at the time did not implement a P3P policy, which might affect how user data is protected and handled.



<hr>

## Exporting Objects and Review them
Now as we have seen 2 `HTTP-GET` request has been made we can exports those objects from Wireshark by following
#### Step-by-Step
>File
>>Export Objects
>>>HTTP

Then we will find out the objects that we can save into our systems.

<img src="https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/14(EXPORTING%20OBJECTS).png" height="526 px" width="429 px" /> <img src= "https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/15(OBJECTS).png" height= "526 px" width= "574 px"/>
Now we have successfully saved `download.html` into our systems.
![(OBJECT OBTAINED FROM HTTP GET METHOD)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/16(OBJECT%20OBTAINED%20FROM%20HTTP%20GET%20METHOD).png)

Similarly we can save the advertisement also as `ADVERTISEMENT.html`, Now we can look into the `download.html` and the `advertisement` file, analyze it (*like: if there any cross-site scripting present or not*) for further use.
#### Overview of download.html
![(DOWNLOAD.HTML FILE OVERVIEW)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/17(DOWNLOAD.HTML%20FILE%20OVERVIEW).png)

#### Overview of Advertisement.html

![(ADVERTISEMENT.HTML FILE OVERVIEW)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/18(ADVERTISEMENT.HTML%20FILE%20OVERVIEW).png)

<hr>

## Genarating I/O Graph and Review
Now we can also analyze the **I/O graph** for that we will move into 
>Statistics
>>I/O Graphs

![(Selecting I/O graph)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/19%20(SELECTING%20I-O%20GRAPHS).png)

Then analysis the traffic for packets
#### Overview of I/O graph
![(Overview of I/O graph)](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/20%20(I-O%20ANALYSIS).png)

We can see the I/O Graph showing traffic spikes, suggesting ad content loading at the starting phase.

<hr>

## Genarating Endpoint location with GeoIP
Also we can download for databases of GeoIP for `ASN`, `Country` and `Cities` in `.mmdb` format for IP address analysis from **MaxMind** install with **Wireshark** to locate out the *Source* & *Destination* address.

for that we have to move to the following order
>Statistics
>>Endpoints
>>>IPv4
>>>>Map
>>>>>Open in Brower

![SELECTING STATICTICS TO ENDPOINTS](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/21%20(SELECTING%20STATICTICS%20TO%20ENDPOINTS).png)

![ENDPOINTS DATA TO MAP](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/22(ENDPOINTS%20DATA%20TO%20MAP).png)

#### Overview of GeoIP locations
![Overview of GeoIP locations](https://raw.githubusercontent.com/Sridip-99/Wireshark-Packet-Analysis-/refs/heads/main/HTTP%20Analysis/Screenshots/23(GeoIP%20Location).png)

If we have set Name resolution in our profile like **Resolve network (IP) addresses** then we can see the public IP name also & troubleshoot if there is any issue or not and deepdive into it. 

<hr>

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

## Session Tracking:
Based on the ad request, some tracking elements were observed (e.g., `random` parameter in the ad request URL), which might be used to avoid caching and serve dynamic ads.

<br>

###### * I HAVE TAKEN HELP FROM `ChatGPT` FOR MY LEARNING & FOR FILLING PURPOSE OF *(INSIGHTS AT A GLANCE)* 
