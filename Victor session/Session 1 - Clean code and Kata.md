


python :
* object oriented is not followed well in py code, so hard to refactor ( bit worse )
* py is like doing a job as quick as possible



points:
* best way to design system is by writing test , so we decouple things well 

--

for katas : https://kata-log.rocks/



--

Put getters and setters only needed.. 

setters make flow complex to debug if code flow are done into layers...

![[Pasted image 20250211164517.png]]



![[Pasted image 20250211163931.png]]


![[Pasted image 20250211163850.png]]



To find data flow of a variable: use Inteliji => Analyse data flow option

--

refactor or do cleaning of code .. as soon as you see any!

![[Pasted image 20250211164900.png]]

renaming, private/public , extract items ( use intelij extract method option) => 100% safer to do now itself


--

Mostly we don't need comments!
Do comprehensive refactoring of variable names.. i.e variable names should say what the variable is holding or trying to do.. 


After debuging for 2hrs, you mostly change just a line in most cases.. 90% time taken by coders is for reading and understanding the code...

make sure have clear flow of things everywhere!

--

extract things into methods:

functions has to be doing one thing at a time.. 

identify the parts of the code for one small purpose and extract it and create a method out of it!



In most cases => extract the block in which you update only one variable ..


--

variable namings: short vs long

local variable can be small.. less than 4 words combination must


--

OOP - Object Oriented Programming

* maps to brain easily => objects and behavior
* push out from the main logic i.e main policy , the details.. to the objects..

--

reduce passing no.of variables to the function..

plan to make func give new things and put that new thing into existing one -> instead of passing existing things and changing inside func.. 

--

do declaration of things nearer to the usage mostly

--

always sync the code with requirement
in requirement said : 
* flow will be more than one then use > 1
* flow will be two or more then use >= 2



DDD : Domain Driven Development - Bring the code closer to the real world


--

always prefer braces for func even its single line, since in python its identation, but others its block inside braces or just next line

--

business logic always be separated and maintained in isolation and placed in extremely comfortable place.. don't pollute normal flow code with null checks and things

Follow layers of abstraction

if any issue like variable having null value.. 
put null pointer exception in constructor itself when trying to set null 



--

don't just fix the bug place, fix the source of bug


--

Handle if with multiple conditions:

Explanation of code done in 3 techniques: 
1) Constants
2) Variables 
3) Methods


![[Pasted image 20250212182908.png]]


![[Pasted image 20250212183515.png]]

![[Pasted image 20250212183539.png]]


constant => to get rid of magic numbers

local variable => to give descriptive name to the value

methods => used repetition  (binded to object maybe )=> when need in more places

make code flow like storytelling..


don't be ashamed to create many variables before conditions...


--


not doing magic, mostly iterate using for ( not while or iterator in most cases)

--

constants:
* helpful for items that come outside from your ssytem
* helpful to mean it and useful in multiple places
* if that magic number used only in one place and direct , don't extract it to make constant ( if done helpful for tracing , but in most cases the code will be simple .. until it will be helpful understand the code well i.e without this hard to read ) - subjective - not should be in code review.. 
* if you want to highlight ( key business concept ) , use constant

![[Pasted image 20250212191243.png]]

--
method name should be start with verb

getStatement , not setStatement

verbs:
get/set => general, will end up code base full of get!


create - vage
compute - doing computation 
generate - similar to geenrate
make
format



booleans:

isActive (prefer) , wasActivated..

--

always go for better for loop for that scenario

forEach or for of ..

mostly do one thing at one for... its okay to go with do 2 loops  ( most no measurable impact )

--

naming function name:
* always name them by hiding implementation

appendFooter is fine, no appendFooterLines ( were many footer lines are done )

--

while refactoring, fix the compilation errors as first priority and fix as soon as possible


--

test => after each refactor, you confirm that you don't break anything by testing things!

TDD - use TDD to train yourself , at the end you made better

--

Intellij => local history option 
show history ...have backup of code until last test passed

--

have deep understandings of datastructure
list, set, map, dic, hashmap, tuple etc..

don't always go for keymap structure when you are need 2 items, have to link one with other.. 

sometimes we just need to iterate them, use List<Tuples>


--
sometimes you don't need to go for fastest refactor, go for safest..

If you are planning to rely on new varaible.. first slowly transfer dependency from old variable to new variable.. then remove the unused old variable.. 

--

Before refactoring a code to put things into single loop or single time.. measure it and saw whether its really matters.. in most cases not!



In java , we can use JMH(JavaMeasruingHardness) to check micro benchmarks


have loop understandability , pure and impure func diff.. 

pure func => no network or db checks too..

![[Pasted image 20250213065626.png]]

![[Pasted image 20250213070303.png]]




--
Variable declaration / intialization:
cpp , others => variables declared at the intial of block
java => declare variables just before you use it

--

Silly mistake: Writing code without understanding the domain requirement ... business logic.. understanding the domain.. frameworks come and go, but not domain things!

--

if you see too many issues or collision when you try to give a variable name.. then the code is become large, time to split it... even db is getting bigger, then table name will mostly comes into colision , so we find different name.. customer and customerDetails are collision result variable change

These things are side effect of some other things, so we have to fix that some other thing i.e source

IF you don't name the things in the project when they are young, its hard for it to name things in later days..


--

do micro commits - better to track and always have safe backup


break your flow into clear phases.. 

adding features and refactorings must be in different commits

![[Pasted image 20250213073827.png]]


--

ask right questions.. the better the code is! 
have debts within yourself and with your team people

--
premature optimisation is route cause for many problems.. 

when to stop refactoring:

* when its readable by colleagues ( watch how they react to the code )
* talking about code

Things to consider and do to understand:
* understand colleagues level of coding skills
* code review
* pair programming


--

MVC - oldest design pattern.. but better one.. don't pollute model with view / presentation things since presentation ones are highly specific , no other use it .. 

--

don't go with nested ifs:
use gaurd clases => give invertion and do early return of things


--

you have to keep extracting classes when ever a class grows
breaking class into pieces is good, but takes time to refactor to do this!

create a new class, then slowly transfer the lowers func to highest func..
get things as param when current class references are in the method..


note : other than calling method from main class, other methods used inside the methods itself must be private

--

enum can accept method, which is abstract method of the enum in java (only java specific)


--

always identify the similar codes and extract common things from them.. 
in intelij , it suggest when you select identical part

don't do extract duplicate codes, when you know the code present in both places completely well

note: we are doing coupling code that is unrelated, so in future its hard to maintain if having any change.. so that time its ok to decouple things and make only certain part of code common or increate parameters.. ( mostly upto 3 or 4 )

do not do copy past logic ever.. just always etract the common logic and make it reusable.. 

--

func - large fun ( victor's metric )

when someone takes more than 5 to 10secs to read your func and say what its doing.. then that func is large func, its time to split it as func is doing too much

measure also cognitive complexity, if large split it..

--

common func update:

when you have common func, caling in 2 place.. 

but when calling from 1 place we have to do something in the middle of the fucn..


then its advisable to split the only common logic func into two func.. and call 2 func in existing func itself ...  and create new func and call 2 func and add a  line inbetween.. just put the explicitly inbetween line of two , then use this for 2nd place where the line needed.
this is func decomposition.


better, but not good one: by adding extra boolean param and based on that doing something inside if .. which works just for one place, but not other as its default is false..  since it violate single responsibility principle and parameters keep on increasing when new code comes in here.. these booleans are disease in code base .. 


broder params are bad -they violate single responsibility principle as the func is doing 2 things..the more real problem is they stimulate people to do same things over and over .. so at the end we end up to having many booleans changing different logic of the flow and also boolan from top layer pass several layers to the bottom which gets scary as its more abstraction.

and also passing callback func as argument is not java way mostly.. 


note: in java you can do functioanInterface


--


don't have anonymous func have more than one line or a func with more than one line please always name it , as it should be readable .. if more than one line, then func is doing something.. 








--
switch case:
* sometimes need default ( sometimes need to throw expection )
* need break at each case
* just equality check, no other possible
* don't grow it, always split into function in each switch
* do early return
* consider using switch expression too

use enums in switches.. if added type and new item added .. it throws compilation error which is good to handle new enum value.. 

--

To check :

Violation by Separation of Abstractions
--

Todos:

![[Pasted image 20250213075603.png]]

--

Things done:
Feature Envy: method should be inside the class which state the use
Pure func: no side effects, referential transparent is good
Refactorings:  Inline parameter, move func
Split loop: broke loop to define single responsibility principles
Immutable objects + validation construtors
code smell : iterating on map => Llist + new Class
Constant to enum
renaming things => consistant naming
bring code close to business requirement
expalanatory variable 
move declaration close to usage
booleans are bad 0 decompose func, 
extract duplicates by extracrt var > extract func > inline var

---
Author:

https://www.victorrentea.ro/




---
Books recommended:

![[Pasted image 20250212184815.png]]


![[Pasted image 20250213075330.png]]



---


![[Pasted image 20250213172337.png]]

![[Pasted image 20250213172426.png]]


--


GPT summary:

# Article: Key Learnings on Clean Code Practices and Refactoring

## Introduction

This article summarizes the essential lessons and code examples from a coding session focused on refactoring, clean code principles, and effective software design. We'll explore various techniques for improving code readability, structure, and maintainability while maintaining the core functionality.

---

### 1. **The Importance of Refactoring**

Refactoring is a key part of maintaining high-quality software. It involves making continuous improvements to the structure of the code without changing its behavior. This helps make the code cleaner and easier to understand for future developers.

#### Example:

```java
// Before refactoring
public int calculatePrice() {
    int p = 100; 
    // Complex logic goes here
    return p;
}

// After refactoring
public int calculateRentalPrice() {
    int basePrice = 100;
    // Clean logic follows
    return basePrice;
}
```

By renaming variables and methods, we make it easier for others to understand what the code does.

---

### 2. **Comprehension Refactoring**

When you reverse-engineer complex code, you should update it to reflect your understanding. By doing this, you reduce ambiguity and help future developers by making the code self-explanatory.

#### Example:

```java
// Original code
int dr = rental.getPrice() * daysRented; 

// Refactored code
int dailyRentalPrice = rental.getPrice() * daysRented;
```

In this case, renaming the variable `dr` to `dailyRentalPrice` provides more clarity about the purpose of the variable.

---

### 3. **Testing as a Learning Tool**

Testing helps ensure that refactoring or new code doesn’t break existing functionality. Writing tests to validate each piece of code helps uncover design flaws and enables safe refactoring.

#### Example:

```python
# Test for a refactored function
def test_calculate_total_rental_price():
    rental = Rental(100, 3)  # price per day, number of days
    assert rental.calculate_total_price() == 300
```

Here, testing ensures the correctness of the refactored `calculate_total_price` method.

---

### 4. **Avoiding Overuse of Getters and Setters**

Excessive use of getters and setters is considered bad practice, as it exposes too much of an object’s internal state. It’s better to use immutable objects where possible and avoid unnecessary setters.

#### Example:

```java
// Before refactoring
public class Car {
    private String model;
    
    public void setModel(String model) {
        this.model = model;
    }
}

// After refactoring
public class Car {
    private final String model;  // immutable

    public Car(String model) {
        this.model = model;
    }
}
```

The refactored code removes the setter, making the `model` immutable.

---

### 5. **Boy Scout Rule**

The Boy Scout Rule states: "Always leave the code cleaner than you found it." This means improving code whenever you encounter it, even if it's just a minor change, like renaming a variable.

#### Example:

```java
// Initial code
int d = getDistance(a, b);

// Refactored code
int distanceBetweenPoints = getDistance(pointA, pointB);
```

This simple renaming aligns with the Boy Scout Rule, making the code easier to read.

---

### 6. **Simplifying Boolean Logic**

When you have complex boolean conditions, it's essential to break them down into simpler, well-named methods or variables.

#### Example:

```java
// Before refactoring
if (age > 18 && !hasCriminalRecord && creditScore > 700) {
    approveLoan();
}

// After refactoring
if (isEligibleForLoan(age, hasCriminalRecord, creditScore)) {
    approveLoan();
}

private boolean isEligibleForLoan(int age, boolean hasCriminalRecord, int creditScore) {
    return age > 18 && !hasCriminalRecord && creditScore > 700;
}
```

This refactoring breaks down the complex `if` statement into a method that clearly communicates the business logic.

---

### 7. **Avoiding Null Checks in Business Logic**

Null checks should be handled outside of core business logic. The goal is to keep business rules free from clutter, allowing the logic to focus on the main functionality.

#### Example:

```java
// Bad practice: null check in business logic
if (customer != null && customer.hasActiveSubscription()) {
    processOrder();
}

// Refactored: null check handled earlier
public void validateCustomer(Customer customer) {
    if (customer == null) {
        throw new IllegalArgumentException("Customer cannot be null");
    }
}

if (customer.hasActiveSubscription()) {
    processOrder();
}
```

This separation keeps the core logic cleaner and easier to maintain.

---

### 8. **Using Meaningful Naming Conventions**

Naming conventions should be meaningful and reflect the action being performed. Avoid generic terms like `get` or `set`, and choose names that describe the method's purpose.

#### Example:

```java
// Before refactoring
public String getDetails() {
    return formatDetails();
}

// After refactoring
public String formatCustomerDetails() {
    return formatDetails();
}
```

The updated method name `formatCustomerDetails` gives more insight into what the method is doing.

---

### 9. **Using Constants Instead of Magic Numbers**

Magic numbers are hard-coded values that can make code difficult to understand. Instead, use constants to give these values meaningful names.

#### Example:

```java
// Before refactoring
if (daysRented > 2) {
    total += 5;
}

// After refactoring
final int BONUS_DAYS_THRESHOLD = 2;
final int BONUS_AMOUNT = 5;

if (daysRented > BONUS_DAYS_THRESHOLD) {
    total += BONUS_AMOUNT;
}
```

By defining constants, the code becomes more descriptive and easier to modify later.

---

### 10. **Utilizing Refactoring Tools**

The author recommends using refactoring tools (like IntelliJ) to automate tasks such as renaming variables, extracting methods, or inlining methods. These tools make refactoring faster and reduce the risk of introducing errors.

---

## Conclusion

By following these clean coding practices—such as simplifying logic, avoiding overuse of setters, using meaningful names, and leveraging testing—you can improve the readability, maintainability, and quality of your code. Clean code not only benefits the current developer but also makes life easier for those who work with the code in the future.







----


Extra: 

why do we get the code smell when we use the tuple instead of POJO 




---



