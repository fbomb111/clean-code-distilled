[Previous: Error Handling](error-handling.md)

# Boundaries

## Using third-party code

- Providers of third-party packages strive for broad applicability
- However, users want an interface that focuses on their needs

Bad:

    Map sensors = new HashMap();
    Sensor s = (Sensor)sensors.get(sensorId);
    
The `get` function of `Map` returns an `Object` that needs to be cast into a `Sensor` and the same cast is littered through 

[Next: Unit Tests](unit-tests.md)
