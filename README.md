# logger-api
Logger API

*API version 1.0*

## **Overview**

The Logger Deep Link API enables external applications to import flight data into Logger using deep links formatted with URL schemes and JSON payloads. This API is designed for seamless integration, enabling the addition of comprehensive flight details with minimal user interaction. This document outlines the structure, requirements, and usage of these deep links.

for support contact support@getlogger.com

## **Deep Link Format**

The deep link for importing flight data into LoggerApp follows this format:

`loggerapp://import/add?package=<JSON_DATA>`

Here, **`<JSON_DATA>`** represents the URL-encoded JSON string containing the flight data entries and metadata.

## **JSON Data Structure**

The JSON data sent through the deep link should be structured as follows:

**Top-Level Structure**

```jsx
{
  "entries": [/*Array of Entry Objects*/],
  "metadata": {
    "appName": "String",
    "appVersion": "String",
    "isUTC": Boolean
  }
}
```

## **Metadata Object**

Metadata object needs to be provided together with valid (non-nil) values in order to parse the JSON.

The **`metadata`** object contains:

- **`appName`**: <String> The name of the application sending the data.
- **`appVersion`**: <String> The version of the application sending the data.
- **`isUTC`**: <Bool> A boolean indicating whether the **time values** and **dates** are in UTC.

*Example:*

```jsx
"metadata": {
    "appName": "My App",
    "appVersion": "1.2.1",
    "isUTC": true
  }
```

## **Entry Object**

Each entry in the **`entries`** array should follow this structure:

```jsx
"entries":[{
  "entryType": "String",
  "departureDate": "YYYY-MM-DD"
// Other properties as decribed in documentation below
},
{
  "entryType": "String",
  "departureDate": "YYYY-MM-DD"
// Other properties as decribed in documentation below
}]
```

As a minimum `**entryType`** and `**departureDate**` need to be provided. Other properties are optional.

## **Core Flight Information**

- These properties with valid values need to be provided in order to parse the JSON

### **`entryType`**  <String>

- “Flight”
- “Dead Head” (currently not supported)
- “Simulator” (currently not supported)
- “Airport Reserve” (currently not supported)
- “Home Reserve” (currently not supported)
- “Home Standby” (currently not supported)
- “Airport Standby” (currently not supported)

### **`departureDate` <String>**

- The date of departure (yyyy-MM-dd format), e.g. 2024-01-22
- If metadata `isUTC` is set to `true` , provide the date for **UTC time zone**
- If metadata `isUTC` is set to `false` , provide the date for **departure time zone**

## **Time Details**

- These properties contain departure and arrival times in **HHmm** or **HH:mm** format
- If metadata `isUTC` is set to `true` , provide times in **UTC**
- If metadata `isUTC` is set to `false` , provide **local times**

### **`departureTime` <String>**

- Actual departure time (off block)

### **`arrivalTime` <String>**

- Actual arrival time (on block)

### **`takeOffTime` <String>**

- Actual take-off time

### **`landingTime` <String>**

- Actual landing time

### **`onDuty` <String>**

- The start time of duty

### `offDuty` <String>

- The end time of duty

### `scheduledDepartureTime` <String>

- The scheduled time of departure

### `scheduledArrivalTime` <String>

- The scheduled time of arrival

## **Airport Details**

### `departureAirport` <String>

- IATA three-letter code for departure airport, or
- ICAO four-letter code for the departure airport (e.g. JFK or KJFK)

### `arrivalAirport` <String>

- IATA three-letter code for arrival airport, or
- ICAO four-letter code for the arrival airport (e.g. JFK or KJFK)

## **Aircraft and Crew Details**

### `aircraftType` <String>

- The type of aircraft used
- As standard practice, provide ICAO four-letter code (e.g. B738, PA28)

### `aircraftRegistration` <String>

- The registration number of the aircraft (tail number)

### `picName` <String>

- The name of the Pilot in Command (PIC).

### `sicName` <String>

- The name of the Second in Command (SIC).

### `pilotFlying` <Bool>

- A boolean indicating if the entry corresponds to the pilot flying the aircraft.

### `licenseHolderLicenseNumber` <String>

- The license number of the license holder.

### `licenseHolderName` <String>

- The name of the license holder.

## **Function Time Details**

- These properties contain time interval information in **HH:mm** or **H:mm** format

### `picTime` <String>

- The time logged as Pilot in Command

### `ifrTime` <String>

- The time flown under Instrument Flight Rules

### `crossCountryTime` <String>

- The time flown over cross-country flights

### `multiCrewTime` <String>

- The time logged in multi-crew operations

### `singleEngineTime` <String>

- The time flown in single-engine aircraft

### `multiEngineTime` <String>

- The time flown in multi-engine aircraft

### `instructorTime` <String>

- The time logged as an instructor

### `dualTime` <String>

- The time logged in dual received (student pilot)

### `sicTime` <String>

- The time logged as Second in Command

### `gliderTime` <String>

- The time flown in a glider

### `simTime` <String>

- The time spent in a simulator

### `pcatdTime` <String>

- The time spent in a PC-based aviation training device

### `rotorcraftTime` <String>

- The time flown in a rotorcraft

### `simulatedInstrumentTime` <String>

- The time spent under simulated instrument conditions

### `nightVisionTime` <String>

- The time flown using night vision goggles.

## Take-off and Landing

### `numberOfDayLandings` <Int>

- The number of landings during daylight.

### `numberOfNightLandings` <Int>

- The number of landings at night.

### `numberOfDayTakeOffs` <Int>

- The number of takeoffs during daylight.

### `numberOfNightTakeOffs` <Int>

- The number of takeoffs at night.

### `bolters` <Int>

- The number of missed carrier landings.

### `catapults` <Int>

- The number of carrier takeoffs.

### `arrests` <Int>

- The number of successful carrier landings.

### `flightCarrierLanding` <Int>

- The total number of carrier landings.

### `waterTakeOff` <Int>

- The number of takeoffs from water.

### `waterLanding` <Int>

- The number of landings on water.

## Operational Values and Measurements

### `flightDistance` <Double>

- The distance flown during the flight, in **nautical miles (NM)**

### `fuelBurned` <Int>

- The amount of fuel burned during the flight, in kgs, litres or gallons.

### `hobbsIn` <Double>

- The Hobbs meter reading at the start of the flight.

### `hobbsOut` <Double>

- The Hobbs meter reading at the end of the flight.

### `tachIn` <Double>

- The tachometer reading at the start of the flight.

### `tachOut` <Double>

- The tachometer reading at the end of the flight.

## Flight Rules

### `numberOfApproaches` <Int>

- The number of approaches performed during the flight.

### `numberOfHolds` <Int>

- The number of holding patterns.

### `approachType` <String>

- The type of approach performed
- Allowed values:
    - "ILS CAT I"
    - "ILS CAT II"
    - "ILS CAT III"
    - "LOC"
    - "VOR"
    - "NDB"
    - "RNP"
    - "GLS"
    - "SLS"
    - "MLS"
    - "PAR"
    - "SRA"
    - "Visual"
    - "Circling"
    - "Monitored"

### `flightRules` <String>

- The flight rules under which the flight was conducted, e.g., "VFR" (Visual Flight Rules) or "IFR" (Instrument Flight Rules).

## Additional Information

### `remark` <String>

- Any additional remarks or notes about the flight.

### `isFavorite` <Bool>

- A boolean indicating whether the flight is marked as a favorite.
- When `true` the entered flight will be favorite in Logger

### `isLocked` <Bool>

- A boolean indicating whether the flight entry is locked for editing.
- When `true` the entered flight will be imported as locked for editing in Logger

## Example Usage

Example of multiple flights with minimum required Data with times and dates in local time zone:

```jsx
loggerapp://import/add?package={
    "entries": [
        {
            "entryType": "Flight",
            "departureDate": "2024-01-01"
        },
				{
            "entryType": "Flight",
            "departureDate": "2024-01-02"
        },
        {
            "entryType": "Flight",
            "departureDate": "2024-01-25"
        }
    ],
    "metadata": {
        "appName": "Your Third Party App",
        "appVersion": "1.0.0",
        "isUTC": false
    }
}
```

Example of single flight with optional data and times and dates in UTC

```jsx
loggerapp://import/add?package={
    "entries": [
        {
            "entryType": "Flight",
            "departureDate": "2024-01-01",
            "departureTime": "0200",
            "takeOffTime": "08:00",
            "landingTime": "10:00",
            "arrivalTime": "1200",
            "departureAirport": "SYD",
            "arrivalAirport": "LAX",
            "numberOfDayLandings": 1,
            "numberOfNightLandings": 0,
            "aircraftType": "A320",
            "aircraftRegistration": "N123AB",
            "picName": "John Doe",
            "sicName": "Jane Doe",
            "remark": "Smooth flight",
            "pilotFlying": true,
            "picTime": "2:00",
            "ifrTime": "02:00",
            "nightTime": "02:00",
            "instructorTime": "02:10",
            "dualTime": "01:10",
            "sicTime": "01:00",
            "approachType": "ILS CAT I",
            "flightRules": "IFR",
            "crossCountryTime": "02:00",
            "numberOfDayTakeOffs": 1,
            "numberOfNightTakeOffs": 0
        }
    ],
    "metadata": {
        "appName": "Your Third Party App",
        "appVersion": "1.0.0",
        "isUTC": false
    }
}
```
