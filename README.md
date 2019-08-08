# Are we there yet? Lessons learnt performance testing a Crypto Exchange API.

Slides for DDDSydney 2019 on 21st September 2019

Ben Shi ([@hbish](https://twitter.com/hbish))

## Summary
Performance testing is an important part of building a highly resilient and scalable system. More often than not performance check has always been a last-minute exercise to satisfy either a last-minute requirement or some requirements from 6 months ago that have been long forgotten. 

With the industry evolving to focus on delivery agility, there has been a heavy emphasis on unit testing and integration testing to ensure the code we ship is bug-free. But what about performance testing? Should it not also align to match the status quo?

My talk aims to share some of my past learnings, in particular, a recent side project testing the performance of a crypto exchange API. I will share how we can apply these learnings in building/designing scalable systems and steps we should take to tackle performance testing correctly to align with the latest industry trends like CI/CD, cloud and microservices.

## Building

Install fusuma

`npm i fusuma -D`

Install deps

`npm install`

Running

`npm run start`

Building

`npm run build`

Generate pdf

`npm run pdf`
