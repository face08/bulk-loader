THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

Memory management can be pretty tricky to get right. If you aren't careful, you will consume more and more memory each item you load. If your website loads many items, this can become an issue.

## How To Clear Memory ##
There are several ways you can tell BulkLoader to release memory. You can do it for an item or an entire BulkLoader instance.

### Use "clearMemory" ###
All functions that fetch content (see AcessingLoadedContents) accept a second optional parameter, a boolean clearMemory. The general form is:
```
bulkLoaderInstance.getContentXXX(itemKey, clearMemory);
```
After fetching some content with clearMemory set to true, bulk loader will remove the object from memory, and any subsequent attempts to fetch that same object will fail.

### Use "remove" ###
You can tell bulk loader to remove an item with the method remove:
```
bulkLoaderInstance.remove(anItemKey);
```

You can remove an item at any time. Remove will, if appropriate and available call unloadAndStop or unload on the Loader instance involved.

### Use "removeAll" ###
You can tell bulk loader to remove all item in an instance:
```
bulkLoaderInstance.removeAll(anItemKey);
```

You can remove all item at any time. Note that the BulkLoader instance itself will be usable after that, but the items just cleared won't be accessible anymore.

### Use "removeAllLoaders" ###
You can clear all instances of BulkLoader with the static method:
```
BulkLoader.removeAllLoaders();
```
This is pretty much dropping a nuke on bulkLoader. Besides traversing all instances and calling removeAll on each one, it will also call "clear" on them ( more on that bellow).


### Use "clear" ###
The clear method will, beyond remove all items, delete everything in BulkLoader. This means that after clear has been called , that instance will be entirely useless. Therefore you won't be able to access any of it's contents, nor add other items, not even retrieve that instance with BulkLoader.getLoader(id);

### Gotchas ###
#### EventListeners ####
Because users can attach event listeners for individual items, if you do and do not use a weak reference you might keep this item in memory, even if you clear it with any of the above techniques.
Also, event listeners attached to BulkLoader instances might not allow removeAll and removeAllLoaders to actually clear memory.

For this reason it's recommended either the usage of weak references or to manually remove the listener once you are done with it.
#### Measuring Leaks ####
Measuring memory leaks is not always as simple as it sounds. Freeing memory with the techniques above won't reclaim memory, but will mark it as available the next time the runtime needs it.
Grant Skinner has an excellent a[article](http://www.adobe.com/devnet/flashplayer/articles/garbage_collection.html) on the runtime's garbage collection, which is pretty much required reading on the subject.

So keep in mind that you can't gauge a leak until you've tried to claim memory back.

The best foolproof away to measure leaks is to use the profiler, call clear-removeAll-etc and see if those instances have been removed from memory.

Alternatively you can use the player 10 profiling api by hand.

#### BinaryItems ####
Regular loaders (movieclips and bitmaps) not always will release memory (as you can have events attached to them, and loader used to have a bug in it). Therefore, sometimes using a BinaryItem will result in more controllable behavior.