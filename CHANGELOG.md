THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

Since google code's web interface has a reasonable changeset list for a while, I am not updating this anymore, for more information:  http://code.google.com/p/bulk-loader/source/list .
  * [r237](https://code.google.com/p/bulk-loader/source/detail?r=237) | debert | 2008-11-22 21:45:58 -0200 (Sat, 22 Nov 2008) | 2 lines
    * Fixed bug where progress information (bytesLoaded and bytesTotal) would be broken for mp4 files ([issue 57](https://code.google.com/p/bulk-loader/issues/detail?id=57)).I am not 100% sure of why this is, but it seems that a mp4 netstreams defaults bytesLoaded to -1 (instead of 0 for FLV's). This would break the check for stream INIT event on VideoItem, never updating the item's status, and therefore the final BulkLoader.getProgressForItems method would not take that VideoItem into consideration. Weird. Thanks for the bug report Chris  meyer

  * [r236](https://code.google.com/p/bulk-loader/source/detail?r=236) | debert | 2008-11-22 20:09:36 -0200 (Sat, 22 Nov 2008) | 2 lines
    * _ADDED_ mp4 extension for video items

  * [r235](https://code.google.com/p/bulk-loader/source/detail?r=235) | debert | 2008-11-22 19:56:03 -0200 (Sat, 22 Nov 2008) | 2 lines
    * _FIXED_ [issue 49](https://code.google.com/p/bulk-loader/issues/detail?id=49): intermitent CAN\_BEGIN\_PLAYING not fireing (thanks dinmore)

  * [r234](https://code.google.com/p/bulk-loader/source/detail?r=234) | debert | 2008-11-22 19:40:26 -0200 (Sat, 22 Nov 2008) | 2 lines
    * Small cosmetic changes

  * [r233](https://code.google.com/p/bulk-loader/source/detail?r=233) | debert | 2008-11-22 19:39:21 -0200 (Sat, 22 Nov 2008) | 2 lines
    * _ADDED_ test for clearMemory and itemsLoaded ([issue 61](https://code.google.com/p/bulk-loader/issues/detail?id=61))

  * [r232](https://code.google.com/p/bulk-loader/source/detail?r=232) | debert | 2008-11-22 14:54:59 -0200 (Sat, 22 Nov 2008) | 2 lines
    * Modified test suite to run through kisstest

  * [r231](https://code.google.com/p/bulk-loader/source/detail?r=231) | debert | 2008-11-22 14:53:52 -0200 (Sat, 22 Nov 2008) | 2 lines
    * Prepared suite to use kisstest. Compiles and runs

  * [r230](https://code.google.com/p/bulk-loader/source/detail?r=230) | debert | 2008-11-22 14:52:31 -0200 (Sat, 22 Nov 2008) | 2 lines
    * basic gui setup for test suite

  * [r229](https://code.google.com/p/bulk-loader/source/detail?r=229) | debert | 2008-11-22 14:52:02 -0200 (Sat, 22 Nov 2008) | 2 lines
    * _ADED_ specific test for onComplete handler on image items

  * [r228](https://code.google.com/p/bulk-loader/source/detail?r=228) | debert | 2008-11-22 14:51:25 -0200 (Sat, 22 Nov 2008) | 2 lines
    * _FIXED_ bug where loadNow would not pause items before removing them from the connection([issue 48](https://code.google.com/p/bulk-loader/issues/detail?id=48)). Thanks dinmore

  * [r227](https://code.google.com/p/bulk-loader/source/detail?r=227) | debert | 2008-11-22 14:51:07 -0200 (Sat, 22 Nov 2008) | 3 lines
    * Changed a few still private properties in VideoItem.
    * _ADED_ Created getter for _canBeginStreaming ([issue 47](https://code.google.com/p/bulk-loader/issues/detail?id=47)), tks dinmore_

  * [r226](https://code.google.com/p/bulk-loader/source/detail?r=226) | debert | 2008-11-22 14:50:47 -0200 (Sat, 22 Nov 2008) | 2 lines
    * _FIXED_ metadata tags causing incorrect auto complete on FlexBuilder ([issue 51](https://code.google.com/p/bulk-loader/issues/detail?id=51)), tks ryan

  * [r225](https://code.google.com/p/bulk-loader/source/detail?r=225) | debert | 2008-11-22 14:50:25 -0200 (Sat, 22 Nov 2008) | 3 lines
    * Made _contents dictionary to use weak keys.
    *_FIXED_Remove clears the key in_contents ([issue 58](https://code.google.com/p/bulk-loader/issues/detail?id=58)), tks i...@bigsource.de

  * [r224](https://code.google.com/p/bulk-loader/source/detail?r=224) | debert | 2008-11-22 14:50:00 -0200 (Sat, 22 Nov 2008) | 2 lines
    * Moved tests back to test, off from trunk. dough

  * [revision 222](https://code.google.com/p/bulk-loader/source/detail?r=222) | debert | 2008-09-28 08:15:26 -0300 (Sun, 28 Sep 2008) | 1 line
    * Moved test suite to main trunk

  * [revision 220](https://code.google.com/p/bulk-loader/source/detail?r=220) : 0.9.9.0 : _2008-09-28 0:44:16 -0300_:
    * _FIXED_ Fixed bug where if the last item to load returned a 404, BulkLoader would still dispatch a COMPLETE event in error.

  * [revision 215](https://code.google.com/p/bulk-loader/source/detail?r=215) : 0.9.9.0 : _2008-06-17 0:44:16 -0300_:
    * _FIXED_ [issue 39](https://code.google.com/p/bulk-loader/issues/detail?id=39): missing import statment in XMLItem, thanks smakinson, harmanjd

  * [revision 213](https://code.google.com/p/bulk-loader/source/detail?r=213) : 0.9.9.0 : _2008-06-7 0:44:16 -0300_:
    * _FIXED_  XML item cast runs inside a try / catch block so not to raise uncaught execptions, thanks Myrkyl


  * [revision 210](https://code.google.com/p/bulk-loader/source/detail?r=210) : 0.9.9.0 : _2008-05-30 0:44:16 -0300_:
    * _CHANGED_ whichLoaderHasItem will return any loader with an item (loaded or not)

  * [revision 208](https://code.google.com/p/bulk-loader/source/detail?r=208) : 0.9.9.0 : _2008-05-30 0:44:16 -0300_:
    * bulkLoader>.pause will not close items that are fully loaded.

  * [revision 206](https://code.google.com/p/bulk-loader/source/detail?r=206) : 0.9.9.0 : _2008-04-01 0:44:16 -0300_:
    * _FIXED_ bug where CAN\_BEGIN\_PLATING would not fire on local files. Thanks  Robby Abaya and gstoner.

  * [revision 205](https://code.google.com/p/bulk-loader/source/detail?r=205) : 0.9.9.0 : _2008-04-01 0:44:16 -0300_:
    * _FIXED_ bug when registering new types ([issue 34](https://code.google.com/p/bulk-loader/issues/detail?id=34)), thanks t.izukawa.

  * [revision 203](https://code.google.com/p/bulk-loader/source/detail?r=203) : 0.9.9.0 : _2008-04-01 0:44:16 -0300_:
    * _FIXED_ small typo on class dictionary ([issue 35](https://code.google.com/p/bulk-loader/issues/detail?id=35)), thanks t.izukawa.

  * [revision 199](https://code.google.com/p/bulk-loader/source/detail?r=199) : 0.9.9.6 : _2008-04-16 2:44:16 -0300_:
    * _ADDED_ BinaryItem and related changes, thanks [alex winx](http://www.winx.ws)  .

  * [revision 196](https://code.google.com/p/bulk-loader/source/detail?r=196) : 0.9.9.6 : _2008-04-16 2:44:16 -0300_:
    * _FIXED_ bug where cleaning listeners for URLItem and XMLItem would fail (thanks Murkyl)

  * [revision 191](https://code.google.com/p/bulk-loader/source/detail?r=191) : 0.9.9.6 : _2008-04-14 2:44:16 -0300_:
    * _ADDED_ clear method that removes the loader from the static dict, and removeAll does not render the bulkloader invalid anymore, thanks Josh (fixed [issue 31](https://code.google.com/p/bulk-loader/issues/detail?id=31)).

  * [revision 189](https://code.google.com/p/bulk-loader/source/detail?r=189) : 0.9.9.6 : _2008-04-12 2:44:16 -0300_:
    * _ADDED_ tests for CAN\_BEGIN\_PLAYING events on VideoItem.

  * [revision 187](https://code.google.com/p/bulk-loader/source/detail?r=187) : 0.9.9.6 : _2008-04-04 2:44:16 -0300_:
    * _FIXED_ [issue 28](https://code.google.com/p/bulk-loader/issues/detail?id=28)  (numConnections loading one more than specified), thanks schickm.

  * [revision 185](https://code.google.com/p/bulk-loader/source/detail?r=185) : 0.9.9.5 : _2008-12-06 2:44:16 -0300_:
    * _FIXED_ [issue 17](https://code.google.com/p/bulk-loader/issues/detail?id=17) loading will not start until a star call is made. This restores the expected behaviour. Added test for this.

  * [revision 179](https://code.google.com/p/bulk-loader/source/detail?r=179) : 0.9.9.5 : _2008-03-06 2:44:16 -0300_:
    * _ADDED_ aditional tests that LoaderItems can access content on the individual item event loop

  * [revision 176](https://code.google.com/p/bulk-loader/source/detail?r=176) : 0.9.9.5 : _2008-03-06 2:44:16 -0300_:
    * _FIXED_ [issue 23](https://code.google.com/p/bulk-loader/issues/detail?id=23), thanks t. izukawa. (bytesTotalCurrent always 0 on events)
    * _ADDED_ tests for bytesTotalCurrent

  * [revision 175](https://code.google.com/p/bulk-loader/source/detail?r=175) : 0.9.9.4 : _2008-03-06 2:44:16 -0300_:
    * _FIXED_ [issue 24](https://code.google.com/p/bulk-loader/issues/detail?id=24) , thanks Marcelo Rigon (isSWF always returning false)

  * [revision 174](https://code.google.com/p/bulk-loader/source/detail?r=174) : 0.9.9.2 : _2008-03-03 2:44:16 -0300_:
    * decreased chance of name collision on preventCache

  * [revision 171](https://code.google.com/p/bulk-loader/source/detail?r=171) : 0.9.9.1 : _2008-03-03 0:44:16 -0300_:
    * code cleanup
    * fixed bug when removing all and they adding new items (loading would not start again), thanks Jens Franke. (and added regression test for that)
    * allowed num connections to change in start even after num connections is made

  * [revision 163](https://code.google.com/p/bulk-loader/source/detail?r=163) : 0.9.9.0 : _2008-03-01 0:44:16 -0300_:
    * Merged li-refactor branch:commits
    * refactored serialized loaderds to use simple inheritance.
    * fixed item creation bug in lazy loaders
    * fixed item creation bug in lazy loaders
    * created getters and setter to make sure percentage loaded will never be NaN or Infinity
    * refactored LazyBulkLoader to use a start instead of a fetch method (much cleaner)
    * updated lazy loader example
    * basic implementation of LazyJSONLoader
    * fixed JSONLazyLoader
    * fixed LazyXMLLoaderTest bug on internals
    * added tests for highestPriority, order on add
    * BACKWARDS INCOMPATIBLE CHANGE: more than one item with the same url.
    * refactored serialized loaderds to use simple inheritance.
    * refactored lazy loader as a proxy
    * improved tests for lazy loaders
    * added base tests for lazy loaders
    * added to lazy xml loader: string subs,  auto id
    * fixed spelling error on numConnection
    * fixed imports for lazy items.
    * rep layout change (took tests out of main rep, added modified asunit)
    * documented new features
    * changed everything to public, with nderscore prefixed to internal items,
    * rep layout change (took tests out of main rep, added modified asunit)
    * documented new features
    * changed everything to public, with underscore prefixed to internal items,
    * added test for onComplete and progress never firing more than once / after all is loaded
    * Added autoId feature and tests (thanks Pedro Moraes)
    * Added tests for loadNow
    * Added string substitution feature (thanks Pedro Moraes) and tests
    * added test for loadNow
    * refactored stats code (into LoadingItem)
    * BACKWARDS INCOMPATIBLE CHANGE: renamed 

&lt;BulkLoader&gt;

.traceStats() to 

&lt;BulkLoader&gt;

.getStats()
    * added tests for resume, resumeAll and pauseAll.
    * code cleanup
    * added tests for RemovePaused and RemovedFailed
    * tested BulkError
    * added tests for type guessing
    * major work on testing logic
    * code cleanup
    * fixed bug in type specific properties , fixed those for video items
    * refactored type system: much cleaner now
    * tests for isLoaded
    * re-implemented code that warns of unused properties
    * tested code above
    * new tests for start, logLevel.
    * BACKWARDS INCOMPATIBLE CHANGE: hasItemInBulkLoader is now a static method (makes more sense this way).
    * created  createUniqueNamedLoader function
    * tested above
    * created default static consts for logLevel and numConnections
    * added test and item for XML
    * added tests for text items (and passes)
    * merged upstream bug fix for lazy items (rev 123)
    * test improvements: IOError works (and passes) on audio, video and loaders.
    * tests passes for Audio
    * test for httpStatus work as expected
    * added test flas
    * test improvments
    * functionality works
    * started test suite
    * Broke dir strucure for packages

  * **[revision 123](https://code.google.com/p/bulk-loader/source/detail?r=123) : 0.9.4.2** : _2008-02-13 0:44:16 -0200_
    * _FIXED_ bug ([issue 20](https://code.google.com/p/bulk-loader/issues/detail?id=20)) where serialized xml lazy loader would not respect the name associated with the xml file, thanks chover2

  * **[revision 114](https://code.google.com/p/bulk-loader/source/detail?r=114) : 0.9.4.2** : _2008-02-05 21:07:16 -0200_
    * _FIXED_ bug ([issue 13](https://code.google.com/p/bulk-loader/issues/detail?id=13)) where adding an additional item on the onComplete handler would raise an error, thanks [Kelvin Luck](http://kelvinluck.com/).

  * **[revision 111](https://code.google.com/p/bulk-loader/source/detail?r=111) : 0.9.4.2** : _2008-02-04 21:07:16 -0200_
    * _FIXED_ implemented work around a FDT parser's bug.

  * **[revision 108](https://code.google.com/p/bulk-loader/source/detail?r=108) : 0.9.4.2** : _2008-01-29 21:07:16 -0200_
    * _FIXED_ bug where removeAll would not allow for the addition of new items, thanks [Jens Franke](http://www.jensfranke.com).

  * **[revision 107](https://code.google.com/p/bulk-loader/source/detail?r=107) : 0.9.4.1** : _2008-01-14 21:07:16 -0200_
    * _FIXED_ bug where adding items with the same url and without an id property would cause the items to be added more than once, thanks Jeff.

  * **[revision 99](https://code.google.com/p/bulk-loader/source/detail?r=99) : 0.9.4.0** : _2008-01-06 21:07:16 -0200_
    * _FIXED_ bug that wouldn't allow sound items to be accessed when streaming had begun but not finished, thank Nick Terry for the bug report.
    * _IMPLEMENTED_ Lazy loading machinery, that allows bulk loaders to be defined in external files (xml) and loaded at runtime. Thanks Dimitar Genov for the code contribution.
    * _ADDED_ examples for basic usage + serialized loaders.

  * **[revision 95](https://code.google.com/p/bulk-loader/source/detail?r=95) : 0.9.3.0** : _2007-12-28 21:07:16 -0200_
    * _FIXED_ bug where START event would fire before a sound content would be accessible, thanks Nick Terry.

  * **[revision 92](https://code.google.com/p/bulk-loader/source/detail?r=92) : 0.9.3.0** : _2007-12-17 21:07:16 -0200_
    * _FIXED_ bug where videos with pausedAtStart would loose their meta data information. Thanks Renato Inamine.

  * **[revision 90](https://code.google.com/p/bulk-loader/source/detail?r=90) : 0.9.3.0** : _2007-12-17 21:07:16 -0200_
    * _IMPLEMENTED_ convenience method getAVM1Movie , thanks profilin54.

  * **[revision 88](https://code.google.com/p/bulk-loader/source/detail?r=88) : 0.9.3.0** : _2007-12-16 21:07:16 -0200_
    * _IMPLEMENTED_ VERSION constant to aid in debugging.

  * **[revision 85](https://code.google.com/p/bulk-loader/source/detail?r=85) : 0.9.3.0** : _2007-12-15 21:07:16 -0200_
    * _IMPLEMENTED_ the loadNow method to force immediate loading.
    * _IMPLEMENTED_ the CAN\_BEGIN\_PLAY event for NetStream objects to flag when a video can begin playback and is expected to stream without interruptions.
    * _FIXED_ bug where the bulk event for all items loaded would fire before the last COMPLETE event for an individual item, thanks [Jens Franke](http://www.jensfranke.com).

  * **[revision 83](https://code.google.com/p/bulk-loader/source/detail?r=83) : 0.9.2.2** : _2007-12-11 21:07:16 -0200_
    * _IMPLEMENTED_ method for getting progress for an arbitrary set of items.

  * **[revision 82](https://code.google.com/p/bulk-loader/source/detail?r=82) : 0.9.2.2** : _2007-12-11 17:07:16 -0200_
    * _FIXED_ bug where netConnection would get garbage collected and freeze videos.

  * **[revision 78](https://code.google.com/p/bulk-loader/source/detail?r=78) : 0.9.2.2** : _2007-12-09 02:07:16 -0200_
    * _IMPLEMENTED_ highestPriority, thanks pedr.browne.


  * **[revision 75](https://code.google.com/p/bulk-loader/source/detail?r=75) : 0.9.2.2** : _2007-12-08 02:07:16 -0200_
    * _IMPLEMENTED_ accessor for itens.
    * _CHANGED_ Exposed isLoaded in LoadingItem

  * **[revision 73](https://code.google.com/p/bulk-loader/source/detail?r=73) : 0.9.2.1** : _2007-12-01 02:07:16 -0200_
    * _FIXED_ bug in removeFailedItems and removeStoppedItems, thanks [Stefan Kratz](http://www.yooba.se/)

  * **[revision 71](https://code.google.com/p/bulk-loader/source/detail?r=71) : 0.9.2** : _2007-12-01 01:07:16 -0200_
    * _IMPLEMENTED_ helper methods to change an item priority after an item has been added. Thanks toby (lowpitch) for the report.
    * _IMPLEMENTED_ helper method to resort the queue by priority.

  * **[revision 69](https://code.google.com/p/bulk-loader/source/detail?r=69) : 0.9.2** : _2007-12-01 00:07:16 -0200_
    * _CHANGED_ the visibility of properties and methods from LoadingItem. Most things were public and this gave the idea that the LoadingItem was to be used stand alone.
    * _IMPLEMENTED_ public accessors for type and id in LoadingItem.
    * Documentation improvements

  * **[revision 65](https://code.google.com/p/bulk-loader/source/detail?r=65) : 0.9.1** : _2007-12-05 23:07:16 -0200_
    * _FIXED_ documentation on LOADER\_TYPES, thanks  szemraj.rafal;
    * _FIXED_ bug on error handling, thanks [Igor Almeida](http://www.ialmeida.com);
    * _IMPLEMENTED_ video and sound types to reflect the players new capabilities (f4v, f4a, etc), thanks [zeh](http://www.zeh.com.br);
    * _IMPLEMENTED_ the "pausedAtStart" property on add for pausing video instances when load operation has begun;
    * _FIXED_ The OPEN ("open") event was not firering when a request has been opened.

  * **[revision 61](https://code.google.com/p/bulk-loader/source/detail?r=61) : 0.9** : _2007-11-26 23:07:16 -0200_
    * _CHANGED_ logLevel and logFunction are instance properties instead of static, thanks Igor Almeida.