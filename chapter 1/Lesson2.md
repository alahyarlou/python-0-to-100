# Chapter 1 — Lesson 2: Operators, Conditions, Truthiness, and Control Flow

This lesson is important because almost every real backend rule eventually becomes a condition.

For example:

```python
if user.is_authenticated and business.is_active:
    ...
```

or:

```python
if membership.role == "owner":
    ...
```

or:

```python
if not phone:
    ...
```

Before Django, we need to understand exactly what these expressions mean.

## 1. Comparison operators

Python gives us these common comparison operators:

```python
==   # equal
!=   # not equal
>    # greater than
<    # less than
>=   # greater than or equal
<=   # less than or equal
```

Example:

```python
rating = 4.7

print(rating > 4)
print(rating < 3)
print(rating == 4.7)
print(rating != 5)
```

Output:

```text
True
False
True
True
```

Every comparison produces a boolean:

```python
True
False
```

So this:

```python
rating > 4
```

is itself an expression whose value is:

```python
True
```

---

# 2. Conditions with `if`

The simplest conditional structure is:

```python
if condition:
    do_something()
```

Example:

```python
is_verified = True

if is_verified:
    print("Business is verified")
```

Output:

```text
Business is verified
```

Python executes the indented block only when the condition is truthy.

Indentation matters in Python.

This is correct:

```python
if is_verified:
    print("Verified")
```

This is not:

```python
if is_verified:
print("Verified")
```

---

# 3. `if` and `else`

Sometimes we want two possible paths:

```python
is_verified = False

if is_verified:
    print("Verified")
else:
    print("Not verified")
```

Conceptually:

```text
          condition
              │
         ┌────┴────┐
         │         │
       True      False
         │         │
         ▼         ▼
   first block   else block
```

This pattern appears everywhere in backend code.

For example:

```python
if user_exists:
    login_user()
else:
    reject_login()
```

---

# 4. `elif`

Use `elif` when there are several possible branches.

```python
rating = 4.3

if rating >= 4.5:
    print("Excellent")
elif rating >= 4:
    print("Very good")
elif rating >= 3:
    print("Good")
else:
    print("Needs improvement")
```

Output:

```text
Very good
```

Python checks from top to bottom.

Once one condition matches, the remaining branches are skipped.

This is important.

Consider:

```python
rating = 4.8

if rating >= 3:
    print("Good")
elif rating >= 4:
    print("Very good")
```

The output is:

```text
Good
```

Even though `rating >= 4` is also true.

Why?

Because the first condition already matched.

So conditions should usually go from more specific to less specific.

Better:

```python
if rating >= 4.5:
    print("Excellent")
elif rating >= 4:
    print("Very good")
elif rating >= 3:
    print("Good")
```

---

# 5. Boolean operators

There are three very important boolean operators:

```python
and
or
not
```

## `and`

Both sides need to be true.

```python
is_verified = True
is_active = True

if is_verified and is_active:
    print("Business can be displayed")
```

Think of `and` as:

```text
True  and True  → True
True  and False → False
False and True  → False
False and False → False
```

A real example:

```python
user_is_authenticated = True
user_is_owner = True

if user_is_authenticated and user_is_owner:
    print("Allow business editing")
```

---

# 6. `or`

At least one side must be true.

```python
is_owner = False
is_admin = True

if is_owner or is_admin:
    print("Access granted")
```

Truth table:

```text
True  or True  → True
True  or False → True
False or True  → True
False or False → False
```

This maps directly to permission logic:

```python
if membership.role == "owner" or membership.role == "admin":
    ...
```

---

# 7. `not`

`not` reverses a boolean meaning.

```python
is_active = False

if not is_active:
    print("Business is inactive")
```

You can think of:

```python
not True
```

as:

```python
False
```

and:

```python
not False
```

as:

```python
True
```

This becomes common later:

```python
if not request.user.is_authenticated:
    ...
```

or:

```python
if not business:
    ...
```

---

# 8. Combining conditions

You can combine them:

```python
is_authenticated = True
is_owner = False
is_admin = True

if is_authenticated and (is_owner or is_admin):
    print("Access granted")
```

Parentheses help make the rule clear.

Without parentheses:

```python
if is_authenticated and is_owner or is_admin:
```

Python applies operator precedence.

That may still work, but it's harder to read.

For permission rules, readability matters.

Prefer:

```python
if is_authenticated and (is_owner or is_admin):
```

---

# 9. Truthiness

This is one of Python's most useful concepts.

A condition does not always have to be literally `True` or `False`.

Example:

```python
name = "Python Cafe"

if name:
    print("A name was provided")
```

Why does this work?

Because non-empty strings are truthy.

But:

```python
name = ""

if name:
    print("A name was provided")
else:
    print("Name is empty")
```

An empty string is falsy.

Common falsy values include:

```python
False
None
0
0.0
""
[]
{}
set()
```

Examples:

```python
if "":
    print("Runs")
```

does not run.

```python
if []:
    print("Runs")
```

does not run.

```python
if None:
    print("Runs")
```

does not run.

---

# 10. Why truthiness matters in real code

Suppose:

```python
description = ""
```

You could write:

```python
if description != "":
    print(description)
```

But Python developers usually write:

```python
if description:
    print(description)
```

Similarly:

```python
categories = []
```

Instead of:

```python
if len(categories) > 0:
```

you'll often see:

```python
if categories:
```

This means:

> If the collection contains something.

---

# 11. `None` checks

There is one important exception.

When you specifically mean:

> This value is `None`

use:

```python
is None
```

not:

```python
== None
```

Correct:

```python
description = None

if description is None:
    print("No description")
```

Also:

```python
if description is not None:
    print("Description exists")
```

Why use `is`?

Because `None` is a singleton object.

This is one of the places where `is` is intentionally appropriate.

---

# 12. Truthiness is not always the same as `None`

This matters a lot.

Consider:

```python
rating = 0
```

This is falsy.

So:

```python
if not rating:
    print("No rating")
```

would run.

But maybe `0` is a legitimate rating.

So sometimes this is wrong.

Better:

```python
if rating is None:
    print("No rating")
```

Now:

```python
rating = 0
```

is distinguishable from:

```python
rating = None
```

This matters often in APIs.

For example:

```text
0
```

can be a valid value.

But:

```text
null
```

means no value.

---

# 13. Membership operators

Python has:

```python
in
not in
```

Example:

```python
role = "owner"

allowed_roles = ["owner", "admin"]

if role in allowed_roles:
    print("Allowed")
```

Output:

```text
Allowed
```

This is much cleaner than:

```python
if role == "owner" or role == "admin":
```

You can write:

```python
if role in {"owner", "admin"}:
    print("Allowed")
```

A set is often a nice choice for membership tests.

---

# 14. `not in`

Example:

```python
status = "pending"

if status not in {"active", "verified"}:
    print("Business cannot be published")
```

This is especially useful for validation.

---

# 15. String membership

`in` also works on strings.

```python
name = "Python Coffee"

print("Coffee" in name)
```

Output:

```text
True
```

Another:

```python
email = "user@example.com"

if "@" in email:
    print("Looks like an email")
```

Of course, this is not enough for real email validation, but the operator itself is useful.

---

# 16. Chained comparisons

Python supports elegant chained comparisons.

Instead of:

```python
if rating >= 0 and rating <= 5:
```

you can write:

```python
if 0 <= rating <= 5:
    print("Valid rating")
```

This reads naturally.

Another example:

```python
age = 24

if 18 <= age < 65:
    print("Allowed range")
```

---

# 17. Common beginner mistake with `or`

This is wrong:

```python
role = "manager"

if role == "owner" or "admin":
    print("Allowed")
```

It looks reasonable, but it does not mean:

```text
role equals owner OR role equals admin
```

Python effectively sees:

```python
(role == "owner") or "admin"
```

And `"admin"` is a non-empty string, therefore truthy.

So this condition is effectively always true.

Correct:

```python
if role == "owner" or role == "admin":
```

Better:

```python
if role in {"owner", "admin"}:
```

---

# 18. Another common mistake

This is also wrong:

```python
if role == ("owner" or "admin"):
```

Because Python evaluates:

```python
"owner" or "admin"
```

first.

Since `"owner"` is truthy, the result is:

```python
"owner"
```

So this becomes:

```python
if role == "owner":
```

Again, use:

```python
if role in {"owner", "admin"}:
```

---

# 19. Short-circuit evaluation

Python's boolean operators don't always evaluate everything.

Consider:

```python
user = None

if user is not None and user["is_active"]:
    print("Active")
```

If:

```python
user is None
```

then the first part:

```python
user is not None
```

is false.

Python knows:

```text
False and anything
```

will always be false.

So it does not evaluate:

```python
user["is_active"]
```

This protects us from an error.

This behavior is called short-circuit evaluation.

---

# 20. Why short-circuiting matters

Imagine:

```python
business = None
```

This would fail:

```python
if business["is_active"]:
```

because `business` is `None`.

But:

```python
if business and business["is_active"]:
```

works safely.

If `business` is falsy, Python stops there.

Later you'll see patterns like:

```python
if request.user.is_authenticated and request.user.is_staff:
```

and:

```python
if membership and membership.role == "owner":
```

---

# 21. Expressions can be stored

Remember that:

```python
rating >= 4
```

returns a boolean.

So you can write:

```python
rating = 4.7

is_high_rating = rating >= 4

print(is_high_rating)
```

Output:

```text
True
```

This can improve readability.

Instead of:

```python
if rating >= 4 and review_count >= 20:
```

you might write:

```python
has_good_rating = rating >= 4
has_enough_reviews = review_count >= 20

if has_good_rating and has_enough_reviews:
    print("Recommended business")
```

Good naming can make business rules much easier to understand.

---

# 22. A realistic business rule

Let's build one.

Suppose:

```python
is_active = True
is_verified = True
rating = 4.4
review_count = 27
```

We want a business to be featured when:

* it is active
* it is verified
* rating is at least 4
* it has at least 20 reviews

We can write:

```python
if (
    is_active
    and is_verified
    and rating >= 4
    and review_count >= 20
):
    print("Featured business")
```

This is already real backend logic.

---

# 23. Nested conditions

You can nest `if` statements:

```python
is_authenticated = True
role = "owner"

if is_authenticated:
    if role == "owner":
        print("Owner dashboard")
```

But avoid unnecessary nesting when a single condition is clearer:

```python
if is_authenticated and role == "owner":
    print("Owner dashboard")
```

Deep nesting often makes code harder to read.

For example, this:

```python
if user_exists:
    if is_active:
        if is_verified:
            if has_permission:
                print("Allowed")
```

is usually worse than:

```python
if user_exists and is_active and is_verified and has_permission:
    print("Allowed")
```

Later we'll learn an even cleaner technique called early return.

---

# 24. Guard clauses — preview

Suppose we eventually write a function like this:

```python
def publish_business(is_active, is_verified):
    if not is_active:
        return "Business is inactive"

    if not is_verified:
        return "Business is not verified"

    return "Business published"
```

Notice the style:

```text
invalid?
→ exit

invalid?
→ exit

everything valid
→ continue
```

This is often easier to read than deeply nested conditions.

We'll explore this more when we learn functions.

---

# 25. Conditional expressions

Python supports a compact conditional expression.

Normal version:

```python
if is_verified:
    label = "Verified"
else:
    label = "Unverified"
```

Short form:

```python
label = "Verified" if is_verified else "Unverified"
```

This is sometimes called a ternary expression.

Use it for simple decisions.

Good:

```python
status_label = "Active" if is_active else "Inactive"
```

Bad:

```python
result = x if a else y if b else z if c else something_else
```

If it becomes hard to read, use normal `if` statements.

---

# 26. Operator precedence

Suppose:

```python
result = True or False and False
```

Python evaluates `and` before `or`.

So this means:

```python
True or (False and False)
```

not:

```python
(True or False) and False
```

A useful simplified precedence order is:

```text
not
and
or
```

But don't try to write clever conditions relying heavily on precedence.

Use parentheses:

```python
if is_authenticated and (is_owner or is_admin):
```

Clarity is more important than cleverness.

---

# 27. Real API-style example

Suppose incoming data is:

```python
business = {
    "name": "Python Coffee",
    "status": "active",
    "is_verified": True,
    "rating": 4.6,
}
```

We can write:

```python
if not business["name"]:
    print("Business name is required")

elif business["status"] != "active":
    print("Business is not active")

elif not business["is_verified"]:
    print("Business is not verified")

elif business["rating"] < 4:
    print("Rating is too low")

else:
    print("Business can be featured")
```

Later DRF serializers and permissions will perform similar decisions, just in a more structured way.

---

# 28. A permission example

Suppose:

```python
user = {
    "is_authenticated": True,
    "is_superuser": False,
}

membership = {
    "role": "manager",
    "status": "active",
}
```

We could define:

```python
has_business_access = (
    user["is_authenticated"]
    and membership["status"] == "active"
    and membership["role"] in {"owner", "admin", "manager"}
)

if has_business_access:
    print("Access granted")
```

This is much closer to the sort of thinking needed in DRF permissions.

---

# 29. Exercise 1 — Business publication

Create:

```python
is_active
is_verified
has_address
has_category
```

A business can be published only if all four are true.

Write the condition.

Test several combinations.

For example:

```text
active       = True
verified     = True
address      = False
category     = True
```

Should it be published?

Think first, then run it.

---

# 30. Exercise 2 — Roles

Create:

```python
role = "manager"
```

Allow access only for:

```text
owner
admin
manager
```

Use `in`.

Then try:

```python
role = "staff"
```

and verify that access is denied.

---

# 31. Exercise 3 — Rating validation

Create:

```python
rating = 4.2
```

The rating must be between:

```text
0 and 5
```

inclusive.

Use a chained comparison.

Then test:

```text
-1
0
3.4
5
6
```

---

# 32. Exercise 4 — `None` versus zero

Run this:

```python
rating = 0

if not rating:
    print("No rating")
```

Then change it to:

```python
if rating is None:
    print("No rating")
```

Explain why the two conditions have different meanings.

This distinction will matter later with API data.

---

# 33. Exercise 5 — Permission rule

Use:

```python
is_authenticated = True
role = "admin"
membership_status = "active"
```

Access should be allowed only when:

* user is authenticated
* membership is active
* role is either `owner` or `admin`

Write one clear boolean expression.

Then test these cases:

```text
authenticated=True, role=admin, status=active
authenticated=False, role=admin, status=active
authenticated=True, role=staff, status=active
authenticated=True, role=owner, status=inactive
```

Predict each result before executing.

---

# 34. Mini project challenge

Create a dictionary:

```python
business = {
    "name": "Python Coffee",
    "status": "active",
    "is_verified": True,
    "rating": 4.6,
    "review_count": 32,
    "categories": ["Cafe", "Restaurant"],
}
```

Then determine whether the business is eligible for a `"recommended"` badge.

Rules:

* name must not be empty
* status must be `"active"`
* business must be verified
* rating must be at least `4.2`
* review count must be at least `20`
* `"Cafe"` must be one of its categories

If every rule passes:

```text
Recommended
```

Otherwise:

```text
Not recommended
```

Try writing the condition yourself.

---

## What you should understand before Lesson 3

You should be comfortable explaining:

```python
if
elif
else
```

and:

```python
==
!=
>
<
>=
<=
```

and:

```python
and
or
not
```

and:

```python
in
not in
```

and the difference between:

```python
if value:
```

and:

```python
if value is None:
```

You should also understand this clearly:

```python
if is_authenticated and role in {"owner", "admin"}:
```

means:

> Both authentication must be true and the role must be one of the allowed roles.

## Next lesson

**Chapter 1, Lesson 3 — Loops, Iteration, `range`, `enumerate`, `zip`, `break`, and `continue`**