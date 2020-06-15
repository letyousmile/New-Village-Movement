# 자료구조

자료구조란 효율적인 접근 및 수정을 가능케 하는 자료의 집합, 관리, 저장을 의미한다. 각 자료들이 어떤 규칙에 의해 나열되고, 자료에 대한 처리를 효율적으로 수행하고자 구분하여 표현한것이다.

## 종류

* 선형구조
  * 배열
  * 리스트
  * 큐
  * 스택
* 비선형구조
  * 그래프
  * 트리

### 배열 (Array)

연관된 데이터를 하나의 변수에 그룹핑하여 관리하는 자료구조.  배열의 크기가 정해져 있고, 인덱스를 통해 조회를 한다.

#### 단점

* 크기를 변경하기엔 낭비가 크다.
* 인덱스를 통해 관리하기에 삽입,삭제 작업시 시간이 오래걸린다.

````java
int[] arr= new int[size]; // 배열 선언

private int[] changeArrSize(int[] arr, int size){
	int[] newArr = new int[size];
  for(int i =0;i<size || i<arr.length;i++){
    newArr[i] = arr[i];
  }
  return newArr;
}

arr = changeArrSize(arr, newSize); // 배열 크기 변경 

private void deleteArrElement(int[] arr , int index){
  arr[index] = null;
  
  // null 들어간 부분을 채우기 위해 포문을 돌아야함
  for(int i = index; i<arr.length-1;i++){ 
    arr[i] = arr[i+1]; 
  }
  
}

deleteArrElement(arr, index); // index에 해당하는 값 삭제

private void insertArrElement(int[] arr, int index, int value){
  if(arr[arr.length-1] != null){ // 배열이 가득 차있을시 공간을 늘려야함
    arr = changeArrSize(arr, arr.length+5);
  }
  for(int i = arr.length-1;i>index;i--){//값을 넣기위해 공간을 확보해야함
    arr[i] = arr[i-1];
  }
  arr[index] = value;
}

insertArrElement(arr, index, value); // 해당 index에 value 삽입

````



### 리스트 (List)

데이터를 빈틈없이 채우고 관리하는 자료구조, 순서가 존재하는 데이터의 모임이다. 자바에선 vector, arraylist, linkedlist 가 존재한다.

* Vector
  * Arraylist 와 다르게 멀티쓰레드 환경에서 동기로 처리되어 속도가 느리다.(거의 사용안함)
* Arraylist
  * Array 와 다르게 사이즈를 처음에 초기화 할 필요없다.
* Linkedlist
  * 중간 삽입, 삭제가 ArrayList 에 비해 빠르다곤 하지만 해보니깐 아니였다.



````java
ArrayList<E> arrayList = new ArrayList<>();
Vector<E> vector = new Vector<>();
LinkedList<E> linkedList = new LinkedList<>();

//ArrayList
st = System.currentTimeMillis();
for(int i =1;i<=50000;i++){
  arrayList.add(0,i);
}
ds = System.currentTimeMillis();
System.out.println("arrayList add 처음부터(50000개): "+ (ds - st));

long st2 = System.currentTimeMillis();
for(int i =0;i<10000;i++) {
	arrayList.add((int)(Math.random()*(50000+i), 1);
}
ds = System.currentTimeMillis();
System.out.println("arrayList add 특정위치(10000개): "+ (ds - st2));


//LinkedList
st = System.currentTimeMillis();
for(int i =1;i<=50000;i++){
	linkedList.add(0,i);
}
ds = System.currentTimeMillis();
System.out.println("linkedList add 처음부터(50000개): "+ (ds - st));

long st1 = System.currentTimeMillis();
for(int i =0;i<10000;i++) {
	linkedList.add((int)(Math.random()*(50000+i)), 1);
}
ds = System.currentTimeMillis();
System.out.println("linkedList add 특정위치(10000개): "+ (ds - st1));
                
                
//result
arrayList add 처음부터(50000개): 155
arrayList add 특정위치(10000개): 26
linkedList add 처음부터(50000개): 14
linkedList add 특정위치(10000개): 463
````

[참고](https://okky.kr/article/567246) 하면 좋을거 같다.

### 큐 (Queue)

FIFO, LILO 의 성격을 띄고 있다. Cpu 스케쥴링에 보통 사용된다. LinkedList를 사용하여 구현이 가능하다.

````java
Queue<E> que = new LinkedList<>();
````

### 스택 (Stack)

큐와는 반대로 LIFO의 성격을 띄고 있으며, 웹이나 모바일에서 방문기록등을 관리하는데 사용한다. 배열을 가지고 구현가능하다.

````java
Stack<E> stack = new Stack<>();
````

### 그래프 (Graph)

노드와 그 노드를 연결하는 간선으로 이루어진 자료구조, 트리와 비슷하지만 다른점이 많기에 따로 만들지 않았을까?

#### 특징

* 무방향, 방향 그래프 존재
* 순환, 비순환 구조를 가짐
* bfs/dfs로 순회
* 부모,자식,루트 노드 존재 x

#### 구현

* 인접 리스트
* 인접 행렬

### 트리 (Tree)

하나의 루트노드를 갖고 있으며 노드와 간선으로 이루어진 자료구조. 방향성이 있는 비순환 그래프의 한 종류이며 노드가 N개 있을 경우 간선은 항상 N-1개 갖는다.

#### 종류

* 이진트리
* 이진탐색트리
* 균형트리
* 등등.. 너무 많다



