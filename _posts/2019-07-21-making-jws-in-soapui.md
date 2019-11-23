---
layout: post
title:  "Creating JWS in SoapUI"
---

(As with all my blog posts), Here is a neat trick someone at showed me today which I thought was absolutely fabulous.

### Use Case

I need to create a JWS on the fly during my SoapUI testsuite's execution. (i.e. I can't create one in [jwt.io](jwt.io) or some other tool and then bake it into my tests.) I need to have valid iat and exp claims, and I might need to dynamically/programmatically vary the claims inside the JWS payload. I was also required to use SoapUI (because I was working at SoapUI-heavy organisation), even though generating dynamic JWS's is **much easier in Postman**.

In the past I've seen people creating HTTP-enabled services that generate custom tokens for their test harnesses however given that requires a whole web-server or application to be spun up just to run a test, that solution feels like using a sledgehammer-to-crack-a-nut to me.

### Trying to Use the "Native" SoapUI Function

Soapui's JWT creation functionality seems to be very unhackable. The first [google result from Smartbear](https://support.smartbear.com/readyapi/docs/projects/requests/auth/types/oauth2/generate-jwt.html) seems to tell me that I need to have the paid-for ReadyAPI, and even then the JWS generation is pretty locked-down to inside the OAuth Authorization Options. _(N.B. I'm not against the paid-for ReadyAPI option, I think it is pretty good- especially the "composite project" stuff for meshing with Version Control. I just know that **a)** waiting for an organisation to procure licenses for ANY product is just the worst thing in a software delivery project **b)** I want something that I can use anywhere, not just places were everyone has ReadyAPI.)_

### Putting a JWS together using the rsasign.js Library

Here is the solution that I've seen work and that I've now implemented.

At a high-level, the instructions are:

1. Download the rsasign.js library
2. Change a Setting on your SoapUI project
3. Update the JS library (Rhino) on the default distribution of SoapUI
4. Copy-paste some code snippets.

#### Notes on Rhino

To understand how SoapUI runs JS, it uses `Rhino`. I had heard of Rhino before but had never really scratched the surface. Rhino is an implementation of JS in Java. SoapUI-5.5.0 (which is the version I have pulled using `brew cask install soapui`) comes with Rhino 1.7R2. You can read more about Rhino on [MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino) or [Github](https://github.com/mozilla/rhino).

##### Disclaimer on the Javascript-in-SoapUI Ecosystem!

_Be aware_- JS snippets for SoapUI tests seem to be pretty rare on Google. SoapUI scripting seems to be pretty-much Groovy-focussed so there doesn't seem to be much in the way of blogs/help. If you are a JS-through-and-through person, _you WILL find the Postman route easier_. Also check out this [compatibility table here](http://mozilla.github.io/rhino/compat/engines.html) if you are a JS aficionado and none of your cool ES6 tricks work (Rhino does not have 100% coverage of ES6).

### Detailed Instructions

1. Use the [free-ish(MIT and BSD)](https://github.com/kjur/jsrsasign/blob/master/LICENSE.txt) library `rsasign.js`. Download [this explicit file](https://github.com/kjur/jsrsasign/blob/master/jsrsasign-all-min.js) or [download a full release](https://github.com/kjur/jsrsasign/releases) and unzip it to your local machine.

2. Open SoapUI and create a New Project.

![](/images/2019-07-21-making-jws-in-soapui.2.png)

3. Set your SoapUI project to run script as Javascript. In the GUI, In the Navigator pane, click your project. Then in the **Project Properties** pane, change the value of the `Script Language` property from **Groovy** to **Javascript**.

![](/images/2019-07-21-making-jws-in-soapui.3.png)

4. Restart SoapUI. (I have found that SoapUI doesn't pick up on the Script Language change without a restart).

5. (Optional) Create a ~~Groovy TestStep~~ Javascript TestStep and paste in the following snippet (Shamelessly cribbed from this [StackOverflow question](https://stackoverflow.com/questions/22989872/how-can-i-get-the-version-of-rhino-from-within-javascript))

```
var Context = org.mozilla.javascript.Context,
    currentContext = Context.getCurrentContext(),
    rhinoVersion = currentContext.getImplementationVersion();

log.info(rhinoVersion);
```

![](/images/2019-07-21-making-jws-in-soapui.4.png)

6. Update the version of Rhino bundled inside SoapUI (these instructions shamelessly cribbed from this [Smartbear forum question](https://community.smartbear.com/t5/SoapUI-Open-Source/How-to-parse-JSON-in-javascript/td-p/133474)).

	a. Pull down the latest version of Rhino from the [Mozilla website here](https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Rhino/Download_Rhino).

	b. In the SoapUI `\lib` folder (`/Applications/SoapUI-5.5.0.app/Contents/java/app/lib` was my filepath), replace `js-1.7R2.jar` with `js-<rhino version>.jar` from the downloaded zip.

	![](/images/2019-07-21-making-jws-in-soapui.5.png)
	![](/images/2019-07-21-making-jws-in-soapui.6.png)
	![](/images/2019-07-21-making-jws-in-soapui.7.png)
	![](/images/2019-07-21-making-jws-in-soapui.8.png)

	c. Restart SoapUI.

	d. (Optional) Run the snippet to print out the Rhino version again.

	![](/images/2019-07-21-making-jws-in-soapui.9.png)

7. Make a Script Teststep and copy-paste the following code, adding the contents of the [minified rsasign file](https://github.com/kjur/jsrsasign/blob/master/jsrsasign-all-min.js) as a massive one-liner and pasting in your own secret key.

```
var navigator = {}; //fake a navigator object for the lib
var window = {}; //fake a window object for the lib

// Minified .js is copy-pasted as a big one-liner
/* * jsrsasign(all) 8.0.12 (2018-04-22) (c) 2010-2018 Kenji Urushima ...snip...

var header = {
    "alg": "PS256",
    "typ": "JOSE",
    "cty": "application/json"
};

var data = {
	"Hello": "World"
}

// Paste in the private key
var secret = "-----BEGIN PRIVATE KEY----- MI...snip...== -----END PRIVATE KEY-----";
var algo = "PS256";

var sHeader = JSON.stringify(header);
var sPayload = JSON.stringify(data);

var sJWT = KJUR.jws.JWS.sign(algo, sHeader, sPayload, secret);

log.info(sJWT);

```

![](/images/2019-07-21-making-jws-in-soapui.10.png)

4. Adapt to suit your needs! Transfer the JWS to your next teststep, load your own header and payload sections, slice this up as necessary.

### Afterword

I'm pretty sure this is one of those scripts that has been cribbed/passed-around/stolen over and over again. The first/earliest place I could find it on Google was on [TheKoguryo's github site](https://thekoguryo.github.io/oci/chapter14/3/4/), however it seems to appear in many different forms in different snippets over SO and the blogosphere.

It was very interesting googling and seeing how other people had used this!

#### References

Helen Kosova's Answer on this Smartbear Forum Post: [https://community.smartbear.com/t5/SoapUI-Open-Source/How-to-parse-JSON-in-javascript/td-p/133474](https://community.smartbear.com/t5/SoapUI-Open-Source/How-to-parse-JSON-in-javascript/td-p/133474)

TheKoguryo's Github Blog where they implement this for Postman, in Korean(?) [https://thekoguryo.github.io/oci/chapter14/3/4/](https://thekoguryo.github.io/oci/chapter14/3/4/)


