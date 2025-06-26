### The Absolute Basics

  * **Your First Go Program:**

    ```go
    package main // Every executable Go program starts here!

    import "fmt" // Need to print? You'll need this.

    func main() { // The entry point. Your code starts running from here.
        fmt.Println("Hello, Gophers! Let's build something awesome.")
    }
    ```

  * **Comments:**

    ```go
    // This is a single-line comment. Quick notes for quick thoughts.
    /*
        This is a multi-line comment.
        For when you have a lot of wisdom
        or a detailed explanation to share.
    */
    ```

-----

### Variables & Constants

  * **Declaring Variables:**

      * **Full Declaration:** 
        ```go
        var name string = "GoPro"       // Variable 'name' of type string, initialized.
        var age int                     // Variable 'age' of type int, defaults to 0.
        age = 3                         // Assign a value later.
        ```
      * **Type Inference:**
        ```go
        var city = "Gopherville" // Go knows 'city' is a string.
        ```
      * **Short Declaration Operator (`:=`):**
        Only for *first declaration* within a function. Go infers the type.
        ```go
        message := "Happy coding!" // Declares and initializes 'message' as a string.
        count := 10                // Declares and initializes 'count' as an int.
        isValid := true            // Declares and initializes 'isValid' as a bool.
        ```
      * **Multiple Declarations:**
        ```go
        var x, y int = 1, 2
        a, b := "hello", "world"
        ```

  * **Constants (Immutable Values):**
    Values that can't change.

    ```go
    const PI = 3.14159     // A floating-point constant.
    const GREETING = "Hi there!" // A string constant.

    // iota: For sequential, auto-incrementing constants (super handy for enums!)
    const (
        Red   = iota // 0
        Green        // 1
        Blue         // 2
    )
    ```

-----

### Data Types

Go is statically typed, but does support type inference for added flexibility.

  * **Basic Types:**

      * **Numbers:** `int`, `int8`, `int16`, `int32`, `int64` (signed integers), `uint`, `uint8`, `uint16`, `uint32`, `uint64`, `uintptr` (unsigned integers). `float32`, `float64`. `complex64`, `complex128`.
      * **Booleans:** `bool` (`true` or `false`).
      * **Strings:** `string` (UTF-8 encoded by default).
      * **Byte:** `byte` (alias for `uint8`).
      * **Rune:** `rune` (alias for `int32`, represents a Unicode code point).

  * **Composite Types:**

      * **Arrays:** Fixed-size collection of *same type* elements. Less common, slices are usually preferred.
        ```go
        var numbers [3]int // Array of 3 integers, default 0, 0, 0
        numbers[0] = 1
        colors := [2]string{"red", "blue"} // Initialized array
        ```
      * **Slices:** Dynamic, flexible, and *the* way to handle lists in Go. Think references to underlying arrays.
        ```go
        var names []string              // Declares a nil slice (zero value)
        ages := []int{25, 30, 35}       // Initialized slice
        names = append(names, "Alice", "Bob") // Add elements (creates new underlying array if capacity exceeded)
        subset := ages[1:3]             // Slicing: creates a new slice referencing a portion of the original
                                        // Result: [30, 35]
        ```
      * **Maps (Like Dictionaries/Hash Maps):** Unordered key-value pairs. Keys *must* be comparable.
        ```go
        // Declaration: map[KeyType]ValueType
        salaries := make(map[string]int) // Create an empty map
        salaries["Alice"] = 60000
        salaries["Bob"] = 75000

        // Literal initialization
        userRoles := map[string]string{
            "admin": "Administrator",
            "guest": "Visitor",
        }

        // Accessing values:
        aliceSalary := salaries["Alice"] // 60000
        // Check for existence:
        if role, ok := userRoles["admin"]; ok { // 'ok' is true if key exists
            fmt.Println("Admin role:", role)
        }
        delete(userRoles, "guest") // Remove an element
        ```
      * **Structs:** Group related fields (of different types) into a single unit. Think as lightweight objects without methods in the OOP sense.
        ```go
        type User struct { // Define a 'User' struct
            ID        int
            Username  string
            Email     string
            IsActive  bool
            // Tagging for JSON serialization/deserialization:
            CreatedAt string `json:"created_at"`
        }

        // Creating instances:
        var u1 User // Default values (0 for int, "" for string, false for bool)
        u1.ID = 1
        u1.Username = "gopher-dev"

        u2 := User{ID: 2, Username: "code-wizard", Email: "wiz@example.com", IsActive: true}

        // Anonymous struct (quick, one-off definitions):
        p := struct { Name string; Age int }{"AnonUser", 99}
        ```

-----

### Control Flow

  * **`if`, `else if`, `else`:** Standard conditional logic. Go doesn't require parentheses around the condition.

    ```go
    if score > 90 {
        fmt.Println("Excellent!")
    } else if score > 70 {
        fmt.Println("Good job!")
    } else {
        fmt.Println("Keep practicing!")
    }

    // Short statement (variable scope limited to if/else blocks)
    if err := someFunctionReturningError(); err != nil {
        fmt.Println("Error occurred:", err)
        return
    }
    // err is not accessible here
    ```

  * **`switch`:** Cleanly handles multiple conditions. No automatic fall-through (unless you use `fallthrough` explicitly).

    ```go
    grade := "B"
    switch grade {
    case "A":
        fmt.Println("Perfect!")
    case "B", "C": // Multiple cases in one line
        fmt.Println("Good!")
    default:
        fmt.Println("Needs improvement.")
    }

    // Switch with no expression (like if/else if chain)
    temp := 25
    switch {
    case temp < 0:
        fmt.Println("Freezing!")
    case temp >= 0 && temp < 20:
        fmt.Println("Cool.")
    default:
        fmt.Println("Warm.")
    }
    ```

  * **`for`:** 

      * **Standard C-style loop:**
        ```go
        for i := 0; i < 5; i++ {
            fmt.Println(i)
        }
        ```
      * **While-style loop (condition only):**
        ```go
        sum := 0
        for sum < 100 {
            sum += 5
        }
        ```
      * **Infinite loop:**
        ```go
        for {
            // Keep running until 'break'
            if condition {
                break
            }
        }
        ```
      * **`for...range` (Iterating over collections - slices, arrays, maps, strings, channels):**
        ```go
        numbers := []int{10, 20, 30}
        for index, value := range numbers {
            fmt.Printf("Index: %d, Value: %d\n", index, value)
        }

        // To iterate only over values:
        for _, value := range numbers { // Use '_' for unused index
            fmt.Println("Value:", value)
        }

        // Iterating over maps:
        ages := map[string]int{"Alice": 30, "Bob": 25}
        for name, age := range ages {
            fmt.Printf("%s is %d years old.\n", name, age)
        }
        ```

-----

### Functions

Go functions can return multiple values.

  * **Basic Function Declaration:**

    ```go
    func add(a int, b int) int { // Parameters with types, return type
        return a + b
    }

    // Shorthand for same-type parameters
    func subtract(a, b int) int {
        return a - b
    }
    ```

  * **Multiple Return Values:**

    ```go
    func divide(numerator, denominator float64) (float64, error) { // Returns a result and an error
        if denominator == 0 {
            return 0, fmt.Errorf("cannot divide by zero") // Use fmt.Errorf for custom errors
        }
        return numerator / denominator, nil // nil means no error
    }

    // Calling and handling multiple returns:
    result, err := divide(10, 2)
    if err != nil {
        fmt.Println("Error:", err)
    } else {
        fmt.Println("Result:", result)
    }
    ```

  * **Named Return Values (Optional but can make code clearer):**

    ```go
    func calculateAreaAndPerimeter(length, width float64) (area float64, perimeter float64) {
        area = length * width
        perimeter = 2 * (length + width)
        return // Returns area and perimeter automatically
    }
    ```

  * **Variadic Functions (Accepts varying number of arguments):**

    ```go
    func sumAll(numbers ...int) int { // '...' indicates variadic
        total := 0
        for _, num := range numbers {
            total += num
        }
        return total
    }

    // Call it:
    total := sumAll(1, 2, 3, 4, 5) // total is 15
    ```

-----

### Pointers

Pointers let you refer to the memory address of a value. Useful for modifying values passed to functions, or for large data structures.

  * **`&` (Address-of Operator):** Gets the memory address of a variable.
  * **`*` (Dereference Operator):** Gets the value at a memory address.
    ```go
    x := 10
    ptr := &x      // ptr now holds the memory address of x
    fmt.Println(ptr)   // Prints memory address (e.g., 0xc0000140a0)
    fmt.Println(*ptr)  // Prints the value at that address (10)

    *ptr = 20      // Change the value at the address ptr points to
    fmt.Println(x)     // x is now 20!
    ```
      * *Pro-Tip:* Go encourages passing values, not pointers, unless you explicitly need to modify the original value or avoid copying large data.

-----

### Methods & Interfaces

Go isn't object-oriented, but methods and interfaces provide powerful, flexible ways to structure code.

  * **Methods:**
    A function attached to a specific type.

    ```go
    type Rectangle struct {
        Width, Height float64
    }

    // This is a method for the Rectangle type.
    // (r Rectangle) is the 'receiver'.
    func (r Rectangle) Area() float64 {
        return r.Width * r.Height
    }

    // Method with a pointer receiver - allows modifying the original struct
    func (r *Rectangle) Scale(factor float64) {
        r.Width *= factor
        r.Height *= factor
    }

    // Usage:
    rect := Rectangle{Width: 10, Height: 5}
    fmt.Println("Area:", rect.Area()) // Calls the Area method
    rect.Scale(2) // Modifies rect's Width and Height
    fmt.Println("Scaled Area:", rect.Area()) // Area is now 100
    ```

  * **Interfaces:**
    A set of method signatures. Any type that implements *all* the methods of an interface implicitly satisfies that interface.

    ```go
    type Shape interface { // Define an interface
        Area() float64
        Perimeter() float64
    }

    // Rectangle already has Area(), let's add Perimeter()
    func (r Rectangle) Perimeter() float64 {
        return 2 * (r.Width + r.Height)
    }

    type Circle struct {
        Radius float64
    }

    func (c Circle) Area() float64 {
        return math.Pi * c.Radius * c.Radius
    }

    func (c Circle) Perimeter() float64 {
        return 2 * math.Pi * c.Radius
    }

    // A function that can accept ANY type that satisfies the 'Shape' interface
    func PrintShapeInfo(s Shape) {
        fmt.Printf("Area: %.2f, Perimeter: %.2f\n", s.Area(), s.Perimeter())
    }

    // Usage:
    myRect := Rectangle{Width: 5, Height: 3}
    myCircle := Circle{Radius: 2.5}

    PrintShapeInfo(myRect)   // Works! Rectangle satisfies Shape
    PrintShapeInfo(myCircle) // Works! Circle satisfies Shape
    ```

      * **Empty Interface (`interface{}` or `any`):** Can hold any value. Use sparingly, as it loses type safety.
        ```go
        var anyValue interface{}
        anyValue = "Hello"
        anyValue = 123
        // type assertion to get original type:
        str, ok := anyValue.(string) // str is "Hello" if anyValue was a string, ok is true
        ```

-----

### Error Handling

Go doesn't have exceptions. It uses explicit `error` return values.

  * **Basic Error Pattern:**
    ```go
    func fetchData(id int) ([]byte, error) {
        if id <= 0 {
            return nil, fmt.Errorf("invalid ID: %d", id) // Return nil for data, error for error
        }
        // ... actual data fetching logic
        return []byte("some data"), nil // Return data, nil for no error
    }

    // How you'll see it everywhere:
    data, err := fetchData(1)
    if err != nil { // ALWAYS check for errors!
        log.Fatalf("Failed to fetch data: %v", err) // log.Fatalf stops program
    }
    fmt.Println("Data fetched successfully:", string(data))
    ```
      * `log.Fatal` / `log.Fatalf` is for unrecoverable errors. For graceful handling, just `return` or handle the error.

-----

### Concurrency

  * **Goroutines (Lightweight Threads):**
    Functions that run concurrently. Just add `go` before a function call.

    ```go
    func sayHello() {
        fmt.Println("Hello from a goroutine!")
    }

    func main() {
        go sayHello() // This runs sayHello in a new goroutine
        fmt.Println("Hello from main!")
        time.Sleep(1 * time.Second) // Give goroutine time to finish before main exits
    }
    // Output might be:
    // Hello from main!
    // Hello from a goroutine!
    ```

  * **Channels (Communicating Safely Between Goroutines):**
    Typed conduits through which you can send and receive values. Prevents race conditions.

    ```go
    func worker(id int, messages chan<- string) { // 'chan<-' means send-only channel
        fmt.Printf("Worker %d: Starting...\n", id)
        time.Sleep(time.Second) // Simulate work
        messages <- fmt.Sprintf("Worker %d: Done!", id) // Send message to channel
    }

    func main() {
        messages := make(chan string) // Create an unbuffered channel

        go worker(1, messages) // Start a worker goroutine
        go worker(2, messages)

        // Receive messages from the channel (blocking operation until message is available)
        msg1 := <-messages
        fmt.Println(msg1)
        msg2 := <-messages
        fmt.Println(msg2)

        close(messages) // Close the channel when done sending (optional, but good practice)
    }
    ```

      * **Buffered Channels:** `make(chan Type, capacity)`. Allows sending `capacity` messages without a receiver being ready.
      * **`select` Statement:** Waits on multiple channel operations. Like a `switch` for channels.
        ```go
        c1 := make(chan string)
        c2 := make(chan string)

        go func() {
            time.Sleep(1 * time.Second)
            c1 <- "one"
        }()
        go func() {
            time.Sleep(2 * time.Second)
            c2 <- "two"
        }()

        for i := 0; i < 2; i++ {
            select {
            case msg1 := <-c1:
                fmt.Println("received", msg1)
            case msg2 := <-c2:
                fmt.Println("received", msg2)
            case <-time.After(3 * time.Second): // Timeout!
                fmt.Println("timeout")
            }
        }
        ```

-----

### Packages & Modules

Go code is organized into packages.

  * **`package main`:** Denotes an executable program. Contains the `main` function.
  * **Other Packages:** Libraries of functions and types.
      * `fmt`: Formatting (print, scan).
      * `io`: Basic I/O primitives.
      * `os`: Operating system interaction.
      * `net/http`: HTTP client/server.
      * `encoding/json`: JSON encoding/decoding.
      * `sync`: Synchronization primitives (Mutexes, WaitGroups).
      * `time`: Time operations.
  * **Importing Packages:**
    ```go
    import (
        "fmt"        // Standard library package
        "net/http"   // Another standard package
        "github.com/gin-gonic/gin" // Third-party package
    )
    ```
  * **Modules (Go 1.11+):**
    Your project's root directory.
    ```bash
    go mod init your-module-name  # Initializes a new module
    go mod tidy                   # Adds missing and removes unused module dependencies
    go build                      # Builds your executable
    go run main.go                # Runs your program
    go test                       # Runs tests
    ```

-----

### Panic & Recover

Go's equivalent of exceptions, but mostly for *unrecoverable* errors. Don't use them for normal error handling.

  * **`panic`:** Stops the normal flow of execution and unwinds the stack.
  * **`defer` and `recover`:** `defer` schedules a function call to be executed when the surrounding function returns (either normally or via `panic`). `recover` can catch a `panic` in a deferred function.
    ```go
    func mayPanic() {
        defer func() { // This deferred function will run even if panic occurs
            if r := recover(); r != nil { // recover() captures the panic value
                fmt.Println("Recovered from panic:", r)
            }
        }()
        fmt.Println("About to panic...")
        panic("something bad happened!") // This will cause a panic
        fmt.Println("This line will not be executed.")
    }

    func main() {
        mayPanic()
        fmt.Println("Program continues after panic recovery.")
    }
    ```
      * *Pro-Tip:* Only use `panic`/`recover` for truly exceptional, unrecoverable errors, or when dealing with APIs that signal fatal errors this way. Prefer `error` returns for expected errors.
