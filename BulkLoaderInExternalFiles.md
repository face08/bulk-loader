THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

new in svn trunk

### Defining Loading assets in External Files ###

BulkLoader has a facitily to define items to be loaded in external files.
Currently, you can define a serialized loader in a xml file or a json file.

It's very simple to use.
```
	// create a lazy loader passing the url to the xml file as a String
	public var lazy : LazyXMLLoader = new LazyXMLLoader("sample-lazy.xml", "myLoader");
	lazy.addEventListener(Event.COMPLETE, onLazyLoaded);
    lazy.addEventListener(ProgressEvent.PROGRESS, onLazyProgress);
    lazy.start();

	
```
The LazyXMLLoader is a BulkLoader instance and can listen for events, fetch items , etc.

If you wish to add event listeners to individual items, you must wait until the external definition is in place. Listen for the "lazyComplete" event for that, example:
```
// create a lazy loader passing the url to the xml file as a String
public var loader : LazyXMLLoader = new LazyXMLLoader("sample-lazy.xml", "myLoader");
// you can listen to events as a group right away
loader.addEventListener(Event.COMPLETE, onLazyLoaded);
loader.addEventListener(ProgressEvent.PROGRESS, onLazyProgress);
loader.start();

// be notified when the external definition is ready, and this BulkLoader instance is ready to start loading assets:
loader.addEventListener("lazyComplete", onBulkLoaderWillStart);

function onBulkLoaderWillStart(evt : Event) : void{
     // you can add events to some specific item:
	loader.get("config").addEventListener(BulkLoader.COMPLETE, parseConfig);
	 // you can add events to some specific item:
	loader.get("background").addEventListener(BulkLoader.COMPLETE, attachBg);
	
}
```

### Basic XML Sample ###
The basic xml format is to have each property defined by a xml node and the files to load as "file" nodes inside a "files" node. A simple xml file:
```
<?xml version="1.0" encoding="UTF-8"?>
<?xml version="1.0" encoding="UTF-8"?>
<BulkLoader >
	<name>lazyTest</name>
	<numConnections>5</numConnections>
	<logLevel>10</logLevel>
	<stringSubstitutions>
		<base_path>http://www.emptywhite.com/bulkloader-assets/</base_path>
		<sample_image>cats.jpg</sample_image>
	</stringSubstitutions>
	<allowsAutoIDFromFileName>true</allowsAutoIDFromFileName>
	
	<files>
		<file>
			<url>{base_path}cats.jpg</url>
		</file>
		<file>
			<url>{base_path}movie.flv</url>
			<pausedAtStart>true</pausedAtStart>
			<checkPolicyFile>true</checkPolicyFile>
			<maxTries>5</maxTries>
			<priority>100</priority>
			<weight>4</weight>
		</file>
		
		<file>
			<url>{base_path}some-text.txt</url>
			<preventCache>true</preventCache>
		</file>
		
		<file>
			<url>{base_path}sound-short.mp3</url>
			<id>the-sound</id>
		</file>
		
		<file>
			<url>{base_path}untyped-image</url>
			<type>image</type>
		</file>
		<file>
			<url>{base_path}samplexml.xml</url>
			<headers>
				<header1>value1</header1>
				<header2>value2</header2>
			</headers>
		</file>
		
	</files>
</BulkLoader>

```

### Basic JSON sample ###
```
{
	"name": "lazyTest",
	"numConnections": 2,
	"logLevel": 0,
	"stringSubstitutions": 
		{
			"base_path": "http:\/\/www.emptywhite.com\/bulkloader-assets\/",
			"sample_image": "cats.jpg"
		},
	"allowsAutoIDFromFileName":true,
	"files": [
		{
			"url": "{base_path}cats.jpg"
		},
		{
			"url":"{base_path}movie.flv",
			"pausedAtStart":true,
			"checkPolicyFile":true,
			"maxTries": 5,
			"priority":100,
			"weight":4
		},
		{
			"url": "{base_path}some-text.txt",
			"preventCache": true
		},
		{
			"url": "{base_path}sound-short.mp3",
			"id":"the-sound"
		},
		{
			"url": "{base_path}untyped-image",
			"type":"image"
		},
		{
			"url": "{base_path}samplexml.xml",
			"headers" :[
			  	{"header1": "value1"},
				{"header2": "value2"}
			]
		}		
	]
}
```