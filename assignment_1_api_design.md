# Flight Booking API Documentation

## Overview

The Flight Booking API allows users to search for flights with criterias, book tickets, booking history. This document outlines the available endpoints, request/response formats, and error codes.

## Base URL

```
https://api.flightbooking.com/v1
```

## Endpoints

### 1. Search Flights

Search for available flights based on criteria

- **URL**: `/flights/search`
- **Method**: `GET`
- **Description**: Search for flights based on criteria such as origin, destination, departure date, return date, one-way/two-way.

#### Header

| Key          | Value              |
| ------------ | ------------------ |
| Content-Type | `application/json` |

#### Request Parameters

| Parameter     | Type    | Description                                           | Default | Required |
| ------------- | ------- | ----------------------------------------------------- | ------- | -------- |
| `origin`      | string  | Departure airport code                                |         | Yes      |
| `destination` | string  | Destination airport code                              |         | Yes      |
| `departure`   | string  | Departure date (YYYY-MM-DD)                           |         | Yes      |
| `return`      | string  | Return date (YYYY-MM-DD)                              |         | No       |
| `passengers`  | int     | Number of passengers                                  | 1       | Yes      |
| `one_way`     | boolean | Is one way only                                       | Yes     | Yes      |
| `class`       | string  | Travel class (`BUSINESS`, `SKYBOSS`, `DELUXE`, `ECO`) |         | No       |

#### Response

| Parameter  | Type   | Description                              | Required |
| ---------- | ------ | ---------------------------------------- | -------- |
| `status`   | string | Response status (success, fail)          | Yes      |
| `total`    | int    | Number of available items                | Yes      |
| `pageSize` | int    | Biggest number of items in response data | Yes      |
| `data`     | array  | Array of flight information              | Yes      |

#### Example

##### Example Request

```
curl -X GET "https://api.flightbooker.com/flights/search?origin=LAX&destination=JFK&departure=2023-11-10&return=2023-11-20&passengers=1&class=BUSINESS" \
-H "Content-Type: application/json"
```

##### Example Response

```
{
    "status": "success",
    "total": ,
    "pageSize": ,
    "data": [
        {
            "flight_id": "FL1234",
            "airline": "VietJet",
            "departure_time": "2024-10-03T10:00:00",
            "arrival_time": "2024-10-03T12:00:00",
            "duration": "2h",
            "aircraft_detail": {
                "manufacturer": "Boeing",
                "model": "MH370",
                "seet_number": 500
            },
            "available_seets": [
                {
                    "class": "BUSINESS",
                    "seet_numbers": 10,
                    "price": "1000000"
                },
                ...
            ]
        },
        ...
    ]
}
```

#### Errors

| Code | Message                                            | Description                            |
| ---- | -------------------------------------------------- | -------------------------------------- |
| 400  | `Invalid Parameters`                               | Missing or incorrect search parameters |
| 404  | `No Flights Available`                             | No flights match the search criteria   |
| 500  | `Internal Error, Please Contact Technical Support` | Internal error                         |

### 2. Booking Flight

Book flights

- **URL**: `/orders`
- **Method**: `POSt`
- **Description**: Book flights with passengers information and seat information.

#### Header

| Key          | Value              |
| ------------ | ------------------ |
| Content-Type | `application/json` |

#### Request Parameters

| Parameter       | Type   | Description                                                | Default | Required |
| --------------- | ------ | ---------------------------------------------------------- | ------- | -------- |
| `passengers`    | array  | Passengers information                                     |         | Yes      |
| `seats`         | array  | Booking seat information                                   |         | Yes      |
| `total_payment` | string | Total payment to verfiry with the backend seat information |         | Yes      |

#### Response

| Parameter        | Type     | Description                                                                                    | Required |
| ---------------- | -------- | ---------------------------------------------------------------------------------------------- | -------- |
| `status`         | string   | Response status (`success`, `error`)                                                           | Yes      |
| `data`           | json     | Order detail in json format                                                                    | Yes      |
| `order_id`       | int      | Order detail                                                                                   | Yes      |
| `order_status`   | string   | Order status (`ORDER_PROCESSING`, `WAIT_FOR_PAYMENT`, `TICKET_PROCESSING`, `TICKET_DELIVERED`) | Yes      |
| `created_date`   | datetime | Order created date (UTC format)                                                                | Yes      |
| `updated_date`   | datetime | Order updated date (UTC format)                                                                | Yes      |
| `customer`       | json     | Customer information                                                                           | Yes      |
| `payment`        | json     | Payment information (empty if there is no payment received)                                    | Yes      |
| `total`          | string   | Total amount of received payment                                                               | Yes      |
| `payment_items`  | string   | Total amount of received payment                                                               | Yes      |
| `payment_id`     | int      | Payment information                                                                            | Yes      |
| `payment_method` | string   | Payment method (`CASH`, `CARD`, `TRANSFER`)                                                    | Yes      |
| `payment_amount` | string   | Payment amount                                                                                 | Yes      |
| `tickets`        | array    | Created tickets (empty if no ticket created)                                                   | Yes      |
| `ticket_id`      | int      | Ticket id                                                                                      | Yes      |
| `flight_id`      | int      | Flight id                                                                                      | Yes      |
| `seat_number`    | int      | Seat number                                                                                    | Yes      |

#### Example

##### Example Request

```
curl -X POST 'https://api.flightbooker.com/flights' \
--header 'Content-Type: application/json' \
--data '
{
    "passengers": [
        {
            "name": "Nguyen Van A",
            "age": 25,
            "phone": "+84 099999999",
            "document_id": "089000000000"
        }
    ],
    "seats": [
        {
            "flight_id": 111,
            "seat_number": 12
        }
    ],
    "total_payment": "1000000"
}'
```

##### Example Response

```
{
    "status": "success",
    "data": {
        "order_id": 1234,
        "order_status": "TICKET_DELIVERED",
        "created_date": "20241001T083000",
        "updated_date": "20241001T093000",
        "customer": {
            "id": 123141,
            "name": "Nguyen Van A",
            "phone": "+84 099999999"
        },
        "payment": {
            "total": "1000000",
            "payment_items": [
                {
                    "payment_id": 1234234,
                    "payment_method": "CARD",
                    "payment_amount": "1000000"
                }
            ]
        },
        "tickets": [
            {
                "ticket_id": 3242134,
                "flight_id": 111
                "seat_number": 12
            }
        ]
    }
}
```

#### Errors

| Code | Message                                            | Description                                                                     |
| ---- | -------------------------------------------------- | ------------------------------------------------------------------------------- |
| 400  | `Invalid Parameters`                               | Missing or incorrect search parameters                                          |
| 404  | `Requested Seats Are Not Availabled`               | Selected seat might be booked                                                   |
| 404  | `Not Match Total Amount With Booking Seats`        | The price of booking seat might changed required to double check and book again |
| 500  | `Internal Error, Please Contact Technical Support` | Internal error                                                                  |

