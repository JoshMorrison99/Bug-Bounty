**Step 1**<br/>
Find self-XSS in headers using this [script](https://github.com/JoshMorrison99/Bug-Bounty/blob/main/Cache%20Poisoning/reflected-headers.yaml). It is better to try for XSS in cookies.

<br/>
<br/>

**Step 2**<br/>
Find cache oracle
- [ ] `X-Cache` response header specifying a cache hit/miss
- [ ] Use `Pragma: akamai-x-get-cache-key` if akamai CDN

<br/>
<br/>

**Step 3**<br/>
Try to cache the self-xss by doing the following:
- [ ] Check for cachable extension
	- [ ] `GET /blog/.js?cb=1`
	- [ ] `GET /blog/.css?cb=1`
	- [ ] `GET /blog/.png?cb=1`

<br/>
<br/>

**When to Give up**<br/>
- [ ]  Cache-Control: no-cache, must-revalidate
- [ ]  Cache-Control: no-cache, private
- [ ]  If the target is Cloudflare and the XSS is in the following then cache poisoning is not possible because those values become part of the cache key if they differ from the origin server.:
	- `x-forwarded-host`
	- `x-host`
	- `x-forwarded-scheme` 
	- `x-http-method-override`
	- `x-http-method`
	- `x-method-override`
	- `x-original-url`
	- `x-rewrite-url`
	- `forwarded`

