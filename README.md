# gondi (Go No Direct Initialize)
A Go linter to detect direct initialization of structs

## Terms

### Constructor

A func that responsible for creating a instance of a struct in the same package. The func also needs to be in the same file where the struct defined.

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

### GONDI001 Initialize a struct via the constructor if there is one

#### Failing example:

```go
package handler

import ("entity")

func main() {
    e := entity.Entity{} // NOOO, linter will indicate error, as Entity has a constructor
}
````

to fix it:

```go
package handler

import ("entity")

func main() {
    e := entity.New() // NOOO, linter will indicate error, as Entity has a constructor
}
````

#### Passing example:

```go
package handler

import ("member")

func main() {
    m := Member{} //pass this rule, as struct `Member` has no constructor
}
````
