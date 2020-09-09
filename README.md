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
