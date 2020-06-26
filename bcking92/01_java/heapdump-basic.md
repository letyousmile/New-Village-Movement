# heapdump
프로세스의 메모리 누수(memory leak)가 발생해 OOM상황이 일어났을 때 Java에서 당시 힙메모리의 사용을 로그파일로 저장할 수 있는데 그것이 heapdump 이다.

OOM상황에서 heapdump떨어뜨리는 방법은 여러가지가 있지만

```shell
java -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=<file-or-dir-path>
```

키워드는 OOM상황이 발생했을 때 해당경로로 heapdump를 떨궈준다. 

heapdump를 분석하는 툴도 여러가지가 있지만 이클립스에서 제공하는 Eclipse Memory Analyzer(MAT)를 사용하면 쉽게 분석할 수 있다.