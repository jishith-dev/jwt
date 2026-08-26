# JWT

JWT (JSON Web Token) library for Zen.

Provides HS256 token signing and verification with standard JWT claims and token lifetime support.

## Features

- HS256 signing and verification
- Base64URL encoding
- `iat`, `exp`, `nbf`
- `iss`, `aud`, `sub`, `jti`
- `expiresIn`
- `notBefore`
- `maxAge`
- `clockTolerance`
- `ignoreExpiration`
- `ignoreNotBefore`
- `noTimestamp`
- Invalid and tampered token detection
- Wrong secret detection
- Malformed token detection

## Installation

~~~text
zen install jwt
~~~

## Import

~~~text
import (JWT, create) from "jwt"
~~~

## Basic Usage

~~~text
import (JWT, create) from "jwt"

JWT jwt = create("my-secret")

Map payload
payload.setString("name", "Jishith")

string token =
  jwt.sign(payload)

screen(token)

Map verified =
  jwt.verify(token)

debug.pretty(verified)
~~~

## Signing with Options

~~~text
Map payload
payload.setString("name", "Jishith")

Map options
options.setLong("expiresIn", 3600L)

string token =
  jwt.signWithOptions(
    payload,
    options
  )

screen(token)
~~~

`expiresIn` is specified in seconds.

## Standard Claims

~~~text
Map options

options.setString("issuer", "zen-app")
options.setString("audience", "users")
options.setString("subject", "user-123")
options.setString("jwtid", "token-001")

string token =
  jwt.signWithOptions(
    payload,
    options
  )
~~~

Supported claims:

- `iat` — issued-at timestamp
- `exp` — expiration timestamp
- `nbf` — not-before timestamp
- `iss` — issuer
- `aud` — audience
- `sub` — subject
- `jti` — JWT ID

## Expiration

~~~text
Map options
options.setLong("expiresIn", 3600L)

string token =
  jwt.signWithOptions(
    payload,
    options
  )
~~~

The library automatically creates the `iat` and `exp` claims.

For example, `3600L` gives the token a lifetime of one hour.

Expired tokens are rejected during verification.

## Not Before

~~~text
Map options
options.setLong("notBefore", 60L)

string token =
  jwt.signWithOptions(
    payload,
    options
  )
~~~

The token becomes valid after the specified number of seconds.

## Verification

~~~text
Map verified =
  jwt.verify(token)

debug.pretty(verified)
~~~

For claim validation:

~~~text
Map verifyOptions

verifyOptions.setString(
  "issuer",
  "zen-app"
)

verifyOptions.setString(
  "audience",
  "users"
)

verifyOptions.setString(
  "subject",
  "user-123"
)

verifyOptions.setString(
  "jwtid",
  "token-001"
)

Map verified =
  jwt.verifyWithOptions(
    token,
    verifyOptions
  )

debug.pretty(verified)
~~~

## Ignore Expiration

~~~text
Map options

options.setBool(
  "ignoreExpiration",
  true
)

Map result =
  jwt.verifyWithOptions(
    token,
    options
  )
~~~

## Ignore Not Before

~~~text
Map options

options.setBool(
  "ignoreNotBefore",
  true
)

Map result =
  jwt.verifyWithOptions(
    token,
    options
  )
~~~

## Clock Tolerance

~~~text
Map options

options.setLong(
  "clockTolerance",
  5L
)
~~~

The value is specified in seconds.

## Max Age

~~~text
Map options

options.setLong(
  "maxAge",
  3600L
)
~~~

`maxAge` limits the maximum age of a token based on its `iat` claim.

## Disable Timestamp

By default, `iat` is automatically added.

~~~text
Map options

options.setBool(
  "noTimestamp",
  true
)
~~~

## API

### create

~~~text
JWT jwt =
  create("my-secret")
~~~

Creates a JWT instance using the supplied secret.

### sign

~~~text
string token =
  jwt.sign(payload)
~~~

Signs a payload using HS256.

### signWithOptions

~~~text
string token =
  jwt.signWithOptions(
    payload,
    options
  )
~~~

Signs a payload using HS256 and the supplied options.

### verify

~~~text
Map payload =
  jwt.verify(token)
~~~

Verifies a JWT using the instance secret.

### verifyWithOptions

~~~text
Map payload =
  jwt.verifyWithOptions(
    token,
    options
  )
~~~

Verifies a JWT with additional validation options.

## Options

| Option | Type | Description |
|---|---|---|
| `expiresIn` | `Long` | Token lifetime in seconds |
| `notBefore` | `Long` | Delay before token becomes valid |
| `maxAge` | `Long` | Maximum token age in seconds |
| `clockTolerance` | `Long` | Allowed clock difference |
| `ignoreExpiration` | `bool` | Ignore `exp` validation |
| `ignoreNotBefore` | `bool` | Ignore `nbf` validation |
| `noTimestamp` | `bool` | Disable automatic `iat` |
| `issuer` | `string` | Expected issuer |
| `audience` | `string` | Expected audience |
| `subject` | `string` | Expected subject |
| `jwtid` | `string` | Expected JWT ID |

## Invalid Tokens

The following are rejected:

- Modified payload
- Modified signature
- Wrong secret
- Malformed token
- Missing signature
- Invalid JWT structure
- Expired token
- Token used before `nbf`
- Invalid issuer
- Invalid audience
- Invalid subject
- Invalid JWT ID

Failed verification returns an empty `Map`.

## Security

JWTs are authenticated using HMAC-SHA256 (HS256).

Never expose the signing secret to untrusted clients.

## Version

1.0.0

## Author

Jishith M P
