+++
author = "Simon Gilbert"
categories = ["Asp.Net MVC", "C#", "Chief Technology Officer", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Hacking", "Microsoft Azure", "Microsoft Visual Studio", "MVC Core", ".Net Core", "Object Oriented Programming", "Performance", "Programming", "Simon Gilbert", "Software Development", "Web Dev", "Web Development", "Security", "Cyber Security", "Cyber Attack", "DDOS", "Protection", "Cloudflare", "SSL Certificate", "TLS Certificate", "HTTPS", "DNS", "DNSSEC", "Secure", "Azure Web App Service"]
date = 2019-03-29T09:33:00Z
description = ""
draft = false
image = "/images/2019/03/simon-gilbert-cto-tech-blog-16.png"
slug = "azure-ssl-tls-cloudflare-csharp-aspdotnetmvccore"
summary = "SSL/TLS certificate & DDOS protection required? Simon Gilbert details Cloudflare on a Microsoft Azure Web App Service running C# ASP.Net MVC Core"
title = "Microsoft Azure Web App Service SSL/TLS Security using Cloudflare - C# ASP.Net MVC Core"
tags = ["C#.Net", "Asp.Net MVC", "Azure Web App Service", "SSL/TLS", "Cloudflare"]
+++


### Security Without Obscurity

Last year Google Chrome 68 was released with one of the main features being the browsers ability to call out websites that follow the **HTTP** protocol…and therefore lack a valid **SSL/TLS** certificate. Without this certificate, the site isn’t running **HTTPS** and your data isn’t encrypted end-to-end (including your bank details when you buy that new item from insertsomebrandhere.com).

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-tls-cert.png" caption="SSL/TLS Certificate" >}}

Unfortunately there is much more to securing your platform that just running **HTTPS**, and we’re now in an age that security through obscurity will no longer suffice, particularly when it comes to man-in-the-middle and distributed denial of service (**DDOS**) attack protection.

When we launched [**my first startup Patrius**](https://www.exchangewire.com/blog/2016/09/08/45221/) a few years ago, we were in fact on the receiving end of intermittent **DDOS** attacks. We learnt a lot from it, not least how capable (and expensive) auto-scaling can be when you're running a load-balanced distributed system globally in the cloud.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-ddos.png" caption="DDOS Cyber Attack" >}}

When your servers are getting hammered the first necessity is to assess the traffic patterns to arrive at the point of threat detection. Following this is the examination point to understand and separate human traffic from the illegal stuff, and then filter it using deep packet inspection and rate limiting. Sounds complicated right? ...Indeed, and we were two devs in a London flat simply trying to introduce ad-fraud detection algorithms and low latency into the AdTech industry. We needed a third party to help mitigate the cyber attacks...

### Enter “Cloudflare”…

> “Cloudflare protects and accelerates any website online. Its web traffic is routed through our intelligent global network, allowing us to block threats and limit abusive bots and crawlers from wasting your bandwidth and server resources.”

...With **Cloudflare** it isn't just about security, it's about **performance** too. However, the functioning of the internet is dependent on **DNS**, and unfortunately given that **DNS** was designed in the 1980's when security was not a primary concern, it isn't secure by design, making it easy for an attacker to masquerade as an authoratative server. Fortunately, **Cloudflare** provide **DNSSEC** protection -

> "If **DNS** is the phone book of the Internet, **DNSSEC** is the Internet’s unspoofable caller ID. It guarantees a web application’s traffic is safely routed to the correct servers so that a site’s visitors are not intercepted by a hidden “man-in-the-middle” attacker."



### DNSSEC

The Domain Name System Security Extensions (**DNSSEC**) strengthens authentication in **DNS** by using digital signatures based on public key cryptography.

> "With **DNSSEC**, it's not **DNS** queries and responses themselves that are **cryptographically** signed, but rather **DNS** data itself is signed by the owner of the data."

Cloudflare sounds amazing doesn't it? I agree…so let’s look at enabling **Cloudflare** on a **Microsoft Azure Web App Service** deployment running **C# ASP.Net MVC Core**...

I'm going to assume that you're already running an existing **Microsoft Azure Web App**.



### Let's Get Secure...

First step, **DNS** hostname mapping for your **Web App** in **Azure** (**Cloudflare** won't accept the default .azurewebsites.net URL).

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-hostname-cname.png" caption="Azure App Service Hostname" >}}



Let's check that it's mapped correctly...

{{< figure src="/images/2019/03/simon-gilbert-cto-blog-site-dns.png" caption="Azure App Service - Deployed" >}}



The next step is to deploy your **ASP.Net MVC Core** source code and then head over to **Cloudflare** and create a free account. We can then add our site and start the process of securing our platform.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-site.png" caption="Cloudflare Platform" >}}



Now we will observe **Cloudflare's** process of running a **DNS** query against your domain to assess exactly what their system has to work with.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-results-1.png" caption="Cloudflare DNS Query Results" >}}



...You will then be asked to switch your name servers to using **Cloudflare's** instead (the names they've used are amusing!)

{{< figure src="/images/2019/03/simon-gilbert-cto-hover-cloudflare-nameserver.png" caption="Cloudflare Name Servers" >}}



Once this step is validated, you'll be informed that **Cloudflare** is now protecting your site.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-protection.png" caption="Cloudflare Protection Active" >}}



At this point it's time to fully activate the free **TLS** certificate from **Cloudflare** by clicking on the security link and going to the "Crypto" section.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-tls-ssl-cert.png" caption="Cloudflare SSL/TLS Certificate - Pending" >}}

Your **TLS** certificate will be activated a short time after this, and at this point you're running **HTTPS** end-to-end.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-tls-ssl-cert-live.png" caption="Cloudflare SSL/TLS Certificate - Active" >}}

A quick check of the TLS certificate in the browser on our production domain -

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-tls-browser.png" caption="Cloudflare SSL/TLS Certificate - In Browser" >}}

...Awesome, our **HTTPS** config is live!!!



### Configuring Our Security Further

Let's ensure that **HTTP** isn't an option anymore, and enforce redirects on all **HTTP** requests to go through the **HTTPS** scheme instead.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-1.png" caption="Cloudflare Protection - HTTPS" >}}



Now to enforce strict transport security (**HSTS**) also...

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-2.png" caption="Cloudflare Protection - HSTS" >}}

Click enable, and we'll configure the settings. The header max age setting at this stage should be fine as 6 months. The other point to note is the subdomain setting - Let's enforce that too.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-3.png" caption="Cloudflare Protection - HSTS Configuration" >}}



Next is the **TLS** version, which is currently in production as version 1.3 since 2018. Given that **TLS 1.2** was released in 2008 you'd really expect most users to be running **TLS 1.3** as a minimum at this stage.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-4.png" caption="Cloudflare Protection - TLS Version" >}}



**HTTPS** rewrites are next. There's no point running **TLS** if you're going to be hitting **JavaScript** and **CSS** resources over HTTP...and this also ties into cross-site scripting **(XSS).**

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-5.png" caption="Cloudflare Protection - HTTPS Rewrites" >}}



### Our Cloudflare Statistics

Now that's configured, let's review some stats on **Cloudflare's** dashboard for our production domain. As you can see, we're already being hit by traffic from different countries.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-stats-1.png" caption="Cloudflare Protection - Web Traffic Requests" >}}



Interestingly we can already see the performance side of Cloudflare kicking in with it's CDN and caching functionality.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-stats-2.png" caption="Cloudflare Protection - Cache Performance Stats" >}}



...So far, we've achieved a free **TLS Certificate** and some performance improvements. Let's see if **Cloudflare** has needed to mitigate anything yet...

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-stats-3.png" caption="Cloudflare Protection - Threats Mitigated" >}}

...and that's exactly the point - You cannot assume security in the the modern digital world...kinda terrifying.



### Mitigating A DDoS Attack Using Cloudflare

Before we wrap things up, the most important part in all of this is the ability to pull the trigger on a **DDoS** attack. You've already loaded the **Cloudflare** gun, so in the event that you do need to help mitigate an attack with immediate affect...click this button.

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-under-attack.png" caption="Cloudflare Protection - Under Attack Mode On" >}}

This will lead to you being in **DDoS Attack** mode, which will force all visitors to your site to be shown a **JavaScript** challenge...

{{< figure src="/images/2019/03/simon-gilbert-cto-cloudflare-ddos-mitigate.png" caption="Cloudflare Protection - Under Attack Mode" >}}

Today we've covered the basics using a free account. Upgrading to a business account offers many more security protection and performance features for your platform. 

Enjoy!