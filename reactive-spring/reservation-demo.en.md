# Reactive MVC Part 1
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
