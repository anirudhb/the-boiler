# The Boiler

## Introduction

**Q: What is the Boiler?**

**A: The Boiler is a _library_ that helps with the build process. It uses multiple checks to prevent MITM attacks.**

## Pushers

**Pushers** are JavaScript files which integrate with the Boiler. They add on to or change existing functionality. For example, if you want to make a pusher which would use `fs` to write to a file (synchronously), it would look like this:
```javascript
let boiler-lib = require("boiler");

let boiler = boiler-lib.takeBoiler("push");
let fs = require("fs");

function writeFile(dx) {
  boiler-lib.util.unmapper(fs.writeFileSync, dx);
  return boiler-lib.util.fn2fn.nextSteam(dx);
}

boiler.push(boiler.asSteam(writeFile), new boiler-lib.SteamMixer({
  set decode(x) {
    return [x.name, x.contents];
  }
  get decode() {
    return boiler-lib.pipes.int.RedirectPipe("{set}decode", takenData=boiler.vars.context.map.steam.previous());
  }
  steam: function(dx) {
    return boiler.vars.context.current.steam(boiler-lib.util.fn2fn.resign(dx));
  }
}, boiler-lib.util.getSignature(boiler.staticSteam(writeFile)));
```

Whoah! Lots of checking there! I'll explain below.
