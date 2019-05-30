# Spring-Zull
Banyaknya Microservices yang ada membutuhkan suatu pintu atau entrypoint untuk seluruh service yang ada, agar dari sisi client hanya mengacu ke 1 service. Maka dari itu muncul lah Spring Zull. Spring Zull sebenarnya merupa microservice yang mendaftarkan dirinya sendiri ke Eureka lalu mengambil seluruh service yang terdaftar di dalam eurika tersebut dan membut reverse proxy, yang memungkinakan kita untuk memanggil semua service itu lewat zull

# Dependencies
Zull</br>
Eureka Discovery</br>
Actuator</br>
Config Client</br>

# How to
1. Tambahkan @EnableZuulProxy pada SpringBootApplicationClass, dengan konfigurasi ini menandakan service ini berpran sebagai proxy atau API Gateway. Setiap Request dan Respon yang masuk dan keluar dapat di intrupt dengan mengunakan Filter 
```
@EnableZuulProxy
public class SpringZullApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringZullApplication.class, args);
	}

	@Bean
	public PreFilter preFilter() {
		return new PreFilter();
	}

	@Bean
	public PostFilter postFilter() {
		return new PostFilter();
	}

	@Bean
	public ErrorFilter errorFilter() {
		return new ErrorFilter();
	}

	@Bean
	public RouteFilter routeFilter() {
		return new RouteFilter();
	}
}
```
```
public class ErrorFilter extends ZuulFilter {

	@Override
	public String filterType() {
		return "error";
	}

	@Override
	public int filterOrder() {
		return 0;
	}

	@Override
	public boolean shouldFilter() {
		return true;
	}

	@Override
	public Object run() {
		System.out.println("Using Route Filter");

		return null;
	}

}
```

2. Agar Zull ini dapat mengambil semua informasi service yang terdapat pada eureka, maka kita harus memberi property
```
---
eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:9083/eureka
    registerWithEureka: true  
    fetchRegistry: true      
server:
  port: 9090
```
```registerWithEureka dan fetchRegistry``. Memungkinkan untuk melakukan registrasi para eureka dan menarik semua service informasi yang terdapat pada server eureka tersebut.
