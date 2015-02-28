Sofia Public Transport API
==========================================

## Description
This is an unofficial documentation for the RESTful Web API hosted by the [Sofia Urban Mobility Centre](http://www.sofiatraffic.bg/). It provides information about routes of the metro, buses, trams, trolleys, their stops, and times of arrival.

This API is currently known to be used in the [SofiaTraffic](https://play.google.com/store/apps/details?id=com.sofiatraffic.android) Android app, developed by the Sofia Urban Mobility Centre.

Documenting the API would hopefully allow developers to build better third-party apps.

## API

**Base URL**: `http://drone.sumc.bg/api`

### POST **/v1/timing**
Returns a list of lines and their arrival times for a given stop. For metro stations, use the `/v1/metro/times` endpoint documented below.

Required parameter:

- `stopCode` — 4-digit stop code, as seen on information plates. Leading zeroes can be omitted.

Example request:
```json
{ "stopCode": 1338 }
```

Example response:
```json
[
    {
        "timing": "13:07:20,",
        "routeId": 1904,
        "type": 1,
        "lineId": 76,
        "lineName": "14"
    },
    {
        "timing": "13:40:49,12:46:34,",
        "routeId": 1842,
        "type": 1,
        "lineId": 38,
        "lineName": "12"
    }
]
```



### POST **/v1/routes/changes**
Returns a list of routes. Each route has (among other properties) a type, a name, and a list of points on the map, including where public transport vehicles go, locations of stops, their names, and locations.

Optional parameter:

 - `from_date` — UNIX timestamp. Only changes to routes made since this point of time will be returned. If this parameter is not sent, or set to `0`, all routes will be returned.

Example request:
```json
{ "from_date": 1425073908 }
```

Example response:
```json
{
    "new": [],
    "updated": [
        {
            "routes_id": 3,
            "type": 2,
            "route_name": "Дружба-2 - Хаджи Димитър      ",
            "line_id": 1,
            "id": 4,
            "line_name": "4",
            "points": [
                {
                    "stop": 1920,
                    "stopCode": 611,
                    "StopName": "Ж.к. Дружба 2",
                    "lat": 42.64,
                    "lon": 23.415833333333,
                    "vehicleType": 2
                },
                {
                    "lat": 42.64,
                    "lon": 23.415555555556
                },
                {
                    "lat": 42.64,
                    "lon": 23.413888888889
                },
                {
                    "stop": 2256,
                    "stopCode": 1930,
                    "StopName": "Ул. Димитър Пешев",
                    "lat": 42.640277777778,
                    "lon": 23.412777777778,
                    "vehicleType": 2
                },
                {
                    "lat": 42.640277777778,
                    "lon": 23.412222222222
                }
            ]
        }
    ]
}
```
**Note**: The example response above has been truncated. Actual response is much longer.


### GET **/v1/metro/all**
Returns a list of all metro stations. Response includes their names, stop codes, and locations, among other properties.

Example response:
```json
[
	{
        "id": 5,
        "route_id": 1,
        "code": 2985,
        "point_id": 105,
        "name": "Лъвов мост",
        "latitude": 42.705369,
        "longitude": 23.323891
    },
    {
        "id": 19,
        "route_id": 1,
        "code": 3013,
        "point_id": 119,
        "name": "Сердика",
        "latitude": 42.697765,
        "longitude": 23.321512
    }
]
```
**Note**: The example response above has been truncated.


### GET **/v1/metro/times/:stationID**
Returns times of arrival of trains in a metro station. Requires metro station ID (use the `/v1/metro/all` endpoint to obtain it).

Example request:

`GET http://drone.sumc.bg/api/v1/metro/times/19`

Example response:
```json
{
    "route_1": "10:02,10:07,10:09,10:15",
    "route_2": "09:55,09:59,10:01,10:05",
    "route_1_name": "Джеймс Баучер - Цариградско шосе",
    "route_2_name": "Цариградско шосе - Джеймс Баучер"
}
```
**Note**: The example response above has been truncated.


### GET **/v1/config**
Returns configuration options. Purpose is currently unknown.

Example response:
```json
{
    "key": "test",
    "date": "2015-02-28 12:35",
    "range": 800
}
```
