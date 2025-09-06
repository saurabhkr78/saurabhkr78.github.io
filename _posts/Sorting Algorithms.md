# Sorting Algorithms | Saurabh Choudhary


  

| Algorithm                 | Best Time Complexity | Average Time Complexity | Worst Time Complexity | Space Complexity | Stable | In-place |
| ------------------------- | -------------------- | ----------------------- | --------------------- | ---------------- | ------ | -------- |
| **Bubble Sort**           | O(n)                 | O(n²)                   | O(n²)                 | O(1)             | Yes    | Yes      |
| **Direct Insertion Sort** | O(n)                 | O(n²)                   | O(n²)                 | O(1)             | Yes    | Yes      |
| **Binary Insertion Sort** | O(n)                 | O(n²)                   | O(n²)                 | O(1)             | Yes    | Yes      |
| **Selection Sort**        | O(n²)                | O(n²)                   | O(n²)                 | O(1)             | No     | Yes      |
| **Shell Sort**            | O(n)                 | O(n^1.3)                | O(n²)                 | O(1)             | No     | Yes      |
| **Merge Sort**            | O(n log n)           | O(n log n)              | O(n log n)            | O(n)             | Yes    | No       |
| **Quick Sort**            | O(n log n)           | O(n log n)              | O(n²)                 | O(log n)         | No     | Yes      |
| **Heap Sort**             | O(n log n)           | O(n log n)              | O(n log n)            | O(1)             | No     | Yes      |
| **Bucket Sort**           | O(n)                 | O(n log(n/m))           | O(n²)                 | O(n)             | Yes    | No       |
| **Counting Sort**         | O(n + k)             | O(n + k)                | O(n + k)              | O(k)             | Yes    | No       |
| **Radix Sort**            | O(nk)                | O(nk)                   | O(nk)                 | O(n + k)         | Yes    | No       |


* * *

Contents

* * *

*   [Bubble Sort](#bubble-sort)
*   [Insertion Sort](#insertion-sort)
    *   [Direct Insertion Sort](#direct-insertion-sort)
    *   [Binary Insertion Sort](#binary-insertion-sort)
    *   [Shell Sort (Hill Sort)](#shell-sort-hill-sort)
*   [Selection Sort](#selection-sort)
*   [Merge Sort](#merge-sort)
*   [Quick Sort](#quick-sort)
*   [Heap Sort](#heap-sort)
*   [Linear Sort](#linear-sort)
    *   [Bucket Sort](#bucket-sort)
    *   [Counting Sort](#counting-sort)
    *   [Radix Sort](#radix-sort)

* * *

Bubble Sort[](#bubble-sort)
---------------------------

**Mechanism:** Bubble Sort is a simple sorting algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. The process is repeated until the list is sorted.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort1.jpg)](/assets/img/note/algorithm/sort1.jpg)

**Advantages:**

*   Simple and easy to understand.
*   Works well for small datasets or nearly sorted arrays.

**Disadvantages:**

*   Inefficient for large datasets with a time complexity of O(n^2).
```
void bubbleSort(int arr[], int n) {
    bool swapped;
    for (int i = 0; i < n - 1; i++) {
        swapped = false;
        // Last i elements are already sorted
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // Swap arr[j] and arr[j+1]
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        // If no two elements were swapped in inner loop, array is sorted
        if (!swapped) break;
    }
}
```
``

Insertion Sort[](#insertion-sort)
---------------------------------

### Direct Insertion Sort[](#direct-insertion-sort)

**Mechanism:** This algorithm iteratively takes one element from the unsorted portion and finds its appropriate place in the sorted portion of the array.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort2.jpg)](/assets/img/note/algorithm/sort2.jpg)

**Advantages:**

*   Simple implementation.
*   Efficient for (nearly) sorted data sets.

**Disadvantages:**

*   Poor efficiency on large, unsorted data sets.

```
void insertionSort(int arr[], int n) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];     // Current element to insert
        int j = i - 1;

        // Move elements of arr[0..i-1] that are greater than key
        // to one position ahead of their current position
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;  // Place key at the correct position
    }
}
```
``
<blockquote class="prompt-tip"> <p><strong>Why is insertion sort more popular than bubble sort</strong> since both bubble sort and insertion sort have the same time complexity O(n^2) and are stable sorting algorithms?</p> <ul> <li><strong>Because data swapping in a bubbling sort is more complex</strong> than data movement in an insertion sort (bubbling requires 3 assignment operations, while insertion only requires 1)</li> </ul> </blockquote>


### Binary Insertion Sort[](#binary-insertion-sort)

**Mechanism:** Similar to direct insertion but uses binary search to find the proper location for insertion, reducing comparisons.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort2.jpg)](/assets/img/note/algorithm/sort2.jpg) _same as the direct insertion sort (except that it’s faster to find the insertion position)_

**Advantages:**

*   Reduced number of comparisons.

**Disadvantages:**

*   More complex than direct insertion sort.
*   Movement of elements is still costly.

``

> **Why is the time complexity of binary insertion sort still n2?**
> 
> *   Although the number of comparisons is reduced, the number of swaps is not, and all the elements still need to be shifted after the position is found.

### Shell Sort (Hill Sort)
[](#shell-sort-hill-sort)

**Mechanism:** An extension of insertion sort that allows the exchange of far apart elements to improve speed. Shell sort improves the efficiency of insertion sort by moving larger elements in advance to reduce the number of reverse order pairs.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort7.jpg)](/assets/img/note/algorithm/sort7.jpg) _from: http://stoimen.com/2012/02/27/computer-algorithms-shell-sort/_

**Advantages:**

*   Better handling of large data sets compared to simple insertion.

**Disadvantages:**

*   Gap sequence choice is critical for performance.
    *   Initially Shell proposed to take `gap = n/2` and `gap = gap/2` up to `gap = 1`. Later Knuth proposed to take `gap = gap/3 + 1`. Neither claim has been proved.


	```
	void shellSort(int arr[], int n) {
    // Start with a big gap, then reduce the gap
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // Perform a gapped insertion sort
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j = i;

            // Shift earlier gap-sorted elements up until
            // the correct location for arr[i] is found
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            arr[j] = temp;
        }
    }
}
	```
``

Selection Sort[](#selection-sort)
---------------------------------

**Mechanism:** Repeatedly finding the minimum element (considering ascending order) from the unsorted part and putting it at the beginning.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort3.jpg)](/assets/img/note/algorithm/sort3.jpg)

**Advantages:**

*   Simplicity and ease of understanding.

**Disadvantages:**

*   Inefficient on large lists.

``

> **Is selection sort a stable sorting algorithm?**
> 
> *   **No.** Selection sort has to find the smallest of the remaining unsorted elements each time and swap places with the previous element, which breaks stability. For example, if you sort a set of 5, 8, 5, 2, 9 using the selection sort algorithm, the first time you find the smallest element, 2, and swap places with the first 5, the order of the first 5 and the middle 5 changes, so it is unstable.
>     *   **It is for this reason that Selection Sort is slightly inferior to Bubble Sort and Insertion Sort.**

```
void selectionSort(int arr[], int n) {
    for (int i = 0; i < n - 1; i++) {
        // Assume the minimum element is the first unsorted element
        int minIndex = i;

        // Find the minimum element in the unsorted part
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }

        // Swap the found minimum with the first unsorted element
        if (minIndex != i) {
            swap(arr[i], arr[minIndex]);
        }
    }
}
```

Merge Sort[](#merge-sort)
-------------------------

**Mechanism:** Merge Sort is a **divide-and-conquer** algorithm that divides the input array into two halves, calls itself for the two halves, and then merges the two sorted halves.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort4.jpg)](/assets/img/note/algorithm/sort4.jpg)

**Advantages:**

*   Stable sorting algorithm.
*   Consistently runs in O(n log n) time.

**Disadvantages:**

*   Requires additional space proportional to the array size.
*   Slightly more complex than other simple sorting algorithms.

```
// Function to merge two subarrays
void merge(int arr[], int left, int mid, int right) {
    int n1 = mid - left + 1;  // size of left subarray
    int n2 = right - mid;     // size of right subarray

    // Create temporary arrays
    int L[n1], R[n2];

    // Copy data to temp arrays
    for (int i = 0; i < n1; i++) L[i] = arr[left + i];
    for (int j = 0; j < n2; j++) R[j] = arr[mid + 1 + j];

    // Merge the temp arrays back into arr[left..right]
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }

    // Copy remaining elements of L[], if any
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    // Copy remaining elements of R[], if any
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

// Function to perform Merge Sort
void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2; // Avoids overflow

        // Sort first and second halves
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);

        // Merge the sorted halves
        merge(arr, left, mid, right);
    }
}
```

Quick Sort[](#quick-sort)
-------------------------

**Mechanism:** Quick Sort is a **divide-and-conquer** algorithm. It picks an element as a pivot and partitions the given array around the picked pivot. There are different versions of quickSort that pick pivot in different ways.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort5.jpg)](/assets/img/note/algorithm/sort5.jpg)

**Advantages:**

*   One of the fastest sorting algorithms for average cases.
*   Space-efficient and does not require additional storage.

**Disadvantages:**

*   Worst-case time complexity can be O(n^2), although this is rare.
*   Not stable, meaning it may change the relative order of elements with equal keys.

``

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort6.jpg)](/assets/img/note/algorithm/sort6.jpg) _partition(int arr\[\], int low, int high)_

* * *

We can use `partition()` in the quick sort algorithm to **find the K largest element in an unordered array in O(n)** time complexity.

```

// Partition function (Lomuto partition scheme)
int partition(int arr[], int low, int high) {
    int pivot = arr[high]; // Choose the last element as pivot
    int i = low - 1;       // Index of smaller element

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]); // Place pivot in correct position
    return i + 1;
}

// Quick Sort function
void quickSort(int arr[], int low, int high) {
    if (low < high) {
        // Partition the array
        int pi = partition(arr, low, high);

        // Recursively sort left and right parts
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}
```

Heap Sort[](#heap-sort)
-----------------------

**Mechanism:** Builds a max heap from the data, then repeatedly extracts the maximum element from the heap and rebuilds the heap until all elements are sorted.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort8.jpg)](/assets/img/note/algorithm/sort8.jpg)

**Advantages:**

*   Good for large data sets, with consistent performance.

**Disadvantages:**

*   Relatively complex algorithm, not stable.

``

> **In practice, why does Quick Sort perform better than Heap Sort?**
> 
> 1.  **Data access** of Heap Sort is **worse** than Quick Sort.
>     *   For quick sort, data is accessed sequentially. Whereas, for heap sort, the data is accessed in jumps. This is not friendly to the CPU cache.
> 2.  In general, Heap Sort has **more data swaps** than Quick Sort.


```
// Function to heapify a subtree rooted at index i
void heapify(int arr[], int n, int i) {
    int largest = i;      // Initialize largest as root
    int left = 2 * i + 1; // Left child
    int right = 2 * i + 2; // Right child

    // If left child is larger than root
    if (left < n && arr[left] > arr[largest])
        largest = left;

    // If right child is larger than largest so far
    if (right < n && arr[right] > arr[largest])
        largest = right;

    // If largest is not root
    if (largest != i) {
        swap(arr[i], arr[largest]);

        // Recursively heapify the affected subtree
        heapify(arr, n, largest);
    }
}

// Main function to perform Heap Sort
void heapSort(int arr[], int n) {
    // Build a max heap
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);

    // Extract elements from heap one by one
    for (int i = n - 1; i > 0; i--) {
        // Move current root to end
        swap(arr[0], arr[i]);

        // Call max heapify on the reduced heap
        heapify(arr, i, 0);
    }
}
```
Linear Sort[](#linear-sort)
---------------------------

Linear Sort refers to a **sorting algorithm that has a linear time complexity (`O(n)`, `O(n + k)`, `O(nk)`)**.
### Bucket Sort[](#bucket-sort)

**Mechanism:** Distributes elements into several “buckets” and sorts these individually.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort9.png)](/assets/img/note/algorithm/sort9.png)

**Advantages:**

*   Fast when the input is uniformly distributed.
*   It Can be applied to external sorting.
    *   External sorting means that the sorted data is stored in an external disk. (Due to the relatively large amount of data and limited memory, it is not possible to load all the data into internal memory)

**Disadvantages:**

*   The requirements for the data to be sorted are very demanding (Ideally, the data is distributed evenly across the buckets).
*   Performance depends on data distribution and bucket count.

```
// Function to perform Bucket Sort
void bucketSort(float arr[], int n) {
    // 1. Create n empty buckets
    vector<vector<float>> buckets(n);

    // 2. Put array elements into different buckets
    for (int i = 0; i < n; i++) {
        int idx = n * arr[i]; // index in bucket
        buckets[idx].push_back(arr[i]);
    }

    // 3. Sort individual buckets
    for (int i = 0; i < n; i++) {
        sort(buckets[i].begin(), buckets[i].end());
    }

    // 4. Concatenate all buckets into arr[]
    int index = 0;
    for (int i = 0; i < n; i++) {
        for (float val : buckets[i]) {
            arr[index++] = val;
        }
    }
}
```

### Counting Sort[](#counting-sort)

**Mechanism:** Counts the occurrences of each value to sort. Counting sort is actually a special case of bucket sorting (Assign a bucket to each possible value so that the data values in each bucket are the same, eliminating the need to sort the buckets).

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort10.jpg)](/assets/img/note/algorithm/sort10.jpg)

**Advantages:**

*   Linear time complexity in the suitable context.

**Disadvantages:**

*   Not suitable for data with large range relative to the number of elements.
*   Not an in-place algorithm.

```
// Counting Sort function
void countingSort(int arr[], int n) {
    // 1. Find the maximum element
    int maxVal = arr[0];
    for (int i = 1; i < n; i++) {
        if (arr[i] > maxVal)
            maxVal = arr[i];
    }

    // 2. Create count array and output array
    vector<int> count(maxVal + 1, 0);
    vector<int> output(n);

    // 3. Store frequency of each element
    for (int i = 0; i < n; i++) {
        count[arr[i]]++;
    }

    // 4. Compute prefix sums to get positions
    for (int i = 1; i <= maxVal; i++) {
        count[i] += count[i - 1];
    }

    // 5. Build the output array (stable sort)
    for (int i = n - 1; i >= 0; i--) {
        output[count[arr[i]] - 1] = arr[i];
        count[arr[i]]--;
    }

    // 6. Copy output back to original array
    for (int i = 0; i < n; i++) {
        arr[i] = output[i];
    }
}
```

### Radix Sort[](#radix-sort)

**Mechanism:** Sorts numbers digit by digit, starting from the least significant digit to the most significant.

[![](https://www.cwblogs.com/assets/img/note/algorithm/sort11.jpg)](/assets/img/note/algorithm/sort11.jpg)

**Advantages:**

*   Can be faster than comparison-based algorithms for large data sets.

**Disadvantages:**

*   Only works for integer keys or types that can be represented as such.
*   Depends on another stable sort algorithm (like counting sort) for sorting digits.

```
// Utility function to get the maximum value in arr[]
int getMax(int arr[], int n) {
    int maxVal = arr[0];
    for (int i = 1; i < n; i++)
        if (arr[i] > maxVal)
            maxVal = arr[i];
    return maxVal;
}

// Stable Insertion Sort for a specific digit (exp = 1, 10, 100, ...)
void insertionSortByDigit(int arr[], int n, int exp) {
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        // Compare digits (using current exp)
        while (j >= 0 && (arr[j] / exp) % 10 > (key / exp) % 10) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}

// Radix Sort using Insertion Sort for each digit
void radixSort(int arr[], int n) {
    int maxVal = getMax(arr, n);

    // Sort by each digit (LSD → MSD)
    for (int exp = 1; maxVal / exp > 0; exp *= 10)
        insertionSortByDigit(arr, n, exp);
}
```
``

* * *

