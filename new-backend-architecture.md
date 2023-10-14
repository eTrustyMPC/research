# New backend architecture

After finishing first prototype and implementing all user stories for our MVP a few very important insights was emerged.

According to our new vision and leveraging better understanding of project goals and features a new technical and functional structure has been created.

This document will describe most important changes that need to be done in order to adapt project to real-life use cases.

*(не, если писать это всё сразу на английском - то я начинаю как-то стремительно охуевать от этой жизни, так что продолжим как обычно по старой схеме: сначала текст на русском, потом перевод)*

## Top Insights Gained

 - Most benefits that our app can provide to users comes from Web 3 ecosystem. So each time when we add new feature we should look at this feature from Web 3 perspective. Are we still following values of new ecosystem? I think this is a good way to find a direction of future grown
 - We are building an application [for two-sided market](https://www.investopedia.com/terms/t/two-sidedmarket.asp). This is a common pattern of economic activity and we should use properties of this pattern if we want to maximize impact of our actions
 - All tender platforms that we are discovered for now have a very low data quality and has no attempts to make this data better: no refine, no enrich, no attempts to create new or use existing data standards
 - The result of low data quality in the system - no ability to implement any form of AMM (Automatic Market Making). That means no order matching between two parties, no ability to facilitate requests for goods/services, manual selection of available tenders, no price discovery mechanisms and so on. All this features are mission-critical for users of two-sided market, and we can provide this functionality using proper set of tools. Thats's why we data collection piplines should become one of essential parts of our platform
 - The main part of the service is backend (as I thought before), not smart contracts
 - Main KPI for users with "Applicant" role: number of new orders (or leads) that our platform provided to them. Our target audience - employees from sales department and company owners. Why they should use our platform? to get more leads/orders
 - Main KPI for users with "Tender Owner" role: number of applications that our platform provided to them.

## Main Lessons Learned

 - Smart contracts cannot be used for managing core application artifacts (I mean "Entities" layer in [clean architecture pattern](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)). They are must be treated as last, most external layer. Persistance to blockchain should be implemented like any other DB persistance. Call to smart contract must be treated as usual call to external service. No need to store workflow definitions inside blockchain (only sync existing ones).
 - Smart contract state is a bad choice if you need a single source of truth. Database with basic CRUD API will work much better.
 - No more vendor lock-in on a specific blockchain (Partisia). Even if this is a main project investor. All feature implementations should be based on a functionality (immutable persistance, ZK-computation, identity verification, etc.), not on specific blockchain.
 - Access control logic also cannot be moved to smart contracts. They just don't have all necessary building blocks to implement it.
 - Zenstack sucks. API generation just not working as expected. Access control policies does not support essential features. Multi-tenancy support based on poorly designed ACL implementation cannot provide functionality that we need. We moving to loopback 4, that support all those features out-of-the-box (they did it 5 years ago, and this still works perfectly).
 - Creating data models from scratch was a second bad architecture decision. Starting from this point we will use [schema.org](https://schema.org) vocabulary as starting point for each model structure in our system. For e-commerce related entities we can use [GoodRelation](http://www.heppnetz.de/ontologies/goodrelations/v1.html) specification. This will help us to avoid reinvention of the wheel as long as possible
 - Auto-tests, written for backend API and Rust contract methods literally saved the project. Several times. We'll keep writing them.

## Technology stack

```
TODO
```