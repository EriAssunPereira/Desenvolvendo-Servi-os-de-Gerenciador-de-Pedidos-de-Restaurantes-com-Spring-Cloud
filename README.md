# Desenvolvendo-Serviços-de-Gerenciador-de-Pedidos-de-Restaurantes-com-Spring-Cloud

Desenvolver serviços para um gerenciador de pedidos de restaurante utilizando Spring Cloud envolve a criação de microserviços independentes que se comunicam entre si e são monitorados utilizando Spring Boot Actuator e Spring Boot Admin. Vamos abordar cada parte deste projeto detalhadamente, desde a configuração inicial até a implementação dos serviços e monitoramento.

### Configuração Inicial do Projeto

1. **Setup do Projeto:**
   - Configure seu ambiente de desenvolvimento Java com JDK e Maven ou Gradle.
   - Crie um novo projeto Spring Boot com Spring Initializr ou sua IDE.

2. **Estrutura do Projeto:**
   Organize o projeto em módulos para cada serviço e um módulo para o servidor de monitoramento.
   ```
   /gerenciador-pedidos-restaurante
   ├── /pedido-service
   │   ├── src/
   ├── /item-service
   │   ├── src/
   ├── /eureka-server
   │   ├── src/
   ├── /admin-server
   │   ├── src/
   ├── /gateway-service
   │   ├── src/
   ├── /config-service
   │   ├── src/
   ├── pom.xml
   ```

### Implementação dos Microserviços

1. **Configuração do Eureka Server:**
   - Utilize o Eureka Server como servidor de descoberta para registrar e descobrir microserviços.

   Exemplo de configuração (`EurekaServerApplication.java`):
   ```java
   // /eureka-server/src/main/java/com/example/eurekaserver/EurekaServerApplication.java

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

   @SpringBootApplication
   @EnableEurekaServer
   public class EurekaServerApplication {

       public static void main(String[] args) {
           SpringApplication.run(EurekaServerApplication.class, args);
       }
   }
   ```

2. **Implementação dos Microserviços:**
   - Crie serviços separados para gerenciar pedidos (`pedido-service`) e itens (`item-service`).
   - Utilize Spring Data JPA para persistência de dados, se necessário.

   Exemplo básico de serviço (`PedidoServiceApplication.java`):
   ```java
   // /pedido-service/src/main/java/com/example/pedidoservice/PedidoServiceApplication.java

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

   @SpringBootApplication
   @EnableEurekaClient
   public class PedidoServiceApplication {

       public static void main(String[] args) {
           SpringApplication.run(PedidoServiceApplication.class, args);
       }
   }
   ```

   Exemplo básico de entidade (`Pedido.java`):
   ```java
   // /pedido-service/src/main/java/com/example/pedidoservice/model/Pedido.java

   import javax.persistence.Entity;
   import javax.persistence.GeneratedValue;
   import javax.persistence.GenerationType;
   import javax.persistence.Id;

   @Entity
   public class Pedido {

       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       private Long id;
       private String descricao;
       private double valorTotal;

       // getters e setters
   }
   ```

### Configuração do Spring Boot Actuator

1. **Configuração do Actuator:**
   - O Spring Boot Actuator fornece endpoints para monitoramento e gerenciamento de aplicativos em execução.

   Exemplo de configuração (`application.properties`):
   ```properties
   # /pedido-service/src/main/resources/application.properties

   management.endpoints.web.exposure.include=*
   ```

### Configuração do Spring Boot Admin

1. **Configuração do Spring Boot Admin:**
   - Utilize o Spring Boot Admin para monitorar os microserviços.

   Exemplo de configuração (`AdminServerApplication.java`):
   ```java
   // /admin-server/src/main/java/com/example/adminserver/AdminServerApplication.java

   import de.codecentric.boot.admin.server.config.EnableAdminServer;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;

   @SpringBootApplication
   @EnableAdminServer
   public class AdminServerApplication {

       public static void main(String[] args) {
           SpringApplication.run(AdminServerApplication.class, args);
       }
   }
   ```

### Configuração do Gateway com Spring Cloud Gateway

1. **Configuração do Gateway:**
   - Utilize o Spring Cloud Gateway como gateway para roteamento e gerenciamento de requisições.

   Exemplo de configuração (`application.yml`):
   ```yaml
   # /gateway-service/src/main/resources/application.yml

   spring:
     cloud:
       gateway:
         routes:
           - id: pedido-service
             uri: lb://pedido-service
             predicates:
               - Path=/pedidos/**
           - id: item-service
             uri: lb://item-service
             predicates:
               - Path=/itens/**
   ```

### Conclusão

Desenvolver serviços para um gerenciador de pedidos de restaurante com Spring Cloud envolve a criação de microserviços independentes, a configuração de um servidor de descoberta (Eureka), o monitoramento com Spring Boot Actuator e Spring Boot Admin, além da configuração de um gateway para roteamento de requisições. Este projeto não apenas demonstra a implementação prática de serviços distribuídos com Spring Cloud, mas também destaca a importância de uma arquitetura de microserviços bem estruturada para aplicações escaláveis e de fácil manutenção.
