# 'The Boiler' Spec
_version 0.1, updated 2018-04-08_

## Outlets
**Outlets** handle taking built/intermediate data and putting it into a final destination (i.e. a file.)

_NEW TERMINOLOGY: dx = Boiler delta manager._

Example:

```javascript
/* Synchronous FS outlet. (FSSync) */
let boiler = require("the-boiler");
let fs = require("fs");

const OUTLET_NAME = "FSSync";

module.exports = {
  outlet: function(dx) {
    let deltas = dx.getDeltas(OUTLET_NAME);
    for (delta of deltas) {
      let filename = delta.userobj;
      // Extract data handle.
      let data = dx.dataHandle(delta);
      // Ask for the data synchronously.
      // We presume this is of type String
      let actData = dx.requestData(boiler.SYNC);
      dx.beginOutlet(delta); // Indicates that this part might fail.
      fs.writeFileSync(filename, actData);
      dx.endOutlet(delta); // Finished outlet.
      // Construct a status.
      let status = boiler.delta.constructFilter({
        ago: "0", // number of cycles ago (we want 0 because it happened this cycle)
        type: "outlet",
      }).filter(dx, delta).status();
      dx.writeDeltaStatus(delta, status);
    }
  }
}
```
