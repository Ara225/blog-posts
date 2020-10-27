I've made an interesting little project (hosted on a <a href="http://ii.aitchisonsoft.co.uk/">subdomain of my personal website</a>). The concept of it is that it's a search engine for icons from web icon packs. 

The indexing is done by running utilities/filterIconPack.py which parses the icon pack's CSS (metadata + links to the CSS are stored in utilities/iconPacks.json) into JSON. 

The frontend is a static site in S3, which makes API calls to a simple AWS lambda + API Gateway backend. The backend searches the icon data JSON file which fuse.js and returns the results.

It hasn't turned out as useful as I had hoped, since the way it's designed excludes many icon packs and I've failed to find a way of automatically collecting pack metadata. However, it's still a passably interesting project, so I decided to share. 