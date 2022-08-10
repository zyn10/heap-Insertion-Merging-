//
//  main.cpp
//  HeapProject
//
//  Created by Zain 👻 on 31/10/2021.
//
//===========================================================//
//===========   Heap Creation Using Left-ist Tree ===========//
//===========================================================//
#include <iostream>
#include<iomanip>
#include<string>
#include<ctime>
using namespace std;
//===========================================================//
//===========   STACK CLASS USING LINKED LIST ===============//
//===========================================================//
struct node
{
    int data;//-------->to store Data
    node* next;//------>next poitner to move next
    node* prev;//------>previous pointer to track data backwards
};
class Stack
{
    int count;// ------>just to count the number of elements
    node* top;// ------>to track the path
public:
    Stack()//constructor
    {
        count = 0;
        top = NULL;
    }
    void push(int);//-->add elements into stack
    int pop();//------->deletes elements from stack
    void Display();//-->Display the elements in stack
    bool isEmpty();
};
//===========================================================//
//===========   ADD ELEMENT IN STACK    =====================//
//===========================================================//
void Stack::push(int input)//the element added in stack
{
    node* temp = new node;
    temp->data = input;//----->Initialising Data
    temp->prev = NULL;//------>previous pointer to track data backwards
    temp->next = NULL;//------>next poitner to move next
    if (!top)//if top is empty
    {
        top = temp;
    }
    else//if not empty then allocate pointers
    {
        temp->prev = top;
        top = temp;
    }
    count++;//add in counter
}
//===========================================================//
//==============   DISPLAY THE STACK    =====================//
//===========================================================//
void Stack::Display()
{
    cout<<"\nAfter Merging we have tacked elements"<<endl<<endl;
    if (isEmpty())//if stack is empty
    {
        cout << "Stack is Empty" << endl;
        return;
    }
    
    Stack temp;
    int num;
    //For display first we need to delete the element in the stack
    //so that we can see the result
    
    for (int i = count; i > 0; i--)
    {
        num = this->pop();
        cout<< num << " , "<<endl;
        temp.push(num);
    }
    while (temp.top != NULL)
    {
        this->push(temp.pop());
    }
}
//===========================================================//
//======================   EMPTY STACK  =====================//
//===========================================================//
bool Stack::isEmpty()
{
    if (!top)//if null then return true
        return true;
    else
    return false;
}
//===========================================================//
//===========   DELETE THE ELEMENTS IN STACK   ==============//
//===========================================================//
int Stack::pop()
{
    if (!top)
    {
        cout << "Null Stack" << endl;
        return '~';
    }
    int p = top->data;
    top = top->prev;
    count--;
    return p;
}
//===========================================================//
//===================   Heap  Tree ==========================//
//===========================================================//
class HeapTree
{
public:
    int element;
    HeapTree *ptrLeft;
    HeapTree *ptrRight;
    int distance;
    HeapTree(int & input, HeapTree *leftIst = NULL,HeapTree *rightIst = NULL, int gap = 0)
    {
        this->element = input;
        ptrRight = rightIst;
        ptrLeft = leftIst;
        distance = gap;
    }
};
//===========================================================//
//===================   LeftIst Heap  Tree ==================//
//===========================================================//
class HeapLeft_Ist
{
private:
    HeapTree *root;
    HeapTree *CombiningRoots(HeapTree *Array1,HeapTree *Array2);
    HeapTree *dummyMerging(HeapTree *Array1,HeapTree *Array2);
    void sliceChild(HeapTree * input)
    //--->here we will slice the remaining childerens in order to proceed
    {
        HeapTree *tmp = input->ptrLeft;
        input->ptrLeft = input->ptrRight;
        input->ptrRight = tmp;
    }
    //===========================================================//
    //================MEMORY RESTORATIION HERE  =================//
    //===========================================================//
    void restoration(HeapTree * tree)//-->restoring Memory
    {
        if (tree != NULL)
        {
            restoration(tree->ptrLeft);
            restoration(tree->ptrRight);
            delete tree;
        }
    }
    HeapTree *duplicating(HeapTree *input);//----> duplicating for backup
public:
    HeapLeft_Ist()//------->Default Constructor
    {
        root = NULL;
    }
   HeapLeft_Ist(HeapLeft_Ist &tree)//------->Copy Constructor
    {
        root = NULL;
        *this = tree;
    }
    ~HeapLeft_Ist()//-------->Destructor
    {
        clear();
    }
    bool checkEmpty()//------>if empty return NULL
    {
        return root == NULL;
    }
    bool isFull()//---------->checking if the memory is full
    {
        return false;
    }
    int &findMin()//--------->finding the minimum
    {
        return root->element;
    }
    
    void Insert(int &element)//------->insertion
    {
        root = CombiningRoots(new HeapTree(element), root);
    }
    void deleteMin()//---------->throw out the old root
    {
        HeapTree *previousRoot = root;
        root = CombiningRoots(root->ptrLeft, root->ptrRight);
        delete previousRoot;
    }
    void deleteMin(int &lowElement)//--------->argument present
    {
        if (checkEmpty())
        {
            cout<<"Heap is Empty"<<endl;
            return;
        }
        lowElement = findMin();
        deleteMin();
    }
    void sort(int *arr,int size)//------------>Sorting Function
    {

        for (int i = 0; i < size; i++) {

            for (int j = 1; j < size; j++) {

                if (arr[j] < arr[j - 1]) {
                    swap(arr[j], arr[j - 1]);
                }

            }
        }

    }
    void clear()//---------------->to make heap empty
    {
        restoration(root);
        root = NULL;
    }
    void HeapMerging(HeapLeft_Ist &input)//-----------> Merge the heap when stack pops up
    {
        if (this == &input)
            return;
        root = CombiningRoots(root, input.root);
        input.root = NULL;
    }

    HeapLeft_Ist & operator =(HeapLeft_Ist &rhs);
};
//===========================================================//
//=================== FUNCTION DEFINITION ===================//
//===========================================================//
HeapTree *HeapLeft_Ist::CombiningRoots(HeapTree * Tree1,HeapTree * Tree2)
{
    if (Tree1 == NULL)  return Tree2;
    if (Tree2 == NULL)  return Tree1;
    if (Tree1->element < Tree2->element)
        return dummyMerging(Tree1, Tree2);
    else
        return dummyMerging(Tree2, Tree1);
}
HeapTree *HeapLeft_Ist::dummyMerging(HeapTree * Tree1,HeapTree * Tree2)
{
    if (Tree1->ptrLeft == NULL)
        Tree1->ptrLeft = Tree2;
    else
    {
        Tree1->ptrRight = CombiningRoots(Tree1->ptrRight, Tree2);
        int dummy1=Tree1->ptrLeft->distance;
        int dummy2=Tree1->ptrRight->distance;
        if ( dummy1<dummy2 )
            sliceChild(Tree1);
        Tree1->distance = Tree1->ptrRight->distance + 1;
    }
    return Tree1;
}
HeapLeft_Ist &HeapLeft_Ist::operator =(HeapLeft_Ist & Tree)
{//operator overloading and deep copy
    if (this != &Tree)
    {
        clear();
        root = duplicating(Tree.root);
    }
    return *this;
}
// Internal method to clone subtree.
HeapTree *HeapLeft_Ist::duplicating(HeapTree * tree)
{
    if (tree == NULL)//if empty return
        return NULL;
    else
        return new HeapTree(tree->element, duplicating(tree->ptrLeft),duplicating(tree->ptrRight), tree->distance);
}
int main(int argc, const char * argv[])
{
    int element;
    int counterr=0;
    Stack obj;
    HeapLeft_Ist obj1,obj2,obj3;

    cout << "How many elements you want to insert in 1st heap "<<endl;
    cin >>counterr;
    int *heap1 = new int(counterr);
    cout << "Enter " << counterr << " items" << endl;
    for (int x = 0; x < counterr; x++)
    {
        cout<<"Input item "<<x+1<<" : ";
        cin >> heap1[x];
    }
    //Tree 2 insertion level wise
    for(int i=0;i<counterr;i++)
    {
        obj1.Insert(heap1[i]);
    }
    int counter2;
    cout << "How many elements you want to insert in 2nd heap "<<endl;
    cin >>counter2;
    int *heap2 = new int(counter2);
    cout << "Enter " << counter2 << " items" << endl;
    for (int x = 0; x < counter2; x++)
    {
        cout<<"Input item "<<x+1<<" : ";
        cin >> heap2[x];
    }
    for(int i=0;i<counter2;i++)
    {
        obj2.Insert(heap2[i]);
    }
   // int tree1[]= {4,19,8,27,20,12,43,15,25};//level wise  for tree 1
   // int tree2[]= {6,8,7,14};//level wise for tree 2
     obj1.deleteMin(element);
     obj.push(element);//pushing in stack to track the copy
     obj2.deleteMin(element);
     obj.push(element);//pushing in stack to track the copy
     obj1.HeapMerging(obj2);
     obj3 = obj1;//replacing
     obj3.deleteMin(element);
     obj.push(element);//pushing in stack to track the copy
     obj3.deleteMin(element);
     obj.push(element);//pushing in stack to track the copy
    
    cout<<"\n\nFirst Tree  Elements"<<endl;
    for(int i=0;i<counterr;i++)
    {
        cout<<heap1[i]<<" ";
    }
    cout<<endl;
    cout<<"\nSecond Tree  Elements"<<endl;
    for(int i=0;i<counter2;i++)
    {
      cout<<heap2[i]<<" ";
    }
    cout<<endl<<endl;
    obj.Display();
    
    cout<<endl<<endl<<endl;
    return 0;
}
