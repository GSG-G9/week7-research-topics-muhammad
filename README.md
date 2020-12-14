# week7-research-topics-muhammad

## HTTP vs HTTPS
 

![HTTP vs HTTPS](/images/http-vs-https.webp  "HTTP vs HTTPS")
 

**http:**
> Hypertext Transfer Protocol (**HTTP**) is an application-layer protocol for transmitting hypermedia documents, such as HTML. It was designed for communication between web browsers and web servers.
---
**https:**
> Hypertext Transfer Protocol Secure (HTTPS) is an extension of the Hypertext Transfer Protocol (HTTP). It is used for secure communication over a computer network, and is widely used on the Internet.

|       | URL      | secure             | port | SSL certificates   | Domain validation  | encryption         |
|-------|----------|--------------------|------|--------------------|--------------------|--------------------|
| http  | http://  | :x:                | 80   | :x:                | :x:                | :x:                |
| https | https:// | :heavy_check_mark: | 443  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |

#### How does HTTPS work? What are TLS/SSL certificates?

+ How does HTTPS work?
	 - HTTPS takes the HTTP protocol, adding an SSL / TLS encryption layer but over a secure SSL connection that encrypts and decrypts their requests and responses.
	 The SSL layer has 2 main purposes:
		 * Verifying that you are talking directly to the server that you think you are talking to.
		 *   Ensuring that only the server can read what you send it and only you can read what it sends back.
	- An **SSL** connection between a client and server is set up by a **handshake**, the goals of which are:
	    * To satisfy the customer that he is talking to the right server (optionally vice versa).
		* That the parties agree on a "cipher suite," which includes the encryption algorithm they will use to exchange data.
		* The parties agree on any necessary keys to this algorithm.

![How TLS work](/images/What-is-TLS-infographic.png  "How TLS work")


To generate a self-signed certificate, run the following in your shell:

<span style="background-color: #000; color: #fff;">openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365</span>

```javascript
const https = require('https');
const fs = require('fs');

const options = {
  key: fs.readFileSync('key.pem'),
  cert: fs.readFileSync('cert.pem')
};

https.createServer(options, function (req, res) {
  res.writeHead(200);
  res.end("hello world\n");
}).listen(8000);
```


----

### Stateless vs stateful authentication

**Stateful**: 
> You can revoke the authentication session on the IdP anytime.

**Stateless**: 
> The session expiration time is set when the authentication token is released. You cannot revoke the session on the IdP.

*    What is session based authentication (stateful) vs token based authentication (stateless)?
		+ **Session Based Authentication**
		In the session based authentication, the server will create a session for the user after the user logs in. The session id is then stored on a cookie on the user’s browser. While the user stays logged in, the cookie would be sent along with every subsequent request. The server can then compare the session id stored on the cookie against the session information stored in the memory to verify user’s identity and sends response with the corresponding state!
![Session Based Authentication flow](/images/Session-Based-Authentication-flow.png "Session Based Authentication flow")
		
		+ **Token Based Authentication**
		Many web applications use JSON Web Token (JWT) instead of sessions for authentication. In the token based application, the server creates JWT with a secret and sends the JWT to the client. The client stores the JWT (usually in local storage) and includes JWT in the header with every request. The server would then validate the JWT with every request from the client and sends response.
![Token Based Authentication flow](/images/Token-Based-Authentication-flow.png "Token Based Authentication flow")

*  What are the advantages and disadvantages of each?
	+ **Session Based Authentication** (stateful):
		- Advantages:
		  <ol>  
			<li> **Revoke the session anytime**</li>  
			<li>**Easy to implement and manage for one-session-sever scenario**</li>  
			<li>**Session data can be changed later**</li> 
			</ol>
		- Disadvantages:
			  <ol>  
			<li> **Increasing server overhead**</li>  
			<li>**Fail to scale**: If the sessions are distributed in different servers, we need to implement a tracking algorithm to link a specific user session and the specific session sever.</li>  
			<li>**Difficult for 3rd party applications to use your credentials**.</li> 
			</ol>
	+ **Token Based Authentication** (stateless):
		- Advantages:
		  <ol>  
			<li> **Lower server overhead**: The great number of session data does not store on the server side.</li>  
			<li>**Easy to scale**: Since the session data is stored on the client side, it does not matter which backend server the request is routed to.</li>  
			<li> **Good to integrate with 3rd party application**</li> 
			</ol>
		- Disadvantages:
			  <ol>  
			<li> **Cannot revoke the session anytime**: Since the user session is stored at client side, the server does not have any rights to delete the session.</li>  
			<li> **Relatively complex to implement for one-session-server scenario**</li>  
			<li>**Session data cannot be changed until its expiration time**</li> 
			</ol>

---
### Session-management in Express
**What is sessions?**
> Because HTTP is stateless, in order to associate a request to any other request, you need a way to store user data between HTTP requests.
> The solution is to store that data server side, give it an "id", and let the client only know (and pass back at every http request) that id. There you go, sessions implemented. Or you can use the client as a convenient remote storage, but you would encrypt the data and keep the secret server-side.

**What are the different ways of managing sessions in express?**
<ol>
<li>Client-Sessions</li>
<li>Cookie Session</li>
<li>Express-sessions</li>
</ol>

```javascript
const express = require('express');  
const session = require('express-session');  
const app = express();
app.use(session({secret: 'secretkey'}));
var sess;  
app.get('/',function(req,res){  
	sess=req.session;  
	sess.email;
	sess.username; 
});

router.post('/login',(req,res)  =>  {  
	sess = req.session;  
	sess.email  = req.body.email;  
	res.end('done');  
});

router.get('/logout',(req,res)  =>  {  
	req.session.destroy((err)  =>  {  
	if(err)  {  
	return console.log(err);  
	}  
	res.redirect('/');  
});

app.use('/', router);  
  
app.listen(process.env.PORT  ||  3000,()  =>  {  
console.log(`App Started on PORT ${process.env.PORT  ||  3000}`);  
});
```

---

### Attacks
**What are the following types of attack?**
 * **Man In The Middle (MITM):** 
			In cryptography and computer security, a **man-in-the-middle**, **monster-in-the-middle**, **machine-in-the-middle**, **monkey-in-the-middle (MITM)** or **person-in-the-middle (PITM)** attack is a cyberattack where the attacker secretly relays and possibly alters the communications between two parties who believe that they are directly communicating with each other.
			
			![MITM](/images/MITM.png "MITM")
				
	**We can defend against XSS by using helmet middleware.**
	+ **Using a VPN**
	+ **Only visiting HTTPS websites**

* **Cross Site Scripting (XSS):**
Cross-site scripting (also known as XSS) is a web security vulnerability that allows an attacker to compromise the interactions that users have with a vulnerable application.
![Cross Site Scripting](/images/cross-site-scripting.svg "cross site scripting")

**We can defend against XSS by using helmet middleware.**
```javascript
var helmet = require('helmet');
app.use(helmet());
```

 At a minimum, disable X-Powered-By header
 ```javascript
app.disable('x-powered-by')
```
Use cookies securely.
Don’t use the default session cookie name.
```javascript
var session = require('express-session')
app.set('trust proxy', 1) // trust first proxy
app.use(session({
  secret: 's3Cur3',
  name: 'sessionId'
}))
```
Set cookie security options.
```javascript
var session = require('cookie-session')
var express = require('express')
var app = express()

var expiryDate = new Date(Date.now() + 60 * 60 * 1000) // 1 hour
app.use(session({
  name: 'session',
  keys: ['key1', 'key2'],
  cookie: {
    secure: true,
    httpOnly: true,
    domain: 'example.com',
    path: 'foo/bar',
    expires: expiryDate
  }
}))
```
> We can use express-sanitizer middleware
> ```javascript
> const  expressSanitizer  =  require('express-sanitizer');
> app.use(express.json());
> app.use(expressSanitizer());
> app.post('/',  function(req,  res,  next)  {
 > // replace an HTTP posted body property with the sanitized string
 > const  sanitizedString  =  req.sanitize(req.body.propertyToSanitize);
 > // send the response -- res.body.sanitized = " world"
 > res.send({ sanitized: sanitizedString });
> });
> ```

* **Cross Site Request Forgery (CSRF):**
Cross site request forgery (CSRF), also known as XSRF, Sea Surf or Session Riding, is an attack vector that tricks a web browser into executing an unwanted action in an application to which a user is logged in.

![csrf cross site request forgery](/images/csrf-cross-site-request-forgery.png "csrf cross site request forgery")

**We can defend against Cross Site Request Forgery (CSRF):**
```javascript
const express = require('express');
const bodyParser = require('body-parser');
const csurf = require('csurf');
const cookieParser = require('cookie-parser');

const PORT = process.env.PORT || 3000;
const app = express();

const csrfMiddleware = csurf({
  cookie: true
});

app.use(bodyParser.urlencoded({
  extended: true
}));
app.use(cookieParser());
app.use(csrfMiddleware);

app.get('/', (req, res) => {
  res.send(`
    <h1>Hello World</h1>
    <form action="/entry" method="POST">
      <div>
        <label for="message">Enter a message</label>
        <input id="message" name="message" type="text" />
      </div>
      <input type="submit" value="Submit" />
      <input type="hidden" name="_csrf" value="${req.csrfToken()}" />
    </form>
  `);
});

app.post('/entry', (req, res) => {
  console.log(`Message received: ${req.body.message}`);
  res.send(`CSRF token used: ${req.body._csrf}, Message received: ${req.body.message}`);
});

app.listen(PORT, () => {
  console.log(`Listening on http://localhost:${PORT}`);
});
// {"csrfToken":"7IAxiaQh-AsfSP3PHHSljjz7nBO5nMb5c4eg"}
```
		
		