curl -i -X DELETE http://localhost:3000/rest/products/search

- 500 internal server error

curl -i -X DELETE http://localhost:3000/rest/products/search

- 204 no content

##### Ch.1 Day 3 - Steal my own session (Academy)

** What i did: Logged in as "wiener", copied the cookie value 'session' and pasted
it on a new private window. On the log in page, i changed the new session value to the
one i had copied, on reaload, i was logged in as wiener without needing a password.

** Proves: the server doesn't know who i am; only knows the session id it received
and anyone with that id is the user. This means that stealing cookies is enough to
steal account (session hijacking)

** How to avoid this: Adding the flag 'HttpOnly'. With this, javascript on the page
(i.e., XSS payload) won't be able to read the cookie with 'document.cookie'

** HttpOnly will not prevent someone copying it from DevTools (like i did) nor
read it if the sites uses HTTP (HTTPS is needed for this)

##### Ch.1 Day 3 - Remove cookie from request (Juice Shop)

** On burp suit sent 'GET /my-account?id=wiener' to the Repeater.
if i send it as is i get the code 200, if remove the cookies i get
302 for /login

** This again only to prove that the session cookie is the only thing that id's me.

##### Ch.1 Day 3 - Curl the Juice Shop and login via Curl

**Header:** {"typ":"JWT","alg":"RS256"}: token signature with RS256.

**Content:** id, email, role, bid (basket) and field `password` with the password hash
MD5 (827ccb0e... = "12345", found by online search).

**Problems:**

- The password hash goes to the Client inside the token, whomever steals the token (i.e., via XSS because
  the cookie is not HttpOnly), thakes the hash.
- MD5 w/out salt: quick and with not public windows. Should be bcrypt/Argon2.
- `totpSecret` also goes on the token: with 2FA active, the second factor would go together

** Anyone can read JWT, the signature only stops it from being altered, and only if the server verifies it.
