# Lab 2 - Analyze DNS & HTTP in Wireshark
[![DNS image](https://www.elegantthemes.com/blog/wp-content/uploads/2018/03/what-is-dns.png)](https://www.elegantthemes.com/blog/wp-content/uploads/2018/03/what-is-dns.png)

[Lab 2 Assignment page on Canvas](https://canvas.uw.edu/courses/1434918/assignments/5995604/)

# Overview

The purpose of this lab is two-fold: 

- Students will gain hands on experience with DNS. 
- Students will become familiar with the basic functionalities of Wireshark, and to be able to capture and analyze network traffic.

We highly recommend using search engines to help you find solutions. As usual, there will be multiple ways to complete this lab. Please use the template provided on Canvas to complete your lab report.

[^]: 

# Instructions



## Part I - website
Pick a popular website that has a lot of content on it. Ideally it should contain advertisements. You will be using it throughout this lab.

**Report**: 

1. What website did you select?

## Part II - dig
Before jumping into more complicated scenarios, we would like you to learn how to use the **`dig`** command as it is an incredibly helpful and simple tool.

!!! Note
    We highly recommend that you use dig for this lab. If it's not installed already on your computer, we've provided some resources at <a href="/resources/dns-clients/#perform-dns-lookups-manually" target="_blank">Managing DNS clients</a> to help you get started. If you are using Windows, a dig .exe file you can download and install exists. Or, you could use your Digital Ocean droplet form Lab 1 and follow the Linux instructions for how to install **`dnsutils`**. Use **`dig `** to lookup the domain name of the website you selected. Open up a new tab in your browser and enter (one of) the IP address(es) returned by dig to discover where it will take you. If dig returned more than one address, open up a second tab and enter it now.

!!! Hint
    Don't panic if you receive an error instead of a live site. Some servers host more than one website and need the name to help route you to the right place. Try another site, e.g., uw.edu, and see if you get different results.

**Report**: 

2. What addresses did the dig command return? Copy dig results to your report (code block or screenshot).

1. What did you observe when you browsed to the IP addresses directly?

1. Aside from **A** records containing IP addresses, did you discover any other DNS records from your query? List at least 3 other record types that DNS manages (use Google to look up other kinds of records if your query returned less than 3 types!)

1. Look closely at the output of the `dig` command. How can you be sure that the query completed successfully? Identify the IP address of the server used to handle your query. If you used PowerShell's `Resolve-DnsName` then you won't see the server IP that answered your request. Instead use `ipconfig /all` and find the IP under `DNS Servers` under your network interface.


## Part III - dev tools
Open up a new tab preferably in a chromium based browser (Chrome, Brave, etc...). Open up the dev tools (right click on page and click Inspect), and go to the *Network* option. 

In that very same tab paste in your selected web page into the URL bar and click enter. You should see all of the **GET** and **POST** requests that the browser made for that webpage load up in the dev tools.

Right-click the header of the Network Log table and select Domain. The domain of each resource is now shown.

[![dev tools show domain](https://developers.google.com/web/tools/chrome-devtools/network/imgs/tutorial/domain.png)](https://developers.google.com/web/tools/chrome-devtools/network/imgs/tutorial/domain.png)


Take some time to scroll through the domains, files, and file types your website was requesting.

**Report**: 

6. From a quick glance what is the most common file type requested?

1. Approximately how many *domains* do you see in that list that don't matchup with the website domain you initially visited?

1. Why do you think this page is getting information from other websites?

1. If you had to guess, how many DNS requests do you think were sent in order to fully load this page?



# Part IV - Wireshark

## Introduction 

Wireshark is a tool designed to record and observe the messages that are sent over a network and to provide an analyst with tools to understand and troubleshoot the protocols associated with those messages.

## Starting a Capture

Open Wireshark and locate the Capture section of the launch screen. This section contains an input field for configuring capture filters and a list of physical and virtual interfaces that can be used to capture packets.

To the right of each interface, you should notice an animated “sparkline” which indicates the level of activity on the associated network. You can launch a capture on your active interface (likely Wi-Fi or Ethernet) by double-clicking on the respective label. 

If a capture filter was specified before you selected the interface, Wireshark ignores any packets that don't match the filter. If the filter input is empty, Wireshark records everything it sees on the wire. 

Recording continues until you press the stop icon in the top toolbar or close the application.

## Exploring a Capture

By default, Wireshark will divide capture-related content among three panes:

*   Packet List
*   Packet Details
*   Packet Bytes

The *Packet List* displays all packets in the current capture, summarizing important details about traffic flow and the type of data contained in the packet. 

The *Packet Details* view initially displays a layered summary of the protocols represented in the selected packet. A more detailed view of each protocol can be explored by clicking the arrow icons to the left of the summary. The order of the protocols in this view corresponds to the order in which each protocol’s header appears in the data frame. 

The *Packet Bytes pane* can be useful for understanding the basic structure of raw network messages. As you navigate through the details in this view, Wireshark will highlight the corresponding bytes in the Packet Bytes pane.

## Narrowing your Search

If you have any software, particularly a web browser, running in the background, you'll notice that Wireshark can quickly accumulate a large list of packets. Fortunately, the software provides a variety of tools to help you more easily locate packets relevant to your search.

The packet list can be modified non-destructively by adding/removing *display filters*[^filters] in the input field below the top toolbar. Display filters are a valuable analytic tool that allows you to remove noise and quickly focus on packets that match certain criteria.

In addition to writing your own display filters, you can access a variety of context-specific analysis tools by right clicking on any row of the Packet List or a field within the Packet Details pane. These tools can help you create a display filter based on the current packet or to investigate a conversation that spans multiple packets.

[^filters]: Do not confuse display filters and capture filters. They serve very different purposes and even use distinct syntax. 



## Examining DNS requests in Wireshark

Before examining DNS requests in Wireshark, you will want to clear your DNS cache. 

!!! Hint
    As applications make DNS queries to obtain the IP addresses of remote  resources, your operating system will start to maintain a cache of  previous responses. These cached responses are used on subsequent  lookups in order to reduce network overhead and speed up the process of  loading the applicable resources.

Check the <a href="/resources/dns-clients/#view-or-clear-your-local-dns-cache" target="_blank">Managing DNS clients</a> resource for instructions on how to clear your DNS cache.




Create a capture of you visiting your website by: 

* Open Wireshark, begin a capture
* Quickly open a new tab and visit your website in your browser
* Once it has fully loaded end your Wireshark capture.

Now **filter** for DNS packets only in the display filter.

* Identify the DNS response containing the information you needed in order to convert your website name into an IP address.
* Use the information contained within that packet for the following deliverable.


!!! Hint
    Learn how to use the search tool to find string content or research display filters that allow you to specify the domain name.

**Report**: 

10. Assuming almost all of the DNS requests you see in Wireshark right now are for the one website you visited, how many DNS requests do you see? 
11. Overall were there less or more DNS queries than you'd expect?
12. How did you identify the DNS packet(s) associated with the website you visited?
13. Provide screenshots of the packet(s) (specifically of the DNS information in Packet Details).
14. List the ip addresses you received for the website from the DNS server that resolved your request.
1. Which *transport layer protocol* (think OSI model) is used to carry the DNS packet?

16. Compare this DNS response to others in the capture (generate more if needed).
17. Which port number(s) are shared in common across these DNS requests?
