+++
author = "Simon Gilbert"
categories = ["C#", "Chief Technology Officer", "C#.Net", "Code", "Coding", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Dev", "Dot Net", "Github", ".Net Core", ".Net Framework", "Nuget", "Object Oriented Programming", "Programming", "Simon Gilbert", "Software Development", "Mono", "Monotouch"]
date = 2019-04-23T10:21:00Z
description = ""
draft = false
image = "/images/2019/05/simon-gilbert-dev-cto-blog-23.png"
slug = "iphone-geo-location-csharp-xamarin-ios"
summary = "Need to track geo location coords? \n Simon Gilbert explains how to handle location updates for the iPhone using C# Xamarin.iOS"
title = "iPhone Geo Location Coordinate Tracking using C# Xamarin.iOS"
tags = ["C#.Net", "Xamarin.iOS"]
+++


### One Step, Two Step, Three Step

Occasionally you might need to track your customers location coordinates in real-time. You could **build an app**, but given that **iOS** and **Android** are supported by separate programming languages, following that route would mean implementing the code base twice...however, there is a way around that...

### Enter "Xamarin"...

**Xamarin** (originally termed "mono"), allows **cross-platform** implementations for **iOS, Android** and **Windows Mobile** using C# .Net.

> "With a C#-shared codebase, developers can use Xamarin tools to write native Android, iOS, and Windows apps with native user interfaces and share code across multiple platforms, including Windows and macOS."

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-1.jpg" caption="iPhone Geo Location - C# Xamarin iOS" >}}

### Let's Code...

For today's implementation, we're going to focus on **Xamarin.iOS**. Let's begin by creating a class to handle the **geo location events**.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-6.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Next, we need a **LocationManager** class that can handle the **geo location** event updates. Inside our class we need an instance of the **CLLocationManager**, and a delegate to handle the events.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-2.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Next we add a method for starting the location updates and the accuracy configuration.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-3.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

We then need to set some initial permissions, which happen to be different for **iOS 8** vs. **iOS 9**...

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-4.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Now to configure everything to run in our **LocationManager's** constructor.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-5.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Now to put our **LocationManager** to use in a **ViewController** with an instance of the class.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-7.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Next, we need a method for **subscribing to he location updates**, and one for **unsubscribing** when the app enters **background mode** also.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-8.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Now in our **ViewController's constructor**, we can execute our method call to start the geo location updates.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-9.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Before we run our implementation, we need to configure the background mode to allow Geo Location updates **_(via the info.plist)_**.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-10.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Following this, some **additional privacy requirements** are needed, to ensure we ask our user to allow the geo location updates to occur whilst using the app.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-11.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Finally, let's run our **iOS** app.

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-12.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Click to allow the permissions as requested by the app...

{{< figure src="/images/2019/05/simon-gilbert-dev-cto-xamarin-13.png" caption="iPhone Geo Location - C# Xamarin iOS" >}}

Enjoy!