---
title: '"Java - Lambda"'
draft: 
tags:
  - java
  - lambdas
  - jdk
---
# Lambdas

Lambdas were replacement of anonymous class. They give is the power of treating code as data. That means, now a function can take another function as input parameter. This makes things easy in a lot of ways. Specially, when we need to do data transformations. We can apply single filter and apply it on a collection and then return the filtered response. 


Lambdas are functional interface (Any interface that has only one abstract method, it can have one or more default and static methods in it). If we don't have lambdas we need to use anonymous class that will look like this. 

```
printObject(myCollection, 
			new CheckMyCollection() {
				public boolean test(Person p) {
					return p.getGender() == Persopn.Sex.MALE
					&& p.getAge() >=16 && p.getAge() <=25;	
				}
			}
		);
```

Lambda version: 

```
printObject(
	roster, (Person p) -> p.getGender() == Person.Sex.MALE && p.geAge() >=16 && p.getAge() <=25
);
```

# Predicate Interface

