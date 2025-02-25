

when you want to do extra in a common func used in many places.. extra things has to be done if only triggered for calling from one region , then how to refactor ?

* don't put condition with booleans , since it spoils single responsibility principle of func and also it makes future code to have extra booleans like this , so code become worse, or boolean can be gone to next layer and inner layers in future easily..


![[Pasted image 20250213181554.png]]




if its simple func.. then better to split the above part and below part in different func.. 
then update old func with just abovePartFunc call and below part func call.. then for new extra flow create a new func.. like old func and add the extra flow in it and use this func where you have the place..


but in nested func  ( pyramid of doom), its hard to split it.. so lets flatten it level by level and do things.. do reiterate of loops if needed, most cases we don't need it..

 normally when you run loop more than once, your CPU do the unrolling the loop which makes the loop run faster... make sure that items no need to by synchronous (no threading, no network response) or else we don't able to split into different loop.. make sure carefully that splitting things really don't have any interdependants ( hidden temporal coupling ).

normal for loops can be more readable by first class functions i.e higher order func like map, filter, reduce..

when refactoring func, don't make subfunc and make it to return multiple items.. no array of objects / objects with key value pair itself..


its not c, so make always the declaration nearer to the usage..
move return closer to the computation i.e do early return

never ever change the collections that is given as parameter... 

make your variable defined in the shortest amount of scope... reassigning things will be nightmare.. 

don't use instance fileds, so altering in the flow have side-effects.. so hard to refactor future.. 



![[Pasted image 20250214083220.png]]


![[Pasted image 20250214084121.png]]


--

in Intellij, we can write own custom refactor flow for anything, which will be handle if you are doing a refactor more.. spend 4hrs to create refactor flow instead of doing refactor manually for 20hrs.. even similar migration code, that migrate your code parts..


--

switch


switch again grow big cases, they can add identation levels

clean switch rules:
* switch should have nothing before and nothing after
* end with default with throw.. 
* each case content to be as short as possible


![[Pasted image 20250214090341.png]]



--

try catch:
* not have them inbetween your business logic, it just surround business logic
* should put only at the boundary of the system

* throw custom errors and catch in global exception layer.. like ExceptionMapper in java



--

Long name for methods / variables:
* mostly it is because larger code base, were generalizing names have conflicts with other names
* if you see func name is too large, crack them into smaller funcs.. 
* sometimes people do long names when they really can't explain whats happening in the code, so naming func give some context well
* have to be in middle ground on names.. hide inner details &  small name -> not always possible.. have some sacrifices

more suggested length  of func is around 20 lines is fine, which fit to the screen


--

extract func usecases:
* reusable code area - Donot Repeat Yourself ( DRY)
* piece of code doing smething - single responsibility principle - make things structured on layers like onion. - do abstraction , in public method call private methods.. 

--
think easier ones:

consier you have list of Ids.. 

consider you are doing request and only for failing one, you have to do something..

instead of collecting the failing ones, collect successful ones and at the end find difference of two list and get the failing ones.. 

don't progressively do any other operation , just collect them and do next operation with collected ones in new loop.. 


--

Common Query Separation principle:

Func should not return and change any variable .. it violates single responsibility principle


--

use standard things always:

\for logger -:
* use slf4j logger in java, which is industry standard
* use standardarized way to representing context that where it is logged, instead of writing func name inside log message custom.. 


--

comments should be mad useless by writing expressive code...

--

never use exceptions for flow controls
* exception should really be exception i.e some error problem
* consider some part of code throws exception 10% of the time, then its not wise usage.. this means that we used exception  as shortcut to skip some upcoming code => use optional in this case and update the flow

--

donot allow object carrying data to talk to db

* hard to test
* hard to reason about since it do temporal coupling
* maybe in unit test, it is simple since we mocking things, but not in original code flow


--

if you have nested if, plan to make those as elif .. or try something to reduce the nested things

--

don't do encryption of password, do just hashing of password.. 

--

don't maintain type in the variable name..
nCustomerCount => number customer count , which is Hungarian notation ( before cpp it were used..), but currently our editor show the variable type easily.

--

you code must to handle any input of that type..

if you are working with page number, also handle what incase page number comesIn in negative values..


--


outcome of refactoring is always not better deal, sometimes you may end up in far worser code than current.. so feel free to revert it back.. 


--

prefer pair programming while refactoring complex code, because doing alone can't help always..

--

don't use var in java mostly, there are places in which var make things worser , decrease readability in java.. 

--

enum are just final constant stuffs, we can hack to make variable values inside constant, but don't use enum like that.. its worst..



--

large method => above 200lines

good function => around 20lines following single responsibility principle

parameters => 3 is better, not greater than 4

don't mutate the parameter mostly..


--

if you need to filter items, 
its recommended to filter in backend itself if possible, since db query operations are faster in all aspects..

if filter rule is very complex, then we are pushed to do it in our application , not on db layer..



--

play with immutable objects mostly.. that helps easy tracking of flow and things.

because people pass mutable objects in inner layers function after function and mutate it in the deep area.

![[Pasted image 20250217105550.png]]


immutability get its benefits more on long lived objects more...


![[Pasted image 20250217105934.png]]



--

Refactoring:

* its additive one
* find better option after implementing one option.. so refactoring of refactored version and goes on...

while refactor big item, take copy of things , do change in new code and slowly transfer old code relying contents to new code.. always check compilation inbetween each steps.. do tiny steps one after another.

--




![[Pasted image 20250214182850.png]]


---

![[Pasted image 20250214083150.png]]

---






