# GraphQL NDC API

This repository provides the information to access and use a prototype GraphQL NDC API, based on the IATA Kronos NDC API. The aim is to demonstrate what an NDC API can look like based on GraphQL.

**Functionalities are limited compared to Kronos.**

**This is a *beta*: changes are to be expected, as well as bugs.**

This API is available (without any API key for now) here (with a GUI): [http://gql.airtech.zone:8000](http://gql.airtech.zone:8000).

Thanks to GraphQL possibilities, the documentation is built-in.

---
Below are examples of the available functionalities.

## Shopping
Shopping for flights based on origin, destination, date and passengers (adult/child). Only simple offers are returned (no à-la-carte).
[Example](http://gql.airtech.zone:8000/?query=query%20shop(%24RQ%3A%20ShoppingRequest!)%20%7B%0A%20%20shop(request%3A%20%24RQ)%20%7B%0A%20%20%20%20ResponseID%0A%20%20%20%20Offer(first%3A%202)%20%7B%0A%20%20%20%20%20%20OfferID%0A%20%20%20%20%20%20OfferItem%20%7B%0A%20%20%20%20%20%20%20%20OfferItemID%0A%20%20%20%20%20%20%20%20Price%0A%20%20%20%20%20%20%20%20FlightService%20%7B%0A%20%20%20%20%20%20%20%20%20%20ServiceID%0A%20%20%20%20%20%20%20%20%20%20PassengerRef%0A%20%20%20%20%20%20%20%20%20%20FlightSegment%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20FlightNumber%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20Service%20%7B%0A%20%20%20%20%20%20%20%20%20%20Name%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&operationName=shop&variables=%7B%0A%20%20%22RQ%22%3A%20%7B%0A%20%20%20%20%22DepartureAirportCode%22%3A%20%22DXB%22%2C%0A%20%20%20%20%22DepartureDate%22%3A%20%222020-12-23%22%2C%0A%20%20%20%20%22ArrivalAirportCode%22%3A%20%22LHR%22%2C%0A%20%20%20%20%22Passenger%22%3A%20%5B%0A%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%22PassengerID%22%3A%20%22PAX1%22%2C%0A%20%20%20%20%20%20%20%20%22PTC%22%3A%20%22ADT%22%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%5D%0A%20%20%7D%7D)

## Booking
Booking a flight based on the offers from the shopping step.
[Example](http://gql.airtech.zone:8000/?query=mutation%20book(%24RQ%3A%20BookingRequest!)%20%7B%0A%20%20book(request%3A%20%24RQ)%20%7B%0A%20%20%20%20OrderID%0A%20%20%20%20Owner%0A%20%20%20%20TotalPrice%0A%20%20%20%20OrderItem%20%7B%0A%20%20%20%20%20%20OrderItemID%0A%20%20%20%20%20%20BasePrice%0A%20%20%20%20%20%20Taxes%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&operationName=book&variables=%7B%0A%20%20%22RQ%22%3A%20%7B%0A%20%20%20%20%22Passenger%22%3A%20%5B%0A%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%22PassengerID%22%3A%20%22PAX1%22%2C%0A%20%20%20%20%20%20%20%20%22PTC%22%3A%20%22ADT%22%2C%0A%20%20%20%20%20%20%20%20%22GivenName%22%3A%20%22Billy%22%2C%0A%20%20%20%20%20%20%20%20%22Surname%22%3A%20%22Bob%22%2C%0A%20%20%20%20%20%20%20%20%22EmailAddressValue%22%3A%20%22billy%40bob.com%22%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%5D%2C%0A%20%20%20%20%22Offer%22%3A%20%5B%0A%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%22ResponseID%22%3A%20%22201-e40e484515d1474c93a92ea9677a49ed%22%2C%0A%20%20%20%20%20%20%20%20%22Owner%22%3A%20%22C9%22%2C%0A%20%20%20%20%20%20%20%20%22OfferID%22%3A%20%22OFFER1%22%2C%0A%20%20%20%20%20%20%20%20%22OfferItem%22%3A%20%5B%0A%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%22OfferItemID%22%3A%20%22OFFERITEM1_1%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22PassengerIDs%22%3A%20%22PAX1%22%0A%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%5D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%5D%0A%20%20%7D%0A%7D)

## Retrieving an order
Retrieving some of the details of an order (created at the booking step) based on its OrderID.
[Example](http://gql.airtech.zone:8000/?query=query%20retrieveOrder(%24RQ%3A%20OrderRetrieval!)%20%7B%0A%20%20retrieveOrder(request%3A%20%24RQ)%20%7B%0A%20%20%20%20OrderID%0A%20%20%20%20Owner%0A%20%20%20%20TotalPrice%0A%20%20%7D%0A%7D&operationName=retrieveOrder&variables=%7B%0A%20%20%22RQ%22%3A%20%7B%0A%20%20%20%20%22OrderID%22%3A%20%22B12GW5%22%0A%20%20%7D%0A%7D)

## Cancelling an order
Cancelling an existing order (created at the booking step) based on its OrderID.
[Example](http://gql.airtech.zone:8000/?query=mutation%20cancel(%24RQ%3A%20CancellationRequest!)%20%7B%0A%20%20cancel(request%3A%20%24RQ)%20%7B%0A%20%20%20%20Success%0A%20%20%7D%0A%7D&operationName=cancel&variables=%7B%0A%20%20%22RQ%22%3A%20%7B%0A%20%20%20%20%22OrderID%22%3A%20%22B12GW5%22%2C%0A%20%20%20%20%22Owner%22%3A%20%22C9%22%0A%20%20%7D%0A%7D)