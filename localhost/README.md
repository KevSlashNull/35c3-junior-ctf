# localhost
## Challenge Details
> We came up with some ingenious solutions to the problem of password reuse. For users, we don't use password auth but send around mails instead. This works well for humans but not for robots. To make test automation possible, we didn't want to send those mails all the time, so instead we introduced the localhost header. If we send a request to our server from the same host, our state-of-the-art python server sets the localhost header to a secret only known to the server. This is bullet-proof, luckily.
> 
> http://35.207.132.47/
> 
> Difficulty Estimate: Medium
> 
> ---
> Good coders should learn one new language every year.
>
> InfoSec folks are even used to learn one new language for every new problem they face (YMMV).
> 
> If you have not picked up a new challenge in 2018, you're in for a treat.
> 
> We took the new and upcoming Wee programming language from paperbots.io. Big shout-out to Mario Zechner (@badlogicgames) at this point.
> 
> Some cool Projects can be created in Wee, like: this, this and that.
> 
> Since we already know Java, though, we ported the server (Server.java and Paperbots.java) to Python (WIP) and constantly add awesome functionality. Get the new open-sourced server at /pyserver/server.py.
> 
> Anything unrelated to the new server is left unchanged from commit dd059961cbc2b551f81afce6a6177fcf61133292 at badlogics paperbot github (mirrored up to this commit here).
> 
> We even added new features to this better server, like server-side Wee evaluation!
> 
> To make server-side Wee the language of the future, we already implemented awesome runtime functions. To make sure our VM is 100% safe and secure, there are also assertion functions in server-side Wee that you don't have to be concerned about.

## Breakdown

According to the challenge description, we are looking for a “localhost header”. By looking through the code of the `server.py` ([source](https://github.com/KevSlashNull/35c3-junior-ctf/blob/master/_paperbots/server.py)), you can quickly find the following method.

```py
@app.after_request
def secure(response: Response):
    if not request.path[-3:] in ["jpg", "png", "gif"]:
        response.headers["X-Frame-Options"] = "SAMEORIGIN"
        response.headers["X-Xss-Protection"] = "1; mode=block"
        response.headers["X-Content-Type-Options"] = "nosniff"
        response.headers["Content-Security-Policy"] = "script-src 'self' 'unsafe-inline';"
        response.headers["Referrer-Policy"] = "no-referrer-when-downgrade"
        response.headers["Feature-Policy"] = "geolocation 'self'; midi 'self'; sync-xhr 'self'; microphone 'self'; " \
                                             "camera 'self'; magnetometer 'self'; gyroscope 'self'; speaker 'self'; " \
                                             "fullscreen *; payment 'self'; "
        if request.remote_addr == "127.0.0.1":
            response.headers["X-Localhost-Token"] = LOCALHOST

return response
```

Note: The `LOCALHOST` variable is the flag as a string. [Learn More](https://github.com/KevSlashNull/35c3-junior-ctf/blob/master/_paperbots/WHAT_ARE_FLAGS.md).

In order to obtain the key, a request has to be made to the server where the `remote_address` is 127.0.0.1 (aka. localhost). That means the challenge is about finding a way to perform a SSRF (Server Side Request Forgery). And unsurprisingly, there is a route on the URL `/api/proxyimage`.

```python
@app.route("/api/proxyimage", methods=["GET"])
def proxyimage():
    url = request.args.get("url", '')
    parsed = parse.urlparse(url, "http")  # type: parse.ParseResult
    if not parsed.netloc:
        parsed = parsed._replace(netloc=request.host)  # type: parse.ParseResult
    url = parsed.geturl()

    resp = requests.get(url)
    if not resp.headers["Content-Type"].startswith("image/"):
        raise Exception("Not a valid image")

    # See https://stackoverflow.com/a/36601467/1345238
    excluded_headers = ['content-encoding', 'content-length', 'transfer-encoding', 'connection']
    headers = [(name, value) for (name, value) in resp.raw.headers.items()
               if name.lower() not in excluded_headers]

    response = Response(resp.content, resp.status_code, headers)
    return response
```

## Solution

And now we just need to find a suitable `url` GET parameter. As the server (`server.py` last line) binds to 0.0.0.0:8075, that can be used as parameter. But the image can not end with a `.jpg`, `.png` or `.gif`. And although custom images can be uploaded (as thumbnail, see `savethumbnail()`), that images will always end with `.png`. And any file other than an image may not be used because the `proxyimage()` requires the `content-type` header to start with `ìmage/`. The `favicon.ico` of
the site is a `image/vnd.microsoft.icon` which satisfies both requirements.

When requesting `http://35.207.132.47/api/proxyimage?url=http://0.0.0.0:8075/favicon.ico`, the flag can be found in the response‘s `X-Localhost-Token`.

```
35C3_THIS_HOST_IS_YOUR_HOST_THIS_HOST_IS_LOCAL_HOST
```

## Comment

It did not took me long to find a way to be able to log in. “Phew, we totally did not set up our mail server yet” helped me to spot the “bug”. Because I solved the challenge in the browser, I was wondering why I was not given the flag once I logged in. I really tend to overlook simple things.
