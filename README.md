
#include <iostream>
#include <string.h>
// #include "linkedlist.h"
#include <stdlib.h>
using namespace std;

struct linked_list{
    int data;
    char name[10];
    linked_list *next;
};
typedef struct linked_list LinkedList;
LinkedList* create_node(int value, char *str_value){
    /*
    input args: 1) integer value
                2) string value
    output      1) return node pointer with input values
    */
    // create new node
    LinkedList *new_node;
    // allocate memory to node
    new_node = (LinkedList*)malloc(sizeof(LinkedList));
    // allocate values to node
    new_node->data = value;
    strcpy(new_node->name,str_value);
    new_node->next = NULL;
    // return node pointer
    return new_node;
}

void action(LinkedList* temp) {
    /*
    Input args: 1) pointer to a node
    output:     1) print the node values
    */
    // print the node values
    cout << "[" << temp->data << "]:-\t[" << temp->name << "] \n";
}

LinkedList* traverse(LinkedList *curr,int flag){
    /*
    Input args: 1) pointer to linked list
                2) flag 0: to display node values
    output:     1) traverse linked list till end and return the last node
    */
    if(curr == NULL){
        return curr;
    }
    // if flag == 0 perform action
    if(flag != 0){
        action(curr);
    }
    while(curr->next != NULL){
        // move to next node
        curr = curr->next;
        // if flag == 0 perform action
        if(flag != 0){
            action(curr);
        }
    }
    if(flag != 0){
        cout << "\n";
    }
    // return the last node of linked list
    return curr;
}

void swap_nodes(LinkedList **head, int X, int Y){
    // swap two nodes of a linked list using its index position
    /*
    Input args: 1) pointer to linked list
                2) index X
                3) index Y
    output:     update linked list
    */
    if(X == Y){
        return;
    }
    // trav X
    LinkedList *prevX = NULL,*currX=*head;
    while(currX != NULL && X > 1){
        prevX = currX;
        currX = currX->next;
        X--;
    }
    
    // trav Y
    LinkedList *prevY = NULL,*currY=*head;
    while(currY != NULL && Y > 1){
        prevY = currY;
        currY = currY->next;
        Y--;
    }
    
    // if any value is NULL no swapping
    if(currX == NULL || currY == NULL){
        return;
    }

    // if it is first node change the head 
    if(prevX == NULL){
        *head = currY;
    }
    else{
        prevX->next = currY;
    }

    // if it is first node change the head 
    if(prevY == NULL){
        *head = currX;
    }
    else{
        prevY->next = currX;
    }

    // swap next nodes 
    LinkedList *temp = currY->next;
    currY->next = currX->next;
    currX->next = temp;
}

void push(LinkedList **head, int value, char *str_value, int flag){
    /*
        Input args: 1) head address
                    2) integer value
                    3) string value
                    4) flag (0: to push at end, and 1: to push at begining)
    */
    // insert values into the linked list
    if(*head == NULL){
        // head points to new node
        *head = create_node(value, str_value);
    }
    else{
        // create a insert node
        LinkedList *node;
        if(flag == 0){
            // traverse to the end (arg2:0 i.e. not print)
            node = traverse(*head, 0);
            // insert node at the end
            node->next = create_node(value, str_value);
        }
        else{
            node = (*head);
            *head = create_node(value, str_value);
            (*head)->next = node;
        }
    }
}

void reverse_list(LinkedList **head){
    // create structute pointers
    /*
    Input args: 1) pointer to linked list
    output:     update reverse linked list head
    */
    LinkedList *prev_node=NULL,*next_node=NULL,*curr_node=*head;
    // travese
    while(curr_node != NULL){
        // save next node
        next_node = curr_node->next;
        // current points to previous node
        curr_node->next = prev_node;
        // previous = current (move next)
        prev_node = curr_node;
        // current moved next
        curr_node = next_node;
    }
    // head points to previous node
    *head = prev_node;
}

LinkedList* merge_sorted_list(LinkedList *curr_1, LinkedList *curr_2){
    // Also preserve the struct of old linked list
    /*
    Input args: 1) pointer to linked list 1
                2) pointer to linked list 2
    output:     1) return the merged linked list head
    */
    // Linked List pointer
    LinkedList *curr;
    // Linked List Node
    LinkedList first;
    // curr points to first Node
    curr = &first;
    // firsts next is NULL; In case both lists are empty not to get segmentation fault
    curr->next = NULL;
    // or 
    // first.next = NULL;

    while(curr_1 != NULL && curr_2 != NULL){
        if(curr_1->data < curr_2->data){
            curr->next = create_node(curr_1->data, curr_1->name);
            curr = curr->next;
            curr_1 = curr_1->next;
        }
        else{
            curr->next = create_node(curr_2->data, curr_2->name);
            curr = curr->next;
            curr_2 = curr_2->next;
        }
    }
    while(curr_1 != NULL){
        curr->next = create_node(curr_1->data, curr_1->name);
        curr = curr->next;
        curr_1 = curr_1->next;
    }
    while(curr_2 != NULL){
        curr->next = create_node(curr_2->data, curr_2->name);
        curr = curr->next;
        curr_2 = curr_2->next;
    }
    // return the first nodes next which keeps the pointer the head of merged list
    return first.next;
}
// or
// LinkedList* merge_sorted_list(LinkedList *head_1, LinkedList *head_2){
//     Also preserve the struct of old linked list
//     LinkedList *curr_1, *curr_2, *merge_head=NULL, *curr;
//     curr_1 = head_1;
//     curr_2 = head_2;
// 
//     while(curr_1 != NULL && curr_2 != NULL){
//         if(curr_1->data < curr_2->data){
//             if(merge_head == NULL){
//                 curr = create_node(curr_1->data, curr_1->name);
//                 merge_head = curr;
//             }
//             else{
//                 curr->next = create_node(curr_1->data, curr_1->name);
//                 curr = curr->next;
//             }
//             curr_1 = curr_1->next;
//         }
//         else{
//             if(merge_head == NULL){
//                 curr = create_node(curr_2->data, curr_2->name);
//                 merge_head = curr;
//             }
//             else{
//                 curr->next = create_node(curr_2->data, curr_2->name);
//                 curr = curr->next;
//             }
//             curr_2 = curr_2->next;
//         }
//     }
//     while(curr_1 != NULL){
//         if(merge_head == NULL){
//             return head_2;
//         }
//         curr->next = create_node(curr_1->data, curr_1->name);
//         curr = curr->next;
//         curr_1 = curr_1->next;
//     }
//     while(curr_2 != NULL){
//         if(merge_head == NULL){
//             return head_2;
//         }
//         curr->next = create_node(curr_2->data, curr_2->name);
//         curr = curr->next;
//         curr_2 = curr_2->next;
//     }
//     return merge_head;
// }

LinkedList* merge_sorted_list_update(LinkedList *Llist_1, LinkedList *Llist_2){
    // This will destroy the structure of old linked lists
    /*
    Input args: 1) pointer to linked list 1
                2) pointer to linked list 2
    output:     1) return the merged linked list head
    */
    LinkedList *tail, first;
    tail = &first;
    first.next = NULL;

    while(1){
        if(Llist_1 == NULL){
            tail->next = Llist_2;
            break;
        }
        else if(Llist_2 == NULL){
            tail->next = Llist_1;
            break;
        }
        if(Llist_1->data < Llist_2->data){
            LinkedList *temp = Llist_1;
            Llist_1 = Llist_1->next;
            tail->next = temp;
        }
        else{
            LinkedList *temp = Llist_2;
            Llist_2 = Llist_2->next;
            tail->next = temp;
        }
        tail = tail->next;
    }
    return first.next;
}

void print_CirLList(LinkedList * head){
    // print a circular linked list till 18 node values
    for(int i=0;i<18;i++){
        action(head);
        head = head->next;    
    }
    cout << "\n";
}

void loop_remove(LinkedList **head){
    /*
    Input args: 1) linked list head pointer;
    Output:     2) updat head reference pointer;
    */
    LinkedList *slow = *head;
    LinkedList *fast = slow->next;
    // find loop: slow fast trace
    while(slow != fast && fast && fast->next != NULL){
        // move one step
        slow = slow->next;
        // move two step
        fast = fast->next->next;
    }

    if(slow == fast){
        // remove loop
        LinkedList *ptr1=*head, *ptr2=fast;
        LinkedList *collision=ptr2;
        
        while(1){
            // set ptr2 to collision point
            ptr2 = collision;
            // completes one cycle and look for ptr2-next is ptr1 or not.
            while((ptr2->next != collision) && (ptr2->next != ptr1)){
                ptr2 = ptr2->next;
            }
            // ptr2 tail node is found
            if(ptr2->next == ptr1){
                break;
            }
            // brink back prt2 to collision
            // ptr2 = ptr2->next; equivalent to: ptr2 = collision;
            // move ptr1 by one
            ptr1 = ptr1->next;
        }
        // put tail's next NULL
        ptr2->next = NULL;
    }
    // return *head;
}

void rotate(LinkedList **head, int k){
    /*
    Rotate a Linked List in counter-clockwise direction
    Input args: 1) reference pointer to head
                2) k value to shift the linked list
    Output:     update the referance pointer
    */
    LinkedList *curr = *head;
    // move current pointer to kth node
    while(k>1 && curr->next != NULL){
        curr = curr->next;
        k--;
    }
    // if k must be 1 if linked_list) not empty
    if(k == 1){
        LinkedList *temp = curr;
        // traverse till the end
        while(curr->next != NULL){
            curr = curr->next;
        }
        //shift the nodes
        curr->next = *head;
        *head = temp->next;
        temp->next = NULL;
    }
    else{
        std::cerr << "\'k\' 0 < k <= len(linked_list) condition not true\n";
    }
}

void circular_split(LinkedList *head, LinkedList **head_1, LinkedList **head_2){
    /*
    Split a Circular Linked List into two halves
    Input args: 1) pointer to linked list
                2) reference pointer to circular linked list 1
                3) reference pointer to circular linked list 2
    Output:     1) update the reference pointers head_1 and head_2 
    */
    LinkedList *currX=head, *currY=head;
    while(1){
        if(currY->next == head || currY->next->next == head){
            break;
        }
        currX = currX->next;
        currY = currY->next->next;    
    }
    *head_1 = head;
    *head_2 = currX->next;
    currX->next = NULL;
    currX = *head_2;
    while(currX->next != *head_1){
        currX = currX->next;
    }
    currX->next = NULL;
}

int main(int argc, char *argv[]){
    
    int n=10;
    char str[10][50] = {"A","quick","brown","fox","jumps","over","the","lazy","dog","."};
    int arr[10] = {2,4,6,8,10,1,2,5,7,9};
    
    // insert values into linked list 1
    LinkedList *head_1=NULL;
    for(int i=0;i<5;i++){
        // 1: to push at the begining 0: to push at the end
        push(&head_1, arr[i], str[i], 0);
    }
    // insert values into linked list 2
    LinkedList *head_2=NULL;
    for(int i=5;i<n;i++){
        push(&head_2, arr[i], str[i], 0);
    }

    // print linked list (arg2: 1)
    cout << "Linked List_1:\n";
    traverse(head_1,1);
    cout << "Linked List_2:\n";
    traverse(head_2,1);
    
    // merge List
    cout << "Merged List\n";
    LinkedList *merge_head = merge_sorted_list(head_1,head_2);
    traverse(merge_head,1);

    cout << "Merged List and update\n";
    // merge and update List
    merge_head = merge_sorted_list_update(head_1,head_2);
    traverse(merge_head,1);

    // swap_nodes by index
    cout << "Swap Nodes by Index:\n";
    int ind_X = 4;
    int ind_Y = 6;
    cout<< "index_X: " << ind_X << "\tindex_Y: " << ind_Y << endl;
    swap_nodes(&merge_head,ind_X,ind_Y);
    traverse(merge_head,1);
    
    // reverse linked list
    cout << "Reverse Linked List:\n";
    reverse_list(&merge_head);
    traverse(merge_head,1);
    
    // create a circular linked list
    cout << "Circular Linked List Circular\n";
    LinkedList *head=merge_head;
    LinkedList *last_node = traverse(head, 0);
    last_node->next = head->next->next->next->next;
    
    // display circular linked list;
    // traverse(head,1);
    print_CirLList(head);

    // find and remove cycle (Floyd's Algorithm)
    cout << "Find and remove cycle (Floyd's Algorithm)\n";
    loop_remove(&head);
    traverse(head,1);

    // rotate a linked list by 'k' in counter-clockwise
    cout << "Roate linked list by 'k' \n";
    int k = 4;
    rotate(&head, k);
    cout << " k=" << k << endl;
    traverse(head,1);
    
    // split a circular linked list into two halves
    traverse(head,0)->next = head;
    // traverse(head,1);
    cout << "Split a Circular linked list into two halves\n";
    head_1 = NULL, head_2 = NULL;

    // split a circular linked list into two halves
    circular_split(head, &head_1, &head_2);
    traverse(head_1,1);
    traverse(head_2,1);


    return 0;
}
