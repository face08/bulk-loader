THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

Once you have a BulkLoader instance, you can add items to be loaded.

The simplest case is just adding the item, with no further options:
```
bulkInstance.add("myfile.txt");
// or you can use a URLRequest:
var request : URLRequest = new URLRequest("myfile.txt");
bulkInstance.add(request);
```

Further options can be specified when adding items to be loaded.
# Options #
BulkLoader.add takes an optional second parameter, an object with the options you wish to set for this particular item. Bellow are all options that are available.

## Using an id for easier identification ##
By passing an id property, you can later on fetch that item by an id, instead of an url. This is more practical since if an url for an item changes, you only need to update the url passed to the .load method, but all further access to that loading item can remain untouched. For example:
```
	bulkInstance.add("all_configs.xml", {id:"config"});
```
Later on, you can refer to the all\_configs.xml file by the used id: "config":
```
   bulkInstance.getXML("config");
```

## Specifing the loading type ##
BulkLoader must now what type of asset you are loading. For the most common cases it will discover from the url passed. So when loading a "background.jpg" BulkLoader knows that it must use a Loader instance internally. There are two situations when that will not work:
a) The url offers no clues about the file type:
Maybe you are using a web-service or some server script that has a non-revealing url such as:
"http://mysite.com/top-ten.php"
BulkLoader doesn't know what the php script will return, so you can specify with the "type" option.
```
// returning a simple text
bulkInstance.add("http://mysite.com/top-ten.php", {type:"text"});

// returning a xml file
bulkInstance.add("http://mysite.com/top-ten.php", {type:"xml"});

// returning a jpeg image:
bulkInstance.add("http://mysite.com/top-ten.php", {type:"image"});
```

Two things to keep in mind:
a) If BulkLoader can't guess from the url extension and no type has been passed as an option, it defaults to a text type.
b) If you specify a "type" option it will override the automatic guessing from the url

See also the LoadingTypes page for more information on what types are available and how to add you own types.

## Priority ##
By default, BulkLoader will load items by the order in which they are added. You can modify this behavior by specifying a "priority" option. The priority option is an integer and the higher the priority, the sooner that item will start to load.
For example, this will load in this order: file\_1, file\_2, file\_3
```
bulkInstance.add("file_1.txt");
bulkInstance.add("file_2.txt");
bulkInstance.add("file_3.txt");
```
If you want file\_3 to be lodaded before file\_1 and file\_2:
```
bulkInstance.add("file_1.txt");
bulkInstance.add("file_2.txt");
bulkInstance.add("file_3.txt", {priority:10});
```

If two items have the same priority, the item added first takes precedence. Priority defaults to 0.

## Prevent caching ##
If you want to make sure the browser won't cache you request, the best way is to append a random bugues string to the url. If the option "preventCache" is set to true, BulkLoader will append a query paremeter:
BulkLoaderNoCache=[RANDOM NUMBER](SOME.md)
to the end of url. For example, to prevend the get\_latest url from caching, you can say:
```
bulkInstance.add("/get_latest/", {preventCache:true});
```
By default preventCache is false.

## Retries ##
Sometimes you want to make sure an asset loads and maybe because of a loaded server or a unstable network you want to make multiple tries for an url.

By default BulkLoader will retry 3 times until it gives up. You can control this behavior with the "maxTries" option, which is an int. To try an asset up to 5 times before giving up:
```
bulkInstance.add("/something.php", {maxTries: 5});
```
This will turn maxTries off:
```
bulkInstance.add("/something.php", {maxTries: 0});
```

## Adding Headers to a loading item ##
If you wish to add additional headers to an item you can pass the "headers" option.
The headers parameter is an array of URLRequestHeaders. For example, suppose a lot of your requests must have some auth headers:
```
var authHeaders : Array = [new URLRequestHeader("apache-secret", "as3_is_fun")];
// use these headers:
bulkInstance.add("/something.php", {headers: authHeaders});
bulkInstance.add("/something-else.php", {headers: authHeaders});
```

Note that this will only work if you add an item by a string. If you use a URLRequest object, those headers will be taken into account.

## Using a context ##
You can also make sure assets are loaded with a given LoaderContext or SoundContext instance.
```
bulkInstance.add("/main.swf", {context: myLoaderContext});
```

## Weight ##
Sometimes you will download many asstes, more that the number of connections the BulkLoader instance will open. In those cases, if you report progress by bytesLoaded, the result will be very flaky because untill all connections are opend the bytesTotal will remain 0. In those cases you can report progress by ratioLoaded. The problem is that sometimes assetshave a very different size in bytes.

The weight option lets you assign an arbitrary weight to an assets, that can be used for progress status.
The weight is an integer and defaults to 1.
See also the ReportingProgress docs.