---
title: '"Proximity Service"'
draft: 
tags:
  - High-Level-Design
  - Estimation
  - architecture
  - Proximity-service
---
# Functional Requirements

1. Return all business based on user's location. 
2. Business owners can add and update their information, but this does't needs to be refelected in real-time.
3. Customer can view detailed information about a business.

# Non functional Requirements

1. Low Latency: User should be able to see results quickly. 
2. Data privacy: As location is sensitive information, follow GDPR. 
3. High availability and scalability on peak hours in dense areas.

# Some basic calculation estimations. 

1. Assuming we have 100 million DAU and 200 million Businesses
2. Second in a day is 84K for simplicity lets take it 100k seconds. Assuming we get 5 queries/day
	1. Search QPS = 5 x 100 million = 500 million(6 zeros)/100000 = 5000 requests/second

# API Design

This api return a list of business given a location. 

```
GET /v1/search/nearby

params: {
	"latitude": decimal, // required 
	"longitude": decimal, // required
	"radius": int // optional, (default 3000 meters, 3 kms)
}

response : {
	"total": 10, 
	"business": [{Business object}]
}

```


For fetching additional attributes after selecting a business 

```

GET /v1/business/:id
POST /v1/business
PUT /v1/business/:id
DELETE /v1/business/:id
```

# Data model

#### Read/Write Ratio

As we can see our system is read heavy, due to two reasons. 
1. Search for nearby
2. View Detailed Information
Write is low because the business actions are into that frequent.

A relational MYSQL can be a good fit for this.
# Data Schema

Key database tables are the business and geospatial index tables.
### Business table

```
_id: PK
address
city
state
country
latitude
longitude
```

# High level Design
```
graph TD
    User --> LoadBalancer

    LoadBalancer --> LBS
    LoadBalancer --> BusinessService

    LBS --> Replica1
    LBS --> Replica2
    LBS --> Replica3

    BusinessService --> PrimaryDatabase

    PrimaryDatabase --> Replica1
    PrimaryDatabase --> Replica2
    PrimaryDatabase --> Replica3

    Replica1 -->|Replicate| PrimaryDatabase
    Replica2 -->|Replicate| PrimaryDatabase
    Replica3 -->|Replicate| PrimaryDatabase

    subgraph "Database Cluster"
        Replica1
        Replica2
        Replica3
        PrimaryDatabase
    end
```


### Location based service

1. It is a read-heavy service with no write requests.
2. QPS is high, especially during peak hours in dense areas. 
3. This service is stateless so it's easy to scale horizontally

### Business service

1. CUD is not high QPS
2. Read is high QPS during peak hours.

### Database Cluster

We can have a master-slave setup, writes will go to master and replicated to slave. This might cause some inconsistency but should not create a problem as the writes doesn't need to have real time changes. 

### Algo to fetch nearby business

1. We as we have lat and long we simply can use that and create a circle witha radius R, 
	1. This will be problematic as we need to scan the whole table and see if it fall into the range
	2. Even creating indexes will not help
	3. We need a wat to Map 2D data to 1D

# Geo Spatial indexing 

We have 2 types

1. Hash based 
	1. Even Grid
	2. GeoHash
	3. Cartesian Tiers
2. Tree
	1. QuadTree
	2. Google S2
	3. RTree

The idea is to divide the map into smaller areas and build indexes.

