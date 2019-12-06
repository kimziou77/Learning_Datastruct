# Tree Project

## Heap
> 힙은 다음의 속성을 가진 완전 이진트리이다.  
> 노드의 데이터 n이 있다면, 데이터n은 자식들의 데이터보다 항상 크다.

#### *힙(Heap)과 우선순위 큐(Priority queue)*
힙은 우선순위 큐로 구현할 수 있다.  

#### issue
1. 힙에 새로운 요소 추가하기
2. 힙에서부터 요소 제거하기

#### 힙에 새로운 요소 추가하기
1. Put the new node in the next available spot
2. Push the new node upward, swapping with its parent until the new node reaches an acceptable location  
   -> this process of pushing the new node upward is called ***reheapification upward***

#### 힙에서부터 요소 제거하기
1. Move the last node onto the root
2. Push the out-of-place node downward, swapping with its larger child until the new node reaches an acceptable location  
   -> The process of pushing the new node downward is called ***reheapification downaward.***

#### 힙 구현하기
같은 depth의 노드들은 인접한 index에 추가한다.
포인터는 사실상 어떠한 형태로도 저장되지 않지만,
우리는 단지 "이 배열은 트리다" 라고 알고있기만 하면 우리는 데이터를 관리할 수 있다.

### 힙 요약
- 힙은 complete binary tree 이다.  
- 힙에 요소를 추가한다면 새로운 entry를 다음 가능한 위치에 배치하고,  
  reheapification upward를 수행한다.
- 가장 큰 요소를 삭제한다면 마지막 노드를 루트로 옮겨서,  
  reheapification downward를 수행한다.

# B-Trees
`unbalanced Tree`의 문제점은 바로! `unbalanced`하다는 점이다.
- `B-Tree`는 각 노드에 여러개의 요소를 넣을 수 있다.
- 중복을 허용하지 않는다.

B-Tree에는 6가지 규칙이 있는데,  

#### *Rule 1:*
The root may have as few as one entry(even no entries if no children)  
루트를 제외하고는 모두 적어도 *MINIMUM개수의 요소(entry)를 가지고 있어야 한다.  
*MINIMUM 상수: 각 노드에 저장될 수 있는 최소노드의 개수

#### *Rule 2:* 
MAXIMUM은 MINIMUM의 두배이다.

#### *Rule 3:*  
B-Tree의 각 요소는 배열에 정렬되어 보관되어있다.

#### *Rule 4:*  
`leaf node`가 아닌 모든 노드는 항상 요소+1개의 자식을 가지고 있다.

#### *Rule 5:*
`leaf node`가 아닌 모든 노드에 대해서:  
(a) An entry at index i is greater than all the entries in subtree number i of the node
(b) An entry at index i is samller than all the entries in subtree number i+1 of the node

\+ traversial을 잘 구현하면 정렬된 상태로 출력 가능하다.

#### *Rule 6:*
B-Tree의 모든 leaf는 같은 깊이를 가지고 있다.  
Q. 어떻게 이게 가능하지..??

- - -
<br>

## B-Tree로 Set 만들기
### ADT
private member  
```cpp
static const size_t MINIMUM = 200;  
static const size_t MAXIMUM = 2*MINIMUM;  
size_t data_count;  
size_t child_count;
item data[MAXIMUM+1];  //최대 MAXIMUM개, 중간과정에서 하나가 더 필요해서 +1 해준다
set * subset[MAXIMUM+2];
```

### Functions for `set` using B-Trees
public : 
- constructor / destructor (x)
- count
- insert
- erase
- clear (x)
- empty (x)
- operator = (x)
  
(x)는 수업때 다루지 않는 함수들임.

#### count function(searching for an item in a B-tree)  
//Q. 근데 이게, 수를 세는 함수 아닌가 왜 ..  
찾으면 1, 못찾으면 0을 반환한다.  

의사코드 
1. 루트에서, `data[i]`>=`target` 를 만족하는 가장 처음 나오는 index를 찾는다.
2. if(data[i]에서 target을 찾으면) return 1;
3. else if(루트가 자식이 없으면) return 0;
4. else return subset[i]->count(target);

### insert function
private :  
`loose_insert`  
add a new entry to the B-Tree with the possibility that the root may have MAXIMUM+1 entries

`fix_excess`  
take care of the extra entry (if any) in the root of a subtree

#### loose_insert function
root may have one extra entry  
의사코드  
1. 루트에서, `data[i]`>=`entry` 를 만족하는 가장 처음 나오는 index를 찾는다.   
(만약 그런 i가 없으면 i = data_count;)  
// *data_count는 관심영역 밖의 index이므로, 못찾았다를 의미한다고 가정
2. if(`data[i]`에서 `entry`을 찾으면) return false;
3. else if(루트가 자식이 없으면)  
    insert the entry to the root at data[i] and return true;
4. else  
    do recursive call and fix the excess problem

4.1 bool b = `subset[i]` ->`loose_insert(entry)`;  
4.2 check if the root of subset[i] now has an excess entry;  
4.3 if so, fix the subset[i] using the fix_excess function;  
4.4 return b;

#### fix_excess function
semi-B-tree인 서브트리를 만든다. Whose root node has an extra entry into a regular B-tree

의사코드  
1. split the node with MAXIMUM+1 entries into two nodes each of which contains MINIMUM entries.
2. The entry in the middle is moved upward to the parent

inserst function  
의사코드  
1. if(! loose_insert(entry)) return false;
   //since entry wasn't added
2. if(data_count>MAXIMUM)
   fix the root of the entire tree
3. return true;

To fix the root of entire tree :
1. Create a new root with no entries and let the old root be the child of the new root
2. Call fix_excess on the old root

### Erase function
private :  
`loose_erase`  
loose_erase removes an entry from the B-tree with the possibility that the root may have 0 entries (but with one child) or the root of an internal

`fix_shortage`  

