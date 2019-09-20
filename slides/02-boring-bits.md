import { Card } from '@fusuma/client';

<!--note
- Let take a look at what we will be talking about today.

- We start off with what I call the boring bits, and thats an intro into performance testing and how cryptocurrency exchanges work.

- We will then move into the lessons learnt section. Where I will be sharing about 6 lessons from the crypto exchange API project

- and finally we will go through some performance engineering tips before I wrap up.
-->
## Agenda

- Boring Bits
- Lessons Learnt
- Performance Engineering
- Wrap up

---
<!-- note
- So just a quick show of hands

- Who here is a developer? and how many of you are tester or QA?

- Who has done some sort of performance testing of APIs?

- Awesome thats a lot more than I expected
-->

## Quick Survey 🗳

---
<!-- sectionTitle: Performance testing overview -->
<!-- note
- So a quick intro those of you who havent had exposure to performance testing

- it is a testing practice performed to determine how a system behaves under load. 

- Its a testing technique used to investigate, measure and validate non-functional attributes of a system, like resource usage, responsiveness, scalability and reliability.
-->

## ...a testing practice performed to determine how a system performs in terms of responsiveness and stability... 

(source: wikipedia)

--- 
<!-- note
- So when most people think about performance testing, they think of something like this

- You start fast. If the system is still working then you go faster. And if its still working you go all out.
-->
## So like?

<div class="grid">
  <div class="column">
    <img src="../static/images/load-fast.gif" alt="slow button bash"/>
    <p>fast</p>
  </div>
  <div class="column">
    <img src="../static/images/load-faster.gif" alt="faster button bash"/>
    <p>faster</p>
  </div>
  <div class="column">
    <img src="../static/images/load-fastest.gif" alt="fastest button bash"/>
    <p>fastest</p>
  </div>
</div>

--- 
<!-- note
- But actually there are several different types of performance testing out there and they achieve different purposes 

- for example a load/spike testing is to check the system's responsiveness under load

- soak testing is where you hit the system with a sustained load over a long period of time to check for memory leaks and what not

- breakpoint testing is where you slowly ramp up the request until your system breaks

- configuration testing is where you test different setup within your system, for example instance size or the number of instances for a particular service etc

- If you are interested in knowing more about all the different type of performance tests. I recommend this book. The art of performance testing. It will go into much more details that I have.
-->
## What flavours does it come in?

- Load testing
- Stress testing
- Soak testing
- Spike testing
- Breakpoint testing
- Configuration testing
- Isolation testing
- Internet testing

<hr /> 

<small>additional reading 

The Art of Application Performance Testing - Ian Molyneaux

</small>

---
<!-- classes: fullscreen-bg performance-testing -->
<!-- note
- majority of the people out there measure response time and throughput

- And this adorable axolotl (ak·suh·laa·tl) is probably going to go very hungry trying to eat if he is responding at the rate its doing.

- So is there anything else we can measure?
-->
## What can we measure?

---
<!-- note
- Absolutely, You can measure from 2 different perspectives, the client or the server

- And when we talk about response time from the client, we are measuring the round trip time with network latency included in it where as from the server perspective were only looking at the how quickly the server can process the request and generate a response.

- From the server side you can also look at availability, CPU, memory usage, disk and network utilization. 

- And if your system has a database, caching layer or queues then you can also look at connection pooling or cache hit/miss ratio or queue depth. And there are a lot more metrics out there than the ones that I have listed here.
-->
## What can we measure?
<div class="wrap grid">
    <div class="column">
        <h4>Client</h4>
        <ul>
            <li>Latency/Response Time - Round Trip</li>
            <li>Throughput</li>
        </ul>
    </div>
    <div class="column">
        <h4>Server</h4>
        <ul>
            <li>Latency/Response Time - Processing Time</li>
            <li>Throughput</li>
            <li>Availability</li>
            <li>Server metrics - CPU, memory, disk I/O, network I/O...</li>
            <li>Connection pooling</li>
            <li>Cache hit/miss ratios</li>
            <li>Queue depth</li>
            <li>and more...</li>
        </ul>
    </div>
</div>

---
<!-- sectionTitle: Crash course on exchanges -->
<!-- note
- Now on to exchanges

- A trading exchange as the name implies is a marketplace where people exchange one thing for another

- So the thing can be anything from fiat money (like aussie dollar or US dollar), stocks, bonds, and in our case crypto-currencies

- If we distill it down, the core function of an exchange is to ensure fair and orderly trading from all participants on the exchange and the fast dissemination of price information as the trades occur
--->
## Crypto Exchange: A Crash Course

---
<!-- note
- Most crypto exchanges are centralized meaning the exchange creates wallets on your behalf and they safeguards your money

- Centralised exchanges require you to perform KYC (know you customer) basically an identity check through some sort of personal identifiable information and there are different levels of KYC which would grant you additional trading volumes, there for you are not anonymous on a centralised exchange

- There is a growing trend moving towards decentralised exchanges (DEXs) after a few hacking incidents such as Mt. Gox, Bitfinex and Binance. Decentralized exchanges allows you to interact on the exchange using your personal wallet. So you have complete control, but it tends to be a lot slower and have lower trading volume.
-->
<img src="../static/images/exchanges.png" alt="Centralized vs Decentralized Exchanges" /> 
<small>source: <a href="https://en.bitcoinwiki.org/wiki/DEXes">bitcoinwiki</a></small>

--- 
<!-- note
- Here are some typical endpoints you may see on a crypto exchange

- Endpoints to retrieve you user and account information like your crypto holding and wallet details. They allow you to deposit and withdraw you cryptocurrency

- Trade/Order endpoints allows you to submit orders on to the exchange. To be executed when another order gets matched on the opposite side. So for example if you submitted an order to buy bitcoin for 10000 and someone else submitted a sell order for the same price, they get matched and executed. These endpoints are the ones where I performed my testing on and its what I will be talking about later on.

- Market data endpoints basically allows you to check symbols/tickers to see prices and the order book. Where all of the orders on the exchange are aggregated by the price and you get to see the volume by supply and demand.

- So exchanges also have a websocket feed to retrieve real-time information like the price of the symbol or if orders gets matched. If you are interested in learning more, then go on a popular exchange and have a look at their API documentation.
-->
<Card
  right={<img src="../static/images/api.png" alt="Centralized vs Decentralized Exchanges" />}
  left={
    <>
      <h2>API</h2>
    </>
  }
/>

--- 
<!-- note
- Now lets take a look at a typical exchange architecture

- This is a extremely simplified view of how a crypto exchange works

- Its usually fronted by the APIs we just talked about and below that we have the order matching engine and asset management service.

- Moving down we have settlement engine and that gets invoked when orders get executed to make sure the funds gets moved in and out of the correct accounts. 

- If you were to deposit or withdraw crypto then there are usually wallet services that handles the communication with the blockchain. 

- And below that we have some common services like KYC, MFA and infrastructure backbone like database, event bus and message queues.

- If you are interested in learning more about crypto exchanges then do checkout this article by Naveen where he talks about it in more detail

-->
## Typical Architecture

```text
+-------------+---------------+
| RPC/Web API | Websocket API |
+----------------------------------------+
| Matching Engine | Asset Management     |
+----------------------------------------+-------------------+       +--------------------+
| Settlement Engine | Account Management | Wallet Management | <---> | Blockchain Network |
+----------------------------------------+---+---------------+       +--------------------+
| KYC Service | MFA/Security Services        |                                |
+--------------------------------------------+                                |
| Networking | Database | Event Bus & MQ     | <------------------------------+
+--------------------------------------------+
| Cold Wallet Storage                        |
+--------------------------------------------+
```

<hr />
<small>additional reading: <a href="https://hackernoon.com/how-do-cryptocurrency-exchanges-work-and-what-technologies-are-driving-disruption-33d0007eb018">How do cryptocurrency exchanges work? and what technologies are driving disruption - Naveen Saraswat</a>
</small>
