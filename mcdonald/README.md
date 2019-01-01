# McDonald
## Challenge Details
> Our web admin name's "Mc Donald" and he likes apples and always forgets to throw away his apple cores..
> 
> http://35.207.132.47:85

If you visted the website, you would have seen the following site.

### Page Source

Note: I have minified the CSS for better readability.

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link rel="stylesheet" href="/assets/css/bootstrap.min.css">
    <title>Mc Donald</title>
    <style>html{position:relative;min-height:100%}body{padding-top:5rem;margin-bottom:60px}.starter-template{padding:3rem 1.5rem;text-align:center}.footer{position:absolute;bottom:0;width:100%;height:60px;line-height:60px;background-color:#f5f5f5}</style>
  </head>
  <body>

    <nav class="navbar navbar-expand-md navbar-dark bg-dark fixed-top">
      <a class="navbar-brand" href="/">Mc Donald</a>
    </nav>

    <main role="main" class="container">

      <div class="starter-template">
        <h1>Mc Donald</h1>
	      <p class="lead">Our web admin name's "Mc Donald" and he likes apples and always forgets to throw away his apple cores...</p>
      </div>
    </main>

    <footer class="footer">
      <div class="container">
        <span class="text-muted">With love from <a href="https://twitter.com/gehaxelt">@gehaxelt</a> for the 35C3 Junior CTF and ESPR :-)</span>
      </div>
    </footer>

    <script src="/assets/js/jquery-3.3.1.slim.min.js"></script>
    <script src="/assets/js/popper.min.js"></script>
    <script src="/assets/js/bootstrap.min.js"></script>
  </body>
</html>
```

### Page Image
![McDonald](https://i.imgur.com/NJA5zfi.png)

## Breakdown

The web page itself nor it‘s headers seem to be interesting. There are no scripts or links to files on the server. The only potential solution that I thought of was that there must be a `.DS_Store` file since the web admin “likes apples and always forgets to throw away **apple cores**”.

You will find the URL of a `.DS_Store` file in the `robots.txt` ([source](https://github.com/KevSlashNull/35c3-junior-ctf/blob/master/macdonald/robots.txt)). There are multiple libraries that can be used to analyze a `.DS_Store`. The creator of the challenge, gehaxelt, even has an [own implementation of that](https://github.com/gehaxelt/Python-dsstore) on his GitHub.

After analyzing, you‘ll get a list of multiple file paths on the server. You can explore all files [here](https://github.com/KevSlashNull/35c3-junior-ctf/blob/master/mcdonald/35.207.132.47_85). More importantly, that list contains a file called `flag.txt` ([source](https://github.com/KevSlashNull/35c3-junior-ctf/blob/master/mcdonald/35.207.132.47_85/backup/b/a/c/flag.txt)) in the directories `backup/b/a/c/`. It contains the flag.

```
35c3_Appl3s_H1dden_F1l3s
```

## Comment

I did not completely solve this challenge on my own because I did not think of a `robots.txt` when I was searching for a `.DS_Store`. A tiny hint on the challenge IRC helped me find the file but I was lost extracting a path from the `.DS_Store`. Finally, someone sent me the link to [this repo](https://github.com/lijiejie/ds_store_exp) with which I was able to obtain the flag. So, although this challenge has 214 solves and was *very* easy after I got some help. But I will learn from this
challenge that I should check common files like `robots.txt` before looking for other ways.
