# gondi (Go No Direct Initialize)
A Go linter to detect direct initialization of structs

## Terms

### Constructor

A func that responsible for creating a instance of a struct in the same package.

e.g.

```go
package entity

type Entity struct{
    FirstName string
    LastName string
}

func New() *Entity { //Constructor for Entity
    return Entity{}
}

func ParseJSON(j json.RawMessage) Entity { //Constructor for Entity
    e := Entity{}
    
    if err := json.Unmarshal(j, e); err != nil{
        return err
    }
    
    return e
}
```

### No constructor struct

A struct that do not have any constructor assoicated with.

e.g.

```go
type Member struct{}

func (m Member) doSomething() Member { // Not a constructor, not initializing a struct
    n := m
    
    //to something
    
    return n
}

//END of file
```


## Rules

### GONDI001 Do NOT initialize a struct direcrly if there is a Constructor

Triggering example:

```go
package handler

import ("entity")

func main() {
    e := Entity{} // NOOO, linter will indicate error
}
````

Passing example:

```go
package handler

import ("member")

func main() {
    m := Member{} //pass this rule, as struct `Member` has no constructor
}
````
    

### GONDI002 If there is a Constructor, it shall be located in the same file

e.g.

```go
//file entity.go
package entity

type Entity struct{}
```

```go
//file new.go
package entity

func new() Entity{ // a constructor, but not in the same file as struct Entity (not in entity.go file)
    e := Entity{}
}
```
