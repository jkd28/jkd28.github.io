---
layout: default
---

## Implementing Priority Queues
As part of my Algorithm Implementation class, I studied the theory and practice behind implementing Priority Queues (PQ) as
as efficiently as possible.  This project acted as a supplement to our studies of PQs.  


# The Assignment:
Write a menu-based user interface driver program (to be run in the terminal, no GUI), but most of the logic will be 
in implementing a **priority queue-based data structure**. The PQ-based data structure will store objects according
to the relative priorities of two of their attributes (Price and Mileage), making it efficient to retrieve objects with the 
minimum value of either attribute. **The data structure should further be indexable** to allow for efficient updates of entered 
items. Users should be able to enter details about cars that they are considering buying. The user should then be 
able to efficiently retrieve the car with the lowest mileage or lowest price. These retrievals should be possible on the set 
of all entered cars or on the set of all cars of a specific make and model (e.g., "lowest price Ford Fiesta", "lowest mileage 
Cadillac Escalade").

# My Implementation
I decided that two seperate queues would be preferable to gather the information/sort the values appropriately.  This means that
each Car objectis stored at least twice, once in a MileageQ (PQ for mileage of a car) and once in a PriceQ (PQ for price of a 
car).  I chose to do this because it was the most straightfoward way I could think of to approach the problem of 
sorting/prioritizing based on two seperate values. 

MileageQ and PriceQ are implemented exactly the same as each other, with a few changes to individual methods in order to keep 
each functioning to the purpose of the class. The Queues work by implementing an array-based heap as discussed in lecture.
This was the most efficient way for me to implement the heap structure, and also was the most familiar to me.

| Operation | Description                                                                                                            | Runtime |
|-----------|------------------------------------------------------------------------------------------------------------------------|---------|
| Insert    | Add the object to the last point of the heap array and swim the value up as necessary                                  | O(logn) |                                                               
| Remove    | Hash vin to get index of removed item, swap it with the last added value, and sink the swapped value down as necessary | O(logn) |  
| Update    | Hash vin to get index of item to update, update the car object as necessary                                            | O(1)    |
| Minimum   | Return the first value in the heap array                                                                               | O(1)    |
