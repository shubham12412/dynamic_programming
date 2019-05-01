# Introduction
Given the weights and profits of ‘N’ items, we are asked to put these items in a knapsack which has a capacity ‘C’. 
The goal is to get the maximum profit from the items in the knapsack. Each item can only be selected once, as we don’t 
have multiple quantities of any item.

# Problem Statement
Given two integer arrays to represent weights and profits of ‘N’ items, we need to find a subset of these items which will 
give us maximum profit such that their cumulative weight is not more than a given number ‘C’. Each item can only be selected 
once, which means either we put an item in the knapsack or we skip it.

### Basic Solution
A basic brute-force solution could be to try all combinations of the given items 

```
for each item 'i' 
  create a new set which INCLUDES item 'i' if the total weight does not exceed the capacity, and 
     recursively process the remaining capacity and items
  create a new set WITHOUT item 'i', and recursively process the remaining items 
return the set from the above two sets with higher profit 
```
