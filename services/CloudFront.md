# Amazon CloudFront
It's a content delivery network (CDN), a system of distributed servers that deliver webpages and other web content to users based on their geographic location, the origin of tha page and content delivery server

## Useful Links
- [AWS - CloudFront FAQ](https://aws.amazon.com/cloudfront/faqs)

## General Notes
- Can be used to delivery your entire website, including dynamic, static, streaming and interactive content using AWS global network of edge locations
- Requests for your content are automatically routed to the nearest edge location, so it is delivered with the best possible performance
- Is usually used to **optimize performance** for users accessing a website backed by S3
- Content is distributed to **edge locations** and cached
- It is separate to an AWS region/AZ (there are much more edge locations)
- **Origin** is the original location of the files, can be a S3 bucket, EC2 instance, ELB or Route53
- **Distribution** is the name of the CDN, consists of a collection of edge locations, which can be of two types:
    - **Web Distribution** for webistes (HTTP/HTTPS)
    - **RTMP Distribution** for media streaming / flash multi-media content (Adobe Real Time Messaging Protocol)
        - [RTMP won't be supported from 31-12-2020](https://forums.aws.amazon.com/ann.jspa?annID=7356) 
- Cache expires automatically in a given TTL (default 24h), you can clear it manually at any time, but you will be charged
- Is optmizied to work with other AWS services, such as S3, EC2, ELB and more, but can also be used with any non-AWS origin server, which stores the original, definitive versions of your files
- Edge location **are not just read only**, you can write too, to PUT an object on S3 bucket for example
    - Edge locations are used by **S3 Transfer Acceleration** to reduce latency for S3 uploads
- You can **restrict bucket access** to enforce all requests to go through CloudFront
- You can **restrict content access** using **signed URLs or signed cookies**, a common use case is to distribute paid content
- **Geo-Restrictions** can be used to allow/deny access per geographic location