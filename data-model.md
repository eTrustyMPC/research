# Data model

Updated app data model, based on [schema.org](https://schema.org) vocabulary.

## Tender Workflow

Updated process definition, based on Schema.org definitions.

## Main Entities

New workflow will be based on 3 main entities:
 - [Demand](https://schema.org/Demand) to describe what needs to be delivered in specific Tender, using each Lot as single Demand
 - [AggregateOffer](https://schema.org/AggregateOffer) to describe Offer from side of Tender Owner 
 - [Offer](https://schema.org/Offer) to describe specific Offer from side of Applicant 
 - [Order](https://schema.org/Order) to describe tender lot winner (or several winners). "Become a winner" now means a very specific thing - making on Order (agreement) between Tender Owner and Applicant. Currently not present in our DB schema.

### Base Action Sequence

1) Tender Owner creates tender and Lots, describing current **Demand**
2) Applicants create **Offer**s that match **Demand** conditions
3) Tender Owner choose one or more **Offer**s to close each previously described **Demand**
4) **Demand** considered *closed* when Tender Owner creates an **Order** based of Applicant's **Offer** for this demand

