# webauthn-delegation

WebAuthn has potential authentication use-cases not discussed or supported by the current model.  Optional support of these use cases up front can prevent barriers to adoption from parties concerned with them. In particular I am considering the following:

- A User grants access to their account to a trusted party that is not in the same room as them.
- A User maintains backup tokens to every RP, without having to register a second Authenticator to every RP.
- A Client maintains SSO for all RP that functions across client devices for a single Authenticator.


Disclaimer: I am not a cryptographer, so one should look at this. But there isn’t anything too exotic here.

The rough cut is as follows: when a client is registering a new key with the server, it also has the key sign a value that is `HMAC(restrictions, secretKey)`. The HMAC and restrictions are shipped off for the server to hold onto. Now anyone that can prove ownership of the secretKey within the restrictions can register a new key with that account. For now these restrictions are how long the token should last, how many uses it gets, and what public keys work with it. 

Put even more roughly, this is a tool to create trusted tokens that exist off of the Authenticator, bootstrapping trust from registration.
