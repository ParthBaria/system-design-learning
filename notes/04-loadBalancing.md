# load balancing

it's  system of maintaining user requestes for mutiple server.

there are two types:
1.HardWare: it uses physical hardware to distribute traffic.
          - layer 4,7 it handles all kind of traffic from http to tcp

2.software: it uses software to distribute traffice.
          - nginx,haproxy,aws elb

# Algorithams

there are two types static and dynamic

## Round Robin

it maintain index of server which has been used to send respose then used another server instead of that one.
it works in sequntional manner.

## weighted Round robin

it takes weight and distribute traffic based on weight where most weighted server has most requests to handle.

## Ip hash

it hashes the source id of client to send request to specific one serve for that partiular client.it maintains caches for per user.
it needs conssitenat hashing.

## least connections

the requests are send to least active server.

# benfits

1. High Availability
2. scalbility


