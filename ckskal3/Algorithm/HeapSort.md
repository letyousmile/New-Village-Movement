# HeapSort
힙이란 완전이진트리의 일종으로 우선순위 큐를 만들기 위한 자료구조이다. 최댓값이나 최솟값을 추출하기 쉽다.

이런 힙을 활용하여 정렬하는 것을 힙소트라 한다.
```java
public class HeapSort {
    //Max Heap 을 구현해 보자
    //부모 노드가 자식 노드보다 무조건 커야함
    static void hSort(int[] arr){
        for(int i =0;i<arr.length;i++){ //힙 구조를 만드는 과정
            heapify(arr, i);
        }

        for(int i =arr.length-1;i>-1 ;i--){ 
            swap(arr, i, 0 );
            heapSort(arr,  i);
        }
    }

    static void heapify(int[] arr, int idx){
        int parent = (idx-1)/2;

        while(parent >=0 ){
            if(arr[parent] < arr[idx]){
                swap(arr, parent, idx);
                idx = parent;
                parent = (idx-1)/2;
            }else{
                break;
            }
        }
    }

    static void heapSort(int[] arr, int last){
        int idx = 0;
        int nextIdxR =  (idx+1)*2;
        int nextIdxL = (idx+1)*2 - 1;
        int compare = 0;
        while(nextIdxL < last ){
           if(nextIdxR < last){
               compare = arr[nextIdxL] > arr[nextIdxR] ?  nextIdxL : nextIdxR;
           }else{
               compare  = nextIdxL;
           }

           if(arr[idx] < arr[compare]){
               swap(arr, idx, compare);
               idx = compare;
               nextIdxR =  (idx+1)*2;
               nextIdxL = (idx+1)*2 - 1;
           }else{
               break;
           }
        }
    }
}

```