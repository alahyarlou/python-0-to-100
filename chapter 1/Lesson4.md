# Chapter 1 — Lesson 4: Functions, Parameters, Return Values, Scope, `*args`, and `**kwargs`

Functions are one of the most important parts of Python.

Before classes, Django views, serializers, model methods, services, selectors, permissions, and managers make sense, you need to understand functions very well.

A function is basically:

> A reusable block of logic that receives input, performs work, and optionally returns output.

---

# 1. Your first function

```python
def greet():
    print("Hello")
```

This defines a function named:

```python
greet
```

But defining it does not execute it.

To execute it:

```python
greet()
```

Output:

```text
Hello
```

Think:

```text
define
  ↓
def greet():
    ...

call
  ↓
greet()
```

---

# 2. Function anatomy

Consider:

```python
def greet_user(name):
    print(f"Hello {name}")
```

We have:

```text
def             → define a function
greet_user      → function name
name            → parameter
:               → function body begins
indented block  → function logic
```

Then:

```python
greet_user("Ali")
```

Output:

```text
Hello Ali
```

Here:

```python
name
```

is the **parameter**.

And:

```python
"Ali"
```

is the **argument**.

---

# 3. Parameter vs argument

This distinction is useful.

Function definition:

```python
def greet(name):
    ...
```

`name` is a parameter.

Function call:

```python
greet("Ali")
```

`"Ali"` is an argument.

So:

```text
parameter = variable declared by function
argument  = actual value passed into function
```

---

# 4. Multiple parameters

```python
def create_business(name, category):
    print(name)
    print(category)
```

Call:

```python
create_business("Python Cafe", "Cafe")
```

Python assigns:

```text
name     = "Python Cafe"
category = "Cafe"
```

---

# 5. Functions should usually return values

Consider:

```python
def add(a, b):
    result = a + b
    print(result)
```

This only prints.

A more reusable function is:

```python
def add(a, b):
    return a + b
```

Then:

```python
result = add(10, 20)

print(result)
```

Output:

```text
30
```

The function produces a value that the caller can use.

---

# 6. `return`

`return` sends a value back to the caller.

```python
def get_business_name():
    return "Python Cafe"
```

Then:

```python
name = get_business_name()
```

Conceptually:

```text
get_business_name()
        ↓
  "Python Cafe"
        ↓
      name
```

---

# 7. `return` stops the function

This is important.

```python
def example():
    print("A")

    return

    print("B")
```

Calling:

```python
example()
```

prints:

```text
A
```

`"B"` never executes.

This is why `return` is useful for guard clauses.

---

# 8. Guard clauses

Suppose:

```python
def can_publish_business(is_active, is_verified):
    if not is_active:
        return False

    if not is_verified:
        return False

    return True
```

This reads naturally:

```text
inactive?
→ False

not verified?
→ False

otherwise
→ True
```

Compare it with unnecessary nesting:

```python
def can_publish_business(is_active, is_verified):
    if is_active:
        if is_verified:
            return True
        else:
            return False
    else:
        return False
```

The first version is much cleaner.

---

# 9. Functions return `None` by default

Consider:

```python
def hello():
    print("Hello")
```

Then:

```python
result = hello()

print(result)
```

Output:

```text
Hello
None
```

Why?

Because a function without an explicit `return` effectively returns:

```python
None
```

This is important later when debugging.

---

# 10. Returning multiple values

Python lets you write:

```python
def get_business_info():
    return "Python Cafe", 4.8
```

Then:

```python
name, rating = get_business_info()

print(name)
print(rating)
```

Output:

```text
Python Cafe
4.8
```

Technically, Python returns a tuple:

```python
("Python Cafe", 4.8)
```

and then unpacking happens.

---

# 11. Positional arguments

Consider:

```python
def create_user(name, phone):
    print(name)
    print(phone)
```

Call:

```python
create_user("Ali", "09123456789")
```

Arguments are matched by position:

```text
first argument  → name
second argument → phone
```

These are positional arguments.

---

# 12. Keyword arguments

You can also write:

```python
create_user(
    name="Ali",
    phone="09123456789",
)
```

These are keyword arguments.

This is often clearer.

Especially when functions have many arguments.

Instead of:

```python
create_business(
    "Python Cafe",
    3,
    8,
    True,
)
```

prefer something like:

```python
create_business(
    name="Python Cafe",
    category_id=3,
    province_id=8,
    is_verified=True,
)
```

Much easier to understand.

---

# 13. Default parameters

You can define defaults:

```python
def create_business(name, is_active=True):
    return {
        "name": name,
        "is_active": is_active,
    }
```

Then:

```python
business = create_business("Python Cafe")
```

Result:

```python
{
    "name": "Python Cafe",
    "is_active": True,
}
```

Or override it:

```python
business = create_business(
    "Python Cafe",
    is_active=False,
)
```

---

# 14. Required parameters should come first

This is valid:

```python
def create_business(name, is_active=True):
    ...
```

This is invalid:

```python
def create_business(is_active=True, name):
    ...
```

Python doesn't allow required positional parameters after default parameters.

So usually:

```text
required first
optional/default after
```

---

# 15. A dangerous default argument mistake

Very important.

Do not usually write:

```python
def add_category(category, categories=[]):
    categories.append(category)
    return categories
```

Now:

```python
print(add_category("Cafe"))
print(add_category("Hotel"))
```

You may expect:

```text
['Cafe']
['Hotel']
```

but you'll get behavior similar to:

```text
['Cafe']
['Cafe', 'Hotel']
```

Why?

Because default argument objects are created when the function is defined, not every time it is called.

The same list is reused.

---

# 16. Correct mutable default pattern

Use:

```python
def add_category(category, categories=None):
    if categories is None:
        categories = []

    categories.append(category)

    return categories
```

Now each call can create its own list.

This pattern appears constantly in professional Python code:

```python
def something(options=None):
    if options is None:
        options = {}
```

Remember this well.

---

# 17. Scope

Variables created inside functions normally belong to that function.

```python
def example():
    message = "Hello"
    print(message)
```

This works inside:

```python
example()
```

But this does not:

```python
print(message)
```

outside the function.

You'll get something like:

```text
NameError
```

because `message` is local to the function.

---

# 18. Local scope

Example:

```python
name = "Global Cafe"

def show_name():
    name = "Local Cafe"
    print(name)

show_name()
print(name)
```

Output:

```text
Local Cafe
Global Cafe
```

There are two separate names.

Conceptually:

```text
global scope:
name = "Global Cafe"

function scope:
name = "Local Cafe"
```

---

# 19. Reading global values

A function can read an outer variable:

```python
tax_rate = 0.1

def calculate_tax(price):
    return price * tax_rate
```

This works.

But functions that heavily depend on global state become harder to test and understand.

Often it's better to pass dependencies explicitly:

```python
def calculate_tax(price, tax_rate):
    return price * tax_rate
```

This becomes very important later for clean architecture.

---

# 20. Avoid `global` in normal application code

Python allows:

```python
count = 0

def increment():
    global count
    count += 1
```

But this often makes code harder to reason about.

In real Django applications, global mutable state is usually a bad idea.

Why?

Because:

```text
multiple requests
multiple workers
multiple processes
multiple servers
```

may all exist.

So this:

```python
current_user = None
```

as global application state would be a terrible design.

---

# 21. Functions are objects

This is a very important bridge toward advanced Python.

Consider:

```python
def greet():
    return "Hello"
```

Now:

```python
print(type(greet))
```

You'll see:

```text
<class 'function'>
```

Functions themselves are objects.

You can assign them:

```python
say_hello = greet
```

Then:

```python
print(say_hello())
```

Output:

```text
Hello
```

Notice:

```python
say_hello = greet
```

not:

```python
say_hello = greet()
```

The first assigns the function object.

The second executes it and assigns its return value.

---

# 22. Function reference vs function call

This distinction is fundamental.

```python
greet
```

means:

> the function object

while:

```python
greet()
```

means:

> call the function

This later helps explain things such as:

```python
permission_classes = [IsAuthenticated]
```

versus:

```python
IsAuthenticated()
```

and decorators and callbacks.

---

# 23. Functions can receive functions

Because functions are objects:

```python
def run_operation(operation, a, b):
    return operation(a, b)
```

Now:

```python
def add(a, b):
    return a + b

result = run_operation(add, 10, 20)
```

Output:

```text
30
```

Conceptually:

```text
add function
    ↓
passed into
run_operation
    ↓
executed there
```

This idea appears everywhere in Python frameworks.

---

# 24. Type hints

Python lets us document expected types.

Without hints:

```python
def add(a, b):
    return a + b
```

With hints:

```python
def add(a: int, b: int) -> int:
    return a + b
```

This says:

```text
a       expected int
b       expected int
returns expected int
```

Important:

Type hints normally do not enforce types automatically at runtime.

Python can still call:

```python
add("a", "b")
```

unless your tooling or code prevents it.

Type hints primarily help:

- developers
- IDEs
- static analyzers
- code readability

---

# 25. Realistic type hints

```python
def can_publish_business(
    *,
    is_active: bool,
    is_verified: bool,
) -> bool:
    return is_active and is_verified
```

Notice something new:

```python
*
```

We'll explain that next.

---

# 26. Keyword-only arguments

Consider:

```python
def create_business(
    *,
    name,
    category,
    province,
):
    ...
```

The standalone:

```python
*
```

means arguments after it must be passed by keyword.

This works:

```python
create_business(
    name="Python Cafe",
    category="Cafe",
    province="Tehran",
)
```

This does not:

```python
create_business(
    "Python Cafe",
    "Cafe",
    "Tehran",
)
```

Why use keyword-only parameters?

Because application/business functions become much clearer.

Compare:

```python
register_user(
    "09123456789",
    "Ali",
    True,
    False,
)
```

with:

```python
register_user(
    phone="09123456789",
    name="Ali",
    is_active=True,
    is_verified=False,
)
```

The second is much easier to read.

---

# 27. This is why you'll see `*` in service functions

Later, I may recommend code such as:

```python
def create_business(
    *,
    owner,
    name: str,
    category,
    province,
):
    ...
```

That is not mysterious Python syntax.

It simply says:

> These parameters must be explicitly named when calling the function.

So:

```python
create_business(
    owner=request.user,
    name="Python Cafe",
    category=category,
    province=province,
)
```

becomes self-documenting.

---

# 28. `*args`

Now another use of `*`.

Suppose you don't know how many positional arguments will be passed:

```python
def show_numbers(*args):
    print(args)
```

Then:

```python
show_numbers(1, 2, 3, 4)
```

Output:

```python
(1, 2, 3, 4)
```

`args` is a tuple.

Conceptually:

```text
1
2
3
4
 ↓
args = (1, 2, 3, 4)
```

The name `args` is conventional, not mandatory.

You could write:

```python
def show_numbers(*numbers):
```

and that is often even clearer.

---

# 29. Using `*args`

```python
def total(*numbers):
    result = 0

    for number in numbers:
        result += number

    return result
```

Call:

```python
print(total(1, 2, 3, 4))
```

Output:

```text
10
```

---

# 30. `**kwargs`

Now:

```python
def show_data(**kwargs):
    print(kwargs)
```

Call:

```python
show_data(
    name="Python Cafe",
    rating=4.8,
    is_verified=True,
)
```

Output conceptually:

```python
{
    "name": "Python Cafe",
    "rating": 4.8,
    "is_verified": True,
}
```

`kwargs` is a dictionary.

Think:

```text
keyword arguments
      ↓
dictionary
      ↓
kwargs
```

---

# 31. Iterating through `kwargs`

```python
def show_data(**kwargs):
    for key, value in kwargs.items():
        print(key, value)
```

Call:

```python
show_data(
    name="Python Cafe",
    rating=4.8,
)
```

Output:

```text
name Python Cafe
rating 4.8
```

---

# 32. `args` and `kwargs` names are conventions

Technically:

```python
def example(*values, **options):
    ...
```

works perfectly.

But you'll usually see:

```python
*args
**kwargs
```

because that's the Python convention.

Later you will encounter this constantly:

```python
def __init__(self, *args, **kwargs):
    super().__init__(*args, **kwargs)
```

If you understand today's lesson, that code will not look mysterious later.

---

# 33. Unpacking with `*`

Suppose:

```python
numbers = [1, 2, 3]
```

and:

```python
def add_three(a, b, c):
    return a + b + c
```

Instead of:

```python
add_three(
    numbers[0],
    numbers[1],
    numbers[2],
)
```

you can write:

```python
add_three(*numbers)
```

Python unpacks:

```python
[1, 2, 3]
```

into:

```python
add_three(1, 2, 3)
```

---

# 34. Unpacking dictionaries with `**`

This is extremely useful.

Suppose:

```python
business_data = {
    "name": "Python Cafe",
    "category": "Cafe",
}
```

and:

```python
def create_business(name, category):
    return {
        "name": name,
        "category": category,
    }
```

You can call:

```python
business = create_business(**business_data)
```

Python converts the dictionary into:

```python
create_business(
    name="Python Cafe",
    category="Cafe",
)
```

This concept becomes extremely important with Django and DRF.

---

# 35. DRF preview: `validated_data`

Later you'll see something like:

```python
validated_data = {
    "name": "Python Cafe",
    "category": category,
    "province": province,
}
```

Then perhaps:

```python
business = create_business(
    owner=request.user,
    **validated_data,
)
```

That means:

```python
business = create_business(
    owner=request.user,
    name="Python Cafe",
    category=category,
    province=province,
)
```

So:

```python
**validated_data
```

is simply dictionary unpacking.

---

# 36. Combining regular parameters and `**kwargs`

Example:

```python
def create_business(owner, **data):
    print(owner)
    print(data)
```

Call:

```python
create_business(
    "Ali",
    name="Python Cafe",
    category="Cafe",
)
```

Then:

```text
owner = "Ali"

data = {
    "name": "Python Cafe",
    "category": "Cafe",
}
```

---

# 37. Function design: one responsibility

Bad:

```python
def handle_business(data):
    # validate input
    # create business
    # send email
    # send SMS
    # generate report
    # update analytics
    # create invoice
    # return API response
    ...
```

This function does too much.

Better thinking:

```python
validate_business_data(...)
create_business(...)
send_business_created_email(...)
record_business_created_event(...)
```

This principle becomes very important later when we design service layers.

---

# 38. Pure functions

A pure function generally:

- depends only on its inputs
- returns an output
- doesn't modify outside state

Example:

```python
def calculate_average_rating(total, count):
    if count == 0:
        return None

    return total / count
```

This is easy to test.

```python
assert calculate_average_rating(20, 4) == 5
```

---

# 39. Functions with side effects

Consider:

```python
def print_business(name):
    print(name)
```

This has a side effect: writing output.

Other side effects include:

```text
writing to database
sending email
sending SMS
writing a file
calling external API
logging
```

Side effects are not bad.

Real applications need them.

But distinguishing:

```text
pure calculation
vs
side effect
```

helps us design better software.

---

# 40. Mutation through function arguments

Remember Lesson 1.

Lists are mutable.

Consider:

```python
def add_category(categories):
    categories.append("Cafe")
```

Then:

```python
categories = []

add_category(categories)

print(categories)
```

Output:

```text
['Cafe']
```

Why?

Because the function receives a reference to the same list object.

Conceptually:

```text
outside:
categories ───────┐
                  ▼
                  []

inside function:
categories ───────┘
```

The function mutates the original object.

---

# 41. Avoid surprising mutations

Compare:

```python
def add_category(categories, category):
    categories.append(category)
```

with:

```python
def with_category(categories, category):
    result = categories.copy()
    result.append(category)

    return result
```

The second version leaves the original untouched.

Which is better depends on your intention.

The important thing is to know whether mutation is happening.

---

# 42. Function naming

Functions usually represent actions.

Good:

```python
create_business()
get_business()
calculate_rating()
send_otp()
validate_phone()
publish_business()
```

Poor:

```python
business()
thing()
do_it()
process()
```

Good names describe intent.

---

# 43. Boolean function naming

Functions returning boolean values often start with:

```text
is_
has_
can_
should_
```

Examples:

```python
def is_business_active(...):
    ...

def has_permission(...):
    ...

def can_publish_business(...):
    ...

def should_send_notification(...):
    ...
```

Then calling code reads naturally:

```python
if can_publish_business(...):
    ...
```

---

# 44. Realistic example

Let's create a business rule.

```python
def can_recommend_business(
    *,
    status: str,
    is_verified: bool,
    rating: float,
    review_count: int,
) -> bool:
    if status != "active":
        return False

    if not is_verified:
        return False

    if rating < 4.2:
        return False

    if review_count < 20:
        return False

    return True
```

Call:

```python
recommended = can_recommend_business(
    status="active",
    is_verified=True,
    rating=4.8,
    review_count=35,
)

print(recommended)
```

Output:

```text
True
```

This is already good application logic.

---

# 45. Why functions matter for Django

Later, you might initially write:

```python
class BusinessAPIView(APIView):
    def post(self, request):
        ...
```

Inside `post`, you might be tempted to put 100 lines of business logic.

Instead, we may extract:

```python
def create_business(
    *,
    user,
    name,
    category,
    province,
):
    ...
```

Then the view can do:

```python
business = create_business(
    user=request.user,
    **serializer.validated_data,
)
```

This is why strong function fundamentals matter before Django architecture.

---

# 46. Preview: methods are functions

Later you'll write:

```python
class Business:
    def publish(self):
        ...
```

That:

```python
publish
```

is still fundamentally a function.

The main difference is that it lives inside a class and receives an instance through:

```python
self
```

So:

```text
function knowledge
       ↓
methods
       ↓
classes
       ↓
Django models
serializers
views
permissions
```

---

# Exercise 1 — Basic function

Write:

```python
def get_business_label(name, is_verified):
    ...
```

Rules:

If verified:

```text
Python Cafe - Verified
```

otherwise:

```text
Python Cafe - Unverified
```

Return the string.

Do not print inside the function.

Then:

```python
label = get_business_label(...)
print(label)
```

---

# Exercise 2 — Guard clauses

Write:

```python
def can_publish_business(
    is_active,
    is_verified,
    has_category,
):
    ...
```

Return `False` immediately for each invalid condition.

Return `True` only if everything passes.

---

# Exercise 3 — Keyword-only function

Write:

```python
def create_business(...):
```

with these arguments:

```text
name
category
province
is_active
```

Make all parameters keyword-only.

Give:

```python
is_active
```

a default of:

```python
True
```

Return a dictionary.

Then call it like:

```python
business = create_business(
    name="Python Cafe",
    category="Cafe",
    province="Tehran",
)
```

---

# Exercise 4 — Mutable default

Explain why this is dangerous:

```python
def add_role(role, roles=[]):
    roles.append(role)
    return roles
```

Then rewrite it safely using:

```python
None
```

---

# Exercise 5 — `*args`

Write:

```python
def average(*numbers):
    ...
```

It should calculate the average.

For:

```python
average(4, 5, 3, 4)
```

return:

```text
4.0
```

Also decide what should happen if no numbers are passed.

---

# Exercise 6 — `**kwargs`

Write:

```python
def print_business_info(**kwargs):
    ...
```

Then call:

```python
print_business_info(
    name="Python Cafe",
    rating=4.7,
    status="active",
)
```

Print each key and value.

---

# Exercise 7 — Dictionary unpacking

Given:

```python
business_data = {
    "name": "Python Cafe",
    "category": "Cafe",
    "province": "Tehran",
}
```

and:

```python
def create_business(name, category, province):
    ...
```

Call it using:

```python
**
```

instead of passing each field manually.

---

# Mini challenge

Create:

```python
def validate_business(
    *,
    name,
    status,
    is_verified,
    rating,
):
    ...
```

Rules:

- if `name` is empty → return `"Name is required"`
- if status isn't `"active"` → return `"Business is inactive"`
- if not verified → return `"Business is not verified"`
- if rating is below `4` → return `"Rating is too low"`
- otherwise → return `"Business is valid"`

Use guard clauses.

Then test at least five different inputs.

---

## What you should understand before Lesson 5

You should be able to explain:

```text
function
parameter
argument
return value
local scope
default argument
positional argument
keyword argument
keyword-only argument
*args
**kwargs
```

And especially these:

```python
def create_business(*, name, category):
```

means:

> `name` and `category` must be supplied by keyword.

This:

```python
create_business(**business_data)
```

means:

> unpack dictionary keys/values into keyword arguments.

And:

```python
def example(*args, **kwargs):
```

means:

> accept arbitrary positional and keyword arguments.

These three ideas will appear constantly once we reach OOP and Django.

## Next lesson

**Chapter 1 — Lesson 5: Lists, Tuples, Dictionaries, Sets, Comprehensions, and Data Transformation**
