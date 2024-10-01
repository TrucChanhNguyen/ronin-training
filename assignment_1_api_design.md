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

#### Request Parameters
