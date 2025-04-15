---
title: "Getting started with Shodan API"
date: 2022-09-06
permalink: /posts/2022/09/06/shodan-api-getting-started/
author: Brijesh Vora
categories: [network-security, shodan, api]
original_url: https://medium.com/@brijesh.vora12/getting-started-with-shodan-api-35196da3c774
---

![Shodan Search Engine Header](/images/shodan-header.webp)

Welcome to Shodan. Shodan is the Search Engine for the Internet of Everything. Learning and using Shodan can be quite scary at first but as you get used to it, you will discover that it has a lot to offer.

From scanning vulnerabilities of an IP, to finding the geographical location, VPN, ASN, country information, etc., all can be requested using the API call. In this post, I'll show you how you can use the Shodan API to find relevant information about an IP address. We are using the Shodan API at Ennetix, Inc to find out open ports, vulnerabilities, and other info.

After making an account with Shodan, grab the API key unique to your account and paste it in the code below (`shodan.py`). This script queries Shodan using your API key and dumps the results into a JSON file.

```python
import shodan
import json
import sys
api = shodan.Shodan(your_api_key)  # private api key
def shodan_fun(IP):
    try:
        ipinfo = api.host(IP)
    except shodan.APIError as e:  # this line is resolved
        ipinfo = {}
    with open("shodan_results.json", 'w') as f:
        json.dump(ipinfo, f, indent=4)
    return ipinfo
shodan_fun(sys.argv[1])
```

Now you can run the below command to get the info of an IP. Instead of `IP`, type the IP address you want to query (e.g., 8.8.8.8):

```bash
$ python3 shodan.py IP
```

## Example Output

Here are examples of the JSON output you might get from the Shodan API:

Let's analyse the output:

![Example Shodan Output](/images/shodan-1.webp)
![Another Example Shodan Output](/images/shodan-2.webp)

From the above JSON output, one can see the tags, country_code, hostnames, domains, location, port, and even product info — OpenSSH in this case.

![Example Vulnerabilities List](/images/shodan-3.webp)

The `vulns` field shows the vulnerabilities from that IP address. So, the attacker might have leveraged this information to access this server and launch an attack on a different machine using this machine. Shodan also shows open ports for that IP. This information can be leveraged in many ways, one of them being to launch a DDoS attack.

Shodan can show the top 10 or top 100 vulnerabilities across the internet, and if that vulnerability is present in your organization IP then it should be resolved quickly before any attack happens.

Shodan can do much more than the data shown here. This is just the tip of the iceberg. It can show Shodan Trends — historical trends, Shodan Monitor — monitor the network for incoming and outgoing requests. It has a database of blacklisted IP addresses which you can download locally with Enterprise access.

For more information and tutorials, visit [https://www.shodan.io/](https://www.shodan.io/).

---

*This blog post was originally published on [Medium]({{ page.original_url }}).*

PS:
Brijesh Vora is a grad student at University of California, Davis and was a software engineering intern at Ennetix, a leading provider of AI-powered analytics solutions for deterministic digital operations. This blog post was developed as a part of Ennetix, Inc.
