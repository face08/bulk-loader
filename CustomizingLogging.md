THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

BulkLoader comes with a flexible and customizable logging facility.

## Logging Levels ##
You can choose at which level you'd like BulkLoader to trace messages.

### Specifing Logging Levels ###
The constructor's 3rd parameter is an int, the logging level.
Examples:
```
// verbose: logs everything that happens
var loader : BulkLoader = new BulkLoader("main", 5, BulkLoader.LOG_VERBOSE)
// errors only:
var loader : BulkLoader = new BulkLoader("main", 5, BulkLoader.LOG_ERRORS)
```

Or you can set the logLevel property at any time:
```
BulkLoader.logLevel = BulkLoader.LOG_INFO;
```

## What each level filters ##
BulkLoader's default logging is LOG\_ERRORS. It's a reasonable level, since it will only trace messages in case something goes wrong.

  * LOG\_SILENT : nothing will be traced.
  * LOG\_ERRORS: the default level. Will log:
    * If an item fails to load.
    * If retrieving content fails.
  * LOG\_INFO: besides everything that LOG\_ERRORS traces:
    * When an item is starting to load.
    * When an item is finished loading.
    * When a loading operation received a response.
    * When all items are loaded.
  * LOG\_VERBOSE: Pretty much everything, usefull for debugging. Besides everything that LOG\_INFO traces:
    * When an item has been added.
    * When an item has been removed.
    * When an item has been stoped.
    * When an item has been resumed.
    * All download stats when all items are loaded.

## Using your own logging function ##
If you'd rather do something else with the messages BulkLoader logs, you can specify you own logging function.
Just set the BulkLoader.logFunction property to another function:
```
<bulkLoader>.logFunction = myLogger;

function myLogger(msg : String) : void{
  // do sothing with that message here
}
```
The logging function will receive one parameter, a string of the message to log.