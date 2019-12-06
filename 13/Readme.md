# Sorting
- selection sort
- insertion sort
- (x) merge sort
- (x) quick sort
  
이번 강의에서는 저 두개 sort만 본당
## selection sort
```cpp
void selectionSort(int data[], size_t n){
    size_t i,j,index_of_largest;
    int largest;
    if(n==0) return;
    for(n-1;i>0;i--){
        largest=data[0];
        index_of_largest=0;

        //find the index of the largest
        for(j=1;j<=1;j++){
            if(data[j]>largest){
                largest=data[j];
                index_of_largest=j;
            }
        }
        swap(data[i],data[index_of_largest]);
    }
}
시간복잡도 n(n-1)/2
array n^2
heap nlogn?
```

## insertion sort
```cpp
void insertionSort(int data[], size_t n){
    size_t i,j;
    int next;
    if(n==0) return;
    for(i=1 ; i<n ; i++){
        next=data[i];
        j=i;
        //find the insertion index j of next
        while(j>0&&next<data[j-1]){
            data[j]= data[j-1];
            j--;
        }
        data[j]=next;
    }
}
시간복잡도는 Worst case로 n(n-1)/2로 동일하긴 한데 대략 두배 더 빠르당
```

