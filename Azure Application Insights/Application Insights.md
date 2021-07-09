# Notes on Application Insights

There doesn't appear to be any official documentation on Azure Monitor Application Insights REST API, so the following provides a little bit of documentation based on simple reverse engineering of the sample in the test director.

## Example

The following shows the message body 

```javascript
 params = [
    	uri: "https://dc.services.visualstudio.com/v2/track",
        // uri: "https://eastus-3.in.applicationinsights.azure.com/v2/track",
        body: [
            time: eventDate, 
            iKey: aiConfig,
            name:"Microsoft.ApplicationInsights." + aiConfig + ".Event", 
            tags:[ 
                "ai.session.id": location.id,
            	"ai.device.id": evt.deviceId,
            	"ai.device.type":"Smartthing",
                "ai.internal.sdkVersion":"Smartthings.0.1",
                "ai.user.id": location.id,
                "ai.operation.id": operationId,
                "ai.operation.name": eventName
                ],
            data: [
                baseType:"EventData",
                    baseData: [
                        ver:2,
                        name: eventName,
                        properties: [ Device: deviceName, Location: location.name ],
                        measurements: [ Value: value.toFloat() ]
                    ]                
                ]
            ]
        ]
```

## Notes

In general, you POST the above information to https://dc.services.visualstudio.com/v2/track or better to the URL in the Application Insights connection string.

The following fields might need a little explaining.

* ai.operation.name: not sure this one is important for separating different kinds of information; e.g. Temperature or Humidity as the event name is included in eventData.
* ai.operation.id: not really important unless you are trying to definte a "call graph" in which case you would also provide a parent id.
* ai.session.id: not important
* ai.user.id: not important
