# Content Delievery Network

A content delivery network (CDN) is a globally distributed network of proxy servers, serving content from locations closer to the user. Generally, static files such as HTML/CSS/JS, photos, and videos are served from CDN, although some CDNs such as Amazon's CloudFront support dynamic content.

the proxy server location are called point of presence. the server inside is edge server.

## working

All proxy server has same ip.the DNS sends request to nearest ip address.

it reduces the high time effect of TLS handshake.

### types of CDNs.

1.push CDN

Push CDNs receive new content whenever changes occur on your server. You take full responsibility for providing content, uploading directly to the CDN and rewriting URLs to point to the CDN. You can configure when content expires and when it is updated. Content is uploaded only when it is new or changed, minimizing traffic, but maximizing storage.

2.pull CDN

Pull CDNs grab new content from your server when the first user requests the content. You leave the content on your server and rewrite URLs to point to the CDN. This results in a slower request until the content is cached on the CDN.

A time-to-live (TTL) determines how long content is cached. Pull CDNs minimize storage space on the CDN, but can create redundant traffic if files expire and are pulled before they have actually changed.

Sites with heavy traffic work well with pull CDNs, as traffic is spread out more evenly with only recently-requested content remaining on the CDN.

#### disadvantages

1.it cost depend on traffic of region.
2.CDN require chnage URLs for static content

popular CDNS: CloudFare,Akamai,AWS CloudFront

