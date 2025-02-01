# Authentication, Authorisation

In this file I document my notes and learnings on Authentication and Authorisation.

**Authentication** - To verify that someone is who they say they are.
**Authorisation** - Determining a user's level of access and then granting access based on that level.

## JSON Web Tokens

### What is it ?
Open Standard [RFC 7519](https://tools.ietf.org/html/rfc7519) that defines how to **securely transmit information** between parties as a JSON object.

### How does it work ?   
Information can be verified and trusted because it is digitally signed. JWTs can be signed using either of the followiing methods.
    - A Secret (Hashed Message Authentication Code) - Keyed HMAC Algorithm 
    - Public/Private Key Encryption -  RSA

### Why is it Useful? 
-  Authentication:
        Once a user has been successfully logged in, the user can make subsequent requests using a JWT to access authenticated routes, services and resources permitted with that token.
-  Information Exchange: 
        It is a good way to securely transmit info because you can be sure that the sender is who they say that they are abd that the content has not changed. 
        This can be used for authorisation information to be exchanged. 
    
### JSON Web Token Structure:
JWTs consist of three parts separated by dots ( ```.``` )

[Header].[Payload].[Signature]

-   ***Header***: Typically consists of the type of token and the hashing algorithm used. e.g:
        ``` 
        {
            'alg': 'HS256',
            'typ': 'JWT'
        }
        ```
        This JSON is Base64Url Encoded to form the first part of the JWT.

-   ***Payload***: Contains the claims (statements) about an entity(typically the user) and additional metadata.
        There are three types of claims, namely; reserved, public, and private claims.

    - Reserved claims - Predefined claimed which are not mandatory but are reccomended. These include: 
        -   iss - issuer
        -   exp - expiration time
        -   sub - subject
        -   aud - audience

    - Public Claims - Defined by those using the JWTs but to avoid collisions, they should be defined in the [IANA JSON Web Token Registry](https://www.iana.org/assignments/jwt/jwt.xhtml). 
        - iat -	Issued At
        - name - Full name
        - preferred_username - Shorthand name by which the End-User wishes to be referred to
        - email - Preferred e-mail address
        - phone_number - Preferred telephone number
        - msisdn - End-user's mobile phone number formatted according to ITU-T recommendation [E.164](https://www.itu.int/rec/T-REC-E.164/en)
        - zoneinfo - Time zone
        - locale - Locale
        - updated_at - Time the information was last updated
        - sid -	Session ID
        - roles - Roles
        - groups - Groups
        - entitlements - Entitlements
        - hwmodel - Model identifier for hardware
        - location - The geographic location
        - htm -	The HTTP method of the request
        - htu -	The HTTP URI of the request (without query and fragment parts)
        - ath -	The base64url-encoded SHA-256 hash of the ASCII encoding of the associated access token's value

    - Private claims - Custom claims created to share information between parties that agree on using them.
        
        An example fo this payload could be:
        ``` 
        {
            'name': 'John Doe',
            'email': 'abc@gmail.com'
            'admin': 'true'
        }
        ```
        This JSON is Base64Url Encoded to form the second part of the JWT.

    -   ***Signature***: To create the signature you make use of the encoded header, encoded payload, a secret, and the algorithm specified in the header. i.e:

        ``` HMACSHA256( base64UrlEncode(header) + '.' + base64UrlEncode(payload), secret ) ```

### How do JSON Web Tokens work? 
In authentication, when the user successfully logs in using their credentials, a JSON Web Token will be returned (since tokens are credentials great care is required to prevent security issues). 
Whenever the user wants to access a protected route, it should sent the JWT, typically in the Authorisation header using the Bearer schemea. i.e ``` Authorization: Bearer <token>```

This is a stateless authentication mechanism as user state is never saved in server memory. The servers protected routes will check for a valid JWT in the authorisation header and if there is, the user will be allowed. JWTs are sel-contained reducing the need to go back and forward to the database.


## Role-based access control (RBAC) vs. Attribute-based access control (ABAC) 


## Open FGA 