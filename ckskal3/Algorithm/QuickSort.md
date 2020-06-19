# QuickSort
분할정복기법을 사용한 정렬 방법으로 O(nlogn)만에 가능하다.

```java
public class QuickSort {
    static void qSort(int[] arr, int st, int ds){
        if(st < ds){
            // 중간값을 피봇으로 설정함으로서 최악을 피한다.
            int mid = (st + ds)/2;
            if(arr[st] > arr[ds]) swap(arr, st,ds);
            if(arr[mid] > arr[ds]) swap(arr, mid, ds);
            if(arr[mid] > arr[st]) swap(arr, st, mid);

            int i = st;
            int j = ds;

            mid = st;

            while(i<j){
                while(arr[i] <= arr[mid] && i < ds){
                    ++i;
                }
                while(arr[j] > arr[mid]){
                    --j;
                }
                if(i<j) {
                    swap(arr, i, j);
                }
            }

            swap(arr,mid, j);
            qSort(arr, st, j-1);
            qSort(arr, j+1, ds);

        }

    }
}

```