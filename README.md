#Hystrix
>서킷브레이커는 닫힌 상태가 기본이고, 상태변이가 발생해 메서드 호출을 막을때가 열린(Open) 상태임
>>(중요)fallback 메서드는 서킷브레이커가 열려 있을때만 호출 되는것이 아니다. 오류가 발생해도 발생. 자세한 상황은 아래에 설명
##@EnableCircuitBreaker :  Circuit Breaker를 활성화
##@HystrixCommand : Hystrix 적용
> @Hystrix에서 fallback method를 지정해 주면, Hystrix 메서드가 실패 했을때 fallback 메서드를 호출하해 처리하게 됨
>> fallback 메서드는 기존 메서드의 반환타입고 파라미터를 동일하게 해줘야함. 접근제어자는 달라도 상관 없음
##HystrixProperty 
> execution.isolation.thread.timeoutInMilliseconds : Hystrix 가 적용된 메서드의 타임아웃을 지정한다. 이 타임아웃 내에 메서드가 완료되지못하면 서킷브레이커가 닫혀있다고 하더라도 fallback 메서드가 호출된다. 보통 외부 API 를 호출하게되면 RestTemplate 과 같은 http client에도 connect, read timeout 등을 지정하게하는데 hystrix timeout은 이를 포함하고 여유를 좀 더 두어 잡는다. 기본값은 1초(1000)
> metrics.rollingStats.timeInMilliseconds : 서킷 브레이커가 열리기위한 조건을 체크할 시간이다. 아래에서 살펴볼 몇가지 조건들과 함께 조건을 정의하게되는데 "10초간 50% 실패하면 서킷 브레이커 발동" 이라는 조건이 정의되어있다면 여기서 10초를 맡는다. 기본값은 10초(10000)
> circuitBreaker.errorThresholdPercentage : 서킷 브레이커가 발동할 에러 퍼센트를 지정한다. 기본값은 50
> circuitBreaker.requestVolumeThreshold : 서킷 브레이커가 열리기 위한 최소 요청조건이다. 즉 이 값이 20으로 설정되어있다면 10초간 19개의 요청이 들어와서 19개가 전부 실패하더라도 서킷 브레이커는 열리지않는다. 기본값은 20
> circuitBreaker.sleepWindowInMilliseconds : 서킷 브레이커가 열렸을때 얼마나 지속될지를 설정한다. 기본값은 5초(5000)
> coreSize : 위에서 별도의 설명은 안했는데 Hystrix 작동방식은 Thread를 이용하는 Thread 방식과 Semaphore 방식이 있다. Thread 를 이용할 경우 core size를 지정하는 속성이다. 넷플릭스에서는 공식 가이드에 왠만하면 Thread 방식을 권장하고있다.(디폴트 설정도 Thread 방식이다.) 기본 coreSize 는 10
                                             

