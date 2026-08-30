# Chapter 1 — Python Foundations for Django

We’ll start from the foundations that later make Django, DRF, classes, serializers, models, and QuerySets understandable.

For this chapter, we’ll work mostly with **plain Python**. No Django yet.

## Lesson 1 — Variables, Values, Objects, and Types

Before learning classes, there is one idea you need to understand very clearly:

> In Python, variables are names that refer to objects.

Consider:

```python
name = "Ali"
```

Many beginners imagine this as:

```text
name contains "Ali"
```

A better mental model is:

```text
        ┌─────────┐
name ──►│  "Ali"  │
        └─────────┘
          object
```

`name` is a **name/reference**.

`"Ali"` is a Python **object**.

This distinction becomes extremely important later when we work with:

```python
user = User(...)
business = Business(...)
queryset = Business.objects.filter(...)
serializer = BusinessSerializer(...)
```

All of these variables refer to objects.

---

# 1. Everything is an object

Consider:

```python
name = "Ali"
age = 25
price = 12.5
is_active = True
```

These values are objects of different types.

We can ask Python about their types:

```python
print(type(name))
print(type(age))
print(type(price))
print(type(is_active))
```

Output:

```text
<class 'str'>
<class 'int'>
<class 'float'>
<class 'bool'>
```

Notice something interesting:

```text
<class 'str'>
```

Python literally says `class`.

That's because:

```python
str
int
float
bool
```

are classes.

And:

```python
"Ali"
25
12.5
True
```

are objects/instances of those classes.

This is our first connection to OOP.

---

# 2. Variables don't have fixed types

Python is dynamically typed.

You can write:

```python
value = 10
print(type(value))

value = "hello"
print(type(value))
```

Output:

```text
<class 'int'>
<class 'str'>
```

The variable `value` didn't transform from an integer into a string.

Instead:

```text
Step 1

             ┌────┐
value ──────►│ 10 │
             └────┘


Step 2

             ┌─────────┐
value ──────►│ "hello" │
             └─────────┘
```

The name simply points to a different object.

This distinction will matter a lot when we start working with objects.

---

# 3. Multiple variables can reference the same object

Consider:

```python
business_name = "Open Cafe"
name = business_name
```

Conceptually:

```text
business_name ──┐
                │
                ▼
           ┌─────────────┐
           │ "Open Cafe" │
           └─────────────┘
                ▲
                │
name ───────────┘
```

Both names may refer to the same object.

Let's investigate:

```python
business_name = "Open Cafe"
name = business_name

print(business_name)
print(name)
```

Both print:

```text
Open Cafe
```

---

# 4. `==` versus `is`

This is an extremely important Python concept.

There are two different questions.

### Question 1

Do these objects have the same **value**?

Use:

```python
==
```

### Question 2

Are these literally the **same object**?

Use:

```python
is
```

Example:

```python
a = [1, 2, 3]
b = [1, 2, 3]

print(a == b)
print(a is b)
```

Usually:

```text
True
False
```

Why?

Because:

```text
a                     b
│                     │
▼                     ▼
┌─────────┐       ┌─────────┐
│ 1, 2, 3 │       │ 1, 2, 3 │
└─────────┘       └─────────┘
```

They contain equal values, but they are two separate objects.

Now:

```python
a = [1, 2, 3]
b = a

print(a == b)
print(a is b)
```

Output:

```text
True
True
```

Because:

```text
a ─────────┐
           ▼
       ┌─────────┐
       │ 1, 2, 3 │
       └─────────┘
           ▲
           │
b ─────────┘
```

This distinction becomes important in Python and Django.

---

# 5. `id()`

Python lets us inspect an object's identity:

```python
a = [1, 2, 3]
b = a

print(id(a))
print(id(b))
```

You'll get numbers similar to:

```text
4376311616
4376311616
```

Same identity.

But:

```python
a = [1, 2, 3]
b = [1, 2, 3]

print(id(a))
print(id(b))
```

will normally produce different identities.

Don't memorize the numbers.

The idea is simply:

```text
id(object)
```

identifies that particular object during its lifetime.

---

# 6. Basic Python types

For Django development, these basic types are especially important.

### Integer

```python
age = 30
count = 100
business_id = 42
```

Type:

```python
int
```

---

### Float

```python
rating = 4.7
price = 19.99
```

Type:

```python
float
```

For actual monetary database values, we'll later learn why `Decimal` is usually preferable to `float`.

---

### String

```python
name = "Coffee House"
slug = "coffee-house"
phone = "+49123456789"
```

Type:

```python
str
```

Notice:

```python
phone = "+49123456789"
```

A phone number should usually be a string, not an integer.

Why?

Because we're not performing arithmetic like:

```python
phone1 + phone2
```

Phone numbers are identifiers/textual data.

---

### Boolean

```python
is_active = True
is_verified = False
```

Type:

```python
bool
```

Useful later for things such as:

```python
business.is_verified
user.is_active
category.is_visible
```

---

### `None`

Python also has:

```python
None
```

Example:

```python
description = None
```

It means roughly:

> No value is currently present.

Check its type:

```python
print(type(None))
```

Output:

```text
<class 'NoneType'>
```

You'll see `None` constantly in Django.

For example:

```python
user = None
business = None
phone_verified_at = None
```

---

# 7. Collections

Real applications rarely deal with one value at a time.

We need collections.

## List

```python
categories = [
    "Restaurant",
    "Cafe",
    "Hotel",
]
```

A list:

- has order
- can change
- can contain duplicate values

Example:

```python
categories.append("Shop")

print(categories)
```

---

## Tuple

```python
coordinates = (35.6892, 51.3890)
```

A tuple is similar to a list, but normally treated as immutable.

We'll discuss immutability shortly.

---

## Dictionary

Extremely important.

```python
business = {
    "name": "Open Cafe",
    "category": "Cafe",
    "is_verified": True,
}
```

Access values:

```python
print(business["name"])
```

Output:

```text
Open Cafe
```

A dictionary represents:

```text
key → value
```

Conceptually:

```text
"name"        → "Open Cafe"
"category"    → "Cafe"
"is_verified" → True
```

You will see dictionaries everywhere in DRF.

For example, serializer data looks conceptually like this:

```python
{
    "name": "Open Cafe",
    "category": 4,
    "province": 8,
}
```

---

## Set

```python
roles = {"owner", "admin", "manager"}
```

Sets are useful when uniqueness matters.

For example:

```python
roles = {"owner", "admin", "owner"}

print(roles)
```

You'll effectively get:

```text
{"owner", "admin"}
```

Duplicates are removed.

---

# 8. Mutable versus immutable

This is one of the most important concepts in Python.

Some objects can be changed after creation.

They're called **mutable**.

Others cannot.

They're **immutable**.

Common immutable types include:

```text
int
float
bool
str
tuple
```

Common mutable types include:

```text
list
dict
set
```

Let's see why this matters.

---

# 9. Mutable object example

```python
categories = ["Cafe", "Hotel"]

categories.append("Restaurant")

print(categories)
```

Output:

```text
['Cafe', 'Hotel', 'Restaurant']
```

The list object itself changed.

Think:

```text
Before

categories
    │
    ▼
┌─────────────────┐
│ Cafe            │
│ Hotel           │
└─────────────────┘


After append()

categories
    │
    ▼
┌─────────────────┐
│ Cafe            │
│ Hotel           │
│ Restaurant      │
└─────────────────┘
```

Same list object, modified.

---

# 10. Why references + mutability matter

Consider carefully:

```python
categories = ["Cafe", "Hotel"]

business_categories = categories

business_categories.append("Restaurant")

print(categories)
```

What do you think it prints?

It prints:

```text
['Cafe', 'Hotel', 'Restaurant']
```

Why?

Because we didn't copy the list.

We created a second reference to the same object:

```text
categories ─────────────┐
                        │
                        ▼
                  ┌────────────┐
                  │ Cafe       │
                  │ Hotel      │
                  │ Restaurant │
                  └────────────┘
                        ▲
                        │
business_categories ────┘
```

Changing it through one variable means the other variable sees the change too.

This causes **many real bugs** in Python.

---

# 11. Copying instead

If you want a separate list:

```python
categories = ["Cafe", "Hotel"]

business_categories = categories.copy()

business_categories.append("Restaurant")

print(categories)
print(business_categories)
```

Output:

```text
['Cafe', 'Hotel']
['Cafe', 'Hotel', 'Restaurant']
```

Now:

```text
categories                 business_categories
    │                              │
    ▼                              ▼
┌─────────┐                 ┌────────────┐
│ Cafe    │                 │ Cafe       │
│ Hotel   │                 │ Hotel      │
└─────────┘                 │ Restaurant │
                            └────────────┘
```

Two separate objects.

---

# 12. Strings are immutable

Consider:

```python
name = "Cafe"
```

You cannot modify the string itself like a list.

For example, this fails:

```python
name[0] = "S"
```

Python raises:

```text
TypeError
```

Instead, operations create a new string:

```python
name = "Cafe"

name = name.upper()

print(name)
```

Now `name` refers to a new string:

```text
Before:

name → "Cafe"


After:

name → "CAFE"
```

The original string wasn't mutated.

---

# 13. Assignment versus mutation

This distinction is critical.

Consider:

```python
number = 10
number = 20
```

That's **assignment**.

`number` now points somewhere else.

But:

```python
numbers = [10, 20]
numbers.append(30)
```

That's **mutation**.

We changed the existing list.

So:

```text
assignment
    ≠
mutation
```

Remember this.

It becomes important when we learn:

- function arguments
- class attributes
- default parameters
- Django model objects
- caching
- transactions

---

# 14. A practical business example

Suppose we represent a business:

```python
business = {
    "name": "Python Cafe",
    "categories": ["Cafe", "Restaurant"],
    "rating": 4.8,
    "is_verified": True,
}
```

Look at every value:

```text
business
│
├── name ──────────► str
│
├── categories ────► list
│                     ├─ str
│                     └─ str
│
├── rating ────────► float
│
└── is_verified ──► bool
```

This is already a little object graph.

Later, instead of a dictionary:

```python
business = {
    "name": "Python Cafe",
}
```

we'll create:

```python
business = Business(
    name="Python Cafe",
)
```

And much later with Django:

```python
business = Business.objects.create(
    name="Python Cafe",
)
```

The fundamental mental model remains:

```text
variable
   ↓
object
   ↓
type/class
```

---

# 15. Type inspection exercise

Try:

```python
business_name = "Python Cafe"
business_id = 10
rating = 4.8
is_verified = False
description = None
categories = ["Cafe", "Restaurant"]
business = {
    "name": business_name,
    "rating": rating,
}

print(type(business_name))
print(type(business_id))
print(type(rating))
print(type(is_verified))
print(type(description))
print(type(categories))
print(type(business))
```

Before executing it, try to predict every answer.

---

# 16. `isinstance()`

There's another useful tool:

```python
isinstance()
```

Example:

```python
name = "Python Cafe"

print(isinstance(name, str))
```

Output:

```text
True
```

Another:

```python
rating = 4.5

print(isinstance(rating, float))
```

Output:

```text
True
```

This asks:

> Is this object an instance of this class/type?

Notice the terminology:

```python
isinstance(name, str)
```

Literally:

> Is `name` an instance of `str`?

This is directly related to OOP.

---

# 17. Your first debugging habit

Whenever you don't understand a Python value, ask:

```python
print(value)
print(type(value))
print(id(value))
```

For example:

```python
categories = ["Cafe"]

print(categories)
print(type(categories))
print(id(categories))
```

Later you'll use better debugging tools, but this simple habit teaches you what Python is actually working with.

---

# Exercise 1

Don't ask AI to generate the answer. Write this yourself.

Create the following variables:

```text
business_name
business_slug
business_rating
business_is_verified
business_description
business_categories
```

Use appropriate Python types.

Example business:

```text
Name: Python Coffee
Slug: python-coffee
Rating: 4.7
Verified: yes
Description: currently unknown
Categories:
- Cafe
- Restaurant
```

Then print:

1. Every variable.
2. The type of every variable.

---

# Exercise 2

Create:

```python
categories = ["Cafe", "Restaurant"]
```

Then:

```python
business_categories = categories
```

Add `"Bakery"` through `business_categories`.

Before running the program, answer:

```text
What will categories contain?
Why?
```

Then run it and check whether your reasoning was correct.

---

# Exercise 3

Now modify Exercise 2 so that changing:

```python
business_categories
```

does **not** modify:

```python
categories
```

Hint:

```python
.copy()
```

---

# Exercise 4 — Important

Predict the output **before executing**:

```python
a = [1, 2]
b = a

print(a == b)
print(a is b)

b.append(3)

print(a)
print(b)
```

Explain why each line produces that result.

---

# Exercise 5 — Mini challenge

Create a dictionary representing one business:

```python
business = ...
```

It must contain:

```text
id
name
slug
is_verified
rating
categories
owner
```

And `owner` should itself be another dictionary containing:

```text
id
name
phone
```

You should end up with a structure conceptually like:

```text
Business
│
├── id
├── name
├── slug
├── rating
├── categories
│   ├── ...
│   └── ...
│
└── owner
    ├── id
    ├── name
    └── phone
```

Don't use classes yet.

---

## What I want you to understand before Lesson 2

Not memorize—**understand** these sentences:

> A variable is a name referring to an object.

> Every Python object has a type.

> Types such as `str`, `int`, and `list` are classes.

> Two variables can refer to the same object.

> `==` compares values, while `is` checks object identity.

> Mutable objects can change in place.

> Lists and dictionaries are mutable.

> Strings and integers are immutable.

> Assignment and mutation are different operations.

If those ideas become intuitive, learning classes later becomes much easier.

### Next lesson

**Chapter 1, Lesson 2 — Operators, Conditions, Truthiness, and Control Flow**

We'll cover `if`, `elif`, `else`, comparisons, `and`, `or`, `not`, truthy/falsy values, membership tests, and then implement real rules such as:

```python
if user_is_verified and business_is_active:
    ...
```
