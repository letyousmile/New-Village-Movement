# Maven
Maven은 Java에서 사용하는 build tool이다. 이클립스, vscode등에 같이 사용할 수 있다.

mvn의 루트 디렉토리 환경변수 이름은 M2_HOME 이고 path에 mvn이 있는 위치를 등록해서 콘솔에서 사용할 수 있다.

## 프로젝트 만들기

```shell
 mvn archetype:generate -DgroupId=com.bcking92 -DartifactId=mavenpjt -DarchetypeArtifactId=maven-archetype-quickstart
```
- archetype:generate -> 생성하겠다
- -DgroupId: 프로젝트를 식별하기 위한 그룹명
- -DartifactId: 프로젝트명
- -DarchetypeArtifactId: 이미 만들어진 구조를 사용하겠다.(maven-archetype-quickstart 구조를 사용하겠다.)

## 컴파일

pom.xml이 있는 위치에서

```shell
mvn compile
```
```shell
[ERROR] Source option 5 is no longer supported. Use 7 or later.
[ERROR] Target option 5 is no longer supported. Use 7 or later.
```
이런 에러가 나타나면 pom.xml을 수정해야함

```xml
<properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```
위와 같이 pom.xml에 properties를 추가해준다.

다시 compile을 실행하면 정상적으로 빌드가 되고 target파일에 compile된 class가 나타난다. 이 class를 java로 실행할 수 있다.

## package
코드를 아예 패키지화 할 수도 있는데 이 때는
```shell
mvn package
```
명령어를 사용한다. 패키징이 완료되면 프로젝트가 압축파일인 jar 파일로 만들어진 패키지가 완성된다.

jar 파일을 실행하기 위해서 `java -cp`를 사용할 수 있다.

- `-cp`: class path

```shell
 java -cp target/mavenpjt-1.0.jar com.bcking92.App
```

## Life Cycle

- maven은 build life cycle 별로 잘 모듈화가 되어있고 단계적으로 실행된다. 예를들어 `mvn compile`을 실행하면 아래 보이는 단계중에 처음부터 compile 단계까지 순차적으로 실행되는 것이다.
- 각각의 단계에는 단계별로 알맞은 역할을 하는 plugin을 사용할 수 있다. plugin의 적용은 pom.xml에서 하게 된다.
- 그 중 process-resources, compile, process-test-resources, test-compile, test, package 단계를 실행하는 plugin은 기본적으로 설정되어 있다.
- plug-in은 내부적으로 작은 프로그램으로 나누어져 있는데 이것을 goal이라고 한다.
- phase : plug-in includes goals

```xml
<phases>
    <phase>validate</phase>
    <phase>initialize</phase>
    <phase>generate-sources</phase>
    <phase>process-sources</phase>
    <phase>generate-resources</phase>
    <phase>process-resources</phase>
    <phase>compile</phase>
    <phase>process-classes</phase>
    <phase>generate-test-sources</phase>
    <phase>process-test-sources</phase>
    <phase>generate-test-resources</phase>
    <phase>process-test-resources</phase>
    <phase>test-compile</phase>
    <phase>process-test-classes</phase>
    <phase>test</phase>
    <phase>prepare-package</phase>
    <phase>package</phase>
    <phase>pre-integration-test</phase>
    <phase>integration-test</phase>
    <phase>post-integration-test</phase>
    <phase>verify</phase>
    <phase>install</phase>
    <phase>deploy</phase>
</phases>
```

- packaging 방식도 jar(java archive) -> war(web application archive)로 pom.xml에서 바꿀 수 있다.

단계별로 어떤 플러그인이 사용되는지 보려면 아래와 같이 입력해본다.
```shell
$ mvn help:describe -Dcmd=compile
```

plug-in은 maven.apache.org에서 구할 수 있다.

