# Tree
> 비 선형구조   
> 트리는 노드의 유한집합 (노드는 비어잇을 수 있다.)

<br>

## DEFINITIONS
#### 부모노드 (Parent)
> 어떤 노드의 부모 : 링크 바로 위에 있는 노드  
> 트리의 부모는 하나이다.

#### 자식노드 (child_ren)
> 자식노드는 여러개 있을 수 있다.

#### 형제 (Siblings)
> 같은 부모를 가지고 있는 관계

#### 루트노드 (Root)
> 부모노드가 없는 특별한 노드

#### 리프노드 (Leaf)
> 자식노드가 없는 노드

#### 조상노드 (Ancestor)
> 부모노드는 조상노드(R)이고,  
> 조상노드의 부모노드도 조상노드(R)이다.  

*R : reculsive 재귀적인 정의

#### 자손노드 (Descendant)
> 자식(child)노드는 자손노드(R)이다.  
> 자손노드의 자식노드도 자손노드(R)이다.

#### 서브트리 (SubTree)
> 서브트리

#### 노드의 Depth
> 루트로부터의 깊이

## Introduction to Tree
### 이진트리 (Binary Tree)
- 모든 노드는 자식을 최대 두개 가질 수 있다.  
- 오른쪽자식 / 왼쪽자식 이다.  
- 왼쪽서브트리(left subtree)는, 루트가 왼쪽자식인 서브트리이다.  
- 오른쪽 서브트리도 위와 같다.

### 포화 이진트리 (Full Binary Tree)
- **모든 리프가 같은**`depth`를 가지고 있다.
- 리프노드가 **아닌** 노드들은 모두 두개의 자식을 가지고 있다.

### 완전 이진트리 (Complete Binary Tree)
- 깊이가 낮은곳부터 깊은곳으로 내려가면서  
- **왼쪽에서부터** 오른쪽으로 배치시켜주는 것

## Tree Representations
### **배열로** `complete Binary Tree` 나타내기
- depth순서대로 왼쪽부터 차례대로 넣는다.  
  - 한 노드가 i번째에 저장되면,
  - 왼쪽자식이 2i+1에 저장된다.
  - 오른쪽자식은 2i+2에 저장된다.

### 연결리스트로 `Binary Tree` 나타내기
노드 하나에 링크두개, 부모노드 접근시 링크+=1  

##### 세 개의 멤버 변수들이 있다.
- `data_field`
- `left_field`
- `right_field`

```cpp
template<class item>
class binary_tree_node{
    private:
      item data_field;
      binary_tree_node * left_field;
      vinary_tree_node * right_field;
      ...
}
```

## Member Function
- constructor
- data() / const data()
- left() / const left()
- right() / const right()
- set_data(data)
- set_left(node)
- set_right(node)
- is_leaf()

### *Constructor*
```cpp
binary_tree_node(const item& init_data = item(),
    binary_tree node * init_left = NULL,
    binary_tree_node * init_right = NULL)
{
    data_field = init_data;
    left_field = init_left;
    right_field = init_right;
}
```

### *data* / *const data*
```cpp
item & data() {return data_field;}
const item & data() const {return data_field;}
```


### *left* / *const left*
```cpp
binary_tree_node * & left() {return left_field;}
const binary_tree_node * left() const { return left_field;}
// 여기서 &가 붙고 n->left() = q; 라고 했을 때 legal state ment 이다.  n->set_left(q); 와 같다.
//Lvalue를 쓸 수 있다.
```


### *right* / *const right*
```cpp
binary_tree_node * & right() { return right_field;}
const binary_tree_node * right() const { return right_field;}
```

### *setter*
```cpp
void set_data(const item & new_data){data_field = new_data;}
void set_left(binary_tree_node * new_left) {left_field = new_left;}
void set_right(vianry_tree_node * new_right){right_field = new_right}
```

### *is_leaf*
```cpp
bool is_leaf() const{
    return (left_field == NULL)&&(right_field == NULL);
}
```
- - -
## Tree Functions
- tree_clear
- tree_copy
- tree traversial

### *tree_clear*
```cpp
template<class item>
void tree_clear(binary_tree_node<item>* & root_ptr){
    if(root_ptr != NULL){
        tree_clear(root_ptr->left());
        tree_clear(root_ptr->right());
        delete root_ptr;
        root_ptr = NULL;
    }
}
```

### *tree_copy*
```cpp
binary_tree_node<item>* tree_copy(binary_tree_node<item>* root_ptr){
    binary_tree_node<item> *l_ptr;
    binary_tree_node<item> *r_ptr;
    if(root_ptr == NULL) return NULL;
    else{
        l_ptr = tree_copy(root_ptr->left());
        r_ptr = tree_copy(root_ptr->right());
        return new binary_tree_node<item>(root_ptr->data(), l_ptr, r_ptr);
    }
}
```

## Tree Traversial : *utility  function*
> 개요 : 트리는 주어진 모든 노드를 접근하는 능력을 갖고 있어야 한다.  
> ex ) printing, updating
##### pre-order / in-order / post-order  
> root를 처리하는 순서에 따라 접두어가 붙어있다.

### 1. Pre-Order Traversal
- 루트를 제일 먼저 본다.
- 왼쪽 서브트리(R)를 본다.
- 오른쪽 서브트리(R)를 본다.
```cpp
template<class item>
void preorder_print(const binary_tree_node<item>* root_ptr){
    if(root_ptr != NULL){
        cout<< root_ptr->data() <<endl;
        preorder_print(root_ptr->left());
        preorder_print(root_ptr->right());
    }
}
```

### *셤 : tree를 주고 traversal 별로 출력해보기..*

### 2. In-Order Traversal
- 왼쪽 서브트리(R)를 본다.
- 루트를 본다.
- 오른쪽 서브트리(R)를 본다.
```cpp
template<class item>
void inorder_print(const binary_tree_node<item>* root_ptr){
    if(root_ptr != NULL){
        inorder_print(root_ptr->left());
        cout<< root_ptr->data() <<endl;
        inorder_print(root_ptr->right());
    }
}
// 괄호를 잘 넣어주면 infix와 유사한 형태로 만들 수 있다.
```

### 3. Post-Order Traversal
- 왼쪽 서브트리(R)를 본다.
- 오른쪽 서브트리(R)를 본다.
- 루트를 본다.
```cpp
template<class item>
void postorder_print(const binary_tree_node<item>* root_ptr){
    if(root_ptr != NULL){
        postorder_print(root_ptr->left());
        postorder_print(root_ptr->right());
        cout<< root_ptr->data() <<endl;
    }
}
// 괄호를 잘 넣어주면 infix와 유사한 형태로 만들 수 있다.
```

### + Backward In-order Traversal
1. **오른쪽**서브트리를 먼저본다.
2. 루트를 본다.
3. 왼쪽 서브트리를 본다.  

90도 틀어진 트리 화면출력에서 유용하다.

```cpp
template<class item, class SizeType>
void print(const binary_tree_node<item>* root_ptr, SizeType depth){
    if(root_ptr!=NULL){
        print(root_ptr->right(), depth+1);
        cout<<setw(4*depth)<<""<<root_ptr->endl;
        //4*depth만큼 공백을 차지, #include <iomanip>
        print(root_ptr->left(),depth+1);
    }
}
```
- - -

<br>

여기다가 함수인자로 함수를 넣어서 하고싶은 작업을 모두 할 수 있도록!  

##### > NOTE : 안되는 컴파일러도 있으니 주의!

#### example1
```cpp
void apply(void f(int &),  int data[], size_t n){
    for(size_t i=0; i<n; i++)
        f(daata[i]);//모든 데이터에 f 처리 하기
}
void add1(int& i) { i+=1; }
void triple(int& i){ i*=3; }
```

#### example2 : template_ver
```cpp
template<class item, class SizeType>
void apply(void f(item &), item data[], SizeType n){
    for(SizeType i=0; i<n; i++>){
        f(data[i]);
    }
}
void add1(int& i) { i+=1; }
void toUpperCase(char & c) { c = toupper(c); }//cctype
```
#### example3 : more general template_ver
```cpp
template<class Process, class item, class SizeType>
void apply(Process f, item data[], SizeType n){
    for(SizeType i=0; i<n; i++>){
        f(data[i]);
    }
}
void add1(int& i) { i+=1; }
void toUpperCase(double d) { cout<< d << endl; } //cctype
```

#### Tree Traversal을 위한 Template Function
```cpp
template<class Process, class BTNode>
void preorder(Process f, BTNode * node_ptr){
    if(node_ptr!=NULL){
        f(node_ptr->data());
        preorder(f,node_ptr->left());
        preorder(f,node_ptr->right());
    }
}
```

## Binary Search Tree : B . S . T
Binary Tree에서 특화시킨것 : 중간(root)를 보고 방향을 정할 수 있다. *정렬선행
1. 왼쪽 서브트리는 주어진 노드보다 작거나 같다.
2. 오른쪽 서브트리는 주어진 노드보다 크다.

가방은 BST로 구현할 수 있다  
: 내가 아이템을 넣거나 빼어도 BST의 형태를 유지해야 한다.

```cpp
template<class item>
class bag{
    private:
      binary_tree_node<item> * root_ptr;
}
```

### BST로 가방 클래스 만들기
#### Member Function
- constructor/destructor
- insert(entry)
- erase(target)
- erase_one(target)
- +=, =
- size()
- count(target)

#### *constructor*
> root_ptr을 NULL로 설정
#### *copy_constructor*
> 소스트리의 복사본을 만들고 root_ptr이 그것을 가리키게 만든다.
#### *Destructor*
> 트리의 모든 노드를 해제한다.
#### *size*
> 트리안의 노드개수를 센다.  
> 1 + size(left subtree) + size(right subtree)
#### *Assignment Operator*
> 1. self-assignment 체크, 만약 그렇다면 return
> 2. 현재 트리가 사용하고 있는 모든 메모리 해제
> 3. RHS의 트리를 자기에게 copy 한다.

#### *insert*
1. 새로운 노드를 만들고, new_ptr이 그것을 가리킨다.
2. 트리가 비어있다면 root_ptr을 new_ptr로 설정한다.
3. 그렇지 않다면, 적절한 leaf_node를 찾을때까지 `binary search`한다. 그리고 찾는다면 왼쪽/오른쪽 자식중에 적절한 자리에 new_ptr을 만든다.
   
##### *Insert(entry) : do a binary search*
```
While(not done){
    case 1 : entry<= node_ptr->data()
        if node_ptr->left() is NULL,
            node_ptr->set_left(new_ptr) and done
        else node_ptr = node_ptr->left()
    case 2 : entry > node_ptr -> data()
        if node_ptr->right() is NULL,
            node_ptr->set_right(new_ptr) and done
        else node_ptr=node_ptr->right()
}
```
#### *erase one(target)*
#### 1
```cpp
bst_remove(binary_tree_node<item>*& root_ptr, const item & target)
```
removes a node with target from the tree

```
bst_remove(root_ptr, target){
    if(target이 비어있다면) return;

    if(target < root_ptr->data())
        bst_remove(root_ptr->left(),target) 
    else if (target > root_ptr->data())
        bst_remove(root_ptr->right(),target)
    else{ //target == root_ptr->data()
        case 4a : 루트의 왼쪽자식이 없는경우
        case 4b : 루트가 왼쪽자식을 가지고 있는 경우
    }
}

case 4a : 루트를 지우고, 오른쪽 자식을 새로운 루트로 만든다.
oldroot_ptr = root_ptr;
root_ptr = root_ptr->right();
delete oldroot_ptr;

case 4b : 가장 큰 data를 왼쪽 서브트리에서 지우고, 가장 큰 데이터를 루트로 copy 한다.
bst_remove_max(root_ptr->left(), root_ptr->data());
```
#### 2
<br>

```cpp
bst_remove_max(binary_tree_node<item>*& root_ptr,item & removed)
```
removes a node with **largest data** from the tree

case 1 : 루트가 오른쪽 자식을 가지고 있지 않을때 (루트가 가장 큰 데이터를 갖고 있다)  
```
removed_item = root_ptr->data() //루트가 max
oldroot = root_ptr;
root_ptr = root_ptr->left(); //루트의 왼쪽 자식을 새로운 루트로 한다.
delete oldroot; //old root를 삭제한다.
```

case 2 : 루트가 오른쪽 자식을 가지고 있을때 (가장큰 데이터가 오른쪽 서브트리에 존재한다.)
```
bsst_remove_max(root_ptr->right(),removed_item);
```
### *+= operator*
이걸 구현하기 위해 일단 insert_all(tree_ptr) 을 정의할 예정임!  
insert_all(tree_ptr)은 tree_ptr이 가리키고 있는 source tree의 각 데이터 아이템을  
우리의 tree에 넣는 함수이다.

##### insert all
```cpp
template <class item>
void bag<item>::insert_all(const binary_tree_node<item>* tree_ptr){
    if(tree_ptr != NULL){
        insert(tree_ptr->data());
        insert_all(tree_ptr->left());
        insert_all(tree_ptr->right());
    }
}
```
##### operator+=
```cpp
template<class item>
void bag<item>::operator+=(const bag<item>& addend){
    if(this==&addend){
        binary_tree_node<item>* add_ptr = tree_copy(addend.root_ptr);
        insert_all(add_ptr);
        tree_clear(add_ptr);
    }
    else{
        insert_all(addend.root_ptr);
    }
}
```