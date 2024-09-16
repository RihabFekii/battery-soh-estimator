# Battery State of Health Estimator

This Python script simulates the estimation of the State of Health (SoH) of a battery controller by querying a FIWARE Context Broker. It calculates the degradation of the battery based on user-defined parameters such as charging behavior, discharging behavior, and temperature. The script runs for a total of 120 seconds, updating the SoH and related attributes in the Context Broker at each iteration.

**Main functionalities:**

- Fetches battery data from a Context Broker
- Calculates battery degradation based on charging, discharging, and temperature
- Updates the Context Broker with new SoH values
- Runs for a specified duration, updating SoH at regular intervals (with a degradation rate)

# Usage

## Cloning the porject

```bash
git clone https://github.com/RihabFekii/battery-soh-estimator.git
```
## Environment Variables

After cloning the repository, make sure to create a `.env`file at the root of your directory. 

The script uses the following environment variables:

- `ENTITY_ID`: The ID of the battery entity in the Context Broker
- `BROKER_URL`: The URL of the Context Broker
- `DEFAULT_DEGRADATION_RATE`: Default degradation rate (default: 0.5)
- `RUN_DURATION`: Total run duration in seconds (default: 120)

```shell
ENTITY_ID=urn:ngsi-ld:BatteryController:battery001
BROKER_URL=http://localhost:1026/ngsi-ld/v1/entities/
DEFAULT_DEGRADATION_RATE=0.5
RUN_DURATION=120
```
PS: Make sure to change the localhost with the Orion service container name as specified in the docker-compose file.  

# Docker environemt set up 

## Step 1: 
The first docker-compose file to fire up, runs an Orion Context Broker and a MongoDB. 

In your termial execute the following commands: 
  ```bash
    cd docker
    docker compose -f docker-compose.yaml up 
  ```

Ensure that the FIWARE Context Broker is running and accessible at http://localhost:1026.

# Context Broker configuration 

## Creating an entity 

When running the orion and mongodb containers, we need to create an entity in the context broker. 
To do that run the following command in your termninal: 

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
### Check the entity created
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
PS: Update the ENTITY_ID constant in the script if you wish to use a different entity.

## Step 1:

After creating the entity in the Orion Context Broker, it is possible now to run the second `docker-comnpose-python.yaml`file that builds the soh_estimator docker image called "battery_controller". 

Run the following command in your terminal: 

  ```bash
    cd docker
    docker compose -f docker-compose-python.yaml up 
  ``` 

Check the Log and if everything works well you should have the followint output: 

```
2024-09-16 16:25:11 Entity updated successfully.
````

