import { Card } from '@fusuma/client';

## Agenda

<!-- contents -->

---
<!-- sectionTitle: Quick Overview of Performance Testing -->
<!-- note: It can also serve to investigate, measure, validate or verify other quality attributes of the system, such as scalability, reliability and resource usage. 
-->

## ...a testing practice performed to determine how a system performs in terms of responsiveness and stability... 

(source: wikipedia)

--- 
## What flavours does it come in?

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
## What flavours does it come in?

- Load testing
- Stress testing
- Soak testing
- Spike testing
- Breakpoint testing
- Configuration testing
- Isolation testing
- Internet testing

---
<!-- classes: fullscreen-bg performance-testing -->
<!-- note
- Majority (90%) of the time people measure response time and throughput
-->
## What can we measure?

---
<!-- sectionTitle: Crash course on exchanges -->
<!-- note
- An trading exchange as the name implies is a marketplace where people exchange one thing for another
- It can be fiat money, physical commodity and in our case crypto-currencies
- Core function of an exchange is to ensure fair and orderly trading and efficient dissemination of price information
- Most exchanges are centralized meaning the exchange creates wallets on your behave and safeguards your money
- But there is a huge trend moving towards decentralised exchanges after a few hacking incidents such as Mt. Gox, Bitfinex and Binance.
-->
## Crypto Exchange: A Crash Course

<img src="../static/images/exchanges.png" alt="Centralized vs Decentralized Exchanges" /> 
<small>source: <a href="https://en.bitcoinwiki.org/wiki/DEXes">bitcoinwiki</a></small>

--- 
<Card
  left={<img src="../static/images/api.png" alt="Centralized vs Decentralized Exchanges" />}
  right={
    <>
      <h2>API</h2>
    </>
  }
/>

--- 
## Typical Architecture
- 

