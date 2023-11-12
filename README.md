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

void remove_node 함수는 노드를 지우는 함수로 
