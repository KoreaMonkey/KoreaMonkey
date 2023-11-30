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

 buildHeap 함수는 배열이 주어졌을 때, heap property를 만족하는 heap을 만들어내는 함수이다. 
