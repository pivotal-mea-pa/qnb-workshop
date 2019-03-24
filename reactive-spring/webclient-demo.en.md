# WebClient Demo
WebClient is a non-blocking, reactive client to perform HTTP requests. It exposes a fluent, reactive API over underlying HTTP client libraries such as Reactor Netty.

It is useful to understand how the WebClient works in order to better understand the other Reactive APIs.

## Download and build the reactive-for-webmvc project

1. Git Clone or download reactive-for-webmvc repo at <https://github.com/Pivotal-Field-Engineering/reactive-for-webmvc>
1. Open a terminal in the `remote-service` directory of the project

1. This is the backend server that the client calls. Run the server
```
$ spring-boot:run
```
1. Open the project in your favorite IDE
1. In the `main-app` project, open the files in the `demo.client` package called `Step1a.java`, `Step2a.java` etc. for examples of how to transistion to use WebClient. Run each of the files to see how the WebClient works as we start to use the API.
1. Open the `remote-service` project to see how to Server is implemented using the Fluent API.

## Related Content
1. [Rossen Stoyanchev SpringOne talk](https://youtu.be/IZ2SoXUiS7M) on the same topic
