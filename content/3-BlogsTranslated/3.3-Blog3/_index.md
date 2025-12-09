---
title: "Blog 3"
date: 2025-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# SMS Onboarding for SaaS, ISVs, and Multi-Tenant Applications with AWS End User Messaging

Author: Tyler Holmes

Publication date: May 13, 2025

Source: AWS End User Messaging, Messaging, SaaS

### Introduction

SMS messaging remains one of the most reliable and effective communication channels. However, for software-as-a-service (SaaS) companies, independent software vendors (ISVs), and multi-tenant solution providers that want to integrate SMS capabilities into their products, the journey can become complex and challenging.

This guide is designed specifically for technology providers—whether you are a SaaS company, an ISV, or any platform that allows your customers to send SMS messages to end users. Throughout this article, we use the following terms:

* **Provider:** An organization that offers SMS capabilities as part of its product or service.
* **Customer:** The entities that use the Provider’s technology to send SMS messages.
* **End User:** The recipients who have agreed to receive SMS messages from the Customer.

Implementing SMS can be complicated, with regulations that vary by country, registration processes that can take weeks or even months, multiple originator types (long codes, short codes, sender IDs, etc.) with different capabilities, and diverse needs across Customers and End Users. These challenges are even more pronounced when you, as a Provider, are offering SMS as a service to your own Customers, who in turn serve End Users.

By the end of this guide, you will understand:

* How opt-in flows affect your architecture.
* Options for structuring your SMS service for Customers.
* Strategies to reduce complexity during SMS implementation.

Let’s dive in.

---

## The Registration Dilemma: Who Owns the Relationship?

One of the most important decisions when registering an SMS originator is determining whose information will be used in the registration. The biggest mistake AWS often sees Providers make is not understanding how their relationship with Customers and Customers’ End Users will impact the system architecture and how they complete the required registration steps.

Mobile network operators want to know who will be sending SMS messages to their subscribers, how that entity collects opt-ins, and what content will be sent. When registering an originator—especially in the United States—you must clearly explain how End Users will opt in and confirm that their data will not be shared with any third parties. Your architecture must ensure:

* A clear opt-in process with a well-defined responsible entity.
* A privacy policy that accurately reflects how data is shared.
* Compliance with third-party data-sharing regulations.

(A deeper analysis of opt-in and registration flows can be found here. Sample clauses for privacy policies can be found here.)

AWS frequently sees Providers register themselves as the originator even though they have no direct relationship with the Customer’s End Users. The decision about whose information to use in registration mainly comes down to one basic question:

> **Who does the End User believe they are forming a relationship with when they provide their phone number?**

The most common scenarios are described below.

---

## Scenario 1: End Users Interact Only with the Customer’s Brand

In most cases, End Users are completely unaware of your existence as the Provider. They believe they are opting in to receive messages directly from your Customer. In this scenario:

* Registration should be done using the Customer’s information. There are many ways you can support this process, and later in this article we’ll discuss several methods to reduce common friction points.
* Messages must appear to be sent from the Customer, not from the Provider, and the name of your service should not appear in the message content.

---

## Scenario 2: End Users Opt In Directly Through the Provider’s Application

In some cases, End Users understand clearly that they are opting in to receive messages through your technology platform, on behalf of your Customers. Opt-in data is not shared with the Customer, and your brand as the Provider is the entity named in all outgoing SMS messages.

This can happen in several ways:

* End Users may opt in using a widget you build that Customers install on their websites or apps.
* A paper form or phone script you provide, which clearly identifies you as the Provider.

AWS often sees this scenario with Providers that offer:

* Third-party payment processing
* Shipping and logistics support
* Customer service platforms
* One-time-password (OTP) capabilities

In this scenario, your company name will typically appear in the messages, and registration will use your company’s information.

> **NOTE:** There are exceptions to these two scenarios and implementations can be complex. If you are a Provider and feel that you don’t fit neatly into either scenario, contact your account manager, open a support case, or talk to an expert before implementing anything.

---

## Architectural Models for Implementing SMS

Let’s explore different architectural models for building an SMS service based on your business needs and your relationship with Customers. Each model has its own characteristics:

---

### 1. “Bring Your Own AWS Account” Model

**Who performs registration and configuration?**

The Customer connects their own AWS account, so registration and configuration happen within the Customer’s account. The information used in registration in this scenario is typically the Customer’s, because it is their account.

**Customer responsibilities:**

* Perform all required registration and configuration steps.
* Integrate their account with the Provider’s service.
* Manage message sending, opt-out lists, etc.
* Pay the AWS bill.

**Provider responsibilities:**

* Provide a user-friendly interface that calls AWS End User Messaging Service APIs using the Customer’s credentials.
* The level of service the Provider offers can vary depending on needs.

**Best suited for:**

Technical Customers who want full control and are already familiar with AWS; Providers who want to avoid the complexity of registration and configuration.

---

### 2. “Provider Account – Manual Registration and Configuration” Model

**Who performs registration and configuration?**

The Provider owns the AWS account and does not give Customers a way to enter their own information, so the Provider enters it on their behalf.

* Customer information is collected manually.
* The Provider handles registration and configuration complexity via the console.

**Customer responsibilities:**

* Provide the necessary information to the Provider for registration.

**Provider responsibilities:**

* Manually collect registration information from Customers.
* Manage complexity on the Customer’s behalf.

This model can be implemented in two ways: using a separate AWS account for each Customer, or using a multi-tenant architecture within a single account.

**Best suited for:**

Providers with a small number of high-value Customers who need hands-on support throughout SMS onboarding.

---

You did an excellent job consolidating all of the SMS deployment models (“Architectural Models for Implementing SMS”) into a complete article that follows the AWS Storage Blog format.

Below is a suggested, fully written-out version in Vietnamese, split into sections with headings, paragraphs, and bullet points like in the example you provided:

*(This meta paragraph is commentary to the reader in Vietnamese; if you don’t need it in English in your final article, you can remove it.)*

---

### 3. “Semi-Automated – Customer Submits” Model

**Who performs registration and configuration?**

The Provider builds mechanisms for Customers to submit registration information, and then programmatically forwards this data to carriers/regulators.

**Customer responsibilities**

* Your platform manages technical configuration and sending capabilities, but Customers remain responsible for compliance.

**Provider responsibilities**

* Provide convenient ways for Customers to submit information (webhooks, forms, APIs).
* Automatically send registration data to regulators.
* Manage technical configuration and sending capabilities.

**Best suited for:**

Providers with moderate technical capabilities who want to reduce friction while preserving clear separation of legal responsibility.

---

### 4. “Fully Automated – Provider Sends” Model

**Who performs registration and configuration?**

Customer information is used in registration, but the Provider fully automates the registration process.

**Customer responsibilities**

* Focus solely on message-level compliance; all technical aspects are handled by the Provider.

**Provider responsibilities**

* Provide ready-to-use, customizable terms of service and privacy policies.
* Provide compliant opt-in channels (web forms, phone scripts, etc.).
* Handle all technical aspects of registration.

**Best suited for:**

Larger Providers serving many Customers with varying levels of technical sophistication.

---

### 5. “Fully Automated Messaging Constrained by Templates” Model

**Who performs registration and configuration?**

Customer information is used for registration, but the Provider processes it automatically.

**Customer responsibilities**

* Centralized compliance: they are only allowed to personalize fields within pre-approved message templates.

**Provider responsibilities**

* Provide a set of pre-approved message templates.
* Centrally manage compliance.
* Simplify registration because content is tightly controlled.

**Best suited for:**

Predictable messaging scenarios such as appointment reminders, delivery notifications, or OTP messages.

---

### 6. “Fully Managed Program” Model

**Who performs registration and configuration?**

Customers authorize the Provider to send messages on their behalf, meaning the Provider owns the relationship with the End User.

**Customer responsibilities**

* Provide only the information necessary to personalize messages (for example, a tracking number).

**Provider responsibilities**

* Manage the entire relationship with End Users.
* Control the entire messaging experience, including collecting opt-ins.

**Example:** A delivery notification service might send:

> “ShipTrack: Your order from ACME Corp will arrive tomorrow. Track it at [link].”

**Best suited for:**

Specialized scenarios where your platform delivers significant value as a clearly identified intermediary.

---

## Shaping Your SMS Service: Strategic Factors to Consider

### Pricing Strategy

When integrating SMS into your product, one of the first factors to consider is how to structure pricing. Unlike many digital services with predictable costs, SMS pricing varies significantly by destination country, originator type, and message volume.

AWS End User Messaging Service charges based on the volume of messages sent to each country, with a different rate per country. Pricing is determined by the country code of the recipient’s device, not their physical location. This means that even if you primarily serve customers in the United States, you may still need to account for international costs when recipients use non-US phone numbers.

There are also one-time and recurring fees to consider. Registrations often incur one-time processing fees, and number providers may charge monthly rental fees ranging from free to over USD 1,000 per month for short codes in some countries. Make sure you consider whether and how these costs will be passed on to your Customers.

When designing your pricing model, consider these common volume-based approaches:

* **SMS Credits:** Create a standardized credit system where Customers buy credits regardless of destination country. You manage the internal conversion between credits and actual cost.
* **Dollar-Based Allocation:** Give Customers a monetary budget that is consumed based on the actual cost of each message.
* **Country-Tiered Pricing:** Group countries into tiers (for example, Tier 1 for North America, Tier 2 for Western Europe) and charge different prices for each tier.
* **Bundled Messaging:** Include a certain number of messages in your base package, with additional charges for overage.

Each approach has trade-offs in simplicity, transparency, and risk management. Your choice should align with your overall business model and customer expectations.

---

### Geographic Considerations

Different countries have different regulatory requirements for SMS messaging, including:

* **Originator support:** Not all countries support all originator types. (See details here.)
* **Originator selection:** When multiple originator types are supported, how will you help Customers choose the right one for each use case? (Read this guide to help decide which originator type fits your use case.)
* **Registration:** Increasingly, countries require registration before you are allowed to send.
* **Quiet hours:** Many countries restrict when promotional messages can be sent.
* **Content restrictions:** Certain content types (gambling, alcohol, adult content, etc.) may be prohibited or heavily regulated. (A more complete list can be found here.)
* **Template requirements:** Some jurisdictions require pre-approval of message templates.
* **Sender ID regulations:** Rules about who can use alphanumeric Sender IDs vary widely.

As a Provider, you must decide which countries you will support and how you’ll maintain compliance across markets. This decision impacts not only pricing, but your entire product architecture—especially if you serve global Customers.

---

## Strategies to Reduce Friction During Implementation

Implementing SMS can be complex for your Customers. Below are several strategies that can simplify and/or streamline the process. Many of these can be combined and can also be offered as value-added services, or even as paid services, for your Customers:

### Provider-Hosted Privacy Policy and/or Terms & Conditions

Create compliant, customizable templates for privacy policies and terms & conditions that your Customers can use. This ensures SMS activities are fully disclosed without requiring Customers to update their own legal documents.

### Online Registration Forms and Flows

Develop user-friendly online forms that collect all required registration information in a guided process. These forms can significantly simplify complex procedures such as brand and campaign registration

### Pre-Approved Opt-In Widgets

Create embeddable widgets—like Figures 1–3 above—that your Customers can add to their websites or apps to implement compliant opt-in flows. These widgets can include all required disclosures and confirmations while remaining easy to integrate.

### Template Library

Provide a library of pre-approved message templates for common use cases. This reduces compliance risk and simplifies sending for your Customers.

### Test Environments

Create sandbox environments where Customers can test their SMS implementations before going live. This helps uncover issues with formatting, opt-in flows, or content compliance.

### Documentation and Training

Develop clear documentation and training resources specific to each originator type and use case. This helps your Customers operate more independently while reducing your support burden.

---

## Conclusion

Integrating SMS capabilities into your platform can significantly increase engagement with your Customers, but the journey can be complex. This guide has explored the key factors that will help you navigate it successfully.

We’ve examined multiple architectural models, each with its own trade-offs between Customer and Provider responsibilities. We’ve also looked at strategic factors such as pricing, geographic regulations, and originator types that must be carefully considered. Finally, we discussed practical strategies to reduce friction for your Customers—such as hosted compliance materials, streamlined registration flows, and pre-approved templates—to simplify integration.

The single most important first step is to clearly understand the relationship between you (as the Provider), your Customers, and their End Users. This determines whose information is used for originator registration and, in turn, shapes the SMS experience.

Ultimately, a successful SMS solution requires balancing technical considerations, regulatory requirements, and customer focus. By leveraging this guide, you can design and implement a service that delivers the best possible experience for both your Customers and their End Users.

---

### Additional resources

* How to plan for SMS workloads
* How to build a compliant opt-in program
* AWS End User Messaging SMS service documentation
* SMS API v2

---

### About Tyler Holmes

Tyler is a Senior Specialist Solutions Architect. He has extensive experience in communications as a consultant, solutions architect, practitioner, and leader at every level from startups to Fortune 500 companies. He has more than 14 years of experience across sales, marketing, and service operations, working for vendors, consultancies, and brands, building teams and growing revenue.
