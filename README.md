# js-call-tracer
Trace your object call chain with the simplest possible code.

Anywhere you have a `(let|const|object.)something = new Something(...)` statement in your code, just wrap it:

```javascript
import { trace } from "/tracer.js";

const something = trace(new Something(...));
```

And you're done. You'll now get stuff like this as part of stdout (unless you override where to log to by passing `{ logger: ... }` as options object to `trace`, with something that has a `log` function):

```
│ —> call: ALOSInterface.lookup([48.8,-123.8])
│ —>   call: ALOSInterface.getTileFor([48.8,-123.8])
│ —>     call: ALOSInterface.getTileName([48.8,-123.8])
│ <—     finished ALOSInterface.getTileName(), return value: "ALPSMLC30_N048W124_DSM.tif", runtime: 0ms
│ —>     call: ALOSInterface.getTileFromCache(["ALPSMLC30_N048W124_DSM.tif"])
│ —>       call: ALOSInterface.getTilePath(["ALPSMLC30_N048W124_DSM.tif"])
│ <—       finished ALOSInterface.getTilePath(), return value: "\\GIS\ALOS World 3D (30m)\data\N045W125_N050W120\ALPSMLC30_N048W124_DSM.tif", runtime: 0ms
│ <—     finished ALOSInterface.getTileFromCache(), return value: ALOSTile:{tilePath: "\\GIS\ALOS World 3D (30m)\data\N045W125_N050W120\ALPSMLC30_N048W124_DSM.tif",pixels: {...},width: 3600,height: 3600,forward: [-124,0.0002777777777777778,0,49,0,-0.0002777777777777778],reverse: [446400,3600,0,176400,0,-3600]}, runtime: 273ms
│ <—   finished ALOSInterface.getTileFor(), return value: ALOSTile:{tilePath: "\\GIS\ALOS World 3D (30m)\data\N045W125_N050W120\ALPSMLC30_N048W124_DSM.tif",pixels: {...},width: 3600,height: 3600,forward: [-124,0.0002777777777777778,0,49,0,-0.0002777777777777778],reverse: [446400,3600,0,176400,0,-3600]}, runtime: 274ms
│ <— finished ALOSInterface.lookup(), return value: 152, runtime: 274ms

╭┈┈
│ —> call: ALOSInterface.lookup([48.9,-123.9])
│ —>   call: ALOSInterface.getTileFor([48.9,-123.9])
│ —>     call: ALOSInterface.getTileName([48.9,-123.9])
│ <—     finished ALOSInterface.getTileName(), return value: "ALPSMLC30_N048W124_DSM.tif", runtime: 0ms
│ —>     call: ALOSInterface.getTileFromCache(["ALPSMLC30_N048W124_DSM.tif"])
│ <—     finished ALOSInterface.getTileFromCache(), return value: ALOSTile:{tilePath: "\\GIS\ALOS World 3D (30m)\data\N045W125_N050W120\ALPSMLC30_N048W124_DSM.tif",pixels: {...},width: 3600,height: 3600,forward: [-124,0.0002777777777777778,0,49,0,-0.0002777777777777778],reverse: [446400,3600,0,176400,0,-3600]}, runtime: 0ms
│ <—   finished ALOSInterface.getTileFor(), return value: ALOSTile:{tilePath: "\\GIS\ALOS World 3D (30m)\data\N045W125_N050W120\ALPSMLC30_N048W124_DSM.tif",pixels: {...},width: 3600,height: 3600,forward: [-124,0.0002777777777777778,0,49,0,-0.0002777777777777778],reverse: [446400,3600,0,176400,0,-3600]}, runtime: 1ms
│ <— finished ALOSInterface.lookup(), return value: 840, runtime: 1ms
╰┈┈
```
