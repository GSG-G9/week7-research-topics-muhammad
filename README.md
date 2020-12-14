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

<span style="background-color:rgba(0, 0, 0); color: #fff">openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365</span>

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
