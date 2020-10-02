# Layered_Architecture_Exercise

Service 객체 
- 비지니스 로직을 수행하는 메소드를 가지고 있는 객체
- 비지니스 로직은 하나의 트랜잭션으로 동작

Transaction
- 특징
  1. 원자성 : 모든 작업이 수행되거나 (commit) 모든 작업이 취소 (rollback)
  2. 일관성 : 트랜잭션의 처리결과가 항상 일관성이 있어야 함
  3. 독립성 : 어느 하나의 트랜잭션이라도 다른 트랜잭션의 연산에 끼어들지 못함
  4. 지속성 : 트랜잭션이 성공적으로 완료되면 그 결과는 영구히 지속되어야 함 
  
설정의 분리

  Spring 설정 파일을 프리젠테이션 레이어쪽과 나머지를 분리할 수 있습니다.
  web.xml 파일에서 프리젠테이션 레이어에 대한 스프링 설정은 DispathcerServlet이 읽도록 하고, 그 외의 설정은 ContextLoaderListener를 통해서 읽도록 합니다.
  DispatcherServlet을 경우에 따라서 2개 이상 설정할 수 있는데 이 경우에는 각각의 DispathcerServlet의 ApplicationContext가 각각 독립적이기 때문에 각각의 설정 파일에서 생성한 빈을 서로 사용할 수 없습니다.
  위의 경우와 같이 동시에 필요한 빈은 ContextLoaderListener를 사용함으로써 공통으로 사용하게 할 수 있습니다.
  ContextLoaderListener와 DispatcherServlet은 각각 ApplicationContext를 생성하는데, ContextLoaderListener가 생성하는 ApplicationContext가 root컨텍스트가 되고 DispatcherServlet이 생성한 인스턴스는 root컨텍스트를 부모로 하는 자식 컨텍스트가 됩니다.
  참고로, 자식 컨텍스트들은 root컨텍스트의 설정 빈을 사용할 수 있습니다.
  
 ================================================================================
 
 인터셉터(Interceptor)
 - Dispatcher servlet에서 Handler(Controller)로 요청을 보낼 때, Handler에서 Dispathcer servlet으로 응답을 보낼 때 동작한다.
 - 관리자만 접근할 수 있는 관리자 페이지에 접근하기 전에 관리자 인증을 하는 용도로 활용
 
 아규먼트 리졸버(Argument Resolver)
 - 컨트롤러 메소드의 인자로 사용자가 임의의 값을 전달하는 방법을 제공하고자 할 때 사용 => 세션에 저장되어 있는 값 중 특정 이름의 값을 메소드 인자로 전달
 - getDefaultArgumentResolvers()메소드를 보면 기본으로 설정되는 아규먼트 리졸버에 어떤 것이 있는지 알 수 있습니다.
 - Map객체나 Map을 상속받은 객체는 Spring에서 이미 선언한 아규먼트 리졸버가 처리하기 때문에 전달 할 수 없습니다.
 - Map객체를 전달하려면 Map을 필드로 가지고 있는 별도의 객체를 선언한 후 사용해야 합니다.
 
 로깅(Logging)
- 정보를 제공하는 일련의 기록인 로그를 생성하도록 시스템을 설정하는 활동
- printlining은 간단한, 보통은 일시적인 로그를 생성하기만 한다.
- 시스템 설계자들은 시스템의 복잡성 때문에 로그를 이해하고 사용해야 한다.
- 로그가 제공하는 정보의 양은, 이상적으로는 프로그램이 실행되는 중에도 설정 가능해야 한다.
- 로그 장점
  - 로그는 재현하기 힘든 버그에 대한 유용한 정보를 제공할 수 있다.
  - 로그는 성능에 관한 통계와 정보를 제공할 수 있다.
  - 설정이 가능할 때, 로그는 예기치 못한 특정 문제들을 디버그하기 위해, 그 문제들을 처리하도록 코드를 수정하여 다시 적용하지 않아도 일반적인 정보를 갈무리할 수 있게 한다.

SLF4J
- logging 관련 라이브러리는 다양한데 이러한 라이브러리들을 하나의 통일된 방식으로 사용할 수 있는 방법을 제공한다.
- 로깅 Facade이다.
- 로깅에 대한 추상 레이어를 제공하는 것이고 interface의 모음이다.

Appender
- ConsoleAppender : 콘솔에 로그를 어떤 포맷으로 출력할지를 설정할 때 사용한다.
- FileAppender : 파일에 로그를 어떤 포맷으로 출력할지를 설정한다.
- RollingFileAppender : 로그의 양이 많아지면, 하나의 파일로 관리하기 어려워지는 경우가 생긴다. 이런 문제를 해결하기 위해 하루 단위로 로그를 관리하고자 할 경우 사용된다.

Log Level
- trace : debug보다 세분화된 정보
- debug : 디버깅하는데 유용한 세분화된 정보
- info : 진행상황 같은 일반 정보
- warn : 오류는 아니지만 잠재적인 오류 원인이 될 수 있는 경고성 정보
- error : 요청을 처리하는 중 문제가 발생한 오류 정보
