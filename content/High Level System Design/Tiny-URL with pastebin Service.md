---
title: '"Tiny-URL Service"'
draft: 
tags:
  - architecture
  - High-Level-Design
---
## System Specs

1. Build a tiny url like website that can support 1 trilion short urls. 
2. Average paste of the content would be 1 KB 
3. Most popular clicks can be in million, average urls have 10k clicks.

## What specific pattern are we solving for here ?

1. Generating Unique short random strings. 
2. how to get the click counts for an item.


## System type

More reads, less writes in comparision. (We need caching in these cases)

## Write throughput: How to handle it ?

Possibilties are high, we need a faster throughput, because as soon as user generates as link, he copy and pastes it and we need an instant redirection. So writes for us are critical. 

1. Multi Leader system 
2. Write back cache 
3. Partitioning the data, helps in writes as well. 


## New stuff 

	1. Predicate locks 
	2. Which database engine to use?
		1. B-Trees
			1. Good for reads bad for writes
		2. LSM
			1. Bad for read, good for writes.



## Stream processing for count 






