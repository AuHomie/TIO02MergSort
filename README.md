Learning Outcomes:

This program will cover the following topics:

Templates
Inheritance
Recursion
Sorting
Memory Management with dynamic allocated memory. 
While this contains many concepts, this assignment largely lets you mimic what has been shown previously. 

As you code this problem, make sure to keep checking back at the Q & A discussion board to help others or get help programming this problem.   You can and should share small code segments on this discussion board to help each other out.

Description:

The Learning Activities walked you through implementing sorting algorithms, with each derived class managing its own sorting details.  In particular, the Quick Sort required recursion and a divide-and-conquer strategy, and has an average complexity of O(n log n)

For this Try-It-Out you will implement a two-way Merge Sort.  This sort algorithm is commonly used for two reasons: 1) it has a worst case complexity of O(n log n), and 2) it has a property called stable, which allows it to be used to have sorted data within sorted data (e.g. a list of names sorted by last name, and within the same last name they are sorted by first name.).  

A difficulty of Merge Sort is that it requires extra memory: two inputs must be sorted into separate output. That issue means many implementations found online aren't generalized or aren't efficient. This Try-It-Out will walk you through implementing a generalized Merge Sort. 

This algorithm uses recursion through a divide-and-conquer strategy.

Complete the following steps:

class MergeSort: The class should inherit the BaseSort class and be templated like the other derived classes.
MergeSort(const unsigned int capacity): Add a constructor which accepts a capacity.  Call the BaseSort constructor, with the first argument containing the description "Two-Way Merge", and the second passing in the capacity.  See the other classes for the pattern to follow.  Add a {} at the end to indicate that this constructor's body is defined here and has no additional logic.
public - void runSort():  The purpose of this method is to simply get the recursion started by calling the private runSort() method.  Thus, the body of this method needs only one line of code: a call to runSort(), passing in 0 for the first index, and this->capacity for the last index.
private - void runSort(const unsigned int firstIndex, const unsigned int lastIndex): This method is the core of the sorting algorithm, which will be described in more detail below.  Overall, every call to this runSort() method processes the array region between firstIndex and lastIndex.  The code works through these steps:
1) A base case to determine if the recursion should stop.
2) Determine where to split the array region into two halves, either of equal size (for even sized regions) or with a size different only by one (for odd sized regions).
3) Recursively calls each runSort() on each half of the array region.
4) Merging together the two sorted array region halves by
  4a) copying each array half into a temporary arrays and
  4b) sorting the each half back into the original array. 
A full description is below:
1) The base case: Verify that the array region is at least two items large, as arrays of size 1 are sorted by definition, and sizes smaller than 1 shouldn't be processed.  Thus, the if statement logic should be:
if lastindex - firstIndex is less than 2, return.
2) Determine how to split the array into two halves: Compute a middle index between the firstIndex and the lastIndex.  Store this in a variable called middleIndex.  This middleIndex is used to split your array region into two parts.  In other words, if firstIndex is 30 and lastIndex is 40, then middleIndex should be 35.  If your array region has an odd number of items, just let middleIndex naturally round down via integer division.  In other words, if firstIndex is 30 and lastIndex is 41, then middleIndex should be 35.
3) Recursively calls each runSort() on each half of the array region.  No special syntax is required for a recursive call, just call runSort() and pass the appropriate two arguments.  The first recursive call is for the first array half region, from firstIndex to middleIndex.  The second call is for the second array half region, from middleIndex to lastIndex.  Note that firstIndex is inclusive and lastIndex is exclusive, or in other words, if firstIndex is 30 and lastIndex is 40, then runSort() will sort items 10 items, 30 through 39.
4a) Merging together the two sorted array region halves by copying each array half into a temporary arrays. After the two recursive calls, each array half region should be sorted.  You must merge these two half regions.  This step is not trivial.  Merging two half regions together cannot be done in-place, or in other words, must utilize separate temporary array space.  So this 4a step becomes a preparation step. The prep work creates two temporary arrays (left half and right half), then copies data from the existing array into these two temporary arrays. Now that the data is prepped in two temp arrays, 4b can merge them back into the original array. 
Compute the size of your left array half into a variable called leftHalfSize.  Use your middleIndex and firstIndex to compute this size.
Compute the size of your right array half into a variable called rightHalfSize.  Use your lastIndex and middleIndex to compute this size.
Create a temporary array to store the left array half.  As this assignment uses templates and dynamic arrays, and these concepts are still somewhat new to most students, here is the line of code you will use:   T* leftArray = new T[leftHalfSize];
Create a temporary array to store the right array half.  Use a similar line of code as the prior bullet point.
Create a loop to copy all values from the left array half into leftArray.  This loop will iterate leftHalfSize times.  Suppose your looping variable is called i.  The leftArray has its first value at index 0 and thus will just use [i].  The this->arr has its first value at firstIndex and will thus use [firstIndex + i]
Create similar code to copy values of your right array half into rightArray.  Note that the this->arr is offset starting at middleIndex. 
...4b) sorting the each half back into the original array.
   
With the prep work done, the merge can start.  This section of code has three arrays total, leftArray, rightArray, and this->arr.  Similarly, create three unsigned int variables to track indexes for the three arrays.  Call these leftIndex, rightIndex, and arrIndex, and initialize leftIndex and rightIndex to 0. Initialize arrIndex to firstIndex.
Merge the smallest values from either the leftArray or the rightArray by creating a while loop with the following structure:
while the left index is less than the left array size and the right index is less than the right array size
  if the left array at the left index is less than or equal to the right array at the right index
    assign this->arr at the arr index the value of the left array at the left index
    increment the left index 
  else
    assign this->arr at the arr index the value of the right array at the right index
    increment the right index
  increment the arr index
That prior while loop runs until either the left array or the right array will be fully used up.  Since one input array is used up but the other is not, then implement this logic afterward to complete the merging logic
while the left index is less than the left array size
  assign this->arr at the arr index the value of the left array at the left index
    increment the left index
    increment the arr index
while the right index is less than the right array size
  assign this->arr at the arr index the value of the right array at the right index
    increment the right index
    increment the arr index
Almost done.  Just reclaim the two dynamic arrays with delete[] leftArray; and delete[] rightArray;.
Expected output:

The program's output should resemble the following.  Note that merge sort will run a bit slower than quick sort and heap sort, largely due to frequent dynamic memory allocations and deallocations (an optimized version of merge sort would utilize a memory pool to avoid this).  If your IDE easily supports running in an optimized release mode and without debug flags, your times will improve by several factors.
