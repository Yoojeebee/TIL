# 서블릿(Servlet)  
서블릿은 웹서버에서 서비스되는 페이지이다. 서블릿을 개발하였으면 해당 서블릿 실행 파일을 웹서버에 올려두어야 한다. 클라이언트는 웹서버에 서비스를 요청할 때 URL 정보를 보낸다.  
<br/>

![url_info](https://github.com/Yoojeebee/TIL/blob/master/images/url_info.jpg?raw=true)  

<br/>

1. <strong>70.12.220.93</strong>  
웹 애플리케이션을 서비스하는 웹서버가 설치된 컴퓨터의 주소.  
컴퓨터 주소는 아이피 주소(IP Address) 또는 도메인 이름(Domain Name)으로 지정.  
만일 서비스를 요청하는 클라이언트와 서버가 같은 컴퓨터에 있다면 컴퓨터의 주소(아이피 또는 도메인) 대신 `localhost` 또는 `127.0.0.1` 이라고 표시해도 된다.  
<br/>

2. <strong>:8080</strong>  
포트 번호로 서버를 찾아가기 위한 정보.  
해당 컴퓨터에서 서비스하는 서버 중 웹서버를 찾는 번호.  
웹서버가 사용하는 포트는 80번으로 예약되어 있으며 포트 번호를 생략시 자동 인식됨.  
80이 아닐시 반드시 명시해주어야 하며, 웹 서버를 설치할 때 설정한 포트 번호를 확인.  
<br/>

3. <strong>/edu</strong>  
웹 애플리케이션 이름.  
웹서버는 애플리케이션 단위로 서비스.  
<br/>

4. <strong>/index.jsp</strong>  
클라이언트가 요청한 최종 문서정보.  
애플리케이션을 찾은 다음에는 해당 애플리케이션에서 서비스하는 문서를 찾아야 된다.  
찾으려는 웹 문서의 경로는 파일시스템의 디렉터리 구조 형태로 지정.  
<br/>

## 웹 애플리케이션 구조  
웹 애플리케이션은 디렉터리나 디렉터리가 압축된 형태로 웹서버에 올려서 서비스하는데, 이 때 웹 애플리케이션이 서비스되는 공통 구조는 아래의 그림과 같다.  
<br/>

![web_application_directory](https://github.com/Yoojeebee/TIL/blob/master/images/web_application_directory.jpg?raw=true)  
<br/>

모든 웹 애플리케이션이 공통으로 가져야 하는 디렉터리와 파일이 있다. 웹 애플리케이션 루트 디렉터리 바로 하위에 `WEB-INF` 디렉터리이며, `WEB-INF` 디렉터리에는 `web.xml` 파일이 있어야 한다.  
  
현재 웹 애플리케이션에서 서비스하려는 클래스 파일이 있다면 `WEB-INF/classes` 디렉터리 하위에 있어야 하며, 클래스 파일들이 jar로 압축되어 있다면 `WEB-INF/lib` 디렉터리에 있어야 한다. 왜냐하면, 클래스 파일들이 `WEB-INF/classes` 또는 `WEB-INF/lib` 에 있어야만 WAS를 구성하는 애플리케이션 서버들이 자동으로 인식할 수 있기 때문이다. `web.xml` 파일과 클래스 파일을 제외한 다른 파일들은 웹 애플리케이션 루트 디렉터리 하위의 어느 곳에 있어도 상관 없다.  
<br/>

## 환경설정 파일  
클라이언트에 서비스하기 위해 웹 애플리케이션을 준비하는 작업에서 웹서버는 각 웹 애플리케이션의 `web.xml`파일을 읽는다. `web.xml`은 웹 애플리케이션의 서비스 처리에 관한 내용이 정의된 파일이며 웹 서버는 정의된 내용대로 웹 애플리케이션을 실행하기 위한 설정을 수행한다.  
만약 `web.xml 파일에 정의한 내용이 논리적으로나 문법적으로 잘못되었다면, 웹 애플리케이션을 서비스하는 데 필요한 준비 설정을 올바르게 할 수 없다. 웹 서버는 웹 애플리케이션 단위로 서비스하며 잘못 작성한 `web.xml 파일 때문에 서비스에 실패한다면, 웹 애플리케이션 안에 있는 모든 파일은 서비스되지 않는다.  
<br/>

## 서블릿 구현  
웹 브라우저에서 클라이언트의 요청에 따라 서버가 실행될 수 있는 자바 프로그램은 서블릿뿐이다. 서블릿만이 웹에서 동작하는 특별한 조건을 가지고 있다는 뜻이며, 이러한 조건은 이미 서블릿 API를 통해 제공하고 있어서 직접 만들지 않아도 된다.  
<br/>

### 서블릿 클래스 간의 관계  
서블릿을 구현할 때는 반드시 `java.servlet.http` 패키지의 `HttpServlet` 클래스이다. `HttpServlet`에는 웹상에서 클라이언트 요청이 있을 때 해당 서블릿을 실행하는 모든 조건이 포함되어 있다. 그래서 모든 서블릿은 반드시 `HttpServlet`을 상속받아야 한다. `HttpServlet`를 상속받지 않는 클래스는 서블릿이라 할 수 없고, 따라서 클라이언트가 실행을 요청하여도 실행되지 않는다.  

<strong>서블릿 프로그램의 클래스 계층 구조</strong>  
![httpServlet](https://github.com/Yoojeebee/TIL/blob/master/images/httpservlet.jpg?raw=true)  
<br/>

### Servlet 인터페이스  
Servlet은 서블릿 프로그램을 개발할 때 반드시 구현해야 하는 메소드를 선언하고 있는 인터페이스이며 `init()` , `destroy()` , `getServletConfig()` , `getServletInfo()` 등 5개의 메소드를 선언하며, 이는 서블릿 프로그램 실행의 생명주기와 연관된 메소드들.  
<br/>

### GenericServlet 클래스  
GenericServlet은 Servlet 인터페이스를 상속하여 클라이언트-서버 환경에서 서버단의 애플리케이션으로서 필요한 기능을 구현한 추상 클래스. `service()` 메소드를 제외한 모든 메소드를 재정의하여 적절한 기능으로 구현하였다. GenericServlet 클래스를 상속하면 애플리케이션의 프로토콜에 따라 메소드 재정의 구문을 적용해야 한다.  
<br/>

### HttpServlet 클래스  
HttpServlet 클래스는 GenericServlet 클래스를 상속하여 `service()`s 메소드를 재정의함으로써 HTTP 프로토콜에 알맞은 동작을 수행하도록 구현한 클래스이다. 즉, HTTP 프로토콜 기반으로 브라우저로부터 요청을 전달받아서 처리하도록 하는 클래스이다. `service()` 메소드에는 요청방식 (GET 또는 POST)에 따라 `doGet()` , `doPost()` 등 정해진 사양의 메소드가 호출되도록 구현되어 있다.  
<br/>

## 서블릿 작성  
![servlet_pratice_1](https://github.com/Yoojeebee/TIL/blob/master/images/servlet_pratice_1.jpg?raw=true)  

src 폴더에 `com.edu.test` 패키지 생성 -> `FirstServlet` 클래스 생성
```java
package com.edu.test;

import javax.servlet.http.HttpServlet;

public class FirstServlet extends HttpServlet {

}
```
<br/>

## 서브릿 실행 순서  
Java SE 프로그램은 개발자가 main() 메소드 안에 구현한 순서대로 실행된다. 즉, 프로그램의 실행 순서를 개발자가 제어한다. 그러나 Java EE 기반 프로그램은 실행의 흐름을 개발자가 제어하는 것이 아닌 컨테이너가 제어한다.  
개발자가 아닌 제3자가 프로그램의 실행 흐름을 제어하는 것을 IoC(Inversion of Control) `제어의 역전` 이라고 한다. 서블릿도 이에 포함된다. Java EE 기반 프로그램을 개발할 때는 먼저 애플리케이션 컨테이너들이 프로그램을 어떤 순서로 동작시키지는지 알고 해당 순서에 맞게 개발해야 한다.  
<br/>

![servlet_flow](https://github.com/Yoojeebee/TIL/blob/master/images/servlet_flow.jpg?raw=true)  
<br/>

   1. <strong>클라이언트로부터 처리 요청 받음</strong>  
   클라이언트가 웹 브라우저를 통해 요청을 보내면 웹서버는 이를 받아서 요청정보의 헤더 안에 있는 URI를 분석. 이때, 요청받은 페이지가 서블릿이면 서블릿 컨테이너에 처리를 넘긴다. 서빌릿 컨테이너는 요청받은 서블릿을 WEB-INF/classes나 WEB-INF/lib 에서 찾아서 실행 준비.  
   <br/>

   2. <strong>최초의 요청 여부 판단</strong>  
   서블릿 컨테이너는 현재 실행할 서블릿이 최초의 요청인지 판단. 최초의 요청은 말 그대로 처음 요청할 때 딱 한번만 실행.  
    <br/>

   3. <strong>서블릿 객체 생성</strong>  
   서블릿 컨테이너는 요청받은 서블릿이 최초의 요청이라면 해당 서블릿을 메모리에 로딩하고 객체를 생성. 일반 자바의 new 명령문과 달리 서블릿은 최초 요청이 들어왔을 때 한 번만 객체를 생성하고 이 때 생성된 객체를 계속 사용.  
   <br/>

   4. <strong>init() 메소드 실행</strong>  
   서블릿 객체가 생성된 다음에 호출되는 메소드로써, Servlet 인터페이스에 선언되어 있고, 기능은 GenericServlet 클래스에 구현되어 있다. 처음 요청 시 서블릿 객체가 생성된 다음 호출되므로 주로 서블릿 객체의 초기화 작업이 구현되어 있다.  
   <br/>

   5. <strong>service() 메소드 실행</strong>  
   실행하는 서블릿의 요청 순서에 상관없이 클라이언트의 요청이 있을 대마다 실행된다. 따라서, service() 메소드에는 실제 서블릿에서 처리해야 하는 내용이 구현되어 있다.  
   <br/>

`service()` 메소드가 끝나면 서버에서의 실행은 끝나며 실행이 완료된 후에는 서블릿 컨테이너가 실행결과를 웹서버에 전달하고, 웹서버는 서비스를 요청한 클라이언트에 응답한다. 이로써 웹에서 하나의 요청에 대한 처리가 완료된다.  
<br/>

```java
package com.edu.test;

import java.io.IOException;

import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServlet;

public class FirstServlet extends HttpServlet {
	
	@Override
	public void init() throws ServletException {
		System.out.println("init() 실행!");
	}
	
	@Override
	public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
		System.out.println("service() 실행!");
	}

}
```
<br/>

## 콜백 메서드와 서블릿 객체의 생명주기  
서블릿에서 콜백 메서드(callback method)란? 어떤 객체에서 어떤 상황이 발생하면 컨테이너가 자동으로 호출하여 실행되는 메소드를 의미. 이러한 콜백 메소드들이 서블릿을 실행. 바로 앞에서 설명했던 HttpServlet 클래스를 상속받은 다음 재정의한 `init()` , `service()` 가 콜백 메서드에 속한다. 이 메서드들은 객체에 어떤 상황(이벤트)이 발생하면 호출된다.  
<br/>

<strong>서블릿의 콜백 메서드</strong>  
![servlet_flow](https://github.com/Yoojeebee/TIL/blob/master/images/servlet_flow.jpg?raw=true)  
<br/>

### 서블릿 객체의 생성  
서블릿 객체가 메모리에 생성되는 시점은 서버 입장에서 클라이언트로부터 최초로 서블릿 실행요청이 있을 때이며, 클라이언트 입장이 아닌 서버 입장이다. 어떤 클라이언트가 요청했는지는 중요하지 않다.  

서버 입장에서 최초로 서블릿 요청이 있을 때, 서블릿 컨테이너는 해당 서블릿 객체를 메모리에 생성한 다음, `intit()` -> `service()` 순으로 실행. 이후 같은 서블릿 실행 요청이 있으면 최초 요청 시 생성한 서블릿 객체의 `service()` 메소드를 실행한다.  

서블릿 요청이 있을 때마다 요청이 최초인지를 판단하는 기준은 객체 생성 여부이다.  
<br/>

### 서블릿 객체의 삭제  
최초 요청 시 생성된 서블릿 객체가 삭제되는 시점은 서버를 중지시켜 웹 애플리케이션 서비스를 중지할 때이다. 웹서버에서는 전체 서비스를 중지할 수도 있고 일부 서비스만 중지할 수도 있다. 어떤 상황이든지 서블릿 객체가 삭제되는 시점은 웹서버에서 웹 애플리케이션 서비스가 중지되는 시점이다. 이 때,  `destroy()` 메소드가 호출되어 실행된다. `destroy()` 메소드에는 자원 해제하는 내용을 구현하면 된다.