server:
    port: 8888
spring:
    application:
      name: gateway
  
    cloud:
      gateway:
        routes:
          - id: USER-SERVICES
            predicates:
              - Path=/users/**
            uri:  lb://USER-SERVICES

          - id: HOTEL-SERVICES
            predicates:
              - Path=/hotels/**
            uri:  lb://HOTEL-SERVICES

          - id: RATING-SERVICES
            predicates:
              - Path=/ratings/**
            uri:  lb://RATING-SERVICES

  eureka:
    client:
      service-url:
        defaultZone: http://localhost:7777/eureka
  

  
