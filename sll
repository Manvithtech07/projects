#include<stdio.h>
#include<stdlib.h>
#include "sll.h"

List *slist_new(){
	List *list = (List*)malloc(sizeof(List));
	list->head = NULL;
	list->tail = NULL;
	list->length = 0;
	return list;
}

//The static keyword means this function can only be used inside this file (slist.c). 
static Node *_list_node_new(int32_t data){
	Node *node = (Node*)malloc(sizeof(Node));
	node->data = data;
	node->next = NULL;
	return node;
}

List *slist_add_head(List *list, int32_t data){
	Node *node = _list_node_new(data);
	node->next = list->head;
	list->head = node;
	if(list->tail == NULL){
		list->tail = node;
	}
	++list->length;
	return list;
}
List *slist_add_tail(List *list, int32_t data){
	Node *node = _list_node_new(data);
	if(list->tail != NULL){
		list->tail->next = node;
		list->tail = node;
	}else{
		list->head = list->tail = node;
	}
	++list->length;
	return list;
}
List *slist_add_at_postion(List *list, int32_t data, uint32_t position){
	if(position<1 || position > list->length+1){
		printf("Invalid position. List has %u elements.\n", list->length);
		return list;
	}
	if(position==1){
		return slist_add_head(list,data);
	}
	if(position==list->length){
		return slist_add_tail(list,data);
	}
	//code starts
	Node *previous = list->head;
	for(uint32_t i=1;i<position-1;++i){
		previous = previous->next;
	}
	Node *node = _list_node_new(data);
	node->next = previous->next;
	previous->next = node;
	++list->length;
	return list;
}

List *slist_delete_head(List *list){
	if(list->head == NULL){
		printf("List is Empty/ Nothing to delete");
		return list;
	}
	Node *node_to_delete = list->head;
	list->head = list->head->next;
	free(node_to_delete);
	--list->length;
	if(list->head == NULL){
		list->tail = NULL;
	}
	return list;
}
List *slist_delete_tail(List *list){
	if(list->head == NULL){
		printf("List is Empty/ Nothing to delete");
		return list;
	}
	if(list->head == list->tail){
		return slist_delete_head(list);
	}
	Node *current = list->head;
	while(current->next != list->tail){
		current = current->next;
	}
	free(list->tail);
	list->tail = current;
	list->tail->next = NULL;
	--list->length;
	return list;

}
List *slist_delete_at_position(List *list, uint32_t position){
	if(position<1 || position>list->length){
		printf("Invalid position. List has %u elements.\n", list->length);
		return list;
	}
	if(position==1){
		return slist_delete_head(list);
	}
	Node *previous = list->head;
	for(uint32_t i=1;i<position-1;++i){
		previous = previous->next;
	}
	Node *node_to_delete = previous->next;
	previous->next = node_to_delete->next;

	if(node_to_delete==list->tail){
		list->tail = previous;
	}
	free(node_to_delete);
	--list->length;
	return list;
}

uint32_t slist_search(List *list, int32_t key){
	uint32_t position = 1;
	Node *current = list->head;
	while(current!=NULL){
		if(current->data==key){
			return position;
		}
		current=current->next;
		position++;
	}
	return 0;
}
void slist_reverse_and_nth_from_end(List *list, uint32_t n){
	if(n<1 || n>list->length){
		printf("Invalid %uth position",n);
		return;
	}
	Node *ref_ptr = list->head;
	for(uint32_t i=1;i<n;++i){
		ref_ptr=ref_ptr->next;
	}
	Node *previous=NULL;
	Node *current=list->head;
	Node *next_node=NULL;
	list->tail=list->head;
	while(current!=NULL){
		next_node=current->next;
		current->next=previous;
		previous=current;
		current=next_node;
	}
	list->head=previous;
	display_list(list);
	printf("%d node from the end is %d\n",n,ref_ptr->data);
}

int32_t slist_get_min_max(List *list, int32_t *min, int32_t *max){
	if(list->head==0){
		return 0;
	}
	*min = list->head->data;
	*max = list->head->data;
	Node *current = list->head->next;
	while(current!=NULL){
		if(current->data < *min){
			*min = current->data;
		}
		if(current->data > *max){
			*max = current->data;
		}
		current= current->next;
	}
	return 1;
}

void slist_remove_duplicates(List *list){
	if(list==NULL || list->head==NULL){
		return;
	}
	Node *current = list->head;
	while(current!=NULL){
		Node *runner = current;
		while(runner->next!=NULL){
			if(runner->next->data==current->data){
				Node *node_to_delete = runner->next;
				if(node_to_delete == list->tail){
					list->tail = runner;
				}
				runner->next = node_to_delete->next;
				free(node_to_delete);
				--list->length;
			}else{
				runner= runner->next;
			}
		}
		current=current->next;
	}
}

void slist_bubble_sort(List *list){
	if(list==NULL || list->head==NULL || list->head->next==NULL){
		return;
	}
	int swapped;
	Node *current;
	do{
		swapped=0;
		current=list->head;
		while(current->next!=NULL){
			if(current->data>current->next->data){
				int32_t temp = current->data;
				current->data = current->next->data;
				current->next->data = temp;
				swapped = 1;
			}
			current=current->next;
		}
	}while(swapped);
}

List* slist_union(List *list, List *list2){
	List *result = slist_new();
	Node *current;
	current=list->head;
	while(current!=NULL){
		slist_add_tail(result,current->data);
		current=current->next;
	}
	current = list2->head;
	while(current!=NULL){
		slist_add_tail(result, current->data);
		current=current->next;
	}
	slist_remove_duplicates(result);
	return result;
}

List *slist_intersection(List *list,List *list2){
	List *result = slist_new();
	Node *current1 = list->head;
	while(current1!=NULL){
		Node *current2 = list2->head;
		while(current2!=NULL){
			if(current1->data == current2->data){
				if (slist_search(result, current1->data) == 0) {
				slist_add_tail(result, current1->data);
			}
			break;
		}
		current2 = current2->next;
	}
	current1=current1->next;
}
	return result;
}

void display_list(List *list){
	printf("List (length %u) HEAD-> ",list->length);
	Node *current = list->head;
	while(current!=NULL){
		printf("%d -> ",current->data);
		current = current->next;
	}
	printf("NULL\n");
}
void slist_free(List *list){
	Node *current = list->head;
	while(current!=NULL){
		Node *p = current;
		current = current->next;
		free(p);
	}
	free(list);
}
