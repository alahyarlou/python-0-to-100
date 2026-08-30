# Chapter 1 — Lesson 3: Loops, Iteration, and Processing Collections

This lesson is about repeating work over collections.

In backend code, you constantly process:

```python
users
businesses
memberships
categories
reviews
querysets
```

So loops are fundamental.

## 1. The `for` loop

Suppose:

```python
categories = ["Cafe", "Restaurant", "Bakery"]
```

You can process each item:

```python
for category in categories:
    print(category)
```

Output:

```text
Cafe
Restaurant
Bakery
```

Conceptually:

```text
categories
   │
   ├── "Cafe"       → category
   ├── "Restaurant" → category
   └── "Bakery"     → category
```

On each iteration, `category` refers to the next object.

---

## 2. Read it like English

This:

```python
for category in categories:
```

can be read as:

> For each `category` in `categories`, do the following.

That naming style matters.

Prefer:

```python
for business in businesses:
```

over:

```python
for x in businesses:
```

Good names make code much easier to understand.

---

# 3. Iterating over strings

Strings are iterable too.

```python
name = "Django"

for character in name:
    print(character)
```

Output:

```text
D
j
a
n
g
o
```

Python processes one character at a time.

---

# 4. Iterating over dictionaries

Consider:

```python
business = {
    "name": "Python Coffee",
    "rating": 4.7,
    "is_verified": True,
}
```

If you write:

```python
for item in business:
    print(item)
```

you get the keys:

```text
name
rating
is_verified
```

That's because iterating directly over a dictionary iterates over its keys.

---

# 5. `.keys()`

You can be explicit:

```python
for key in business.keys():
    print(key)
```

Usually this is unnecessary because:

```python
for key in business:
```

means the same thing.

---

# 6. `.values()`

To iterate over values:

```python
for value in business.values():
    print(value)
```

Output:

```text
Python Coffee
4.7
True
```

---

# 7. `.items()`

This is very important.

```python
for key, value in business.items():
    print(key, value)
```

Output:

```text
name Python Coffee
rating 4.7
is_verified True
```

Notice:

```python
key, value
```

Python is unpacking two values on every iteration.

You'll see this often.

---

# 8. A practical validation loop

Suppose:

```python
business = {
    "name": "Python Coffee",
    "slug": "python-coffee",
    "province": "Berlin",
}
```

You can inspect every field:

```python
for field, value in business.items():
    print(f"{field}: {value}")
```

This pattern appears later in:

* serializers
* form handling
* error normalization
* configuration
* API payload processing

---

# 9. `range()`

`range()` is useful when you need a sequence of numbers.

```python
for number in range(5):
    print(number)
```

Output:

```text
0
1
2
3
4
```

Important:

```python
range(5)
```

does not include `5`.

Think:

```text
start at 0
stop before 5
```

---

# 10. Custom range

You can specify a starting point:

```python
for number in range(1, 6):
    print(number)
```

Output:

```text
1
2
3
4
5
```

And a step:

```python
for number in range(0, 10, 2):
    print(number)
```

Output:

```text
0
2
4
6
8
```

---

# 11. Don't use `range()` unnecessarily

Beginners often write:

```python
categories = ["Cafe", "Hotel", "Bakery"]

for i in range(len(categories)):
    print(categories[i])
```

This works, but if you only need each category, prefer:

```python
for category in categories:
    print(category)
```

Cleaner and more Pythonic.

Use indexes only when you actually need indexes.

---

# 12. `enumerate()`

If you need both:

* item
* index

use `enumerate()`.

```python
categories = ["Cafe", "Hotel", "Bakery"]

for index, category in enumerate(categories):
    print(index, category)
```

Output:

```text
0 Cafe
1 Hotel
2 Bakery
```

You can also start at `1`:

```python
for index, category in enumerate(categories, start=1):
    print(index, category)
```

Output:

```text
1 Cafe
2 Hotel
3 Bakery
```

This is much better than manually managing a counter.

---

# 13. `zip()`

Suppose you have:

```python
names = ["Cafe A", "Cafe B", "Cafe C"]

ratings = [4.3, 4.8, 4.1]
```

You can process corresponding items together:

```python
for name, rating in zip(names, ratings):
    print(name, rating)
```

Output:

```text
Cafe A 4.3
Cafe B 4.8
Cafe C 4.1
```

Conceptually:

```text
Cafe A ↔ 4.3
Cafe B ↔ 4.8
Cafe C ↔ 4.1
```

---

# 14. `zip()` stops at the shortest collection

Consider:

```python
names = ["A", "B", "C"]
ratings = [5, 4]
```

Then:

```python
for name, rating in zip(names, ratings):
    print(name, rating)
```

Output:

```text
A 5
B 4
```

`C` is not processed because there is no third rating.

Remember that.

---

# 15. `break`

`break` exits a loop immediately.

Example:

```python
roles = ["staff", "manager", "owner", "admin"]

for role in roles:
    print(role)

    if role == "owner":
        break
```

Output:

```text
staff
manager
owner
```

Python stops the loop after finding `"owner"`.

---

# 16. Realistic `break` example

Suppose:

```python
businesses = [
    {"name": "Cafe A", "slug": "cafe-a"},
    {"name": "Cafe B", "slug": "cafe-b"},
    {"name": "Cafe C", "slug": "cafe-c"},
]
```

We want to find `"cafe-b"`:

```python
target_slug = "cafe-b"
found_business = None

for business in businesses:
    if business["slug"] == target_slug:
        found_business = business
        break

print(found_business)
```

Once found, there's no reason to keep searching.

Later Django will often do this database-side instead:

```python
Business.objects.filter(slug="cafe-b").first()
```

But understanding loops helps you understand what the ORM is abstracting.

---

# 17. `continue`

`continue` skips the rest of the current iteration and moves to the next one.

Example:

```python
ratings = [4.5, None, 3.8, None, 5.0]

for rating in ratings:
    if rating is None:
        continue

    print(rating)
```

Output:

```text
4.5
3.8
5.0
```

`None` values are skipped.

---

# 18. `continue` in real backend logic

Suppose:

```python
businesses = [
    {"name": "Cafe A", "status": "active"},
    {"name": "Cafe B", "status": "inactive"},
    {"name": "Cafe C", "status": "active"},
]
```

If we only want active businesses:

```python
for business in businesses:
    if business["status"] != "active":
        continue

    print(business["name"])
```

Output:

```text
Cafe A
Cafe C
```

This style keeps nesting shallow.

---

# 19. Nested loops

A loop can contain another loop.

```python
businesses = [
    {
        "name": "Cafe A",
        "categories": ["Cafe", "Bakery"],
    },
    {
        "name": "Restaurant B",
        "categories": ["Restaurant", "Fast Food"],
    },
]
```

You can write:

```python
for business in businesses:
    print(business["name"])

    for category in business["categories"]:
        print("-", category)
```

Output:

```text
Cafe A
- Cafe
- Bakery
Restaurant B
- Restaurant
- Fast Food
```

Conceptually:

```text
business
   │
   └── categories
          │
          └── loop
```

---

# 20. Avoid deeply nested loops

This can quickly become difficult:

```python
for business in businesses:
    for member in business["members"]:
        for permission in member["permissions"]:
            for action in permission["actions"]:
                ...
```

Sometimes nesting is unavoidable, but it can be a sign that you should:

* extract functions
* reorganize data
* use better queries
* move logic elsewhere

Later, with Django ORM, bad nested loops can also create severe database performance problems.

---

# 21. The `while` loop

A `while` loop continues while a condition remains true.

Example:

```python
attempts = 0

while attempts < 3:
    print("Attempt:", attempts)
    attempts += 1
```

Output:

```text
Attempt: 0
Attempt: 1
Attempt: 2
```

---

# 22. Updating counters

This:

```python
attempts += 1
```

means:

```python
attempts = attempts + 1
```

Similarly:

```python
count -= 1
```

means:

```python
count = count - 1
```

---

# 23. Be careful with infinite loops

This is dangerous:

```python
attempts = 0

while attempts < 3:
    print(attempts)
```

Why?

Because `attempts` never changes.

The condition remains:

```python
0 < 3
```

forever.

So the loop never ends.

---

# 24. `while True`

Sometimes infinite loops are deliberate:

```python
while True:
    ...
```

but then you need some way out:

```python
while True:
    value = input("Type quit to stop: ")

    if value == "quit":
        break
```

You'll see similar patterns in workers, queues, and long-running processes, although frameworks usually manage them for you.

---

# 25. Counting manually

Suppose:

```python
ratings = [5, 4, 3, 5, 2]
```

You want to count how many ratings are at least 4:

```python
count = 0

for rating in ratings:
    if rating >= 4:
        count += 1

print(count)
```

Output:

```text
3
```

This teaches a fundamental pattern:

```text
initial value
↓
loop
↓
update accumulator
↓
final result
```

---

# 26. Summing manually

```python
ratings = [4.5, 5.0, 3.5]

total = 0

for rating in ratings:
    total += rating

print(total)
```

Output:

```text
13.0
```

Python already provides:

```python
sum(ratings)
```

But learning the manual version helps you understand iteration.

---

# 27. Finding a maximum manually

```python
ratings = [4.1, 3.9, 4.8, 4.5]

highest = ratings[0]

for rating in ratings:
    if rating > highest:
        highest = rating

print(highest)
```

Output:

```text
4.8
```

Later you can simply use:

```python
max(ratings)
```

But understanding the algorithm matters.

---

# 28. Building a new list with a loop

Suppose:

```python
businesses = [
    {"name": "A", "status": "active"},
    {"name": "B", "status": "inactive"},
    {"name": "C", "status": "active"},
]
```

We want active businesses:

```python
active_businesses = []

for business in businesses:
    if business["status"] == "active":
        active_businesses.append(business)

print(active_businesses)
```

This pattern is extremely important.

Later we'll simplify it with comprehensions:

```python
active_businesses = [
    business
    for business in businesses
    if business["status"] == "active"
]
```

And much later Django will express the same idea as:

```python
Business.objects.filter(status="active")
```

See the progression:

```text
manual Python loop
        ↓
list comprehension
        ↓
Django QuerySet filter
```

Understanding the first makes the third less magical.

---

# 29. Transforming data

Suppose:

```python
businesses = [
    {"name": "Cafe A"},
    {"name": "Cafe B"},
    {"name": "Cafe C"},
]
```

You only want names:

```python
names = []

for business in businesses:
    names.append(business["name"])

print(names)
```

Output:

```text
['Cafe A', 'Cafe B', 'Cafe C']
```

This is a transformation.

Input:

```text
business dictionaries
```

Output:

```text
business names
```

You'll do this constantly in API development.

---

# 30. Filtering and transforming together

```python
businesses = [
    {"name": "Cafe A", "status": "active"},
    {"name": "Cafe B", "status": "inactive"},
    {"name": "Cafe C", "status": "active"},
]
```

We want names of active businesses:

```python
active_names = []

for business in businesses:
    if business["status"] != "active":
        continue

    active_names.append(business["name"])
```

Result:

```text
['Cafe A', 'Cafe C']
```

---

# 31. Looping over copied data versus original data

Be cautious when modifying a collection while iterating over it.

Bad pattern:

```python
numbers = [1, 2, 3, 4]

for number in numbers:
    if number % 2 == 0:
        numbers.remove(number)
```

This may behave unexpectedly because you're changing the collection you're currently iterating over.

A safer approach is often to create a new collection:

```python
odd_numbers = []

for number in numbers:
    if number % 2 != 0:
        odd_numbers.append(number)
```

Later comprehensions make this even cleaner.

---

# 32. Loop `else`

Python has a feature many beginners don't know.

A loop can have an `else` block:

```python
roles = ["staff", "manager", "admin"]

for role in roles:
    if role == "owner":
        print("Owner found")
        break
else:
    print("Owner not found")
```

Output:

```text
Owner not found
```

The `else` executes if the loop completes without `break`.

If owner exists:

```python
roles = ["staff", "owner", "admin"]
```

then:

```text
Owner found
```

and the `else` doesn't run.

This can be useful, although many teams prefer other patterns because loop `else` is less familiar.

---

# 33. Unpacking in loops

Remember:

```python
for key, value in business.items():
```

This is tuple unpacking.

Conceptually, `.items()` produces pairs similar to:

```python
("name", "Python Coffee")
("rating", 4.7)
```

Python takes:

```python
("name", "Python Coffee")
```

and assigns:

```text
key   = "name"
value = "Python Coffee"
```

We'll learn unpacking in more depth later.

---

# 34. Real example: memberships

Suppose:

```python
memberships = [
    {
        "business": "Cafe A",
        "role": "owner",
        "status": "active",
    },
    {
        "business": "Cafe B",
        "role": "staff",
        "status": "inactive",
    },
    {
        "business": "Cafe C",
        "role": "manager",
        "status": "active",
    },
]
```

We want names of businesses the user currently has active access to:

```python
accessible_businesses = []

for membership in memberships:
    if membership["status"] != "active":
        continue

    if membership["role"] not in {"owner", "admin", "manager"}:
        continue

    accessible_businesses.append(
        membership["business"]
    )

print(accessible_businesses)
```

Result:

```text
['Cafe A', 'Cafe C']
```

This is already very similar to real authorization/business logic.

---

# 35. How Django will eventually replace some loops

Suppose you write:

```python
active_businesses = []

for business in businesses:
    if business["status"] == "active":
        active_businesses.append(business)
```

With Django ORM, the database should usually do this work:

```python
active_businesses = Business.objects.filter(
    status="active"
)
```

That distinction will become very important.

You generally don't want:

```python
for business in Business.objects.all():
    if business.status == "active":
        ...
```

if the database can simply filter:

```python
Business.objects.filter(status="active")
```

Why?

Because the second version avoids loading irrelevant rows.

So loops are fundamental, but knowing **where the loop should execute** is part of professional backend development.

---

# 36. N+1 query preview

Consider:

```python
for membership in memberships:
    print(membership.business.name)
```

In plain Python, this looks harmless.

But with Django ORM, depending on how `memberships` was loaded, each:

```python
membership.business
```

could execute another SQL query.

Then:

```text
1 query for memberships
+
100 queries for businesses
=
101 queries
```

This is the famous N+1 query problem.

Later we'll solve it with:

```python
.select_related("business")
```

Understanding loops will help you understand exactly why that problem exists.

---

# Exercises

## Exercise 1 — Active businesses

Given:

```python
businesses = [
    {"name": "A", "status": "active"},
    {"name": "B", "status": "inactive"},
    {"name": "C", "status": "active"},
    {"name": "D", "status": "pending"},
]
```

Print only:

```text
A
C
```

Use:

```python
for
if
continue
```

---

## Exercise 2 — Count verified businesses

Given:

```python
businesses = [
    {"name": "A", "is_verified": True},
    {"name": "B", "is_verified": False},
    {"name": "C", "is_verified": True},
    {"name": "D", "is_verified": True},
]
```

Count how many businesses are verified.

Expected result:

```text
3
```

Don't use a built-in shortcut.

Use a loop and a counter.

---

## Exercise 3 — Find owner membership

Given:

```python
memberships = [
    {"business": "A", "role": "staff"},
    {"business": "B", "role": "manager"},
    {"business": "C", "role": "owner"},
    {"business": "D", "role": "admin"},
]
```

Find the first membership whose role is:

```text
owner
```

Then stop looping.

Use:

```python
break
```

---

## Exercise 4 — `enumerate()`

Given:

```python
categories = [
    "Cafe",
    "Restaurant",
    "Hotel",
]
```

Print:

```text
1. Cafe
2. Restaurant
3. Hotel
```

Use:

```python
enumerate(..., start=1)
```

---

## Exercise 5 — Nested data

Given:

```python
businesses = [
    {
        "name": "Cafe One",
        "categories": ["Cafe", "Bakery"],
    },
    {
        "name": "Food House",
        "categories": ["Restaurant", "Fast Food"],
    },
]
```

Print:

```text
Cafe One
- Cafe
- Bakery

Food House
- Restaurant
- Fast Food
```

Use nested loops.

---

# Mini challenge

Use:

```python
businesses = [
    {
        "name": "Cafe A",
        "status": "active",
        "is_verified": True,
        "rating": 4.7,
    },
    {
        "name": "Cafe B",
        "status": "inactive",
        "is_verified": True,
        "rating": 4.9,
    },
    {
        "name": "Cafe C",
        "status": "active",
        "is_verified": False,
        "rating": 4.8,
    },
    {
        "name": "Cafe D",
        "status": "active",
        "is_verified": True,
        "rating": 3.8,
    },
]
```

Build a new list called:

```python
recommended_businesses
```

A business is recommended only if:

```text
status == active
AND
is_verified == True
AND
rating >= 4.2
```

The final result should contain only the business names.

Don't modify the original list.

---

## What you should understand before Lesson 4

You should be able to explain the difference between:

```python
for
while
break
continue
```

and understand:

```python
enumerate()
zip()
range()
```

You should also recognize these three common loop patterns:

```text
filter
transform
accumulate
```

For example:

```python
for business in businesses:
```

can be used to:

```text
filter      → select active businesses
transform   → extract business names
accumulate  → count verified businesses
```

And the most important connection to Django is:

```text
Python collection processing
        ↓
Django QuerySet processing
```

### Next lesson

**Chapter 1 — Lesson 4: Functions, Parameters, Return Values, Scope, `*args`, and `**kwargs`**

This will be one of the most important lessons before we start OOP, because methods in classes are fundamentally functions attached to classes.
