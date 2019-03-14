## Deploy Pizza Store App on PCF

#### Step 1: create a manifest.yml for deploying the app on to PCF. We need to bind this app to PCC and mysql instances created in Exericese 01.

```
---
applications:
- name: pizza-store-pcc-client
  random-route: true
  path: target/pizza-store-pcc-client-0.0.1-SNAPSHOT.jar
  services:
  - workshop-pcc
  - workshop-mysql
```

#### Step 2: Deploy the app on PCF

```
cf push
```

## Run the Pizza Store App

#### Setup the database

http://<app-url>/loaddb?amount=100

```
New customers successfully saved into Database
```

http://pizza-store-pcc-client.xyz.numerounocloud.com/showdb

```
First 10 customers are show here: 
Customer [id=0FsIMYG30, name=Brody England, email=england@gmail.com, address=59644 New York, birthday=1973-07-16T03:34:20.016Z]
Customer [id=2afB7G57Z, name=Ella Robbins, email=ella.robbins@mail.com, address=72508 San Francisco, birthday=1942-05-23T20:49:41.209Z]
Customer [id=2dO6dSUIs, name=Eva Fischer, email=fischer@mail.com, address=66604 New York, birthday=1949-05-29T22:01:01.136Z]
Customer [id=2mDAi0qRr, name=Gianna Merritt, email=merritt@yahoo.com, address=25734 Washington, birthday=1969-01-20T14:08:36.941Z]
....
```

#### Pizza Store APIs

http://<app-url>/pizzas

```
Lets Order Some Pizza 
-------------------------------
types: plain, fancy

GET /orderPizza?email={emailId}&type={pizzaType} - Order a pizza 
GET /orders?email={emailId} - get specific value 

```

#### Order a Pizza using `/orderPizza` API

http://<app-url>/orderPizza?email=XXX&type=fancy

Note: find a customer email address from `/showdb` API

http://pizza-store-pcc-client.xyz.numerounocloud.com/orderPizza?email=lucynorton@gmail.com&type=fancy

###### Result

Cache Miss Scenario

```
Result [Pizza{name='fancy', toppings=[arugula, chicken], sauce='pesto', Customer='Customer [id=05eKpgOFA, name=Lucy Norton, email=lucynorton@gmail.com, address=48665 Washington, birthday=1965-02-10T06:20:27.828Z]'}] 
Cache Miss for Customer [true] 
Read from [MYSQL] 
Elapsed Time [234 ms]
```

Data Returned From Cache 
```
Result [Pizza{name='fancy', toppings=[arugula, chicken], sauce='pesto', Customer='Customer [id=05eKpgOFA, name=Lucy Norton, email=lucynorton@gmail.com, address=48665 Washington, birthday=1965-02-10T06:20:27.828Z]'}] 
Cache Miss for Customer [false] 
Read from [PCC] 
Elapsed Time [2 ms]
```