---
layout: page
type: text
title: Film in the Cloud
categories: 
- programming
- pictures
---
After a long hiatus I finally managed to get some films off to be developed which meant I could complete [this set](http://www.flickr.com/photos/i-5-m/sets/72157629646923030/). In the process of sorting those out I realised that I'd completely forgotten about the other films I'd had developed last year which also were not uploaded anywhere yet. And then I recalled the second big problem I have with my photography - not only is there the issue of getting stuff developed in the first place, but then I face just as big a hurdle in getting on the computer to process and upload it. I must have just given up at some point last year, hence completely forgetting about these other films.

I've decided I can finally be Apple free (not that I want to be - I just can't get on that computer). To date I've used Aperture (and before that iPhoto) to store and process all my film scans before uploading to Flickr. And since I've mostly always entered tags, titles, and descriptions within Aperture before uploading, it also acts as a handy Flickr backup. 

So that means that for everything to date, I can just leave my Aperture library as is to act as a historical record. But From now on, instead of using Aperture, I'm going to start uploading straight to Flickr and do all my editing/sorting/selection on there; editing will be very minimal, perhaps a crop and a straighten - "keeping in real!", or rather more to suit the backup strategy. For the purposes of backup, well, since I already have the negatives and the CDs of scans, all I really need to backup from Flickr is the metadata. And the likelihood is that there is already a script of some sort out there that does this via the Flickr API.

The only thing I need to do is to somehow maintain a record of the "links" between the photos on Flickr and the physical world, i.e. which image on which CD, etc. It would be fairly easy at upload to use machine tags to add a reference to the CD number or job number from the lab, but for backup purposes this still means manually linking up metadata with each frame from a roll, etc. It would be better if I could link to individual photos. One idea I had was just to ensure I always upload all frames from a CD and just "hide" those I don't want to use. That way I could use the date order of them to know which frame is which. However, this feels a bit clunky to me (I'll come to the limitations of Flickr as a workflow tool) so really it would be best to record the filename on the CD (or frame/neg number on the rare occasions I scan myself). Unfortunately though, [Flickr doesn't store the original file name](http://www.flickr.com/help/forum/39182/?search=originals) during upload so I will have to add it manually before/during upload.

I came up with this little Haskell script to help with adding these filename tags via [exiv2](http://www.exiv2.org/). I'm just using Haskell as an attempt to learn it - this is the same way I learnt Ruby by writing little scripts to help automate things. It could be implemented in any scripting language (bash or even something like Automator / Applescript).


{% highlight haskell %}

--exiv2tag
--tags photos with filename
--(Not really a proper Haskell programme)
--Uses rawSystem to call exiv2 externally that does the actual tagging

import System.Environment
import System.Cmd
import System.Directory
import Control.Monad

dispatch :: String -> [String] -> IO ()
dispatch "tag" = tag
dispatch "check" = check

tagprefix = "film:filename="

main = do
	files <- liftM (filter (`notElem` [".", ".."])) (getDirectoryContents ".")
	(command:_) <- getArgs
	dispatch command files

tag files = 
	mapM_ (\file -> rawSystem "exiv2" ["-M", "add Iptc.Application2.Keywords String "++tagprefix++file, file]) files
	
check files = 
	mapM_ (\file -> rawSystem "exiv2" $ ["-P", "I"]++[file]) files

{% endhighlight %}

It just gets a list of files in the current directory and adds a machine tag to each with the filename in this format `film:filename=<filename>`. It also includes a `check` command so I can see that it's added the tags.

## A Few Notes on Flickr as a Workflow Tool.

It's been awhile since I properly looked at alternatives, but one of the reasons I still stick with Flickr is that there has been a hell of a lot of effort put in over the years to provide good organisational tools. The Organizer itself hasn't seen a lot of recent development, but is still light years ahead of what is available in [OpenPhoto](http://theopenphotoproject.org/) for instance (nothing). Coupled with the [new Uploader](http://code.flickr.com/blog/2012/04/25/raising-the-bar-on-web-uploads/) it means I can comfortably eschew Aperture.

My current approach:

1. Tag the images with the filename (see above)
2. Upload via the new Uploader. Remove images I don't want, rotate if needed, add tags, add to sets, add titles and descriptions, change permissions  - roughly in that order. How much of this I do here depends on how brave I feel, since the nerve wracking side of this is that none of this is committed until you hit the "Upload to Photostream" button, so if your browser session crashed you'd have to start again. Having said that I've left it running in this stage over a day or so and had no trouble.
3. After upload use the Organizer to fix dates and add to map, etc. If I'd only done removal and rotations in the Uploader then I would have kept all images as private and added them to an "Incoming" set. I'd work with this set in Organizer to finish off adding metadata.

Having said, that it still isn't a proper workflow tool like Aperture (but I guess that was never its intention). A couple of things I miss are related to rating images:

- No way to quickly rate images. It would be nice to be able to do this in the new Uploader. In theory you could use tags for ratings, but it wouldn't be possible to sort by them so it's of limited use
- No equivalent to "rejecting" an image. As I mentioned above, it would be nice to upload a whole CD, but reject/hide images I don't want. The nearest equivalent to this in Flickr is making the image private, but this means I'd see a load of dud images in my photostream when browsing.

But that's all. I'm really quite happy with this approach, I wish I'd though to tag images with the filename and CD number before now!
