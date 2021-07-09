## Json Web Token (JWT)  

JWT is a claims that encoded as a JSON object in a JWS and/or JWE struct. The different of JWS and JWE is discussed in [RFC7516](https://datatracker.ietf.org/doc/html/rfc7516#page-24).
But the JWT has three part. Noticed, JWT has the number of segments diffence between JWS and JWE. We need to distinguish between part and segment

1. JOSE header (header)  
2. JWT claim (payload)  
3. JWT signature (signature)  

## Json Web Siganture (JWS)  
A data structure represent a digitally signed or MACed message. It's a type representation of JWT. For more information [RFC7515](https://datatracker.ietf.org/doc/html/rfc7515)  

### Json Web Key Set (JWKS)  
A data structure as Json that represent a set of JWKs. It's must presented as a array of keys. Each element in array is a JWK. For more information [RFC7517](https://datatracker.ietf.org/doc/html/rfc7517).
If you have more than one JWK, you need define `kid` in `JOSE HEADER` 

### Json Web Key (JWK)  
A data structure that represent a cryptography key. For more information [RFC7517](https://datatracker.ietf.org/doc/html/rfc7517)  

***Example***   
Sample of JWKS that contains 2 JWK
```
{
  "keys" : [ {
    "kty" : "RSA",
    "kid" : "1438289820780",
    "use" : "sig",
    "alg" : "RS256",
    "n" : "idWPro_QiAFOdMsJD163lcDIPogOwXogRo3Pct2MMyeE2GAGqV20Sc8QUbuLDfPl-7Hi9IfFOz--JY6QL5l92eV-GJXkTmidUEooZxIZSp3ghRxLCqlyHeF5LuuM5LPRFDeF4YWFQT_D2eNo_w95g6qYSeOwOwGIfaHa2RMPcQAiM6LX4ot-Z7Po9z0_3ztFa02m3xejEFr2rLRqhFl3FZJaNnwTUk6an6XYsunxMk3Ya3lRaKJReeXeFtfTpShgtPiAl7lIfLJH9h26h2OAlww531DpxHSm1gKXn6bjB0NTC55vJKft4wXoc_0xKZhnWmjQE8d9xE8e1Z3Ll1LYbw",
    "e" : "AQAB"
  }, {
    "kty" : "RSA",
    "kid" : "1438289856256",
    "use" : "sig",
    "alg" : "RS256",
    "n" : "zo5cKcbFECeiH8eGx2D-DsFSpjSKbTVlXD6uL5JAy9rYIv7eYEP6vrKeX-x1z70yEdvgk9xbf9alc8siDfAz3rLCknqlqL7XGVAQL0ZP63UceDmD60LHOzMrx4eR6p49B3rxFfjvX2SWSV3-1H6XNyLk_ALbG6bGCFGuWBQzPJB4LMKCrOFq-6jtRKOKWBXYgkYkaYs5dG-3e2ULbq-y2RdgxYh464y_-MuxDQfvUgP787XKfcXP_XjJZvyuOEANjVyJYZSOyhHUlSGJapQ8ztHdF-swsnf7YkePJ2eR9fynWV2ZoMaXOdidgZtGTa4R1Z4BgH2C0hKJiqRy9fB7Gw",
    "e" : "AQAB"
  } ]
}
```  
For this challenge, When decode JWT. I see it has `jku`.  
>The "jku" (JWK Set URL) Header Parameter is a URI [RFC3986] that
   refers to a resource for a set of JSON-encoded public keys, one of
   which corresponds to the key used to digitally sign the JWS  
   
![image](https://user-images.githubusercontent.com/22276823/125032095-fcfdf380-e07c-11eb-8695-d800d6ffd895.png)  

![image](https://user-images.githubusercontent.com/22276823/125032209-26b71a80-e07d-11eb-99fe-5b4baa065134.png)  

I try to create a new JWT with redirecting JWKS to mine.  

First, create a new `private key` with `openssl genrsa -out priv.pem` and create a new `public key` with `openssl rsa -in priv.pem -pubout -out pub.key`  

![image](https://user-images.githubusercontent.com/22276823/125032953-38e58880-e07e-11eb-9a9b-1f3272d96369.png)  

Next, create a new JWKS file with the new `public key`. I need to extract the `n` and `e` parameter from `public key`. Be awared, It is represented as a Base64urlUInt-encoded value.
![image](https://user-images.githubusercontent.com/22276823/125033809-52d39b00-e07f-11eb-806b-e3a833a4b5b4.png)  

Now, input the result to JWKS file. Run the mini server that can return the file when we submit token  

![image](https://user-images.githubusercontent.com/22276823/125034019-9d551780-e07f-11eb-95c6-af8b62fef4cc.png)  

![image](https://user-images.githubusercontent.com/22276823/125034673-62071880-e080-11eb-88b3-67c92ae79b5f.png)  








