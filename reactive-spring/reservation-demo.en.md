# Reactive MVC
In this lab we will learn to run a Spring MVC project. In Part 2 we will run the reactive version of it.

## Download the reservation-demo project

1. Git Clone or download reservation-demo repo at <https://github.com/Pivotal-Field-Engineering/reservation-demo>
1. Open all the projects in your favorite IDE

1. Open a terminal in the `reservation-service` directory of the project

1. This is the server that the `reservation-ui` calls. Run the server
```
spring-boot:run
```
1. Let's save some data to the server using curl commands
```
curl --request POST \
  --url http://localhost:8080/reservations \
  --header 'content-type: application/json' \
  --data '[{
	"id": null,
	"name": "Milan",
	"status": "booked"
}]
'
```
1. Confirm data is saved
```
curl --request GET \
  --url http://localhost:8080/reservations
```
1. Open a terminal in the `reservation-ui` project. Run the UI
```
$ spring-boot:run
```
1. Open a browser <http://localhost:8081> to see the UI.
1. Do some more bookings and refresh the UI to see the bookings.
1. Stop both the server and ui process by `Ctrl+c` in the terminal windows.

1. Open a terminal in the `reactive-reservation-service` directory. Run the service
```
spring-boot:run
```
1. Let's save some data to the server using curl commands
```
curl --request POST \
  --url http://localhost:8080/reservations \
  --header 'content-type: application/json' \
  --data '[{
	"id": null,
	"name": "New Delhi",
	"status": "booked"
}]
'
```
1. Confirm data is saved
```
curl --request GET \
  --url http://localhost:8080/reservations
```
1. Notice the type of the Id is different.
1. Let's stream some data! Run the following command to see simulated events for one of the reservations. Install HTTPie <https://httpie.org/> if you don't have it. Copy one of the values from the `id` field and run
```
http --stream --json http://localhost:8080/reservations/id/events
```
1. Notice the events are being pushed from the server
1. Open a terminal in the `reservation-ui` project. Run the UI
```
$ spring-boot:run
```
1. Open a browser <http://localhost:8081> to see the UI.
1. Notice there are no values. See the stacktrace in the terminal of the 'reservation-ui' and see if you can see how to fix it. Hint: The type of the id is different.

## Related Content
1. [Josh Long SpringOne talk](https://youtu.be/l7VBdWhtl7A)
1. [Another Josh Long SpringOne talk](https://youtu.be/1W5_tOiwEAc)
