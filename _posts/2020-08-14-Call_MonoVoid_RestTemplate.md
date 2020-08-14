---
title: WebFlux Service and RestTemplate
author: Shilan Kalhor
date: 2020-08-14 15:34:00 +0800
categories: [Coding, Springboot, RestTemplate, WebClient]
tags: [Java, SpringBoot, RestTemplate, WebClient]
toc: false
---

# Calling a service that returns `Mono<Void>` with RestTemplate
I had a rest service that was implemented by Spring WebFlux. Was a POST that returned Mono<Void>.
The client however was still using RestTemplate.
Calling it with postForEntity(...) or exchange(...) method worked at first glance but then I realized there is a problem!
The POST was doing it's job however when cliented attempted to log the actual message set in the body in case of errors, let's say HttpStatus error 400), the body was Null!
	However all the response headers including the HttpStatus and description were there.
Checking everythin on service and debugging RestTemplate code, I found out the header content-length was 0! Checking it with ARC and Postman, the length was something like 240.
	Debugging further in RestTemplate, found out here there is a check for `if (getHeaders().getContentLength() == 0) ` and if the Content-length is zero which is set to 0 by RestTemplate for Mono<Void> returning services, the hasMessagingBody with be set to false.
	Here is the RestTemplate implementation:
	
```
/**
 * Indicates whether the response has a message body.
 * <p>Implementation returns {@code false} for:
 * <ul>
 * <li>a response status of {@code 1XX}, {@code 204} or {@code 304}</li>
 * <li>a {@code Content-Length} header of {@code 0}</li>
 * </ul>
 * @return {@code true} if the response has a message body, {@code false} otherwise
 * @throws IOException in case of I/O errors
 */
public boolean hasMessageBody() throws IOException {
	HttpStatus status = HttpStatus.resolve(getRawStatusCode());
	if (status != null && (status.is1xxInformational() || status == HttpStatus.NO_CONTENT ||
			status == HttpStatus.NOT_MODIFIED)) {
		return false;
	}
	if (getHeaders().getContentLength() == 0) {
		return false;
	}
	return true;
}
```
# Solution
Potentially there is a bug in RestTemplate that sets the content-length to 0. In my world there are two solutions to this:
I had two solutions for it:

## Change the Service
If you can, you may change the service to return a none-webflux Object. This is easy by using `.block()` method before returning from service method.
## Use WebClient
If just like our case, changing the rest service is either impossible or expensive, try to use `WebClient` instead of `RestTemplate`.
The new Spring WebClient is much cleaner and in my opinion has better options for setting the uri and authorization token.
Note that you don't need to use Reactive in your entire project to just use WebClient, you just need to add few dependencies and you are good to go:

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-webflux</artifactId>
</dependency>
<dependency>
    <groupId>io.projectreactor</groupId>
    <artifactId>reactor-core</artifactId>
    <version>3.3.4.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.projectreactor</groupId>
    <artifactId>reactor-spring</artifactId>
    <version>1.0.1.RELEASE</version>
</dependency>
```
So just go ahead and don't be afraid of using WebClient in your application, you can gradually replace all the RestTemplate usages with WebClient in your project.
Though there is no *Rush* migrating to WebClient but you'd better know that in future releases the RestTemplate will get deprecated:
[RestTemplate Deprecating](https://github.com/spring-guides/gs-consuming-rest/issues/28)
