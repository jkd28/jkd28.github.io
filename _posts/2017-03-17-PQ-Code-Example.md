## Priority Queue Example Code

This is a sample of the code I used to complete the Priority Queue Assignment for COE 1501: Algorithm Implementation.

[Back to PQ Project Description]({{ site.baseurl }}{% post_url 2017-03-17-PQ-Car-Application %})

{% highlight Java %}  
/*
  Created by John Dott
  3/16/17
  COE 1501: Algorithm Implementation
  This is a class that acts as a Priority Queue for the price of cars, with the
  lowest price having the highest priority. This code is incredibly similar to 
	MileageQ.java, with just a few key calculations changed to reflect the sorting
	by price instead of mileage.
*/
import java.util.HashMap;
public class PriceQ{
	private Car[] q;  // the array to store the cars we are queueing
	private int last; // the next index to be inserted to in order to maintain the heap property
	private HashMap<String, Integer> indirect;	// the data structure for maintaining indirection to the PQ

	// constructor
	public PriceQ(){
		q = new Car[100]; // arbitrary size of 100
		last = 0;
		indirect = new HashMap<String, Integer>(100);	// also arbitrary size of 100
	}

	/******ACTION METHODS******/
	// get car with lowest price
	public Car getMin(){
		return q[0];
	}

	// insert a car into the PQ
	public void insert(Car car){
		q[last] = car;  // add item to last place in the array
		indirect.put(car.getVIN(), last);	// insert the item into indirection structure
		swim(last);     // now swim the value up to proper location
		last++;         // increment last position counter
		// perform a resize if necessary
		if(last == q.length) resizeQueue();
		return;
	}

	// remove any node from the PQ given the car's VIN
	public Car remove(String vin){
		int loc = getQIndex(vin);	// get the location of the car from the HashMap
		// if vin does not exist in the queue, terminate
		if(loc == -1) return null;
		Car temp = q[loc];
		swap(loc,--last); //decrement the last position counter, then swap the min value with the last value added
		q[last] = null;
		sink(loc);  // sink the swapped value down to a proper location

		// after the sink is finished, remove the car from the indirection structure
		indirect.remove(vin);
		return temp;
	}
		
	// update the mileage, given a valid vin number
	public int updateMileage(String vin, int newMiles){
		int location = getQIndex(vin);
		if(location != -1){ // check for vin existence
			q[location].setMileage(newMiles);
			return 1; //success 
		} else {
			return 0; //failed
		}
	}
		
	// update the color, given a valid vin
	public int updateColor(String vin, String newColor){
		int location = getQIndex(vin);
		if(location != -1){ // check for vin existence
			q[location].setColor(newColor);
			return 1; //success 
		} else {
			return 0; //failed
		}
	}
		
	// update the price given a vin, this requires reordering of the PQ 
	public int updatePrice(String vin, double newPrice){
		int location = getQIndex(vin);
		if(location != -1){ // check for vin existence
			double oldPrice = q[location].getPrice(); //save old number of miles
			q[location].setPrice(newPrice);
			// maintain heap property
			if(newPrice > oldPrice){
				//the car has decreased in priority
				sink(location);
				return 1;
			} else if (newPrice < oldPrice){
				//the car has increased in priority
				swim(location);
				return 1;
			} else if (newPrice == oldPrice){
				//no priority change, do nothing
				return 1;
			}
			return 1; //success 
		} else {
			return 0; //failed
		}
	}
		
	/********HELPER METHODS********/
	// swim a value up the heap to the proper location
	private void swim(int index){
		int parentIndex = getParentIndex(index);
		if (parentIndex<0 || parentIndex==index) return; // if there is no parent, terminate
		// if there is a parent node, we want to compare the price with the current node
		double parentPrice = q[parentIndex].getPrice();
		double currentPrice = q[index].getPrice();
		if(currentPrice < parentPrice){
			//if the parent's price is greater than our price, we need to swap
			swap(parentIndex, index);
			//after swap, continue to swim the value
			swim(parentIndex);
		}
		return;
	}

	// sink a value down the heap to the proper location
	private void sink(int index){
		// calculate locations of the children
		int leftChildIndex  = (2*index)+1;
		int rightChildIndex = (2*index)+2;

		// check existence of left child
		if(q[leftChildIndex] == null || leftChildIndex == last) return;  //no left child means we cannot proceed further
		if(q[leftChildIndex].getPrice() < q[index].getPrice()){
			// if the left child is of higher priority
			swap(leftChildIndex, index); // swap values
			sink(leftChildIndex);   //continue to sink the value
			return;
		}
		if(q[rightChildIndex] == null || rightChildIndex == last) return; // no right child exists, terminate
		if(q[rightChildIndex].getPrice() < q[index].getPrice()){
			//if the right child is of higher priority
			swap(rightChildIndex, index);
			sink(rightChildIndex);  //continue to sink the value
			return;
		}
		// should not reach this point, but return just in case
		return;
	}

	// get the index of a given vin using indirection
	private int getQIndex(String vin){
		if(indirect.containsKey(vin)) return indirect.get(vin);
		else return -1;
	}
		
	// calculate the location of a parent node given an index
	private int getParentIndex(int i){
		int parent = (int)Math.floor((i-1.0)/2.0);
		return parent;
	}

	// swap two nodes in the PQ by indexes
	private void swap(int first, int second){	
		Car temp = q[first];
		q[first] = q[second];
		q[second] = temp;

		//update entries in the indirection HashMap
		indirect.put(q[first].getVIN(), first);
		indirect.put(q[second].getVIN(), second);
		return;
	}

	// dynamically resize the queue by doubling its capacity
	private void resizeQueue(){
		int newSize = q.length*2; // set new size to double the previous
		Car[] newQ = new Car[newSize];
		for(int i = 0; i < q.length; i++){
			newQ[i] = q[i];
		}
		q = newQ; //set reference to old queue to new queue
		return;
	}

/*
	//function to print the Queue, strictly for testing
	public void printQ(){
		for(int i = 0; i < last; i++){
			System.out.println(i + ": " + q[i].getPrice());
		}
		return;
	}
*/
}
{% endhighlight %}

[Back to PQ Project Description]({{ site.baseurl }}{% post_url 2017-03-17-PQ-Car-Application %})
