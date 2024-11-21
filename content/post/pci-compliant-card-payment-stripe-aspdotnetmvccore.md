+++
author = "Simon Gilbert"
categories = ["Asp.Net MVC", "C#", "Chief Technology Officer", "C#.Net", "Computer Science", "Cross-Platform", "C Sharp", "CTO", "Dot Net", "Microsoft Visual Studio", "MVC Core", ".Net Core", ".Net Framework", "Nuget", "Object Oriented Programming", "Simon Gilbert", "Stripe", "PCI", "PCI DSS", "PCI Compliant", "PCI Compliance", "Payment", "Payment Provider", "Credit Card", "Credit Card Payment", "Code", "Coding", "Dev", "Software Development", "Programming", "Web Dev", "Web Development"]
date = 2019-03-25T21:19:00Z
description = ""
draft = false
image = "/images/2019/03/simon-gilbert-cto-tech-blog-post-twelve.png"
slug = "pci-compliant-card-payment-stripe-aspdotnetmvccore"
summary = "Need to process credit card payments in your code? Simon Gilbert explains how to accept PCI Compliant card payments using the leading payment provider Stripe, and C# ASP.Net MVC Core in Visual Studio for Mac."
title = "Cross-Platform (PCI Compliant) Credit Card Payments using Stripe - C# ASP.Net MVC Core"
tags = ["C#.Net", "Asp.Net MVC", "Stripe"]
+++


### PCI Compliance Matters

You wouldn’t risk using a website that didn’t have a **TLS** certificate would you? ...exactly, and credit card payments are no different.

Every time you enter your card details into a websites payment form, you are theoretically risking submitting those details, in full, back to the server to be stored and used by anyone who has access to the back-end of that platform and it’s accompanying database.

Fortunately, there’s a method to help ensure you can provide a fully secure implementation when concerning this feature -

### PCI DSS

> “The Payment Card Industry Data Security Standard (PCI DSS) is a set of security standards designed to ensure that ALL companies that accept, process, store or transmit credit card information maintain a secure environment.”

PCI DSS, aka "PCI Compliance" is the implementation that all online card payments should follow to ensure that your credit card details are not revealed to the people whom you are executing the payment to.

### Introducing “Stripe”

Stripe was founded in 2011 by the Collison brothers. Their platform allows you to accept credit card payments over the internet...and it doesn’t stop there. Stripe’s API is very clean, and their anti-fraud monitoring and analytical data goes far beyond the likes of their competitors.

It’s worth noting that **Stripe** actually pride themselves on being a “developers first” business...unsurprising, given both the founders are programmers at heart.

### Card Payment Process Flow

Rather than a user having to post back their card details to your server, the details are sent client-side to the payment provider (Stripe) using JavaScript. At this point the details are checked to ensure they are valid for use, and an authentication token is returned to the executing code statement, which can then be submitted to the server and processed directly through the Stripe API. The only piece of data relating to the users credit card that is being sent to your server is the token which authenticates the payment itself. This ensures that the users private card payment details remain a secret for their eyes only...PCI Compliant card payments are just a few steps away.

_Before we get started, it's worth highlighting that card payments are a complicated process will multiple permutations and potential exception points. Given this, I'll be coding a "happy path" scenario only for this example._

### Prerequisites

Before we get started you’ll need [a Stripe account](https://dashboard.stripe.com/register) and an understanding of JavaScript...I’m already assuming you’re used to coding C# ASP.Net MVC Core.

### Let’s Code...

The first thing to do is pull in a copy of the **Stripe.Net** package from **Nuget**.

Once that's done, let's code our input and output data models for the view. Our input will need a token property for the authentication token we receive back from Stripe, and an email property so we can submit a receipt to our user.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-card-payment-model.png" caption="C# ASP.Net MVC Core - Stripe View Model" >}}

Once a successful payment is made, we'll need to pull back some of the key data from the response object, in order to verify that the payment has gone through.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-card-payment-receipt.png" caption="C# ASP.Net MVC Core - Stripe View Model Receipt" >}}

Now that we have our input and output data models, it's time to code our card payment form. We're going to make use of [Stripe's Element examples](https://stripe.dev/elements-examples/), allowing us to utilise the client-side validation that Stripe have made available for it's consumers. We will need the HTML, CSS and JavaScript that is associated with the card payment form. _You'll note that I've added some default placeholders..._

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-card-elements-form.png" caption="C# ASP.Net MVC Core - Stripe Card Elements Form" >}}

Now at this point, the Stripe card element's form will indeed generate a token for us, but a few amends are necessary in order for us to complete the payment process flow in full.

...The **CardPaymentViewModel** we created initially needs to be bound to the view and used to created two hidden fields for both the email address and the token properties.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-card-form-hidden-fields.png" caption="C# ASP.Net MVC Core - Stripe Hidden Fields" >}}

Now let's bind the token and email address to those hidden fields in our javascript code, and ensure that only these two specific data properties on the form are posted back to the server, once the token is generated.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-card-elements-javascript.png" caption="C# ASP.Net MVC Core - Stripe JavaScript" >}}

Next, we need to code our service wrapper around the **Stripe API**. Here we make use of Stripe's **ChargeService** class, which we use to submit a **ChargeCreateOptions** object to and receive a **Charge** object back from.

There are a few key properties to highlight here. The **ChargeCreateOptions** object has a **TransferGroup** property which is an ID field that we can use for the payment transaction itself. The token we received initially will be assigned to the **SourceId** property, and the users email will be assigned to the **ReceiptEmail** property. Finally, we need to ensure that the **Capture** property is set to "true", so that the card is actually charged for the payment...

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-charge-service.png" caption="C# ASP.Net MVC Core - Stripe Card Payment Service" >}}

Next we need to map our payment receipt data. In an ideal world we would use AutoMapper, but for simplicity I've mapped this data by hand as follows.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-payment-receipt-mapping.png" caption="C# ASP.Net MVC Core - Stripe Payment Receipt Mapping" >}}

Our service is coded, so let's inject it into our controller.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-card-payment-controller.png" caption="C# ASP.Net MVC Core - Stripe Card Payment Service Controller Injection" >}}

Now let's obtain the two necessary keys from your Stripe account dashboard - The secret key and the publishable key.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-dashboard-keys.png" caption="C# ASP.Net MVC Core - Stripe Dashboard" >}}

We then map our keys into our **appsettings** file.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-keys.png" caption="C# ASP.Net MVC Core - Stripe API Keys" >}}

At this point, the publishable key needs to be embedded into our JavaScript.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-publishable-key.png" caption="C# ASP.Net MVC Core - Stripe Key Embed JavaScript" >}}

Finally we can configure **Stripe** with our secret **ApiKey** and dependency inject our service.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-configuration.png" caption="C# ASP.Net MVC Core - Stripe Configuration" >}}

### Testing Our Credit Card Payments

Let's now test our implementation. Stripe supply a series of test card numbers for us to use. I'm specifically using the UK test numbers...

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-test-card-numbers.png" caption="C# ASP.Net MVC Core - Stripe Test Card Numbers" >}}

The first thing to try is an invalid card number, to see how Stripe's default client-side validation handles recognizing this at the point where we submit the payment.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-card-validation.png" caption="C# ASP.Net MVC Core - Stripe Card Validation" >}}

Very cool right? Next let's use one of the valid test card numbers and see how the form changes.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-postcode-validation.png" caption="C# ASP.Net MVC Core - Stripe Valid Card" >}}

...Note how the visa icon has appeared next to the card number, indicating that it's valid. Another thing to point out is that the postcode field has appeared (and if we chose to use an American test card number, this would actually say "Zipcode").

Finally let's submit fully valid details (a valid Stripe test card number and postcode, plus any values that you like for the expiry data and CVC number).

_In this case, I chose to use the postcode of Westminster Abbey._

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-card-valid-details.png" caption="C# ASP.Net MVC Core - Stripe Valid Payment Details" >}}

Our details look valid, so let's submit them and pay our £99 bill, with the hope of viewing a successful payment receipt back from **Stripe**.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-payment-receipt.png" caption="C# ASP.Net MVC Core - Stripe Payment Receipt" >}}

...and there you have it, PCI Compliant credit card payments using the Stripe API and C# ASP.Net MVC Core!

### We're Not Quite Finished...

Remember the Stripe dashboard that we hooked into earlier? Let's log back in and view our credit card payment transactions...

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-dashboard-one.png" caption="C# ASP.Net MVC Core - Stripe Dashboard Payment" >}}

As you can see, Stripe identify the credit card payment as successful, and specifically being for the customer name we entered at the point of payment.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-dashboard-two.png" caption="C# ASP.Net MVC Core - Stripe Dashboard Risk Analysis" >}}

Next we can see that Stripe's risk evaluation for the credit card payment was classed as "normal".

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-dashboard-three.png" caption="C# ASP.Net MVC Core - Stripe Dashboard Tracking" >}}

Remember the description and the ID that we assigned to the TransferGroup field as part of the **ChargeCreateOptions** object when submitting the payment? As you can see, **Stripe** track this information with the payment, along with the postcode data we submitted. You can also see that **Stripe's** checks against the credit card CVC number and postcode were indeed successful.

{{< figure src="/images/2019/03/simon-gilbert-cto-tech-blog-stripe-dashboard-four.png" caption="C# ASP.Net MVC Core - Stripe Dashboard IP Address" >}}

The final interesting piece of data that is tracked by **Stripe** at the point of processing a P**CI Compliant** credit card payment, is the registering of he device IP Address that I used when submitting the payment, along with the operating system, browser and device type that I was running. Kinda scary...albeit necessary.

Enjoy!