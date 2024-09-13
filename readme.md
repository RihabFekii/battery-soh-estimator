# Battery State of Health Estimator

This Python script simulates the estimation of the State of Health (SoH) of a battery controller by querying a FIWARE Context Broker. It calculates the degradation of the battery based on user-defined parameters such as charging behavior, discharging behavior, and temperature. The script runs for a total of 120 seconds, updating the SoH and related attributes in the Context Broker at each iteration.

## Docker environemt set up 

This is a simple set up with an Orion Context Broker and a MongoDB. 

```bash
  cd docker
  docker-compose up 
```

# Usage

Ensure that the FIWARE Context Broker is running and accessible at http://localhost:1026.
Update the ENTITY_ID constant in the script if you wish to use a different entity.

Run the script:

```bash
python battery_soh_estimator.py
```

# Context Broker configuration 

## Creating an entity 
```shell
curl -iX POST 'http://localhost:1026/ngsi-ld/v1/entities/' \
-H 'Content-Type: application/json' \
--data-raw '{
      "id": "urn:ngsi-ld:BatteryController:battery001",
      "type": "BatteryController",
      "chargingBehaviour": {
            "type": "Property",
            "value": 10
      },
      "dischargingBehaviour": {
            "type": "Property",
            "value": 10
      },
      "temperature": {
            "type": "Property",
            "value": 25
      },
      "sohPercentage": {
            "type": "Property",
            "value": 100
      },
      "sohCurrentState": {
            "type": "Property",
            "value": "Very Good"
      },
      "sohTimeToCritical": {
            "type": "Property",
            "value": 120
      }
}'
```
## Check the entity created
Run this command in the terminal to GET the entities
```shell
curl -iX GET 'http://localhost:1026/ngsi-ld/v1/entities/urn:ngsi-ld:BatteryController:battery001' \
-H 'Content-Type: application/json'
```

You should have a response such as: 

```json
{
  "id": "urn:ngsi-ld:BatteryController:battery001",
  "type": "BatteryController",
  "chargingBehaviour": {
    "type": "Property",
    "value": 10
  },
  "dischargingBehaviour": {
    "type": "Property",
    "value": 10
  },
  "temperature": {
    "type": "Property",
    "value": 25
  },
  "sohPercentage": {
    "type": "Property",
    "value": 100
  },
  "sohCurrentState": {
    "type": "Property",
    "value": "Very Good"
  },
  "sohTimeToCritical": {
    "type": "Property",
    "value": 120
  }
}
```

