# 20240724

## SUMO

### Dependencies
The required .xml files could be found in the `<input>`:
```
<input>
        <net-file value="data/osm.net.xml.gz"/>
        <route-files value="data/osm.passenger.trips.xml"/>
        <additional-files value="data/osm.poly.xml.gz"/>
</input>
```
Additionally, the default gui could be found in:
```
<gui_only>
        <gui-settings-file value="data/osm.view.xml"/>
</gui_only>
```

### FCD files
Rather than running `sumo -c osm.sumocfg --fcd-output fcd.xml`, we could directly add `<fcd-output>` in `<output>`:
```
<output>
        <fcd-output value="newFCD.xml"/>
</output>
```

### Reproducing .sumocfg with .xml files
As long as we have the `.rou.xml`, `.trips.xml`,  `.poly.xml` as required, we could reproduce the simulation. However, the vehicle trajectory is generated by default. We could manually alter them as we wish using `TraCI`.

## TraCI Commands

Detailed commands could be checked on [TraCI](https://sumo.dlr.de/docs/TraCI.html#traci_commands), the following might be useful for manipulation.

### For individual vehicle control:

 State Changing
  - Traffic Objects
    - [Change Vehicle State](https://sumo.dlr.de/docs/TraCI/Change_Vehicle_State.html)
    - [Change Person State](https://sumo.dlr.de/docs/TraCI/Change_Person_State.html)
    - [Change Vehicle Type State](https://sumo.dlr.de/docs/TraCI/Change_VehicleType_State.html)
    - [Change Route State](https://sumo.dlr.de/docs/TraCI/Change_Route_State.html)
  - Network
    - [Change Edge State](https://sumo.dlr.de/docs/TraCI/Change_Edge_State.html)
    - [Change Lane State](https://sumo.dlr.de/docs/TraCI/Change_Lane_State.html)

### For data retrival:

Value Retrieval
  - Traffic Objects
    - [Vehicle Value Retrival](https://sumo.dlr.de/docs/TraCI/Vehicle_Value_Retrieval.html)
    - [Person Value Retrival](https://sumo.dlr.de/docs/TraCI/Person_Value_Retrieval.html)
    - [Vehicle Type Value Retrival](https://sumo.dlr.de/docs/TraCI/VehicleType_Value_Retrieval.html)
    - [Route Value Retrival](https://sumo.dlr.de/docs/TraCI/Route_Value_Retrieval.html)

## Result with testWithAcc.py

```python
if 'SUMO_HOME' in os.environ:
    tools = os.path.join(os.environ['SUMO_HOME'], 'tools')
    sys.path.append(tools)
else:
    sys.exit("please declare environment variable 'SUMO_HOME'")
```
First we need to check if `SUMO_HOME` is an environment variable, which is usually done automatically when downloading sumo.

```python
def run():
    # main logic of using TraCI
    step = 0
    for step in range(3600):
        if 'veh0' in traci.vehicle.getIDList():
            traci.vehicle.setSpeed('veh0', 5.0)
            traci.vehicle.setAccel('veh0', 1)
        traci.simulationStep()

    traci.close()
    sys.stdout.flush()
```
The  `run()` function contains the main logic for TraCI. The example shown above simply sets the `acceleration` and `speed` manually for `veh0`, which would otherwise be set by sumo. The generated csv files by `testWithAcc.py` and by default are shown in the `.csv` folder.

