# Java 파일 크기 구하기

Java에서 파일 크기를 구하는 방법은 두가지가 있다. File 모듈을 이용하는 것과 Files 모듈을 이용하는 것

1. File 모듈이용하기
- file 모듈을 이용해서 크기를 구하는 것은 아주 간단하다.

    ```java
    File file = new File(FILE_PATH);
    long fileSize = file.length();
    ```

2. Files 모듈이용하기
- files 모듈을 이용해서 크기를 구하려면 바이트로 읽어와서 구하게 된다.

    ```java
    byte[] encoded = Files.readAllBytes(Paths.get(FILE_PATH));
    long fileSize = encoded.length();
     ```
- 위 방법을 응용해서 같은 파일을 다른 Charset으로 인코딩 했을 때 용량도 구할 수 있다. Byte 배열을 String으로 변환 후 다시 Byte 배열로 변환하는 과정에서 인코딩을 다르게 해주면 된다.

    ```java
    // 파일을 바이트배열로 읽어서
    byte[] encoded = Files.readAllBytes(Paths.get(FILE_PATH));
    // (기존에 UTF-8로 인코딩 되어있던 파일을)String 객체로 변환 후
    String temp = new String(encoded, StandardCharsets.UTF_8);
    // String 객체를 다시 EUC-KR 인코딩 하여 바이트배열로 가져와서 길이를 측정하면 끝~
    long fileSize = temp.getBytes("EUC-KR").length;
    ```