THIS WIKI HAS MOVED to the new github page, please access it at http://github.com/arthur-debert/BulkLoader/wiki .

BulkLoader has a few useful features to control loading order. To understand how BulkLoader queues order it's items, there are a few things to understand first.

Glossary:
  * We call a **connection**, a loading operation that starts.
  * We call a **host name** the host part of the loading url (example.com, www.example.com, something.example.com). Most commonly it's the domain name and subdomains (if any). Different subdomains on the same domain are different hosts.

## Priority ##
The "priority" prop for bulkLoader.add sets the order of loading. High numbers will begin load first. If two items have the same value for priority, the one added first will take precedence.

### BulkLoader can control when connections are open, not when they are completed. ###
This is a common misconception. If we are loading, say image.jpg and movie.swf, we might start the loading operation in one order, but we do not know who finishes first. For example, if image.jpg has 10kb and movie.swf has 10MB, even if we start to load the swf first, image.jpg will probably finish loading first (much less bytes to transfer). We say probably, because many other factors play a role in this. Maybe the image is generated through a backend script, that takes a long time to create. Maybe the swf is already on the browser's cache. HTTP does not guarantee any order at all. So among other factors the time of completion for the connection depends on:
  * file sizes
  * server performance
  * proxy servers
  * caches between the flash code and the file (including user's browser)
  * tcp routing
In short, many factors not under BulkLoader's control at all. This is the way the web works, and nothing much can be done about it.

If you **need** to make the loading sequential, meaning that you must be sure of loading order, then the only way to do this is not to load in parallel. That way you can be sure that when a new connection is open, the previous one is done. This can be achieved setting the numConnections param to the constructor or start method to 1.

### Controlling parallel connections. ###
Using parallel connections improves the total loading time, because of RTT. Google has a good explanation here more in details here[1](http://code.google.com/speed/page-speed/docs/rtt.html). This means that you want to open several connections at the same time. There is a limit to this though, opening 200 connections simultaneously is not good. It's hard to have a final number, but for most cases between 2 and 5 host names with 2 connections per host name is reasonable (your milage may vary).

The numConnections param described above says that BulkLoader will make up to that number of parallel connections. It's an upper limit, it will never be more than that, but it might be less. This is caused by connection pooling on host names, or host sharding.

HTTP determines that clients should not make more that 2 connections to the same host at one time. This is configurable both on the server and on the client (browser), but unless you are controlling both end points, it's safe guess that only two connections will be open at the same time. So you want to:
- load from more that one host at the same time
- keep the number of connections per host low
- share resources between hosts.
Sometimes, this is something that comes naturally from you content, parts of it are accessing flickr photos, other twitter feeds and others are loading from you server. But you can also shard content on your own host, for example, mirroring files across a few subdomains. That way you can open more connections at the same time. Again, google has a very good write up on this[2](http://code.google.com/speed/page-speed/docs/rtt.html#ParallelizeDownloads).

By default, BulkLoader will parse your URLS and parallelize loadings between hosts. The number of connections per host name, is controlled with the maxConnectionsPerHost property. This has a few important implications on the loading queue.

Priority only determines which hosts we will open connection from first, and the order of loading between items on the same host. You might end up with an item with a higher priority beginning loading after an item with a lower priority. For example, if you have 6 items to load from example.com, with priorities higher than 100, and 4 items with 0 priority on media.example.com, BulkLoader will open two items on example.com first (they have higher priority), respecting the priority between them. Then it will load two items from media.example.com. By then, you might have items with a 0 priority loading first that items with priority 100, because that host name is full ( the connections for it have reached the maxConnectionsPerHost). This might get confusing in some situations, but should work fine most of the time. You just need to think it through if you need the loading order to be predictable. Again, if you must be absolutely sure of loading order, you must either set numConnections to 1 (effectively disabling parallel downloads) or set your priorities in tandem with the host names.