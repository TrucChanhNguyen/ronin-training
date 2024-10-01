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

### Header
| Key          | Value              |
| ------------ | ------------------ |
| Content-Type | `application/json` |

#### Request Parameters
| Parameter     | Type    | Description                                   | Required |
| ------------- | ------- | --------------------------------------------- | -------- |
| `origin`      | string  | Departure airport code                        | Yes      |
| `destination` | string  | Destination airport code                      | Yes      |
| `departure`   | string  | Departure date (YYYY-MM-DD)                   | Yes      |
| `return`      | string  | Return date (YYYY-MM-DD)                      | No       |
| `passengers`  | int     | Number of passengers                          | Yes      |
| `one_way`     | boolean | Is one way only                               | Yes      |
| `class`       | string  | Travel class (BUSINESS, SKYBOSS, DELUXE, ECO) | No       |


