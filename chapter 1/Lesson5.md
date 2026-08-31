# Chapter 1 — Lesson 5: Lists, Tuples, Dictionaries, Sets, Comprehensions, and Data Transformation

In real Django and DRF code, you constantly work with collections.

Examples:

```python
users
businesses
memberships
categories
errors
serializer.validated_data
serializer.data
querysets
```

So this lesson is about learning how to **store, access, filter, transform, merge, and inspect data** cleanly.

---

# 1. Lists

A list is an ordered, mutable collection.

```python
categories = [
    "Cafe",
    "Restaurant",
    "Bakery",
]
```

You can access elements by index:

```python
print(categories[0])
print(categories[1])
```

Output:

```text
Cafe
Restaurant
```

Indexes start at `0`.

So:

```text
index:      0             1          2
         "Cafe"     "Restaurant"   "Bakery"
```

---

# 2. Negative indexes

Python lets you access elements from the end:

```python
categories = ["Cafe", "Restaurant", "Bakery"]

print(categories[-1])
```

Output:

```text
Bakery
```

And:

```python
print(categories[-2])
```

gives:

```text
Restaurant
```

---

# 3. Changing a list

Lists are mutable:

```python
categories = ["Cafe", "Restaurant"]

categories[0] = "Coffee Shop"

print(categories)
```

Output:

```python
["Coffee Shop", "Restaurant"]
```

The list object was modified.

---

# 4. Adding items

## `.append()`

Adds one item:

```python
categories = ["Cafe"]

categories.append("Restaurant")

print(categories)
```

Result:

```python
["Cafe", "Restaurant"]
```

---

## `.extend()`

Adds multiple items from another iterable:

```python
categories = ["Cafe"]

categories.extend([
    "Restaurant",
    "Bakery",
])

print(categories)
```

Result:

```python
["Cafe", "Restaurant", "Bakery"]
```

Important distinction:

```python
categories.append(["Restaurant", "Bakery"])
```

creates:

```python
[
    "Cafe",
    ["Restaurant", "Bakery"],
]
```

But:

```python
categories.extend(["Restaurant", "Bakery"])
```

creates:

```python
[
    "Cafe",
    "Restaurant",
    "Bakery",
]
```

---

# 5. Inserting items

```python
categories = ["Cafe", "Bakery"]

categories.insert(1, "Restaurant")
```

Result:

```python
["Cafe", "Restaurant", "Bakery"]
```

The first argument is the index.

---

# 6. Removing items

## `.remove()`

Removes by value:

```python
categories = ["Cafe", "Restaurant", "Bakery"]

categories.remove("Restaurant")
```

Result:

```python
["Cafe", "Bakery"]
```

If the value doesn't exist, Python raises an error.

---

## `.pop()`

Removes and returns an item.

```python
categories = ["Cafe", "Restaurant", "Bakery"]

removed = categories.pop()

print(removed)
print(categories)
```

Output:

```text
Bakery
['Cafe', 'Restaurant']
```

You can specify an index:

```python
removed = categories.pop(0)
```

---

# 7. Membership checks

You've already seen:

```python
if "Cafe" in categories:
    print("Found")
```

And:

```python
if "Hotel" not in categories:
    print("Not found")
```

---

# 8. List length

```python
categories = ["Cafe", "Restaurant", "Bakery"]

print(len(categories))
```

Output:

```text
3
```

This becomes useful for:

```python
if len(categories) > 5:
    ...
```

Although for simple existence checks:

```python
if categories:
```

is usually cleaner.

---

# 9. Slicing

Slicing lets you take part of a list.

```python
numbers = [10, 20, 30, 40, 50]
```

Then:

```python
print(numbers[1:4])
```

Output:

```python
[20, 30, 40]
```

The rule:

```text
[start : stop]
```

includes `start`, excludes `stop`.

So:

```python
numbers[1:4]
```

means indexes:

```text
1, 2, 3
```

---

# 10. Omitting slice boundaries

```python
numbers[:3]
```

means:

```python
numbers[0:3]
```

Result:

```python
[10, 20, 30]
```

And:

```python
numbers[2:]
```

means:

```python
[30, 40, 50]
```

---

# 11. Copying with slicing

This:

```python
copy_of_numbers = numbers[:]
```

creates a shallow copy of the list.

Equivalent commonly to:

```python
copy_of_numbers = numbers.copy()
```

We'll discuss shallow vs deep copying later.

---

# 12. Sorting lists

```python
ratings = [4.8, 3.1, 4.2, 5.0]

ratings.sort()
```

Result:

```python
[3.1, 4.2, 4.8, 5.0]
```

Descending:

```python
ratings.sort(reverse=True)
```

Result:

```python
[5.0, 4.8, 4.2, 3.1]
```

---

# 13. `sorted()` versus `.sort()`

This distinction is important.

`.sort()` modifies the existing list:

```python
ratings.sort()
```

`sorted()` returns a new list:

```python
ratings = [4.8, 3.1, 4.2]

sorted_ratings = sorted(ratings)

print(ratings)
print(sorted_ratings)
```

Original remains unchanged.

I usually prefer `sorted()` when I don't want mutation.

---

# 14. Sorting dictionaries by a field

Consider:

```python
businesses = [
    {"name": "Cafe A", "rating": 4.2},
    {"name": "Cafe B", "rating": 4.9},
    {"name": "Cafe C", "rating": 3.8},
]
```

You can sort using:

```python
sorted(
    businesses,
    key=lambda business: business["rating"],
    reverse=True,
)
```

We'll properly learn `lambda` later, but conceptually:

```text
sort businesses
using each business's rating
highest first
```

Later Django will often do this database-side:

```python
Business.objects.order_by("-rating")
```

---

# 15. Tuples

A tuple is ordered but immutable.

```python
coordinates = (35.6892, 51.3890)
```

Access works like lists:

```python
print(coordinates[0])
```

But this fails:

```python
coordinates[0] = 40
```

because tuples cannot be changed in place.

---

# 16. Why tuples exist

Tuples are useful when a group of values represents a fixed structure.

For example:

```python
location = ("Tehran", "Tehran Province")
```

or:

```python
dimensions = (1920, 1080)
```

They also appear naturally when Python returns multiple values.

Example:

```python
def get_coordinates():
    return 35.6892, 51.3890
```

This returns a tuple.

---

# 17. Tuple unpacking

```python
coordinates = (35.6892, 51.3890)

latitude, longitude = coordinates
```

Now:

```python
print(latitude)
print(longitude)
```

This same concept appeared with:

```python
for key, value in dictionary.items():
```

---

# 18. Unpacking with `_`

Sometimes you don't care about one value:

```python
name, _ = ("Python Cafe", 4.8)
```

Conventionally:

```python
_
```

means:

> I'm intentionally ignoring this value.

---

# 19. Dictionaries

Dictionaries are one of the most important Python structures for API work.

```python
business = {
    "id": 1,
    "name": "Python Cafe",
    "rating": 4.7,
    "is_verified": True,
}
```

They map:

```text
key → value
```

---

# 20. Accessing dictionary values

```python
print(business["name"])
```

Output:

```text
Python Cafe
```

But if the key doesn't exist:

```python
business["province"]
```

you get:

```text
KeyError
```

Sometimes that's exactly what you want.

Other times you want safer access.

---

# 21. `.get()`

```python
province = business.get("province")
```

If `"province"` doesn't exist:

```python
province
```

becomes:

```python
None
```

You can provide a default:

```python
province = business.get(
    "province",
    "Unknown",
)
```

Then:

```text
Unknown
```

---

# 22. `[]` versus `.get()`

Use:

```python
business["name"]
```

when the field **must exist**.

Use:

```python
business.get("description")
```

when absence is acceptable.

This distinction can improve code quality.

If `name` is absolutely required, silently returning `None` may hide a bug.

---

# 23. Adding and changing dictionary values

```python
business["rating"] = 4.9
```

Changes the rating.

Add a new field:

```python
business["status"] = "active"
```

Dictionaries are mutable.

---

# 24. Deleting dictionary values

```python
del business["rating"]
```

Or:

```python
rating = business.pop("rating")
```

`pop()` also returns the removed value.

---

# 25. Dictionary membership

Important:

```python
"name" in business
```

checks keys.

Example:

```python
if "name" in business:
    print("Name exists")
```

It does not check values.

---

# 26. Nested dictionaries

Real API payloads are often nested.

```python
business = {
    "id": 1,
    "name": "Python Cafe",
    "owner": {
        "id": 7,
        "name": "Ali",
    },
}
```

Access:

```python
owner_name = business["owner"]["name"]
```

Think:

```text
business
   ↓
owner
   ↓
name
```

This will look very familiar when we reach nested DRF serializers.

---

# 27. Dictionaries containing lists

```python
business = {
    "name": "Python Cafe",
    "categories": [
        "Cafe",
        "Restaurant",
    ],
}
```

Access:

```python
print(business["categories"][0])
```

Output:

```text
Cafe
```

---

# 28. Lists containing dictionaries

This is very common:

```python
businesses = [
    {
        "id": 1,
        "name": "Cafe A",
    },
    {
        "id": 2,
        "name": "Cafe B",
    },
]
```

Then:

```python
print(businesses[1]["name"])
```

Output:

```text
Cafe B
```

---

# 29. `.update()`

You can merge/update dictionary fields:

```python
business = {
    "name": "Python Cafe",
    "status": "pending",
}

business.update({
    "status": "active",
    "rating": 4.8,
})
```

Result:

```python
{
    "name": "Python Cafe",
    "status": "active",
    "rating": 4.8,
}
```

Existing keys are overwritten.

New keys are added.

---

# 30. Dictionary unpacking with `**`

Suppose:

```python
business = {
    "name": "Python Cafe",
    "status": "active",
}
```

You can create a new dictionary:

```python
updated_business = {
    **business,
    "rating": 4.8,
}
```

Result:

```python
{
    "name": "Python Cafe",
    "status": "active",
    "rating": 4.8,
}
```

The original remains unchanged.

---

# 31. Overriding with dictionary unpacking

Order matters.

```python
business = {
    "name": "Python Cafe",
    "status": "pending",
}
```

Then:

```python
updated = {
    **business,
    "status": "active",
}
```

Final status:

```text
active
```

But:

```python
updated = {
    "status": "active",
    **business,
}
```

Final status becomes:

```text
pending
```

because later entries override earlier ones.

---

# 32. Sets

A set stores unique values.

```python
roles = {
    "owner",
    "admin",
    "manager",
}
```

Sets do not behave like lists.

You don't normally access:

```python
roles[0]
```

because sets are not index-based sequences.

---

# 33. Why sets are useful

Suppose:

```python
categories = [
    "Cafe",
    "Cafe",
    "Restaurant",
    "Bakery",
    "Restaurant",
]
```

Convert to set:

```python
unique_categories = set(categories)
```

Result conceptually:

```python
{
    "Cafe",
    "Restaurant",
    "Bakery",
}
```

Duplicates disappear.

---

# 34. Set membership

Sets are excellent when asking:

```text
Is this value allowed?
```

Example:

```python
allowed_roles = {
    "owner",
    "admin",
    "manager",
}

if role in allowed_roles:
    ...
```

This is clearer than many repeated comparisons.

---

# 35. Adding to a set

```python
roles.add("staff")
```

Removing:

```python
roles.remove("staff")
```

There is also:

```python
roles.discard("staff")
```

Difference:

* `.remove()` raises an error if missing
* `.discard()` does not

---

# 36. Set operations

Suppose:

```python
user_roles = {
    "manager",
    "editor",
}

required_roles = {
    "owner",
    "admin",
    "manager",
}
```

Intersection:

```python
user_roles & required_roles
```

Result:

```python
{"manager"}
```

So:

```python
if user_roles & required_roles:
    print("User has an allowed role")
```

This can be useful in permission logic.

---

# 37. Union

```python
a = {"owner", "admin"}
b = {"manager", "staff"}

all_roles = a | b
```

Result:

```python
{
    "owner",
    "admin",
    "manager",
    "staff",
}
```

---

# 38. Difference

```python
allowed = {
    "owner",
    "admin",
    "manager",
}

user_roles = {
    "manager",
    "staff",
}
```

Then:

```python
user_roles - allowed
```

gives:

```python
{"staff"}
```

Meaning:

> roles the user has that aren't in the allowed set.

---

# 39. Comprehensions

Now we reach one of Python's most useful features.

Remember Lesson 3:

```python
names = []

for business in businesses:
    names.append(business["name"])
```

We can write:

```python
names = [
    business["name"]
    for business in businesses
]
```

This is a **list comprehension**.

Read it as:

> Build a list containing each business's name for every business in businesses.

---

# 40. Start with normal loops

Before using comprehensions, always understand the normal loop version.

Normal:

```python
squares = []

for number in range(5):
    squares.append(number * number)
```

Comprehension:

```python
squares = [
    number * number
    for number in range(5)
]
```

Result:

```python
[0, 1, 4, 9, 16]
```

---

# 41. Filtering with comprehensions

Normal:

```python
active_businesses = []

for business in businesses:
    if business["status"] == "active":
        active_businesses.append(business)
```

Comprehension:

```python
active_businesses = [
    business
    for business in businesses
    if business["status"] == "active"
]
```

Read:

> give me each business where status is active.

---

# 42. Filtering + transforming

Suppose:

```python
businesses = [
    {"name": "Cafe A", "status": "active"},
    {"name": "Cafe B", "status": "inactive"},
    {"name": "Cafe C", "status": "active"},
]
```

We want names of active businesses:

```python
active_names = [
    business["name"]
    for business in businesses
    if business["status"] == "active"
]
```

Result:

```python
["Cafe A", "Cafe C"]
```

This combines:

```text
filter
+
transform
```

---

# 43. Comprehensions should remain readable

Good:

```python
active_names = [
    business["name"]
    for business in businesses
    if business["status"] == "active"
]
```

Potentially bad:

```python
result = [
    x["name"].upper()
    for x in businesses
    if x["status"] == "active"
    and x["rating"] > 4
    and x["province"]["id"] in allowed
    and ...
]
```

If a comprehension becomes difficult to understand, use a normal loop or extract functions.

Readability matters more than being clever.

---

# 44. Set comprehensions

You can build sets:

```python
categories = {
    business["category"]
    for business in businesses
}
```

This automatically produces unique categories.

Example:

```python
businesses = [
    {"category": "Cafe"},
    {"category": "Restaurant"},
    {"category": "Cafe"},
]
```

Result:

```python
{
    "Cafe",
    "Restaurant",
}
```

---

# 45. Dictionary comprehensions

Suppose:

```python
businesses = [
    {"id": 1, "name": "Cafe A"},
    {"id": 2, "name": "Cafe B"},
]
```

You can create a lookup dictionary:

```python
business_by_id = {
    business["id"]: business
    for business in businesses
}
```

Result:

```python
{
    1: {
        "id": 1,
        "name": "Cafe A",
    },
    2: {
        "id": 2,
        "name": "Cafe B",
    },
}
```

Then lookup becomes:

```python
business_by_id[2]
```

instead of searching the entire list repeatedly.

---

# 46. Why lookup dictionaries can matter

Imagine 10,000 businesses.

Bad conceptual pattern:

```python
for id_ in requested_ids:
    for business in businesses:
        if business["id"] == id_:
            ...
```

You're repeatedly scanning the list.

A dictionary gives direct access:

```python
business_by_id[id_]
```

Understanding data structures helps with performance.

---

# 47. `any()`

`any()` answers:

> Is at least one item truthy?

Example:

```python
permissions = [
    False,
    False,
    True,
]

print(any(permissions))
```

Output:

```text
True
```

A real example:

```python
roles = [
    "staff",
    "manager",
    "editor",
]

has_privileged_role = any(
    role in {"owner", "admin", "manager"}
    for role in roles
)
```

Result:

```python
True
```

---

# 48. `all()`

`all()` asks:

> Are all items truthy?

```python
conditions = [
    True,
    True,
    True,
]

print(all(conditions))
```

Output:

```text
True
```

Real example:

```python
can_publish = all([
    business["is_active"],
    business["is_verified"],
    bool(business["name"]),
])
```

But often direct boolean expressions are clearer:

```python
can_publish = (
    business["is_active"]
    and business["is_verified"]
    and bool(business["name"])
)
```

Use whichever is more readable.

---

# 49. `any()` with comprehensions

Suppose:

```python
memberships = [
    {"role": "staff"},
    {"role": "manager"},
    {"role": "editor"},
]
```

Check whether any membership grants management access:

```python
has_management_access = any(
    membership["role"] in {
        "owner",
        "admin",
        "manager",
    }
    for membership in memberships
)
```

This is powerful and readable once you're comfortable with comprehensions.

---

# 50. `all()` with validation

Suppose:

```python
categories = [
    "Cafe",
    "Restaurant",
    "Bakery",
]
```

Check that all names are non-empty:

```python
all_have_names = all(
    bool(category)
    for category in categories
)
```

---

# 51. `sum()` with conditions

Suppose:

```python
businesses = [
    {"is_verified": True},
    {"is_verified": False},
    {"is_verified": True},
]
```

Remember:

```python
True == 1
False == 0
```

So technically:

```python
verified_count = sum(
    business["is_verified"]
    for business in businesses
)
```

returns:

```text
2
```

This works, although sometimes an explicit expression is clearer:

```python
verified_count = sum(
    1
    for business in businesses
    if business["is_verified"]
)
```

---

# 52. `min()` and `max()`

```python
ratings = [4.5, 3.2, 4.9, 4.1]

highest = max(ratings)
lowest = min(ratings)
```

You can also work with dictionaries:

```python
best_business = max(
    businesses,
    key=lambda business: business["rating"],
)
```

Again, we'll properly cover lambdas later.

---

# 53. `sum()` and average

```python
ratings = [4.5, 4.0, 5.0]

average = sum(ratings) / len(ratings)
```

But remember the empty-list case:

```python
ratings = []
```

Then:

```python
len(ratings)
```

is zero.

So:

```python
sum(ratings) / len(ratings)
```

would raise:

```text
ZeroDivisionError
```

A safer function:

```python
def average(numbers):
    if not numbers:
        return None

    return sum(numbers) / len(numbers)
```

---

# 54. `map()` and `filter()`

Python has:

```python
map()
filter()
```

but in modern Python, comprehensions are often clearer.

Instead of:

```python
names = list(
    map(
        lambda business: business["name"],
        businesses,
    )
)
```

I usually prefer:

```python
names = [
    business["name"]
    for business in businesses
]
```

Instead of:

```python
active_businesses = list(
    filter(
        lambda business: business["status"] == "active",
        businesses,
    )
)
```

prefer:

```python
active_businesses = [
    business
    for business in businesses
    if business["status"] == "active"
]
```

For everyday Django code, readability wins.

---

# 55. Nested comprehensions

You can do this:

```python
businesses = [
    {
        "name": "A",
        "categories": ["Cafe", "Bakery"],
    },
    {
        "name": "B",
        "categories": ["Restaurant"],
    },
]
```

Flatten all categories:

```python
categories = [
    category
    for business in businesses
    for category in business["categories"]
]
```

Result:

```python
[
    "Cafe",
    "Bakery",
    "Restaurant",
]
```

But nested comprehensions can quickly become confusing.

A normal nested loop may be easier for beginners:

```python
categories = []

for business in businesses:
    for category in business["categories"]:
        categories.append(category)
```

Both are correct.

Understand the loop first.

---

# 56. Unique flattened values

We can combine a set comprehension:

```python
categories = {
    category
    for business in businesses
    for category in business["categories"]
}
```

Now duplicates disappear.

---

# 57. Grouping data

Suppose:

```python
businesses = [
    {"name": "A", "category": "Cafe"},
    {"name": "B", "category": "Restaurant"},
    {"name": "C", "category": "Cafe"},
]
```

We want:

```python
{
    "Cafe": ["A", "C"],
    "Restaurant": ["B"],
}
```

One straightforward approach:

```python
grouped = {}

for business in businesses:
    category = business["category"]

    if category not in grouped:
        grouped[category] = []

    grouped[category].append(
        business["name"]
    )
```

This is an important data-transformation pattern.

---

# 58. `.setdefault()`

The previous code can be shortened:

```python
grouped = {}

for business in businesses:
    grouped.setdefault(
        business["category"],
        [],
    ).append(
        business["name"]
    )
```

But don't use shortcuts until you understand the longer version.

Readability first.

Later we'll also learn `defaultdict`.

---

# 59. Avoid modifying lists while iterating

Remember:

```python
businesses = [...]

for business in businesses:
    if business["status"] == "inactive":
        businesses.remove(business)
```

This is risky.

Instead, create a filtered collection:

```python
businesses = [
    business
    for business in businesses
    if business["status"] != "inactive"
]
```

Much safer.

---

# 60. Shallow copy problem

This is subtle and important.

Suppose:

```python
business = {
    "name": "Cafe A",
    "categories": ["Cafe"],
}

copied_business = business.copy()
```

Now:

```python
copied_business["categories"].append(
    "Restaurant"
)
```

What happens to:

```python
business["categories"]
```

It also changes.

Why?

Because `.copy()` only made a **shallow copy**.

Conceptually:

```text
business ─────────────┐
                      │
                  categories
                      │
                      ▼
                ["Cafe", ...]
                      ▲
                      │
copied_business ──────┘
```

The outer dictionaries are different objects, but the nested list is still shared.

We'll cover deep copying later.

---

# 61. Real DRF-like data transformation

Suppose an API gives:

```python
payload = {
    "businesses": [
        {
            "id": 1,
            "name": "Cafe A",
            "status": "active",
        },
        {
            "id": 2,
            "name": "Cafe B",
            "status": "inactive",
        },
    ]
}
```

You could produce frontend-friendly options:

```python
options = [
    {
        "value": business["id"],
        "label": business["name"],
    }
    for business in payload["businesses"]
    if business["status"] == "active"
]
```

Result:

```python
[
    {
        "value": 1,
        "label": "Cafe A",
    }
]
```

This is exactly the kind of transformation you'll see around APIs.

---

# 62. Real validation-error transformation

Suppose:

```python
errors = {
    "name": ["This field is required."],
    "phone": ["Invalid phone number."],
}
```

You want the first error for each field:

```python
first_errors = {
    field: messages[0]
    for field, messages in errors.items()
    if messages
}
```

Result:

```python
{
    "name": "This field is required.",
    "phone": "Invalid phone number.",
}
```

This kind of logic is very relevant to DRF frontend integration.

---

# 63. Choosing the correct collection

Use a **list** when:

* order matters
* duplicates are allowed
* you want indexing

Example:

```python
businesses = [...]
```

Use a **tuple** when:

* structure is fixed
* mutation is undesirable

Example:

```python
coordinates = (lat, lng)
```

Use a **dictionary** when:

* you need key → value relationships
* you're representing structured objects/data

Example:

```python
business = {
    "id": 1,
    "name": "Cafe",
}
```

Use a **set** when:

* uniqueness matters
* membership checking is important

Example:

```python
allowed_roles = {
    "owner",
    "admin",
}
```

Choosing the right collection is part of good software design.

---

# 64. Connection to Django

Eventually:

```python
businesses = Business.objects.filter(
    status="active"
)
```

returns a QuerySet rather than a list.

But many operations feel conceptually similar.

Plain Python:

```python
active_businesses = [
    business
    for business in businesses
    if business["status"] == "active"
]
```

Django:

```python
active_businesses = Business.objects.filter(
    status="active"
)
```

Plain Python:

```python
names = [
    business["name"]
    for business in businesses
]
```

Django:

```python
names = Business.objects.values_list(
    "name",
    flat=True,
)
```

Plain Python:

```python
businesses = sorted(
    businesses,
    key=lambda b: b["rating"],
    reverse=True,
)
```

Django:

```python
businesses = Business.objects.order_by(
    "-rating"
)
```

This is why learning collection transformation now matters.

---

# Exercise 1 — Filter businesses

Given:

```python
businesses = [
    {
        "name": "Cafe A",
        "status": "active",
    },
    {
        "name": "Cafe B",
        "status": "inactive",
    },
    {
        "name": "Cafe C",
        "status": "active",
    },
]
```

Create:

```python
active_businesses
```

using a list comprehension.

It should contain only active businesses.

---

# Exercise 2 — Transform businesses

Using the same data, create:

```python
active_business_names
```

Expected:

```python
[
    "Cafe A",
    "Cafe C",
]
```

Use one comprehension.

---

# Exercise 3 — Unique categories

Given:

```python
businesses = [
    {"category": "Cafe"},
    {"category": "Restaurant"},
    {"category": "Cafe"},
    {"category": "Bakery"},
]
```

Create a set containing unique categories.

Expected conceptually:

```python
{
    "Cafe",
    "Restaurant",
    "Bakery",
}
```

---

# Exercise 4 — Build lookup dictionary

Given:

```python
businesses = [
    {"id": 10, "name": "Cafe A"},
    {"id": 20, "name": "Cafe B"},
]
```

Build:

```python
business_by_id
```

so that:

```python
business_by_id[20]
```

returns:

```python
{
    "id": 20,
    "name": "Cafe B",
}
```

Use a dictionary comprehension.

---

# Exercise 5 — Permission check

Given:

```python
memberships = [
    {"role": "staff"},
    {"role": "editor"},
    {"role": "manager"},
]
```

Use:

```python
any()
```

to determine whether the user has one of:

```python
{
    "owner",
    "admin",
    "manager",
}
```

Expected:

```python
True
```

---

# Exercise 6 — Group businesses

Given:

```python
businesses = [
    {
        "name": "Cafe A",
        "category": "Cafe",
    },
    {
        "name": "Pizza House",
        "category": "Restaurant",
    },
    {
        "name": "Cafe B",
        "category": "Cafe",
    },
]
```

Create:

```python
grouped
```

with this shape:

```python
{
    "Cafe": [
        "Cafe A",
        "Cafe B",
    ],
    "Restaurant": [
        "Pizza House",
    ],
}
```

Use a normal loop first.

---

# Exercise 7 — Shallow copy

Predict what this prints before running it:

```python
business = {
    "name": "Cafe A",
    "categories": ["Cafe"],
}

copied = business.copy()

copied["categories"].append(
    "Restaurant"
)

print(
    business["categories"]
)

print(
    copied["categories"]
)
```

Then explain **why**.

This exercise is important.

---

# Mini challenge

Given:

```python
businesses = [
    {
        "id": 1,
        "name": "Cafe A",
        "status": "active",
        "is_verified": True,
        "rating": 4.8,
        "categories": [
            "Cafe",
            "Bakery",
        ],
    },
    {
        "id": 2,
        "name": "Cafe B",
        "status": "inactive",
        "is_verified": True,
        "rating": 4.9,
        "categories": [
            "Cafe",
        ],
    },
    {
        "id": 3,
        "name": "Food House",
        "status": "active",
        "is_verified": True,
        "rating": 4.3,
        "categories": [
            "Restaurant",
        ],
    },
]
```

Build:

```python
recommended
```

containing dictionaries in this shape:

```python
{
    "id": 1,
    "label": "Cafe A",
    "rating": 4.8,
}
```

Rules:

* status must be `"active"`
* verified must be `True`
* rating must be at least `4.5`

Use a list comprehension.

Expected result:

```python
[
    {
        "id": 1,
        "label": "Cafe A",
        "rating": 4.8,
    }
]
```

---

## What you should understand before Lesson 6

You should be able to choose between:

```text
list
tuple
dict
set
```

and explain why.

You should understand:

```python
.append()
.extend()
.pop()
.get()
.update()
.copy()
```

and:

```python
[
    ...
    for ...
    if ...
]
```

as well as:

```python
{
    key: value
    for ...
}
```

and:

```python
{
    value
    for ...
}
```

You should also understand:

```python
any()
all()
sum()
min()
max()
sorted()
```

And especially this progression:

```text
raw collection
     ↓
filter
     ↓
transform
     ↓
group/index
     ↓
useful application data
```

That pattern appears everywhere in backend development.

## Next lesson

**Chapter 1 — Lesson 6: Exceptions, Error Handling, `try` / `except` / `else` / `finally`, Raising Errors, and Designing Custom Exceptions**
