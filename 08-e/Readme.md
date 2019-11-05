# Queue
> 순서가 있는 자료구조로,  
> front_ptr(remove되는쪽)과 rear_ptr(insert되는쪽)이 있다.  
> **First-in-First-out (FIFO)**

### STL
- pop()
- push(const Item& entry)
- empty()
- size()
- front()

#### Queue Error
1. Queue overflow
2. Queue underflow
   

### Applications
1. Recognizing palindromes
2. Car Wash Simulation

#### 1. Recognizing `Palindromes`

*Palindromes*
> 앞으로 읽으나 뒤로 읽으나 똑같은 String

ex
```
radar
Able was I ere I saw Elba
```

스택과 큐를 사용하여 `string`이 `palindrome`인지 검사한다.

```cpp
#include <queue>
#include <stack>
#include <cctype>
#include <iostream>
void main(void){
    queue<char> q;
    stack<char> s;
    char letter;
    int mismatches = 0;
    
    while(cin.peek() !='\n'){
        cin >> letter;
        if(isalpha(letter)){
            q.push(toupper(letter));
            s.push(toupper(letter));
        }
    }

    while((!q.empty())&&(!s.empty())){
        if(q.front() != s.top())
            mismatches++;
        q.pop();    s.pop();
    }

    if(mismatches == 0)
        cout << "That is a palindrome."<<endl;
    else
        cout << "That is not a palindrome"<<endl;
        
    return EXIT_SUCCESS;
}
```


#### 2. Car Wash Simulation
`Simulation` : 상황을 주고 원하는 상황을 도출하기까지의 과정

3 Objects:  
- washer
  - 한대의 차를 세차하는데에 걸리는 시간
  - 동작한다면 세차를 마무리 하는데 까지 필요한 시간

- bool_source  
  - 확률값 : 매 초당 차가 도착할지 안할지를 알려준다.
    
- averager  
  - 총 세차 개수
  - 모든 차의 세차 시간


수도코드  
```
void car_wash_simulate ( wash_time, arrival_prob, total_sim_tim )
1. Declare a queue of unsigned integers (for customer arrival times)

2. for(cur_sec=1;cur_sec<=total_sim_time;++cur_sec){
    bool_source에게 새로운 차가 도착했는지 묻는다.
    만약 도착했다면, 큐에 cur_sec을 넣는다(대기)
    if (washer가 아무것도 안하고 있고, 큐가 비어있지 않다면){
        큐에서 제거하고 이거를 next라고 부르자. (cur_sec - next)해서 averager에게 전달해준다.
        그리고 washer에게 car을 씻으라고 말해준다.
    }
    indicate to the washer that another simulated second has passed
}

3. The simulation is completed. used the averager to print how many cars were washed and the average waiting time for all customers
```
#### Washer
```cpp
class washer{
    unsigned int seconds_for_wash;
    unsigned int wash_time_left;
    public:
        washer(unsigned int s =60);
        void one_second();
        void start_washing();
        bool is_busy() const {return(wash_time_left>0);}
}
```
```cpp
washer::washer(unsigned int s){
    seconds+for_wash = s;
    wash_time_left=0;
}
void washer::one_second(){
    if(is_busy())
        wash_time_left--;
}
void washer::start_washing(){
    assert(!is_busy());
    wash_time_left = seconds_for_wash;
}
```

#### Bool_source
```cpp
class bool_source{
    double probability;
    public:
        bool_source(double p = 0.5);
        bool query() const;
}
```
```cpp
bool_source::bool_source(double p){
    assert(p>=0);
    assert(p<=1);
    probability = p;
}
bool bool_source::query() const{
    return (rand() < probability * RAND_MAX);
}
```
#### Averager
```cpp
class averager{
    size_t count;
    double sum;
    public:
    averager();
    void next_number(double value);
    size_t how_many_numbers() const{return count;}
    double average() const;
}
```

```cpp
averager::averager(){
    count=0;
    sum=0;
}
void averager::average() const{
    assert(how_many_numbers() > 0);
    return sum/count;
}
```

#### Car_wash_Simulate
```cpp
void car_wash_simultate(unsigned int wash_time, double arrival_prob, unsigned int total_time){
    queue<unsigned int> arrival_times;
    unsigned int next;
    bool_source arrival(arrival_prob);
    washer machine(wash_time);
    averager wait_times;
    unsigned int current_second;
    for(current_second=1;current_second<=total_time;++current_second){
        if(arrival.query())
            arrival_times.push(current_second);
        if(!machine.is_busy()&&!arrival_times.empty()){
            next=arrival_times.front();
            arrival_times.pop();
            wait_times.next_number(current_second-next);
            machine.start_washing();
        }
        machine.one_second();
    }
}
```

## Queue 구현하기
1. first와 last가 아이템들이 더해지고 지워질 때마다 증가한다.
문제점 : 배열에 공간이 있어도 새로운 아이템을 추가할때 last가 end라면 추가하지 못할 수 있다.
2. 아이템 하나가 지워지면, 남은 모든 아이템이 한칸씩 움직인다 first가 항상 0이 될때까지  
문제점 : 매우 비효율 적이다.
3. `Circular array` approach  
   rear가 배열의 end에 도달하면, 단순히 배열의 맨 앞을 다시 재사용하면 된다.(원형이라고 생각하면 된다.)

**Circular array approach**  
Q. How to determine the next index?  
A. next_index = (current_index +1) % CAPACITY

얘네는 3가지의 멤버 변수들을 사용한다  
fisrt, last, and count   
 
그리고 private 멤버 함수인 next_index가 인덱스를 계산해준다.  

비어있는 큐를 위해 last is some valid index and
first==next_index(last)


### *Array Implementation*
```cpp
template<class Item>
class queue{
    Item data[CAPACITY];
    size_t first;
    size_t last;
    size_t count;
    size_t next_index(xize_t i)const{
        return (i+1) % CAPACITY;
    }

    public:
        static const size_t CAPACITY=30;
        queue();
        void pop();
        void push(const Item& entry);
        bool empty() const {return (count==0);}
        Item front() const;
        size_t size() const {return count;}
}
```
```cpp
template<class Item>
queue<Item>::queue(){
    count=0;
    first=0;
    last= CAPACITY-1;
}
template<class Item>
Item queue<Item>::front() const{
    assert(!empty());
    return data[first];
}
template<class Item>
void queue<Item>::pop(){
    assert(!empty());
    first=next_index(first);
    count--;
}
template<class Item>
Item queue<Item>::push(const Item& entry){
    assert(size()<CAPACITY);
    last = next_index(last);
    data[last]=entry;
    count++;
}
```
### *LinkedList Implementation* 87p
두개의 포인터와 한개의 count 변수를 사용한다.  
front_ptr  
rear_ptr  
count  
```cpp
template<class Item>
class queue{
    private:
        node<Item> *front_ptr;
        node<Item> *rear_ptr;
        size_t count;
    public:
        queue();
        queue(const queue<item>& source);
        ~queue();
        void pop();
        void push(const Item& entry);
        void operator= (const queue<Item>& source);
        bool empty() const{return (count==0);}
        Item front() const;
        size_t size() const{ return count;}
}
```
<br>

```cpp
template<class Item>
queue<Item>::queue(){
    count=0;
    front_ptr=NULL;
}

template<class Item>
queue<Item>::queue(const queue<Item>& source){
    count = source.count;
    list_copy(source.front_ptr, front_ptr,rear_ptr);
}

template<class Item>
queue<Item>::~queue(){
    list_clear(front_ptr);
}

template<class Item> 94p
??? queue<Item>::operator =(const queue<Item>& source){
    if(this==&source) return;
    list_clear(front_ptr);
    lsit_copy(source.front_ptr,front_ptr,rear_ptr);
    count=source.count;
    ?????return?
}

template<class Item>
Item queue<Item>::front() const{
    assert(!empty());
    return front_ptr->data();
}

template<class Item>
void queue<Item>::pop(){
    assert(!empty());
    list_head_remove(front_ptr);
    count--;
}

template<class Item>
void queue<Item>::push(const Item& entry){
    if(empty()){
        list_head_insert(front_ptr,entry);
        rear_ptr=front_ptr;
    }
    else{
        list_insert(rear_ptr,entry);
        rear_ptr=rear_ptr->link();
    }
    count++;
}

```
### Priority Queue 100p
우선순위 큐
우선순위가 높은애가 먼저 나온다.
비선형구조임
STL에서는 priority_queue<Item>과 같이 사용한다.
'<' operation이 정의되어 있는 경우에만 사용할 수 있따.
*multi set도 동일하다

### Deque : Double Ended Queue
deque<int> dq;

# //다시 좀더 나중에 정리할 것