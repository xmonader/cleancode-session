## Dependency Injection: Making Code Testable and Flexible

We explain the concept of dependency injection (DI) in Python and its benefits for writing maintainable and testable code.

### The problem:

- Slow Tests: Code that relies on external services or side effects (like time.sleep) can make tests slow.
- Untestable Logic: Functionality that depends on specific implementations (like a database or email service) is difficult to test in isolation.
- Tight Coupling: Code that's tightly coupled to specific dependencies can be inflexible and difficult to adapt.

```python
def add(a, b):
    time.sleep(1) ## simulate some sort of a time delay
    return a+b


add(1,2) == 3
add(4,5) == 9
```

If we run this 100 times, it will cost us 100 seconds, which isn't optimal. What we want to do is be able to "override" the behavior of sleep during testing so we can save time

### Systematic approach

1- Identify the side effects and the dependencies. 
2- Create input parameters for your function to represent these dependencies 
3- Replace the dependencies with these input parameters


#### Example

```python
def add(a, b):
    time.sleep(1) ## simulate some sort of a time delay
    return a+b
```
- In this code the problem (the dependency) is `time.sleep` it will cause the execution to block for 1 second
- create a new function that takes a `sleep_func` parameter
- replace `time.sleep` with `sleep_func`
- at your convenience you can call `add(1,2, time.sleep)` or `add(1,2, sleep_func)`


```python
def add(a, b, sleep_func):
    sleep_func(1) ## simulate some sort of a time delay
    return a+b

add(1,3, time.sleep) == 4
add(4,5, time.sleep) == 9
## this will wait for 2 seconds, given that we use the real implementation of sleep


def fakesleep():
    return

add(1,3, fakesleep) == 4
add(4,5, fakesleep) == 9

## this won't wait  2 seconds as it uses the fake implementation of sleep
```


```python
def create_weekly_report(username):
    user = db.get_user_by_name(username)
    email = user.email
    phone = user.phone
    movies = db.get_top_movies(10)
    emailservice.send(email, "Weekly Report", f"Here is your weekly report {movies}")
    smsservice.send(phone, "we send you the report!!)

```

While this looks like a typical code it has so many issues
- not testable (except if your run everything in a real setup)
- not flexible to change
- depending on global variables that can be changed behind your back

So, Let's refactor! grab your marker and mark the dependencies/side effects

- Mark the dependencies/side effects
```python
app = App()
db = app.db
emailservice = app.emailservice
smsservice = app.smsservice

def create_weekly_report(username):
    user = db.get_user_by_name(username) ## MARK
    email = user.email
    phone = user.phone
    movies = db.get_top_movies(10) ## MARK
    emailservice.send(email, "Weekly Report", f"Here is your weekly report {movies}") ## MARK
    smsservice.send(phone, "we send you the report!!") ## MARK

```


- Replace the dependencies with these input parameters


```python
app = App()
db = app.db
emailservice = app.emailservice
smsservice = app.smsservice

def create_weekly_report(username, get_user_func, get_top_movies_func, send_email_func, send_sms_func):
    user = get_user_func(username) 
    email = user.email
    phone = user.phone
    movies = get_top_movies_func(10) 
    send_email_func(email, "Weekly Report", f"Here is your weekly report {movies}") 
    send_sms_func(phone, "we send you the report!!")

```
Now we improved our code, not to depend on global variables, not hard to test, and flexible to change. let's prove that


for a real setup we can do the following
```python
create_weekly_report("username", db.get_user_by_name, db.get_top_movies, emailservice.send, smsservice.send)

```
and if we want to control any piece of input, or even override its behavior we can send whatever function we want to


```python

def fake_get_user_by_name(name):
    return User(name, email="fakeemail@gmail.com", phone="=1231230")

def fake_get_movies(n):
    return ["The hobbit", "harry potter 1", ...]

def fake_send_email(*args):
    return

def fake_send_sms(*args):
    return 


create_weekly_report("username", fake_get_user_by_name, fake_get_movies, fake_send_email, fake_send_sms)

```
So here we can run the weekly report without having actual db server, actual email service, and actualy sms service!



Have to say it's bit cumbersome. the signature could get way too long, e.g if we need more database functions there will be tonnes of parameters representing thee dependencies. what we can do is utilize the idea of methods, we can group them under a single object e.g `RealDB` or `FakeDB` e.g

```python
class DB:   ## An interface
    def get_user_by_name(self, username):
        ...
    def get_movies(self, n):
        ...

class RealDB(DB):
    def get_user_by_name(self, username):
        ...
    def get_movies(self, n):
        ...

class FakeDB(DB):
    def get_user_by_name(self, username):
        ...
    def get_movies(self, n):
        ...


def create_weekly_report(username, dbobj:DB, send_email_func, send_sms_func):
    user = dbobj.get_user_by_name(username) 
    email = user.email
    phone = user.phone
    movies = dbojb.get_movies(10) 
    send_email_func(email, "Weekly Report", f"Here is your weekly report {movies}") 
    send_sms_func(phone, "we send you the report!!")

```

Now we have dbobj that should be a DB (that groups all of the functionalities related to the DB) and we can either pass a RealDB instance if we need to run in real setup or FakeDB when we want to run in testing setup