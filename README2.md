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
 
