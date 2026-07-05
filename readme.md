# System Design

System design case studies, mocks, and reference materials.

## Case Studies

| # | File | Description |
|---|------|-------------|
| 1 | [ChatApp](1.ChatApp.md) | Basic chat application |
| 2 | [Rate Limiter](2.RateLimiter.md) | Rate limiting algorithms & design |
| 3 | [Notification System](3.NotificationSystem.md) | Push/email/SMS notification delivery |
| 4 | [File Storage System](4.FileStorageSystem.md) | Distributed file storage |
| 5 | [Consistent Hashing](5.ConsistentHashing.md) | Consistent hashing deep-dive |
| 6 | [Key-Value Store](6.KeyValueStorage.md) | Distributed key-value storage |
| 7 | [ID Generator](7.ID-Generator.md) | Unique ID generation (Snowflake etc.) |
| 8 | [URL Shortener](8.URL-Shorten.md) | URL shortening service |
| 9 | [Web Crawler](9.WebCrawler.md) | Scalable web crawler |
| 10 | [News Feed](10.NewsFeed.md) | News feed system |
| 11 | [Search Autocomplete](11.SearchAutoComplete.md) | Search autocomplete & top-K |
| 12 | [YouTube](12.YouTube.md) | Video streaming platform |
| 13 | [Google Drive](13.GoogleDrive.md) | Cloud file storage |
| 14 | [Proximity Service](14.ProximityService.md) | Location-based proximity search |
| 15 | [Nearby Friends](15.NearbyFriends.md) | Real-time nearby friends |
| 16 | [Google Maps](16.GoogleMaps.md) | Mapping & navigation system |
| 17 | [Distributed Msg Queue](17.DistributedMsgQueue.md) | Distributed message queue |
| 18 | [Metrics Monitor & Alert](18.MetricsMonitor&AlertSystem.md) | Metrics monitoring & alerting |
| 19 | [Ad Click Aggregation](19.AdClickEventAggregation.md) | Ad click event aggregation |
| 20 | [Hotel Reservation](20.HotelManagement.md) | Hotel reservation system |
| 21 | [Distributed Email](21.DistributedEmailService.md) | Distributed email service |
| 22 | [S3 Object Storage](22.S3-Object-Storage.md) | S3-like object storage |
| 23 | [Gaming Leaderboard](23.RealTimeGamingLeaderBoard.md) | Real-time gaming leaderboard |
| 24 | [Payment System](24.PaymentSystem.md) | Payment processing system |
| 25 | [Digital Wallet](25.DigitalWallet.md) | Digital wallet |
| 26 | [Stock Exchange](26.StockExchange.md) | Stock exchange system |

---

## Real-world Systems

The following materials can help you understand general design ideas of real system architectures behind different companies.

### Facebook

- [Facebook Timeline: Brought To You By The Power Of Denormalization](https://goo.gl/FCNrbm)
- [Scale at Facebook](https://goo.gl/NGTdCs)
- [Building Timeline: Scaling up to hold your life story](https://goo.gl/8p5wDV)
- [Erlang at Facebook (Facebook chat)](https://goo.gl/zSLHrj)
- [Facebook Chat](https://goo.gl/qzSiWC)
- [Finding a needle in Haystack: Facebook's photo storage](https://goo.gl/edj4FL)
- [Serving Facebook Multifeed: Efficiency, performance gains through redesign](https://goo.gl/adFVMQ)
- [Scaling Memcache at Facebook](https://goo.gl/rZiAhX)
- [TAO: Facebook's Distributed Data Store for the Social Graph](https://goo.gl/Tk1DyH)

### Amazon

- [Amazon Architecture](https://goo.gl/k4feoW)

### Netflix

- [A 360 Degree View Of The Entire Netflix Stack](https://goo.gl/rYSDTz)
- [It's All A/Bout Testing: The Netflix Experimentation Platform](https://goo.gl/agbA4K)
- [Netflix Recommendations: Beyond the 5 stars (Part 1)](https://goo.gl/A4FkYi)
- [Netflix Recommendations: Beyond the 5 stars (Part 2)](https://goo.gl/XNPMXm)

### Google

- [Google Architecture](https://goo.gl/dvkDiY)
- [The Google File System (Google Docs)](https://goo.gl/xj5n9R)
- [Bigtable: A Distributed Storage System for Structured Data](https://goo.gl/6NaZca)

### Instagram

- [Instagram Architecture: 14 Million Users, Terabytes Of Photos, 100s Of Instances, Dozens Of Technologies](https://goo.gl/s1VcW5)

### Twitter

- [The Architecture Twitter Uses To Deal With 150M Active Users](https://goo.gl/EwvfRd)
- [Scaling Twitter: Making Twitter 10000 Percent Faster](https://goo.gl/nYGC1k)
- [Announcing Snowflake](https://goo.gl/GzVWYm) (Snowflake is a network service for generating unique ID numbers at high scale with some simple guarantees)
- [Timelines at Scale](https://goo.gl/8KbqTy)

### Other Companies

- [How Uber Scales Their Real-Time Market Platform](https://goo.gl/kGZuVy)
- [Scaling Pinterest](https://goo.gl/KtmjW3)
- [Pinterest Architecture Update](https://goo.gl/w6rRsf)
- [A Brief History of Scaling LinkedIn](https://goo.gl/8A1Pi8)
- [Flickr Architecture](https://goo.gl/dWtgYa)
- [How We've Scaled Dropbox](https://goo.gl/NjBDtC)
- [The WhatsApp Architecture Facebook Bought For $19 Billion](https://bit.ly/2AHJnFn)

## Company Engineering Blogs

Here is a list of engineering blogs of well-known large companies and startups:

- [Airbnb](https://medium.com/airbnb-engineering)
- [Amazon](https://developer.amazon.com/blogs)
- [Asana](https://blog.asana.com/category/eng)
- [Atlassian](https://developer.atlassian.com/blog)
- [Bittorrent](http://engineering.bittorrent.com)
- [Cloudera](https://blog.cloudera.com)
- [Docker](https://blog.docker.com)
- [Dropbox](https://blogs.dropbox.com/tech)
- [eBay](http://www.ebaytechblog.com)
- [Facebook](https://code.facebook.com/posts)
- [GitHub](https://githubengineering.com)
- [Google](https://developers.googleblog.com)
- [Groupon](https://engineering.groupon.com)
- [Highscalability](http://highscalability.com)
- [Instacart](https://tech.instacart.com)
- [Instagram](https://engineering.instagram.com)
- [LinkedIn](https://engineering.linkedin.com/blog)
- [Mixpanel](https://mixpanel.com/blog)
- [Netflix](https://medium.com/netflix-techblog)
- [Nextdoor](https://engblog.nextdoor.com)
- [PayPal](https://www.paypal-engineering.com)
- [Pinterest](https://engineering.pinterest.com)
- [Quora](https://engineering.quora.com)
- [Reddit](https://redditblog.com)
- [Salesforce](https://developer.salesforce.com/blogs/engineering)
- [Shopify](https://engineering.shopify.com)
- [Slack](https://slack.engineering)
- [Soundcloud](https://developers.soundcloud.com/blog)
- [Spotify](https://labs.spotify.com)
- [Stripe](https://stripe.com/blog/engineering)
- [System design primer](https://github.com/donnemartin/system-design-primer)
- [Twitter](https://blog.twitter.com/engineering/en_us.html)
- [Thumbtack](https://www.thumbtack.com/engineering)
- [Uber](http://eng.uber.com)
- [WhatsApp](https://github.com/Bhup-GitHUB/wp-des)
- [Yahoo](https://yahooeng.tumblr.com)
- [Yelp](https://engineeringblog.yelp.com)
- [Zoom](https://medium.com/zoom-developer-blog)

---

[engineering blogs](https://github.com/kilimchoi/engineering-blogs)

[machine-coding](https://workattech.github.io/machinecoding/)

[lld & machine coding](https://github.com/lavakumarThatisetti/Machine-Coding-Round)

[lld primer](https://github.com/prasadgujar/low-level-design-primer)

[awesome-low-level-design](https://github.com/ashishps1/awesome-low-level-design)

[awesome-system-design-resources](https://github.com/ashishps1/awesome-system-design-resources)

[system-design-101](https://github.com/ByteByteGoHq/system-design-101)

---

to do:

[upi](https://codefarm0.medium.com/designing-a-upi-payment-system-real-time-processing-bank-integration-transaction-routing-0bb4bb93e12a)
