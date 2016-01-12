
THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

This is a (not so) short list of the questions asked the most by users. I've tried either to answer them fully, or point to other places where more information is available.

### Is Bulk Loader- bloated? ###
Yes it is. The inside joke is it should be renamed to BloatedLoader.
The truth is that, as most things, it started small. Then came features, edge-cases, convenience functions, better structure, and it became big.
BulkLoader takes about 16kb compiled, which is pretty big for a loading manager. But it's intended audience, are developers with big and complex apps or websites. Most likely they are loading anywhere from 1MB to many megabytes. Under those circumstances, 16kb is justifiable for saving hours of development time.
A litmus test: if in your project it's more important to save 10kb then gaining many hours of development time, BulkLoader is not a good fit for it.
Another take : if someone would provide a patch with a clean cut, minimized, version that would still be useful for most people, I'll be glad to take that in. But I can't forsee what that would look like.

### Progress takes too long to show! BytesLoaded is zero for a long time! ###
That is, unfortunately not something that BulkLoader can solve for you. A useful progress indication must know the total number of bytes to load, and the flash player can only figure that out once all connections are opened and all response headers have been returned. If you are loading many assets, it will take long. To be more precise, it will only report anything useful when the last item has begun loading, and almost all other items are done loading.  BulkLoader does, however, offers other progress indicators that can help in those cases. Get the full details here: http://code.google.com/p/bulk-loader/wiki/ReportingLoadingProgress.

### Complete never fires if there is an error? ###
The COMPLETE event means that all items have loaded successfully. If an error ocurrs, it will never fire. You should attach an error event  to the BulkLoader intance, and on that handler take actions to recover from that error (such as removing failed items). Then the COMPLETE event will fire.
Note that Marko has a patch the creates a COMPLETE\_WITH\_ERROR events here ( http://code.google.com/p/bulk-loader/issues/detail?id=79#makechanges ), for those who would not rather have an error listener attached.

### Pause and resume are not working! ###
Yes they are, at least they should be. There are test cases covering it, and if something is broke I'll be glad to fix it.
The issue here is that BulkLoader will do as much as the runtime allows for resuming. But how much it actually resumes a previously interrupted connection is dependent on other factors such as browser, player version and browser settings. If you are unsure if it is working make a simple test swf, start loading items, and call pauseAll. Then monitor those connections (hhtpfox or better yet webkit's developer panel will show you exactely what is going on).

### Priorities are crazy ###
Due to hostname sharding, the way priorities work might be confusing. You can read more on HowPrioritiesAndLoadingOrderWork.

### Something does not work! ###
Yes it does. Maybe it does not do what you expect, or what you wished it would do. Or maybe there is a bug in your code.
Yes, there are bugs in BulkLoader, but most things should really work. If you find that something is not working, just do the best practices for any programming issues:
  * Search the mailing list, the faq, the wiki, the documentation, chances are it's been answered somewhere.
  * Isolate the code that relates to BulkLoader, excluding the chance of having your application logic cause the bug.
  * Find a minimum test case that exposes the problem.
  * Post the test case on the mailing list, I'll take a look when possible.
  * From there, we should be able to determine what the issue is.

I know it sounds smug, but I will not debug your application. 9 out of 10 times, it's a bug on the application code, not a BulkLoader instance. Your are better equipped to determine how your application should behave then I am.

### Works locally, does not work online! ###
It does. BulkLoader itself doesn't do any magic related to local or online files. BulkLoader itself is not even aware of where it's running or where it is loading assets from. Most of the time the issues relate either to missing assets / urls or security issues. The flash player security model is very complex and confusing. It depends on a lot of things: the  urls and their hosts, how the swfs are compiled, if you are using assets as data or as assets and the crossdomains involved. It's very time consuming to go through all of this for each user encountering an issue.
If you have things running locally, but they fail once online, the first step you must take, is to gather information on what is going on. Some tips:
  * BulkLoader is pretty good at displaying the relevant information.  Set the log level (bulkLoader.logLevel = BulkLoader.LOG\_VERBOSE) and read what is coming out of the trace statments. The player's trace, when ran on the debugging player will write to a text file. If you don't know how to read the trace output on the player, google is your friend [http://www.google.com/search?hl=en&rls=en-us&q=flash+player+trace+text+file&btnG=Search ](.md).
  * Use a tool to inspect the requests the player is making. Webkit's [http://www.webkit.org](.md) inspector panel is wonderful, but other might enjoy httpfox or firebug. This will give you a good indication of crossdomains being loaded and so forth.
  * Check out how the LoaderContext can be specified with BulkLoader ( hint: bulkLoader.add("some.swf", {context:myLoadingContext}); ).


### Videos' audio are playing while loading!? ###
Yes they are. The flash player, once it starts to stream a NetStream object, will, in fact stream it, making the the sound audible even if your NetStream is not visible or added to the display list. The solution is to pause it. BulkLoader can do it for you if you ask it politely, by setting the "pauseAtStart" property to true:
```
bulkLoader.add("video.flv", {"pausedAtStart":true});
```

### getBitmap() always returns the same Bitmap, how do I clone it? ###
The flash player, when loading an image, binds that loaded asset to a Bitmap reference. This means, that all bulkLoader.getBitmap("some-id") calls will return the same Bitmap object. The code bellow:
```
var bmp1 : Bitmap = bulkLoader.getBitmap("logo");
addChild(bmp1); // works as expected
var bmp2 : Bitmap = bulkLoader.getBitmap("logo"); // ohoh, bmp2 is the same object as bmp1 !
addChild(bmp2);// works, but you only have one bitmap, not two!
```
The solution is simple for Bitmap objects, just create a new one from the bitmap data:
```
var bmp1 : Bitmap = new Bitmap(bulkLoader.getBitmapData("logo").clone());
var bmp2 : Bitmap = new Bitmap(bulkLoader.getBitmapData("logo").clone()); // this works, they are cloning bitmaps 
```
The reason for BulkLoader not doing this automatically, is that there are performance and memory penalties for doing so, so it delegates that option to the user.
This is true for a variety of loading types. A few can be easily cloned: text, xml, images. DisplayObjects do not allow for easy cloning, so for those cases (swfs, flvs) you might simply add the same urls with different ids.

### I want to have extra data associated to a key ###
Sometimes, when using lazyloaders (XML or JSON), you want to stick arbitrary, application data to a given key.
While BulkLoader does not support that out of the BOX, it's easy enough to extend it. This is a high level overview of how to do it:
```
class UserDataBulkLoader extends BulkLoader

	public var userData : Dictionary
	
	override public function add(key:*, props:*=null) : LoadingItem{
		if (props.hasOwnProperty('userData')){
			userData[key] = props.userData;
		}
		return super.add(key, props)
	}
	
	override public function remove(key) : Boolean{
		del userData[key];
		return super.remove(key);
	}
	
	public function getUserData (key :*) : *{
		return userData[key];
	}
}
```
The code above is just an implementation roadmap, and hasn't been tested, but should get you very close to it.
If using a xml o json loader, you might have to set it to extend your derived class and make any special arrangements to parse the node to the UserDataBulkLoader.


### I would like to thank you making a donation. ###
Thank you for your gesture, but I do not accept donations. Honestly, if I though I could get rich with it, I would, but it doesn't look very likely ;). Instead all of the bellow will show you gratitude just fine:
  * A simple thank you email.
  * A simple thank you email to the mailing list.
  * A code / documentation contribution (see how to contribute bellow).
  * Answering newbies questions on the mailing list.
  * Post on your blog or tweet how BulkLoader has helped you: spread the joy.

If you still feel none of the above is really enough, then just contribute back to the community: write or help out with other open source projects that save you, and countless others many hours of grunt work.

### How can I contribute? ###
BulkLoader is an open source project, and as such there are a lot of ways you can help out. Any of these will be very much appreciated:
  * Answering people's question on the mailing list.
  * Write a well though bug report at the issue tracker
  * Help to triage, investigate, and fix open bug report
  * Write patches that fixes bug or implement something important. If you are considering writing a new feature, it is best to discuss your ideas on the mailing list so that you don't waste your time on something that is not aligned to the core objectives of the project .

### Can I have commit privileges? ###
This one surprises me, people ask this often. I have had about ten such requests. No one that has asked has ever written patches or contributed to the project in any way.
Becoming a committer means that you will have full powers to do anything that you'd like, for the good or not. It means that I trust your code 100%. It means that I know you won't break things, that you will always run the test suite before a commit. In short, becoming a committer is a process, and that privilege is the final touch. If you show a history of good contributions (see "How can I contribute?" above), I'll be glad to ask you to become a committer. But as the name implies, a committer takes commitment, you must show that you have the ability and availability to help out.
Honestly, I'd love to have other committers. Even though it is a small project, all of it's tasks (writing documentation, mailing list, bug fixes, triaging bug reports ) take time. Many times, requests on the mailing list or at the issue tracker are left too long without a response, but I simply do not have that much time to devote into it. I'd love to share the work.