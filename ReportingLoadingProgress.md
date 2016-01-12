THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

BulkLoader has very flexible ways to report the status of loading for it's items.

## By bytes / percentage ##
The most straight forward way is to use the amount of bytes loaded.
Each BulkProgressEvent has the following properties that indicate progress by bytes:
  * bytesLoaded -> (int) The number of bytes loaded, a sum of all items.
  * bytesTotal -> (int) The number of bytes loaded, a sum of all items. bytesTotal will be 0 until all items have begun loading.
  * bytesTotalCurrent -> (int) The number of bytes loaded, a sum of all items.
  * percentLoaded -> (Number) The ratio ( 0 -> 1) of bytesLoaded / bytesTotal.

While checking progress by bytes is the most accurate way, it can present problems. BulkLoader will use a pooling mechanism so the number of open connections don't exceed a certain value. Now, suppose you are using 5 connections and downloading 20 items. The bytesTotal cannot be determined until all 20 items have begun loading, and at least 15 items are already loaded. This means that the percentLoaded will be 0 almost until all items are loaded, and then it will jump abruptly. Worse, each time a new connection is open, the bytesTotal would jump around (it would add the bytesTotal for the new item). To mitigate this issue, bytesTotal will remain 0 until all file sizes are known. If you wish to know the sum of know total bytes use bytesTotalCurrent.

So while being very accurate, using bytesTotal is not recommended unless the number of items is small.
When loading a larger number of items, the two methods below are better alternatives.

## By ratio ##
The ratio count is a good way to track a large number of items.

Each BulkProgressEvent has the following properties that help checking the ratio status:
  * itemsLoaded -> (int) the number of items loaded.
  * itemsTotal -> (int) the number of all items to load.
  * ratioLoaded -> (Number) A helper, a number between 0 and 1 that indicates itemsLoaded / itemsTotal.

Counting by ratio will give you an indicator that is immediately available, no matter how many items there are to load.

This is the best way to track loading status when you have a large number of items to load of roughly the same size, for example all 40 thumbnails of an image gallery. If you have many items to load, and their sizes vary a lot, weighted progress is the way to go.

## By weight ##
This way is a bit more complex, but very useful. Lets imagine a very common scenario for loading:
  * a 1.2 mb swf file (main.swf)
  * a 20 kb xml file (config.xml)
  * a 20 kb xml file (strings.xml)
  * a 300 kb mp3 file(soundtrack.mp3)
  * a 120 kb background file (bg.jpg)
  * 20 jpeg thumbnails of around 10 kb.

If you use progress by bytes, your progress count will be zero for a long time, than jump to almost complete at the end.
If you use a ratio count, the items that are very large will skew the results. In this case we are loading 25 items. By ratio, each item corresponds to 4%. But the main.swf file is pretty large and the progress status will seem to be frozen while it's loading, since while it 4% by ratio, it's roughly 66% of the total bytes we must load.

The solution here is to give BulkLoader a hint of how large the files are. This way, BulkLoader will calculate a proportional status, with no sudden jumps or freezes, but it requires a bit of planning.
In our example, we can give weights based on each items size (at leats roughly)
  * main.swf -> 1200
  * config -> 20
> ...
When adding each item, we give this hint as a "weight" option:
```
bulkInstance.add("main.swf", {weight:1200});
bulkInstance.add("config.xml", {weight:20});
bulkInstance.add("strings.swf", {weight:20});
bulkInstance.add("soundtrack.mp3", {weight:300});
bulkInstance.add("background.jpg", {weight:200});
for each (thumb  in thumbs){
	bulkInstance.add(thumb, {weight:20});
}
```
Now you can track a reasonable estimate of loading progress. Using the following properties of the BulkProgressEvent object:
  * weightPercent -> (Number) A number between 0 and 1 that indicates progress regarding weights.

This is the smoother way to track progress for a large number of items when their sizes vary greatly.
A few things to keep in mind:
  * What matters is the proportion of weights, not an absolut number. On the example above you could establish that each weight would correspond to 20kb, so each thumb would have a weight of 1, the background a weight of 10, the main.swf a weigth of 60 and so forth.
  * Items have a default weigth of 1, so if you don't set their weights the weightPercent will be the same as the ratioLoaded.