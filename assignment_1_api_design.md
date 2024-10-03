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
