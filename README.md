## 프로젝트 설명

### 리스트 관련 함수


``` C
void remove_node(struct linked_list* list, int node_location) 
//This function removes a node with node_location.
{                                  
	if (list->list_type == 1) {                                                                  
		printf("Function remove_node: The list type is stack. Removal is not allowed.\n");       
		return;
		//If the type of list is stack, print the error message.
	}

	if (node_location < 1 || node_location > list->num_nodes) {                                   
		printf("Function remove_node: Node_location typed is out of boundary\n");
		return;
		//If there is no node at node_location, print the error message.
	}

	if (list->num_nodes == 1) {                                                                   
		remove_list(list);
		return;
		//If there is only one node in the list, remove the node and remove the list also since there is nothing left.
	}

	// Remove a node at a specific location
	struct linked_node* current = list->head;
	int position = 1;

	while (position < node_location) {
		current = current->next;
		position++;
		//Move the current to the node where you want to remove it.
	}

	if (current->prev != NULL) {
		current->prev->next = current->next;
		//If the found node is not the first located node, change the next of the previous node of the node you want to remove to the next node of the node you want to remove.
	}
	else {
		list->head = current->next;
		current->next->prev = NULL;
		//If the node you want to remove is the first node, change the node indicated by the head and change the prev address of the next node to NULL.
	}

	if (current->next != NULL) {
		current->next->prev = current->prev;
		//If the node you want to remove is not the last node, change the prev of the next node of the node you want to remove to the previous node of the node you want to remove.
	}
	else {
		list->tail = current->prev;
		current->prev->next = NULL;
		//If the node you want to remove is the last node, change the node indicated by the tail, and change the next value of the previous node of the node you want to remove to NULL.
	}

	free(current);
	list->num_nodes--;
	//Reduce the value of num_nodes by one.
}

```

 remove_node 함수는 리스트 안의 특정 위치에 존재하는 노드를 삭제하는 함수이다. 리스트의 타입이 스택일 경우와 리스트 안에 노드가 하나도 없는 경우 에러 메세지를 출력한 후 함수를 탈출하게 하였다. 만약 리스트 안에 노드가 오직 1개만 존재하는 경우에는 주어진 remove_list 함수를 활용하여 리스트자체도 삭제시켜버렸다. 리스트 안에 노드가 2개 이상인 경우에는 노드가 제일 앞에 있는 경우와 제일 뒤에있는 경우, 중간에 있는 경우를 나누어 각각의 상황에 알맞게 prev와 next를 변경시켜주었다. 삭제 과정을 수행한 후에는 num_node의 값을 1 감소시켰다.
 


```c
void before_insert(struct linked_list* list, int newVal, int loc) 
// If list->num_node = 0 while list->num_node = 0 does not need to be considered because you create a node at first.
{ 
	if (list == NULL) {                                                     
		printf("Function before_insert: There is no such list\n");
		return;
		// Error if list does not exist.
	}

	if (list->list_type != 0 && list->list_type != 2) {                     
		printf("Function before_insert: The list type is not normal\n");
		return;
		//Error if list_type is not 0 or 2.
	}

	struct linked_node* new_node = create_node(newVal);
	

	if (loc == 1) {                                                    //If you insert the first node.
		new_node->next = list->head;                                   //The next of the new node points to the node indicated by the head.
		list->head->prev = new_node;                                   //Prev of the next node of the new node points to the new node.
		list->head = new_node;                                         //The place where the head points is a new node.
		list->num_nodes++;                                             //Increase the value of num_nodes by one.
	}
	else {                                                             //If you insert a node in the middle.
		struct linked_node* current = list->head;                      //Declare the current pointer to locate where to insert.
		int position = 1;                                              //Declare int position to tell you where you are.
		while (position < loc) {                                       //Move current to next until it is in the position to insert.
			current = current->next;                                        
			position++;
		}
		                                                                //Current arrives to the location to insert New and existing nodes
		new_node->prev = current->prev;                                 //Assign the prev value of the new node to the prev of the node indicated by current.
		new_node->next = current;                                       //Assign the next value of a new node to the address value of the node that current points to.
		current->prev->next = new_node;                                 //Assign the next of the previous node of the node that current points to as a new node.
		current->prev = new_node;                                       //Assign the prev of the node that current points to as a new node.

		list->num_nodes++;                                              //Increase the value of num_nodes by one.
	}
}

```

 before_insert 함수는 지정한 위치에 특정한 값을 저장한 노드를 삽입하는 함수이다. 리스트 자체가 존재하지 않을 경우와 리스트 타입이 올바르지 않을 경우 에러 메세지를 출력한 후 함수를 탈출하도록 하였다. 삽입하고 싶은 노드의 위치와 새로운 노드에 저장하고 싶은 값을 지정하면 삽입할 위치가 처음인 경우와 그렇지 않은 경우로 나누어 각각의 상황에 알맞게 노드들의 prev와 next를 변경시켜주었다. 삽입과정을 수행한 후에는 num_nodes의 값을 1 증가시켰다.


```c
void search_node(struct linked_list* list, int node_value) {                    //It is a function of finding the location of a node with a value of node_value.
	
	struct linked_node* current = list->head;                                   //Declaring the current pointer for node discovery. 
	int position = 1;                                                           

	while (current != NULL) {                                                   //Repeat until current encounters NULL because there is no value while navigating the node.
		if (current->node_id == node_value) {                                   //When you find the desired node, it tells you the location of the node and exits the function.
			printf("Function search_node: Node_ID %d is located in %dth node.\n", node_value, position);
			return;                                                            
		}

		current = current->next;                                                  //If the condition of the if statement is not met, point to the next node.
		position++;                                                              
	}   

	printf("Function search_node: No such node id in the list\n");                //Output an error message if there is no value.
	return;
}
```
 search_node 함수는 리스트 안에서 특정한 값을 저장하고 있는 노드의 위치를 탐색하는 함수이다. 입력받은 값을 가진 노드가 없는 경우 에러메세지를 출력하고 함수를 탈출하도록 하였다. 탐색 방법으로는 처음 노드에 저장된 다음 주소를 이용하여 제일 마지막 노드까지 이동하며 입력받은 값과 노드에 저장된 값을 비교하는 방법으로 실행된다. 특정한 값을 가진 노드를 찾았다면 그 위치를 출력하고 함수를 탈출하도록 하였다.

 
```c
void convert_to_circularLinkedList(struct linked_list* list) {                           
	//This function converts normal Linked list to Circular linked list.
	
	if (list->list_type != 0) 
	{
		printf("Function convert_to_CircularLinked: The list type is not normal.\n");
		return;
	//This function should be used to the list with linked_list, so if list is Stack or already Circular_linked_list, print error message.
	}
	// Make the Linked list Circular
	list->tail->next = list->head;
	list->head->prev = list->tail;

	// Update list type to Circular Linked list
	list->list_type = 2;

	printf("Function convert_to_CircularLinkedList: Complete the converting to circular linked list.\n");
	return;
}

```

 convert_to_circularLinkedList 함수는 일반적인 linked list를 circular linked list로 바꿔주는 함수이다. 만약 바꾸려는 리스트의 타입이 normal상태가 아니라면 에러메세지를 출력하고 함수를 탈출하도록 하였다. 일반적인 linked list를 circular linked list로 바꿔주는 방법으로는 head가 가리키는 노드의 이전 주소를 tail이 가리키는 노드의 주소로, tail이 가리키는 노드의 다음 주소를 head가 가리키는 노드의 주소로 변경시켜 줌으로 실행한다. 그 이후에 list_type를 변경시켜주고 함수를 탈출한다.


```c
void rotate_circularLinkedList(struct linked_list* list, int rotate_num) {
	//This function rotates the Circular Linked List.

	if (list->list_type != 2) {
		printf("Function rotate_circularLinkedList: The list type is not Circular Linked List.\n");
		return;
	//This function should be used to the list with Circular linked list, so if list is Stack or Linked_list print error message.
	}

	rotate_num = rotate_num % list->num_nodes; 
	// Adjust rotation_num appropriately.

	for (int i = 0; i < rotate_num; i++) {
		list->head = list->head->next;
		list->tail = list->tail->next;
	//By moving the head and tail, the same result as the list rotates.

	}

	printf("Function rotate_circularLinkedList: Complete the %d time rotation.\n", rotate_num);
	return;
}
```

  rotate_circularLinkedList 함수는 circular linked list를 rotate_num만큼 회전시켜주는 함수이다. 만약 리스트의 타입이 circular list가 아니라면 에러메세지를 출력하고 함수를 탈출하도록 하였다. 회전시켜주는 방법으로는 rotate_num과 리스트 안의 총 노드의 갯수와의 관계를 이용하여 rotate_num을 적절한 값으로 변경시킨 후 head와 tail의 next주소를 rotate_num만큼 현재 가리키는 노드의 다음노드의 주소로 변경시킴으로 실행한다.




### 스택 관련 함수


```c
int check_empty(struct Stack* stack) 
//Check if the stack is empty through the values of top.
{
	if (stack->top == -1) {
		return 1; // Stack is empty
	}
	else {
		return 0; // Stack is not empty
	}
}

```

 check_empty 함수는 스택이 비어있는지 확인하여 비어있으면 1, 그렇지 않으면 0을 반환하는 함수이다. 실행 방법으로는 top에 저장된 값이 -1일 경우 비어있는 경우, 그렇지 않은 경우는 비어있지 않다는 점을 이용하였다.
