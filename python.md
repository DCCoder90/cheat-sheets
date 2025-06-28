### The Absolute Basics

  * **Variables (Assignments):**
    Assigning values to names. Python is dynamically typed, so no explicit type declaration is needed.

    ```python
    # Your app's name
    app_name = "SomeService"
    # How many times you've restarted your dev environment today
    restarts_today = 7
    # Is it deployed?
    is_deployed = True
    ```

  * **Code Blocks (Indentation):**
    Python uses indentation (usually 4 spaces) to define code blocks, instead of braces or keywords.

    ```python
    # This is a 'function' block.
    def greet(name):
        print(f"Hello, {name}!") # This line is part of the 'greet' block.
        print("Nice to see you.") # So is this one.

    # This is an 'if' block.
    if is_deployed:
        print("Application is running smoothly!")
    else:
        print("Deployment pending...")
    ```

  * **Comments:**

    ```python
    # This function handles all the cat GIFs.
    # Seriously, all of them.
    '''
    Multi-line comment for when you have a lot
    of wisdom to impart. Or just a long rant.
    '''
    """
    Another way to write multi-line comments,
    often used for docstrings.
    """
    ```

-----

### Data Types

  * **Strings (`str`):**
    Immutable sequences of characters.

    ```python
    greeting = "Hello, developer!"
    # f-strings for dynamic strings (Python 3.6+).
    user_name = "DevOps Dan"
    full_greeting = f"Hey there, {user_name}! Ready to code?"
    ```

  * **Numbers (`int`, `float`, `complex`):**

    ```python
    port = 8080       # int
    version = 1.23    # float
    complex_num = 3 + 2j # complex
    ```

  * **Booleans (`bool`):**
    `True` or `False`. Case-sensitive.

    ```python
    debug_mode = True
    production_ready = False
    ```

  * **Lists (`list`):**
    Mutable, ordered collections of items. Enclosed in square brackets `[]`.

    ```python
    favorite_languages = ["Go", "Python", "JavaScript"]
    # Mix and match
    mixed_bag = ["one", 2, True]
    ```

  * **Tuples (`tuple`):**
    Immutable, ordered collections of items. Enclosed in parentheses `()`.

    ```python
    coordinates = (10, 20)
    # coordinates[0] = 5 # This would cause an error
    ```

  * **Dictionaries (`dict`):**
    Mutable, unordered collections of key-value pairs. Enclosed in curly braces `{}`.

    ```python
    user_details = {
        "name": "Ops Dan",
        "email": "dan@dev.com",
        "role": "Cloudy McCloudster"
    }
    ```

  * **Sets (`set`):**
    Mutable, unordered collections of unique elements. Enclosed in curly braces `{}`.

    ```python
    unique_ids = {1, 2, 2, 3, 1} # Evaluates to {1, 2, 3}
    ```

  * **None (`NoneType`):**
    Represents the absence of a value, similar to `null` in other languages.

    ```python
    # Use this when a value is optional or not set.
    optional_setting = None
    ```

-----

### Expressions & Operators

  * **References (Variables, Attributes, Indices):**

    ```python
    # Grab a variable's value
    aws_region = "us-east-1"
    region = aws_region

    # Accessing list elements by index
    first_lang = favorite_languages[0] # "Go"

    # Accessing dictionary values by key
    user_name = user_details["name"] 
    ```

  * **Operators:**
    All your basic math (`+`, `-`, `*`, `/`, `//` (floor), `%` (modulo), `**` (exponent)), comparisons (`==`, `!=`, `<`, `<=`, `>`, `>=`), and logic (`and`, `or`, `not`).

    ```python
    is_admin = user_details["role"] == "admin" or user_details["name"] == "root"
    ```

  * **Conditional (Ternry Operator):**
    `value_if_true if condition else value_if_false`

    ```python
    is_prod = True
    instance_type = "m5.large" if is_prod else "t3.micro"
    ```

  * **f-strings (Interpolation - Python 3.6+):**
    A powerful way to embed expressions inside string literals.

    ```python
    environment = "dev"
    server_id = f"app-{environment}-{1}" # app-dev-1
    ```

-----

### Built-in Functions

  * **Strings:**

      * `"HELLO".lower()` -\> `"hello"`
      * `"world".upper()` -\> `"WORLD"`
      * `"-".join(["app", "01"])` -\> `"app-01"`
      * `"a,b,c".split(",")` -\> `['a', 'b', 'c']`
      * `"  trim me  ".strip()` -\> `"trim me"`

  * **Collections:**

      * `len(["a", "b", "c"])` -\> `3`
      * `list_data = ["x", "y", "z"]; list_data[1]` -\> `"y"`
      * `my_dict = {"a": "1", "b": "2"}; my_dict.get("b", "default")` -\> `"2"`
      * `my_dict.keys()` -\> `dict_keys(['a', 'b'])`
      * `my_dict.values()` -\> `dict_values(['1', '2'])`

  * **Numeric:**

      * `abs(-5)` -\> `5`
      * `math.ceil(3.14)` (requires `import math`) -\> `4`
      * `math.floor(3.99)` (requires `import math`) -\> `3`
      * `round(3.14159, 2)` -\> `3.14`

  * **Type Conversions:**

      * `int("123")` -\> `123`
      * `str(123)` -\> `"123"`
      * `float("3.14")` -\> `3.14`
      * `list(my_tuple)`
      * `tuple(my_list)`

-----

### Input & Output

  * **User Input:**

    ```python
    user_name = input("Enter your name: ")
    print(f"Hello, {user_name}!")
    ```

  * **Printing to Console:**

    ```python
    print("Hello, World!")
    print("Name:", user_name, "Age:", 30)
    ```

  * **File Handling:**

    ```python
    # Writing to a file
    with open("my_file.txt", "w") as f:
        f.write("Hello from Python!\n")
        f.write("This is a new line.")

    # Reading from a file
    with open("my_file.txt", "r") as f:
        content = f.read()
        print(content)

    # Reading line by line
    with open("my_file.txt", "r") as f:
        for line in f:
            print(line.strip()) # .strip() removes leading/trailing whitespace includiing newline
    ```

-----

### Control Flow

  * **Conditional Statements (`if`/`elif`/`else`):**

    ```python
    age = 20
    if age < 18:
        print("Minor")
    elif age < 65:
        print("Adult")
    else:
        print("Senior")
    ```

  * **For Loops:**
    Iterating over sequences (lists, tuples, strings, dictionaries, ranges).

    ```python
    # Iterate over a range of numbers
    for i in range(5): # 0, 1, 2, 3, 4
        print(i)

    # Iterate over a list
    fruits = ["apple", "banana", "cherry"]
    for fruit in fruits:
        print(fruit)

    # Iterate with index
    for index, fruit in enumerate(fruits):
        print(f"{index}: {fruit}")

    # Iterate over dictionary items
    for key, value in user_details.items():
        print(f"{key}: {value}")
    ```

  * **While Loops:**
    Repeats as long as a condition is true.

    ```python
    count = 0
    while count < 3:
        print(count)
        count += 1
    ```

  * **Loop Control (`break`, `continue`):**

    ```python
    for i in range(10):
        if i == 3:
            continue # Skip to the next iteration if i is 3
        if i == 7:
            break    # Exit the loop if i is 7
        print(i)
    ```

-----

### Functions

  * **Defining a Function:**
    Reusable blocks of code.

    ```python
    def greet(name, greeting="Hello"): # 'greeting' has a default value
        """
        This function greets a person.
        Args:
            name (str): The name of the person.
            greeting (str): The greeting message (default is "Hello").
        Returns:
            str: The full greeting message.
        """
        return f"{greeting}, {name}!"

    message = greet("Alice")
    print(message) # Hello, Alice!
    print(greet("Bob", "Hi")) # Hi, Bob!
    ```

  * **Arbitrary Arguments (`*args`, `**kwargs`):**

      * `*args`: Collects positionl arguments into a tuple.
      * `**kwargs`: Collects keyword arguments into a dictionary.

    <!-- end list -->

    ```python
    def sum_all(*numbers):
        return sum(numbers)

    print(sum_all(1, 2, 3, 4)) # 10

    def create_user(**details):
        print("Creating user with details:")
        for key, value in details.items():
            print(f"  {key}: {value}")

    create_user(name="Charlie", age=40, city="London")
    ```

  * **Lambda Functions (Anonymous Functions):**
    Small, single-expression functions.

    ```python
    add_one = lambda x: x + 1
    print(add_one(5)) # 6
    ```

-----

### Data Structures & Comprehensions

  * **List Comprehensions:**
    Concise way to create lists.

    ```python
    squares = [x**2 for x in range(5)] # [0, 1, 4, 9, 16]
    even_numbers = [x for x in range(10) if x % 2 == 0] # [0, 2, 4, 6, 8]
    ```

  * **Dictionary Comprehensions:**

    ```python
    squares_dict = {x: x**2 for x in range(5)} # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
    ```

  * **Set Comprehensions:**

    ```python
    unique_letters = {char for char in "hello world" if char.isalpha()} # {'h', 'e', 'l', 'o', ' ', 'w', 'r', 'd'}
    ```

-----

### Object-Oriented Programming (OOP) Basics

  * **Classes & Objects:**
    Blueprints for creating objects (instances).

    ```python
    class Dog:
        # Class attribute
        species = "Canis familiaris"

        # Constructor (initializer)
        def __init__(self, name, age):
            self.name = name  # Instance attribute
            self.age = age    # Instance attribute

        # Instance method
        def bark(self):
            return f"{self.name} says Woof!"

        # Another instance method
        def description(self):
            return f"{self.name} is {self.age} years old."

    # Create objects (instances)
    my_dog = Dog("Buddy", 3)
    your_dog = Dog("Lucy", 5)

    print(my_dog.description()) # Buddy is 3 years old.
    print(your_dog.bark())    # Lucy says Woof!
    print(Dog.species)        # Canis familiaris
    ```

  * **Inheritance:**
    Creating new classes from existing ones.

    ```python
    class GoldenRetriever(Dog):
        def __init__(self, name, age, likes_swimming=True):
            super().__init__(name, age) # Call parent class constructor
            self.likes_swimming = likes_swimming

        def bark(self): # Method overriding
            return f"{self.name} says happy Woof!"

    golden = GoldenRetriever("Goldie", 2)
    print(golden.description())      # Goldie is 2 years old.
    print(golden.bark())           # Goldie says happy Woof!
    print(golden.likes_swimming)   # True
    ```