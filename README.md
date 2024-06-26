# 기본 설정

### 프로그램 Gradle 설정

```
plugins {
	id 'java'
	id 'org.springframework.boot' version '3.2.4'
	id 'io.spring.dependency-management' version '1.1.4'
}

group = 'hello'
version = '0.0.1-SNAPSHOT'

java {
	sourceCompatibility = '17'
}

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
	useJUnitPlatform()
}
```

gradle-wrapper.properties
```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-7.3-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

# 섹션 1 : 타임리프 - 기본기능

## 타임리프 특징
1. 서버 사이드 HTML 렌더링 (SSR)
2. 네츄럴 템플릿
3. 스프링 통합 지원

## 서버 사이드 HTML 렌더링 (SSR)
- 타임리프는 백엔드 서버에서 HTML을 동적으로 렌더링 하는 용도로 사용된다.

## 네츄럴 템플릿
- 타임리프는 순수 HTML을 최대한 유지하는 특징이 있다.
- 타임리프로 작성한 파일은 HTML을 유지하기 때문에 웹 브라우저에서 파일을 직접 열어도 내용을 확인할 수 있고, 서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다. 
- JSP를 포함한 다른 뷰 템플릿들은 해당 파일을 열면, 예를 들어서 JSP 파일 자체를 그대로 웹 브라우저에서 열어보면 JSP 소스코드와 HTML이 뒤죽박죽 섞여서 웹 브라우저에서 정상적인 HTML 결과를 확인할 수 없다. 오직 서버를 통
해서 JSP가 렌더링 되고 HTML 응답 결과를 받아야 화면을 확인할 수 있다.
- 반면에 타임리프로 작성된 파일은 해당 파일을 그대로 웹 브라우저에서 열어도 정상적인 HTML 결과를 확인할 수 있다. 물론 이 경우 동적으로 결과가 렌더링 되지는 않는다. 하지만 HTML 마크업 결과가 어떻게 되는지 파일만 열어도 바
로 확인할 수 있다.
- 이렇게 순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿(natural templates)이라 한다.
  
## 스프링 통합 지원
타임리프는 스프링과 자연스럽게 통합되고, 스프링의 다양한 기능을 편리하게 사용할 수 있게 지원한다.


## 기본표현식

```
간단한 표현:
◦ 변수 표현식: ${...}
◦ 선택 변수 표현식: *{...}
◦ 메시지 표현식: #{...}
◦ 링크 URL 표현식: @{...}
◦ 조각 표현식: ~{...}
• 리터럴
◦ 텍스트: 'one text', 'Another one!',…
◦ 숫자: 0, 34, 3.0, 12.3,…
◦ 불린: true, false
◦ 널: null
◦ 리터럴 토큰: one, sometext, main,…
• 문자 연산:
◦ 문자 합치기: +
◦ 리터럴 대체: |The name is ${name}|
• 산술 연산:
◦ Binary operators: +, -, *, /, %
◦ Minus sign (unary operator): -
• 불린 연산:
◦ Binary operators: and, or
◦ Boolean negation (unary operator): !, not
• 비교와 동등:
◦ 비교: >, <, >=, <= (gt, lt, ge, le)
◦ 동등 연산: ==, != (eq, ne)
• 조건 연산:
◦ If-then: (if) ? (then)
◦ If-then-else: (if) ? (then) : (else)
◦ Default: (value) ?: (defaultvalue)
• 특별한 토큰:
◦ No-Operation: _

타임리프 유틸리티 객체들
#message : 메시지, 국제화 처리
#uris : URI 이스케이프 지원
#dates : java.util.Date 서식 지원
#calendars : java.util.Calendar 서식 지원
#temporals : 자바8 날짜 서식 지원
#numbers : 숫자 서식 지원
#strings : 문자 관련 편의 기능
#objects : 객체 관련 기능 제공
#bools : boolean 관련 기능 제공
#arrays : 배열 관련 기능 제공
#lists , #sets , #maps : 컬렉션 관련 기능 제공
#ids : 아이디 처리 관련 기능 제공, 뒤에서 설명
```

### 타임리프 유틸리티 객체
https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#expression-utility-objects

### 유틸리티 객체 예시
https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-b-expressionutility-objects

## 주석
```
<h1>1. 표준 HTML 주석</h1>
<!--
<span th:text="${data}">html data</span>
-->
<h1>2. 타임리프 파서 주석</h1>
<!--/* [[${data}]] */-->
<!--/*-->
<span th:text="${data}">html data</span>
<!--*/-->
<h1>3. 타임리프 프로토타입 주석</h1>
<!--/*/
<span th:text="${data}">html data</span>
/*/-->
```

1. 표준 HTML 주석
자바스크립트의 표준 HTML 주석은 타임리프가 렌더링 하지 않고, 그대로 남겨둔다.

2. 타임리프 파서 주석
타임리프 파서 주석은 타임리프의 진짜 주석이다. 렌더링에서 주석 부분을 제거한다.

3. 타임리프 프로토타입 주석
- 타임리프 프로토타입은 약간 특이한데, HTML 주석에 약간의 구문을 더했다.
- HTML 파일을 웹 브라우저에서 그대로 열어보면 HTML 주석이기 때문에 이 부분이 웹 브라우저가 렌더링하지 않는다.
- 타임리프 렌더링을 거치면 이 부분이 정상 렌더링 된다.


# 섹션 2 - 타임리프 - 스프링 통합과 폼

1. 환경설정
```
plugins {
id 'java'
id 'org.springframework.boot' version '3.2.4'
id 'io.spring.dependency-management' version '1.1.4'
}

group = 'hello'
version = '0.0.1-SNAPSHOT'

java {
	sourceCompatibility = '17'
}

configurations {
	compileOnly {
		extendsFrom annotationProcessor
	}
}

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	compileOnly 'org.projectlombok:lombok'
	annotationProcessor 'org.projectlombok:lombok'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
	useJUnitPlatform()
}
```

문제점  package javax.annotation does not exist
1. javax.annotation.PostConstruct는 Java EE에서 제공하던 어노테이션입니다.
2. Java EE가 Jakarta EE로 바뀌면서, 패키지 이름도 javax에서 jakarta로 변경돼 javax.annotation.PostConstruct 대신 jakarta.annotation.PostConstruct를 사용해야 합니다.
```
import jakarta.annotation.PostConstruct;
```

``` Gradle 추가
dependencies {
    // Jakarta Annotations API
    implementation 'jakarta.annotation:jakarta.annotation-api:2.0.0'
}
```
