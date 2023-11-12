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

 remove_node 함수는 리스트 안의 특정 위치에 존재하는 노드를 삭제하는 함수이다. 리스트의 타입이 스택일 경우와 리스트 안에 노드가 하나도 없는 경우 에러 메세지를 출력한 후 함수를 탈출하게 하였다. 만약 리스트 안에 노드가 오직 1개만 존재하는 경우에는 주어진 remove_list 함수를 활용하여 리스트자체도 삭제시켜버렸다. 리스트 안에 노드가 2개 이상인 경우에는 노드가 제일 앞에 있는 경우와 제일 뒤에있는 경우, 중간에 있는 경우를 나누어 각각의 상황에 알맞게 prev와 next를 변경시켜주었다. 삭제 과정을 수행한 후에는 num_node의 값을 1 줄여주었다.
 


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

 before_insert 함수는 지정한 위치에 특정한 값을 저장한 노드를 삽입하는 함수이다. 리스트 자체가 존재하지 않을 경우와 리스트 타입이 올바르지 않을 경우 에러 메세지를 출력한 후 함수를 탈툴하도록 하였다.

