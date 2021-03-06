# The Things Network

Based on code from another [app](https://github.com/jordenc/webhook.manager).  

Make Homey part of a global loRaWAN network using The Things Network (TTN) & webhooks.

### Support
If you like this app & want to support further development please consider buying me a [coffee](https://www.buymeacoffee.com/oohehir).  

Forum discussion on this [app](https://community.athom.com/t/48869).  

### Note
This app does NOT implement loRa on Homey, rather it uses the internet to connect to TTN servers & pass data back & forth. From the TTN servers the data is relayed via LoRaWAN gateways to end devices. For more info start [here](https://www.thethingsindustries.com/docs/getting-started/quick-start/)   

The app assumes that your end device is connected correctly to the TTN stack v3.

### Setup (using webhook template) - preferred method
1. After installing the app on Homey, go to the app 'Settings' page. It shows the 'Webhook ID' & 'Keypath Value'.
2. On your TTN console (for [Europe](https://eu1.cloud.thethings.network/console/applications/)) select: 'Relevant Application> Integrations > Webhooks > '+ Add Webhook'> Homey Template. Fill in the details from step 1.
3. Save!

### Setup (using custom webhook) 
1. After installing the app on Homey, go to the app 'Settings' page. It shows the 'Webhook ID' & 'Keypath Value'.
2. On your TTN console (for [Europe](https://eu1.cloud.thethings.network/console/applications/)) select: 'Relevant Application> Integrations > Webhooks > '+ Add Webhook'> Custom Template. Fill in the details as follows:  
    webhook ID - a meaningful name for your own use  
    webhook format - JSON   
    BaseURL: https://webhooks.athom.com/webhook/60df8366132fac0be2bb85eb   
3. Add an additional header using authorization:  
    x-user-id      Keypath_Value_from_settings  
4. Tick 'Uplink message' enabled box 
5. Save!

![Custom Webhook Settings](./pictures/CustomWebhookSettings.png)

### Flowcards
Flows can be triggered by  
1. Uplink message
2. Uplink message from specific device
3. Uplink message from specific application  

### Payload Formatters
Due to the low data rate of loRa data is made as compact as possible when transmitted 'over the air'. Examples of encoding schemes to compact data are shown [here](https://www.thethingsnetwork.org/docs/devices/bytes/). Using TTN payload [formatters](https://www.thethingsindustries.com/docs/integrations/payload-formatters/) it is possible to convert this 'packed data' into more meaningful states & values before they are sent by webhook to Homey. 

Two strings (state1 & state2) & two values (value1 & value2) are extracted by this app & events can then be triggered, using appropriate logic cards, depending on their contents. An example is shown below.

```javascript
function decodeUplink(input) {
  
  // Assume 4 bytes received, e.g. 0x01, 0x9F
  // state1 & state2 must be strings/ word
  // value1 & value2 must be numbers
  
  // More examples here:
  // https://www.thethingsnetwork.org/docs/devices/bytes/
  
  var data = {};
  
    if (input.bytes[0] == 1){
      data.state1 = "On";
    } else{
      data.state1 = "Off";
    }
    
    if (input.bytes[1] == 1){
      data.state2 = "Open";
    } else{
      data.state2 = "Closed";
    }
    
    data.value1 = 5.2 * input.bytes[2];
    data.value2 = 25 + input.bytes[3];
    
  return {
    data: data,
    warnings: [],
    errors: []
  };
}
```
### Test
It is possible to test that the integration works without an actual loRa end device by simulating an uplink message from TTN.  

This is accomplished by:  
1. Setting up the payload formatter as above.
2. Setting up the Homey [Flow](https://homey.app/f/FUTQcG/)
3. Simulating an uplink message ('Relevant Application> Integrations > End Devices > Relevant End Device > Messaging & use the message as below).  

![Uplink Test Message](pictures/UplinkTestMesg.png)

**Version 0.0.1**
- Initial release
