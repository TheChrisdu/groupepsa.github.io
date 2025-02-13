---
section: mobile-sdk
categorie: API Reference
layout: api-reference
version:
  available-since: 2.0.X
name: pims.vehicle.informations
subname: remote
domain: vehicle
type:
    - get
paramsget:
  - name: actionType
    required: true
    type: String
    description: Action name, in this case `remote`.
    unit-value: 
      - "remote: 'get vehicle information from server'"
    example: remote
  - name: action
    required: true
    type: String
    description: Name of the action to perform, in this case *Remote*.  Required only for remote service .
    unit-value: 
      - "get: 'Get vehicle information from server, if a network problem occurred, the last vehicle information from cache will be returned.'"
      - "refresh: 'Force the refresh of vehicle information from server, if: (1) the charge state is in progress or (2) the vehicle is plugged but disconnected ; a wake up remote will be send to the vehicle.'"
      - "refreshWithWakeUp: 'force the refresh information with send a wake up remote.'"
    example: refresh
dataget:
- ref: vehicle
  type: Object
  name: vehicle
  description: Vehicle informations.
data_example: |-
  {
      "resources": {
          "status": {
            "autonomy":145.0,
            "lastUpdate": "2022-02-14T13':'41':58Z",
            "fuelInformation": {
                "autonomy":145.0,
                "chargingInformation": {
                  "chargingMode":null,
                  "chargingRate":0.0,
                  "isPlugged":false,
                  "chargingState":null,
                  "remainingTime": "PT0S"
                },
                "chargingLevel":42.0,
                "lastUpdate": "2022-02-14T13':'41':57Z",
                "consumption":0.0,
                "type": "fuel"
            },
            "doorStateInformation": {
                "isRearRightDoorOpened":false,
                "isTrunkDoorOpened":false,
                "isLocked":true,
                "isRearWindowDoorOpened":false,
                "isPassengerDoorOpened":false,
                "isRearLeftDoorOpened":false,
                "state": [
                  "trunkLocked",
                  "locked"
                ],
                "isDriverDoorOpened":false
            },
            "precondInformation": {
                "lastUpdate": "2022-02-14T13':'41':57Z",
                "precondScheduling": [
                  {
                      "hour":1,
                      "isEnabled":false,
                      "occurrence": [
                        "mon"
                      ],
                      "slot":1,
                      "minute":48
                  }
                ],
                "status": "disabled"
            },
            "electricInformation": {
                "autonomy":0.0,
                "chargingInformation": {
                  "chargingMode": "no",
                  "chargingRate":0.0,
                  "isPlugged":false,
                  "nextDeferredChargingDate": "2022-02-14T18':'00':10.715Z",
                  "chargingState": "disconnected",
                  "remainingTime": "PT0S"
                },
                "chargingLevel":1.0,
                "lastUpdate": "2022-02-14T13':'41':57Z",
                "consumption":0.0,
                "type": "electric"
            },
            "vehicleType": "hybrid"
          }
      },
      "attributes": {
          "engine": "phev",
          "vin": "VR3ATTENTKY102274",
          "brand": "peugeot"
      }
    }
errorget:
  - code: 2101
  - code: 2102
  - code: 2000
  - code: 2201
  - code: 2202
  - code: 2203
  - code: 2204
  - code: 2301
  - code: 2302
short: This API allows to retrieve Remote vehicle informations.
component:
  - LongRangeRemote
---