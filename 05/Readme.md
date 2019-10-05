# Linked Lists
ㅠㅠ 이번장은 너무 길어서 다시한번 읽어봐야할 필요가 있따 ㅜㅠㅜ
## Linked List  
> 물리적으로는 떨어져 있지만 논리적으로 연속, 따라서 메모리 관리가 효율적임.

`Linked List`의 핵심적인 `Node Class`를 살펴보자.
```cpp
class node{
    value_type data_field;
    node *link_field;

    public:
        typedef double value_type;

        node(const value_type & init_data = value_type() , node* init_link=NULL);
        void set_data(const value_type & new_data);
        void set_link(node * new_link);
        value_type data() const;

        /* const용 link와 일반 link */
        const node * link() const;
        node * link();
}
node * head_ptr; // 첫번째 노드를 계속해서 tracking 할 수 있는 변수.
node * tail_ptr; // List의 가장 마지막 노드를 가리키는 포인터 
```
Empty List 에는 헤드포인터에 NULL이 저장되어있다.  
constant NULL은 `cstdlib`에 들어가있다???? 

### Node Constructor
```cpp
// node constructor
node (const value_type & init_data = value_type(), node* init_link = NULL ){
    data_field = init_data;
    link_field = init_link;
}

// 일반 버전 link
node * link() { return link_field; }

// const 버전 link
const node * link() const { return link_field; }
// const node* 를 반환하는 const link 함수!!
```


## `A Linked List Toolkit!!!!`

- Get `Length` of linked list
- `Insert` a node into a linked list
- `Remove` a node from a linked list
- `Search` for data item in a linked list
- `Locate` a node at a certain position in a linked list
- `Copy` one linked list to another
- `Clear` a linked list (empty list)

 ### `Length` function
```cpp
size_t list_length(const node * head_ptr){
    size_t answer = 0;
    const node *cursor;
    for(cursor = head_ptr; cursor != NULL; cursor=cursor->link())
        ++answer;
    return answer;
}
```
 
 ### `Insert` function
 ```cpp
//head insert
void list_head_insert (node* & head_ptr, const node::value_type & entry){
    head_ptr =new node(entry,head_ptr);
}
Q const node::value_type & entry 여기서 왜 node 가 생성되는지 모르겠어
A 생성자의 원형을 보면 다음과 같은데, 여기서 첫번째 인자에 들어가는 데이터 타입이라 그런듯
node (const value_type & init_data = value_type(), node* init_link = NULL )

//list_insert
void list_insert (node * previous_ptr, const node::value_type & entry)
// head_ptr -> previous_ptr 로 바뀌었다.
void list_insert_ver1 (node * previous_ptr, const node::value_type & entry){
    node * insert_ptr;
    insert_ptr=new node;
    insert_ptr->set_data(entry);
    insert_ptr->set_link(previous_ptr->link());
    previous_ptr->set_link(insert_ptr);
    // 너무 긺! ver_2로 가보자
}
void list_insert_ver2 (node * previous_ptr, const node::value_type & entry){
    previous_ptr->set_link (new node (entry,previous_ptr->link()));
}
// 노드를 하나 생성하여서 기존 previous의 링크를 새롭게 생성한 노드에 넘겨주고
// previous_ptr의 링크에 새로 생성한 노드의 주소를 넘겨주자.

 ```

### `Search` function
```cpp
node * list_search (node * head_ptr, const node:: value_type & target);
const node * list_search (const node * head_ptr, const node::value_type & target);

node * list_search (node * head_ptr, const node:: value_type & target){
    node *cursor;
    for(cursor=head_ptr; cursor != NULL; cursor = cursor->link()){
        if (target == cursor->data())
            return cursor;
    }
    return NULL;
}
```
### `Locate` function
n번째 노드 포인터를 달라고 하는 함수
```cpp
node * list_locate (node *head_ptr, size_t position);
const node * list_locate(const node * head_ptr, size_t position);
//precondition : position>0
node * list_locate (node *head_ptr, size_t position){
    node * cursor;
    assert (position > s0);
    size_t i;
    for(cursor=head_ptr, i=1; (cursor != NULL && i<position); i++)
        cursor=cursor->link();
}
```
### `Copy` function
주어진 연결리스트의 복사본을 만드는 함수
```cpp
void list_copy(const node* source_ptr, node* & head_ptr, node* & tail_ptr){
    head_ptr = NULL; tail_ptr = NULL;
    if(source_ptr == NULL) return;
    list_head_insert(head_ptr, source_ptr->data());
    tail_ptr = head_ptr;
    // 여기까지 하나 insert

    source_ptr = source_ptr->link(); //다음칸으로
    
    while(source_ptr != NULL){
        list_insert(tail_ptr, source_ptr->data()); // 한개 넣고
        tail_ptr = tail_ptr->link();
        source_ptr =source_ptr->link();
        // source와 tail이 한칸씩 뒤로 간다.
    }
}
```
### `Remove` function
```cpp
void list_head_remove (node* & head_ptr){
    node * remove_ptr;
    remove_ptr = head_ptr;
    head_ptr = remove_ptr->link();
    delete remove_ptr;
}
void list_remove (node * previous_ptr){
    node * remove_ptr;
    remove_ptr = previous_ptr->link();
    previous_ptr -> set_link(remove_ptr->link());
    delete remove_ptr;
}
```
### `Clear` function
모든 노드를 지워서 list를 비운다!
```cpp
void list_clear(node *& head_ptr){
    while(head_ptr != NULL)
        list_head_remove(head_ptr);
    //반복적으로 머리를 없앤다.
}
```

**Linked list Bag class의 장점**  
가방의 용량을 신경쓰지 않아도 된다(메모리측면)  
**Linked list Bag class의 단점**  
random access 불가능하다(효율성측면)


## Bag class with a LInked List
class implementation
- Constructor / Destructor
- " = " , " += " Operator
- `erase_one` function
- `count` function
- `grap` function
- `insert` function
- " + " Operator

### 1. Constructor / Destructor
```cpp
// Default Constructor
bag::bag(){
    head_ptr = NULL;
    many_nodes = 0;
}

// Copy constructor
bag::bag(const bag& source){
    node *tail_ptr;
    list_copy(source.head_ptr, head_ptr, tail_ptr);
    many_nodes = source.many_nodes;
}

// Destructor
bag::~bag(){
    list_clear(head_ptr);
    //many_nodes=0; not necessary
}
```
### 2. Operator " = / +="
```cpp
bag& bag::operator = (const bag &source){
    node * tail_ptr;
    if (this == &source) reutnr *this;
    list_clear(head_ptr);
    list_copy(source.head_ptr, head_ptr, tail_ptr);
    many_nodes = source.many_nodes;
    return *this;
}

void bag::operator +=(const bag& addend){
    node * copy_tail_ptr, copy_head_ptr;
    if (addend.many_nodes>0){
        list_copy (addend.head_ptr, copy_head_ptr, copy_tail_ptr);
        copy_tail_ptr->set_link (head_ptr);
        head_ptr = copy_head_ptr;
        many_nodes += addend.many_noes;
    }
}
```
### 3. Erase
```cpp
bool bag::erase_one(const value_type & target){
    node * target_ptr;
    target_ptr = list_search (head_ptr, target);
    if (target == NULL) return false;
    target_ptr -> set_data(head_ptr->data());
    list_head_remove (head_ptr);
    many_nodes--;
    return true;
}
```
### 4. Count
```cpp
bag::size_type bag::count(const value_type & target)const{
    size_type answer = 0;
    const node* cursor;
    cursor =list_search(head_ptr, target);
    while(cursor!=NULL){
        answer++;
        cursor = cursor -> link();
        cursor = list_search(cursor, target);
    }
    return answer;
}
```
### 5. Grap
```cpp
bag::value_type bag::grap(){
    size_type i;
    const node * cursor;
    assert(size()>0);
     i = (rand() % size())+1;
     cursor = list_locate(head_ptr, i);
     return cursor->data();
}
```
### 6. Insert
```cpp
void bag::insert(const value_type & entry){
    list_head_insert(head_ptr, entry);
    many_nodes++;
}
```

## `Dynamic Arrays` vs `Linked List` vs `Doubly LinkedList`
- 배열은 random access에 제일 좋다.
- 연결리스트는 삽입,삭제에 좋다
- Resizing can be inefficient for a dynamic array





<br><br><br>

- - -
이건 static 가방의 bag인데 참고 ㅎㅎㅎ
<br>
<br>

```cpp
class bag{

        int data[CAPACITY];  //node * head_ptr;
        size_t used; // size_type many_nodes;

    public:
        static const size_t CAPACITY = 20;
        bag();

        void insert(const value_type& new_entry);
        /* precondition : 가방은 꽉자있지 않다. */
        /* postcondition : 가방에 new_entry가 추가된다. */

        std::size_t size() const;
        /* postcondition : 가방 안에 있는 정수 개수를 반환한다. */

        void remove(const value_type& target);
        /* postcondition : 타겟이 가방안에 있으면 삭제, 아니면 변하지 않는다. */
}
```