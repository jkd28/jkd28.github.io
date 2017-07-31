---
layout: default
---

## Implementing Priority Queues
As part of my Algorithm Implementation class, I studied the theory and practice behind implementing Priority Queues (PQ) as
as efficiently as possible.  This project acted as a supplement to our studies of PQs.  
My codebase for this project is a bit too large to demonstrate all of it in a page like this, but please feel free to view 
[my GitHub page](https://github.com/jkd28/COE1501-Project3) for the project.  

[View a Sample of my Priority Queue Implementation]({{ site.baseurl }}{% post_url 2017-03-17-PQ-Code-Example %})

### The Assignment:
Write a menu-based user interface driver program (to be run in the terminal, no GUI), but most of the logic will be 
in implementing a **priority queue-based data structure**. The PQ-based data structure will store objects according
to the relative priorities of two of their attributes (Price and Mileage), making it efficient to retrieve objects with the 
minimum value of either attribute. **The data structure should further be indexable** to allow for efficient updates of entered 
items. Users should be able to enter details about cars that they are considering buying. The user should then be 
able to efficiently retrieve the car with the lowest mileage or lowest price. These retrievals should be possible on the set 
of all entered cars or on the set of all cars of a specific make and model (e.g., "lowest price Ford Fiesta", "lowest mileage 
Cadillac Escalade").

### My Implementation
I decided that two seperate queues would be preferable to gather the information/sort the values appropriately.  This means that
each Car object is stored at least twice, once in a **MileageQ** (PQ for mileage of a car) and once in a **PriceQ** (PQ for price of a 
car).  I chose to do this because it was the most straightfoward way I could think of to approach the problem of 
sorting/prioritizing based on two seperate values. 

MileageQ and PriceQ are implemented exactly the same as each other, with a few changes to individual methods in order to keep 
each functioning to the purpose of the class. The Queues work by implementing an array-based heap as was discussed in our lecture.
This was the most efficient way for me to implement the heap structure, and also was the most familiar to me.  

### Runtime Analysis
In addition, I had to provide a Runtime Analysis for each operation.  In the following table, each runtime is noted in Big-O
notation, where *n* is the number of cars:

| Operation | Description                                                                                                            | Runtime |
|-----------|------------------------------------------------------------------------------------------------------------------------|---------|
| Insert    | Add the object to the last point of the heap array and swim the value up as necessary                                  | O(logn) |                                                               
| Remove    | Hash vin to get index of removed item, swap it with the last added value, and sink the swapped value down as necessary | O(logn) |  
| Update    | Hash vin to get index of item to update, update the car object as necessary                                            | O(1)    |
| Minimum   | Return the first value in the heap array                                                                               | O(1)    |

### Interesting Points  
The most significant implementation detail is the use of Hashing.  Hashing the vin number made the most sense to me, since it would
always be unique to the vehicle, unlike the other attributes.  It is the hashing that gives this iimplementation of Priority
Queue the "Indexable" trait.  This allowed me to keep an index of each vehicle as it was added to the data structure, and 
significantly increased the runtimes of the available operations such as updates and removes, which would normally require
us to seek the desired item through the entirety of the data structure.

### Challenges and Resolutions
The most difficult part of this project was balancing the runtime complexity and space complexity of the data structure.
Since the project was based on efficiency in terms of the speed of operations, I tended to focus solely on runtime complexity
in my design.  These tradeoffs were further emphasized by the time constraints placed on finishing the project for the class. 

Unfortunately, this meant that I sacrificed a more space than may have been necessary.  My implementation requires that 
each Car be stored in at least two separate locations in the data structure. Of course, this is highly inefficient in terms 
of space.  If I were to do this project again, I would try to design a system which resolves this issue, perhaps by using a 
single data structure to store both Price and Mileage priority information.  
