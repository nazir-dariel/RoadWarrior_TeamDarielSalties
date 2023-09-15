# Context 
Context is king.  There's a business need, and technology is applied to meet that need. Ultimately, it starts with the business, not the technology.  Thus, before we decide how to use technology to *solve* the business problem, we should first seek to *understand* the problem. 
 

In this section, we unpack Road Warrior's requirements and the thought process that led us to our architecture.  We've <mark>highlighted</mark> the items we believe to be architecturally significant – those items that will actually influence the architecture of the solution. 

## Problem Statement and Requirements 
### The Road Warrior 
A new startup wants to build the next generation online trip management dashboard to allow travellers to see all of their existing reservations organized by trip either online (web) or through their mobile device. 


### Users 
<mark>2 million active</mark> users/week 

Total users: <mark>15 million (user accounts) </mark>


### Requirements 

* <mark>Poll email</mark> looking for travel-related emails 
* Filter and whitelist certain emails 
* The system must <mark>interface with</mark> the agency’s existing airline, hotel, and car rental interface system to update travel details (delays, cancellations, updates, gate changes, etc.). 
* Updates must be in the app <mark>within 5 minutes</mark> of an update (better than the competition). 
* Customers should be able to add, update, or delete existing reservations manually as well. 
* Items in the dashboard should be able to be grouped by trip, and once the trip is complete, the items should automatically be removed from the dashboard. 
* Users should also be able to share their trip information by interfacing with standard social media sites or allowing targeted people to view your trip. 
* <mark>Richest</mark> user interface possible across all deployment platforms. 
* Provide end-of-year summary <mark>reports</mark> for users with a wide range of metrics about their travel usage. 
* Road Warrior gathers <mark>analytical data</mark> from users trips for various purposes - travel trends, locations, airline and hotel vendor preferences, cancellation and update frequency, and so on. 
* Must <mark>integrate<mark> seamlessly with <mark>existing travel systems</mark> (i.e, SABRE, APOLLO). 
* Must integrate with preferred travel agency for quick problem resolution (help me!) 
* Must work <mark>internationally</mark>. 
* Users must be able to access the system at all times (max 5 minutes per month of unplanned downtime) 
* Travel updates must be presented in the app within 5 minutes of generation by the source 
* <mark>Response time</mark> from web (800ms) and mobile (first-contentful paint of under 1.4 sec) 

## Assumptions 

In addition to everything explicitly covered by the brief, we have made the following assumptions:
* Selling aggregated/summarized on trends, vendor preferences, etc. will be a revenue stream for Road Warrior. 

# Architecture Characteristic Analysis 

The brief and requirements led us to believe that certain architecture characteristics are particularly important. 

* **Scalability** – we have 2 million active weekly users and up to 15 million active users in total; the other 13 million users can become active at any time (maybe travel picks up because of the next FIFA world cup) and our solution needs to deal with that. 

* **Performance** – we differentiate ourselves from our competition by being faster than alternatives. 

* **Interoperability** – we don't exist in a vacuum; Road Warrior integrates with travel agencies, email services providers, APIs, etc.   

* **Usability** – we want to offer our users rich experiences that work seamlessly across multiple channels. 

* **Resiliency** – we cannot allow more than 5 minutes of unplanned downtime per month; imagine the knock our reputation will take if someone is stuck at the airport and misses their flight because Road Warrior is down. 

* **Security** – customers trust us enough to give us custodianship of their data.  In particular, that data includes their emails, which underpins almost everything we do.  If email is compromised, customers risk compromising everything that uses multi-factor authentication – from their Spotify accounts to their online banking profiles.  With great power comes great responsibility.  

# Architecture Principles 

Architecture decisions are hard.  There are rarely answers that are obviously right or obviously wrong – in architect speak "it depends".  Much of what we do comes down to balancing trade-offs.  By establishing our architecture principles upfront, we have something to help guide our decisions. 

1. Design for scalability 
2. Design for resiliency 
3. Design for simplicity 
4. Gather only what data we need - don’t take on unnecessary risk 
5. Make decisions that benefit the business and the customer – e.g. vendor lock-in is okay, if it provides clear benefits 