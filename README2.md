## 프로젝트 2 설명

### Heap 관련 함수

``` C
void buildHeap(HEAP h, int* array, int arraySize)
{
	//Check if the heap exists, and if it does not exist, an error message is output.
	if (h == NULL)
	{
		printf("Error: No heap exists.\n");
		return;
	}
	

	//Put each node in the heap using the insertNode function. There is a function that satisfies the heat order property and the heat shape property in the insertNode function.
	//Then this function will re-arrange the given array following the heap property and store to the heapData of the heap h.
	for (int i = 0; i < arraySize; i++)
	{
		insertNode(h, array[i]);
	}

}
```

 buildHeap 함수는 배열이 주어졌을 때, heap property를 만족하는 heap을 만들어내는 함수이다. heap을 만드는 방법에는 두가지가 있다. 배열에 있는 데이터를 index 순서대로 다 넣은 다음 heap property에 맞게 노드의 위치를 바꿔주는 방법과 데이터를 하나하나 insert하는 방법이 있는데 이 중에서 후자의 방법을 택했다. insertNode 안에 heap property를 맞춰주는 기능이 내포되어있기에 order를 조정하는 것은 buildHeap 함수에 표면적으로 드러나 있지는 않지만, 올바르게 실행한다. 만약에 만들어진 heap이 없다면 에러메세지를 출력하도록 하였다.
 
``` C
void deleteHeap(HEAP h)
{
	//Check if the heap exists, and if it does not exist, an error message is output.
	if (h == NULL)
	{
		printf("Error: No heap exists.\n");
		return;
	}

	//Every node freed separately.
	for (int i = (h->size) - 1; i >= 0; i--) {
		h->heapData[i] = NULL;
	}

	//The array of heap freed.
	free(h->heapData);

	 //Free the heap structure.
	free(h);

	//Print message “Successfully deleted.”.
	printf("Successfully deleted.\n");
}
```

 deleteHeap 함수는 heap을 삭제하는 함수이다. heap을 삭제할 때 배열의 모든 노드를 NULL처리 해준 다음 메모리 할당을 해제해주었고, 전체 heap 또한 삭제해주었다. 올바르게 작동한 이후에는 "Successfully deleted."라는 메세지를 띄우게 하였고, 만약에 만들어진 heap이 없다면 에러메세지를 출력하도록 하였다.


``` C
int findDepth(HEAP h)
{
	//Check if the heap exists, and if it does not exist, an error message is output.
	if (h == NULL)
	{
		printf("Error: No heap exists.\n");
		// Return -1 value to indicate an error.
		return -1;
	}

	//Using the correlation between the index of the parent node and the index of the child node, 
	// divide it by 2 until a value greater than 0 is obtained, and find the number of times it is divided.
	int depth = 0;
	int index = h->size;

	while (index > 0)
	{
		depth++;
		index /= 2;
	}

	return depth;
}
```

 findDepth함수는 heap의 depth를 반환해주는 함수이다. 부모노드의 index와 자식노드의 index간의 상관관계를 이용하여 깊이를 측정하는 알고리즘을 작성하였다. 만약 부모노드의 index가 1로 시작하게 된다면 마지막 노드의 index는 heap에 존재하는 모든 노드의 갯수와 같기 때문에, 그 값을 0 보다 작은 값이 나올 때 까지 2로 계속 나누어주어 나눈 횟수를 반환하였다. 만약에 만들어진 heap이 없다면 에러메세지를 출력하도록 하였다.

 
``` C
void insertNode(HEAP h, int value)
{
	//Check if the heap exists, and if it does not exist, an error message is output.
	if (h == NULL)
	{
		printf("Error: No heap exists.\n");
		return;
	}

	// Increase the size of the heap.
	h->size++ ;

	// Insert the new value at the end of the heap.
	h->heapData[(h->size)-1] = value;

	int i = (h->size) - 1;

	/*Since the index of the root node starts from 0, 
	the index of the left child node doubles the index value of the parent node and adds 1, 
	and the index of the right child node doubles the index value of the parent node and adds 2. 
	The node to be newly added is placed in the index at the end, and if the value is larger than the parent node, 
	the swap function is used to change the position.*/
	while (i >= 1 && h->heapData[i] > h->heapData[(i - 1)/ 2])
	{
		swap(&h->heapData[i], &h->heapData[(i - 1) / 2]);
		i = (i-1) / 2;
	}

}
```

 insertNode 함수는 새로운 노드를 기존의 heap에 삽입하고 order property 조정해주는 기능을 한다. 우선 heap의 사이즈를 1 늘린다음 새로 추가할 값을 배열의 가장 마지막 위치에 넣어준다. 그 후 부모노드와의 크기비교를 통해 부모노드보다 값이 크면 swap 함수를 실행하도록 하였고, 반복문을 활용해 order property를 맞출 때 까지 실행하도록 하였다. 루트 노드의 인덱스는 0부터 시작하므로 좌측 자식 노드의 인덱스는 부모 노드의 인덱스 값을 2배로 하여 1을 더한 값이고, 우측 자식 노드의 인덱스는 부모 노드의 인덱스 값을 2배로 하여 2를 더한 값임을 이용했다. 만약에 만들어진 heap이 없다면 에러메세지를 출력하도록 하였다.

``` C
int dequeueHeap(HEAP h)
{
	//Check if the heap exists, and if it does not exist, an error message is output.
	if (h == NULL || h->size == 0)
	{
		printf("Error: No heap exists.\n");
		return -1; 
	}								

	//Saves the value in a variable to return the value that was dequeued.
	int dequeuedValue = h->heapData[0];

	//Change the root node to the last node.
	h->heapData[0] = h->heapData[(h->size) - 1];

	// Decrease the size of the heap.
	h->size--;

	int current = 0;

	//It is the process of properly adjusting the heap property.
	while ((current * 2) + 1 < h->size )  // Check if there is a child.
	{
		int child = (current * 2) + 1;  // Left child index.

		
		// Compare which of the two children is bigger. 
		//The previous condition is whether there is a child on the right, and the condition on the right is a size comparison.
		if (child + 1 < h->size && h->heapData[child] < h->heapData[child + 1]) 
		{
			child++;
		}

		// Swap if the current node is smaller than the larger child
		if (h->heapData[current] < h->heapData[child])
		{
			swap(&h->heapData[current], &h->heapData[child]);
			current = child;
		}
		else
		{
			break; 
		}
	}


	return dequeuedValue;
}
```

 dequeueHeap 함수는 루트노드를 heap으로부터 dequeue하는 기능을 한다. dequeue를 실행한 후에 order을 조정해주어 heap의 property를 올바르게 유지하였다. dequeue한 값을 반환하기 위해 변수를 선언해 루트노드의 값을 저장한 후, heap의 가장 마지막 index에 있는 값을 루트노드로 옮긴 후 heap size를 1 감소시켰다. 그 다음 order property를 올바르게 하기 위해 자식노드와의 크기비교를 진행하였다. 앞선 함수에서도 사용하였듯 부모노드와 자식노드의 index 상관관계를 기반으로 order을 맞추었다. 우선 child 간의 크기 비교를 통해 더 큰 child값을 찾은 후 부모노드의 값과 비교를 하여 swap함수를 사용하여 위치를 바꾸어 주었다. order를 조정한 후에는 dequeue한 값을 반환해 주었다.  만약에 만들어진 heap이 없다면 에러메세지를 출력하도록 하였다.


 ``` C
void heapSort(HEAP h, int *heapsort, int count)
{
	buildHeap(h, heapsort, count);
}
```

 heapSort 함수는 배열에 있는 값을 heapsort 하는 함수이다. main함수를 살펴보니 정렬한 결과를 heap의 형태로 출력을 하기에 buildHeap 함수를 활용하였다.

 
 ``` C
void bubbleSort(int* Array)
{
	//If the value is small compared, the swap function is executed.
	for (int i = 0; i < 19; i++)
	{
		for (int j = 19 ; j >= i+1; j--)
		{
			if (Array[j] < Array[j - 1])
			{
				swap(&Array[j], &Array[j - 1]);
			}
		}
	}
}
```
 bubbleSort함수는 배열에 존재하는 값들을 크기에 맞게 bubblesort하는 기능을 한다. bubble sort는 배열안에 존재하는 값들을 비교하여, index가 더 큰 것에 저장된 값이 더 작다는 조건이 만족되면 매번 swap을 실행하는 정렬방법이다. 배열의 크기가 20으로 고정되었기에 따로 사이즈와 관련된 변수는 선언하지 않았으며, swap함수를 활용하였다.

  ``` C
void insertionSort(int* Array)
{ 
	for (int i = 1; i < 20; i++) {
		int key = Array[i];
		int j = i - 1;
		while (j >= 0 && Array[j] > key) {
			swap(&Array[j + 1], &Array[j]);
			j -= 1;
		}
	}
}
```

  insertionSort함수는 배열에 존재하는 값들을 크기에 맞게 insertionsort하는 기능을 한다. insertion sort는 배열안에 존재하는 값들을 비교하여 정렬된 부분과 정렬되지 않은부분으로 나누어 정렬되지 않은 부분에서의 값들은 정렬된 부분에 있는 값들과 비교하여 알맞은 위치에 넣는 알고리즘이다. 올바른 위치를 찾은 후 swap함수를 사용하여 값들의 교환을 하였으며,  배열의 크기가 20으로 고정되었기에 따로 사이즈와 관련된 변수는 선언하지 않았다.

   ``` C
void selectionSort(int *Array)
{
	
	for (int i = 0; i < 19; i++)
	{
		int low = i;
		for (int j = i + 1; j < 20 ; j++) {
			if (Array[j] < Array[low]) {
				low = j;
			}
		}
		swap(&Array[i], &Array[low]);

	}
}
```

selectionSort함수는 배열에 존재하는 값들을 크기에 맞게 selectionsort하는 기능을 한다. selection sort는 비교를 통해 가장 작은 값을 기억한 후 비교가 완료되면 올바른 위치로 값을 정렬하는 기능이다. 올바른 위치를 찾은 후 swap함수를 사용하여 값들의 교한을 하였으며,  배열의 크기가 20으로 고정되었기에 따로 사이즈와 관련된 변수는 선언하지 않았다.
