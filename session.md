# Session

A simple RESTful key-value storage for storing and fetching session data. Comes with a simple client side library. Uses MongoDB for persistence.

## Usage

```
go install ...
```

### HTTP API

Check out the API docs at: http://docs.oryplatformsessionserver.apiary.io/

### Client

**Create a new session:**  
```go
import "github.com/ory-platform/session-server/client"

func main() {
    session, err := client.New("session-server.my.domain.com:80", oauthToken)
    err = session.Set("foo", "bar")
    r, err := session.Get("foo") // "bar"
    err = session.Unset("foo")
}
```

**Open an existing session:**  
```go
import "github.com/ory-platform/session-server/client"

func main() {
    session, err := client.Open("session-server.my.domain.com:80", oauthToken, sessionToken)
    r, err := session.Get("key") // "value"
    err = session.Set("foo", "bar")
    err = session.Unset("key")
}
```

**Delete an existing session:**  
```go
import "github.com/ory-platform/session-server/client"

func main() {
    err := client.Remove("session-server.my.domain.com:80", oauthToken, sessionToken)
}
```

**Flush (remove) all sessions:**  
```go
import "github.com/ory-platform/session-server/client"

func main() {
    err := client.Flush("session-server.my.domain.com:80", oauthToken)
}
```

**Set any kind of value:**  
*You can set strings, ints or floats or almost any kind of struct (structs will be encoded as a gob byte stream).*  
```go
import "github.com/ory-platform/session-server/client"

type A struct {
    B string
    C string
}

func main() {
    session, err := client.Open("session-server.my.domain.com:80", oauthToken, sessionToken)
    
    a := A{"b", "c"}
    
    session.Set("a", a)
    session.Set("b", 123)
    session.set("c", "c")
}
```
