
// postgresql 실행하는 방법
brew services start postgresql@14
psql postgres


1. request가 들어오면 JwtAuthFilter에서 jwt token이 있는지 검증한다.
해당 token의 유무 여부는 request.getHeader("Authorization")을 가지고 확인할 수 있으며,"Bearer "로 시작하는 내용이 있어야 한다.

2. 다음으로는 token을 가지고 username을 가지고 와야 한다. 그래서 토근을 가지고 jwtService에서 해당 기능을 만든다.
jjwt-api, jjwt-impl, jjwt-jackon 를 gradle에 implement 해야 한다.

3. jwt token이란 무엇인지 먼저 이해해보자.
jwt = json web token 
jwt는 세가지로 구성되어 있다.
   1) header
   헤더에는 타입과 사용된 알고리즘의 정보가 기록된다. 
   {
       "alg":"HS256",
       "typ":"JWT"
   }
   2) payload
   추가적인 클레임 정보들이 기록되어 있다. 클레임이란 정보의 조각을 말한다. 클레임에는 registred claim, public claim, private claim 이렇게 세종류가 있다.
   {
      "sub": "1234567890",
      "name": "John Doe",
      "iat": 1516239022
   }
   3) verify signature
   암호화 키를 가지고 있는 것이다.   
   HMACSHA256(
      base64UrlEncode(header) + "." +
      base64UrlEncode(payload),
   ) secret base64 encoded

4. extractAllClaims 을 통해서 정보를 가지고 와야 하는데 거기에서 signInKey가 필요하다. jwt의 경우 최소 256bit의 무작위 랜덤 키가 필요하다.
   C77FB8812EA276DCB51E343E2FAA9