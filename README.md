Forked from https://github.com/gwwfps/onetime
Added C200 example in README
onetime
=======

An one-time password generation library written in Go, implementing 
HOTP ([RFC-4226](http://tools.ietf.org/html/rfc4226)) and 
TOTP ([RFC-6238](http://tools.ietf.org/html/rfc6238)).

Example usage 
-------------

Simple 6-digit HOTP code:
```go
import "onetime"

var secret = []byte("SOME_SECRET")
var counter = 123456
var otp = onetime.Simple(6) 
var code = otp.HOTP(secret, counter)
```

Google authenticator style 8-digit TOTP code:
```go
import "onetime"

var secret = []byte("SOME_SECRET")
var otp = onetime.Simple(8) 
var code = otp.TOTP(secret)
```

9-digit 5-second-step TOTP starting on midnight 2000-01-01 UTC, using SHA-256:
```go
import (
    "crypto/sha256"
    "onetime"
    "time"
)

var secret = []byte("SOME_SECRET")
var ts, _ = time.ParseDuration("5s")
var t = time.Date(2000, time.January, 1, 0, 0, 0, 0, time.UTC)
var otp = onetime.OneTimePassword{Digit: 9, TimeStep: ts, BaseTime: t, Hash: sha256.New} 
var code = otp.TOTP(secret)
```

C200 6-digit 60-second-step  TOTP code calculated with the current time 
and the given secret and OTP value.Correct cloc skew +- 1 min
```go
	token := string("Token Digit")
	secret, _ := hex.DecodeString("you hex encoded secret")
	var step, _ = time.ParseDuration("60s")
	otp := onetime.OneTimePassword{6, step, time.Unix(0, 0), sha1.New}
	//Return reslult bool and error
	result, err := otp.COTP(secret, token)
	if err != nil {
		fmt.Println("Otp error", err)
	}
	if !result {
		fmt.Println("Auth failed")
	} else {
		fmt.Println("Auth succes")
	}

```


License
-------
This library is released under a simplified BSD license.
