#+TITLE: Notes for CS 152
#+CATEGORY: CS 152

* <2015-08-31 Mon>

** Lab 1 solution
*** max-num
Without let bindings
#+begin_src scheme
(define (max-num lst)
  (cond [(empty? lst) (error "empty")]
        [(= (length lst) 1) (car lst)]
        [else (if (> (car lst) (max-num (cdr lst)))
                  (car lst)
                  (max-num (cdr lst)))]))
#+end_src

With let bindings
#+begin_src scheme
(define (max-num lst)
  (cond [(empty? lst) (error "empty")]
        [(= (length lst) 1) (car lst)]
        [else (let ((head (car lst))
                    (max-tail (max-num (cdr lst))))
                (if (> head max-tail)
                    head
                    max-tail))]))
#+end_src

With helper method
#+begin_src scheme
(define max-num (lst)
  (let-rec ([my-max (lambda (curr-max lst)
                      (cond [((empty? lst) curr-max)]
                            [else (if (> (car lst) curr-max)
                                      (my-max (car lst) (cdr-lst))
                                      (my-max curr-max (cdr lst)))]))])
           (if (empty? lst)
               (error "empty")
               (my-max (car lst (cdr lst))))))
#+end_src

#+begin_src scheme
(define (max-num lst)
  (cond [(empty? lst) (error "empty")]
        [(= (length lst) 1) (car lst)]
        ;; [else (if ((> car lst) (cadr lst))
        ;;           (max-num (cons (car lst (cddr lst))))
        ;;           (max-num (cons (cadr lst (cddr lst)))))]
        [else (max-num (cons (if (> (car lst) (cadr lst)) (car lst) (cadr lst))
                             (cddr lst)))]
        ))
#+end_src

*** fizzbuzz
Base code
#+begin_src scheme
(define (fizzbuzz n)
  (print (fizzbuzz1 1 n)))
#+end_src

#+begin_src scheme
(define (fizzbuzz1 i n)
  (if (= i n)
      ""
      (string-append (cond [(and (is-divisible-by i 3) (is-divisible-by 5)) "fizzbuzz"]
                           [(and (is-divisible-by i 3)                      "fizz")]
                           [(and (is-divisible-by i 5)                      "buzz")]
                           [else                                            i])
                     " "
                     (fizzbuzz1 (+ i 1) n))))
#+end_src

* <2015-09-02 Wed> Higher Order Functions
[[file:~/Documents/School/cs152/slides/CS152-Day04_HigherOrderFunctions.pdf][Slides]]
** Higher Order Functions

** Qualities of Functional Programming

1. Procedures and functions clearly distinguish incoming values (parameters) from outgoing values (results).
   - e.g., C/C++ results may be a parameter that you pass by reference (pointer)
2. (Purely functional) No assignment
3. (Purely functional) No loops
4. Value of a function depends only on the value of its parameters.

** Function that adds in a list of numbers

#+begin_src scheme
(define (sum-list lst)
  (cond [(empty? lst) 0]
        [(else (+ (car lst) (sum-lst (cdr lst))))]))

(define (mult-list lst)
  (cond [(empty? lst) 1]
        [(else (+ (car lst) (mult-list (cdr lst))))]))
#+end_src

#+begin_src scheme
(define (combine-lst fn acc lst)
  (cond [(empty? lst) acc]
        [else (combine-lst fn
                           (fn acc (car lst))
                           (cdr lst))]))

(combine-lst + 0 '(1 2 3 4))
(foldl + 0 '(1 2 3 4))
(foldl * 1 '(1 2 3 4))
#+end_src


#+begin_src scheme
(define (add-one-to-each-elem lst)
  (cond [(empty? lst) '()]
        [else (cons (+ 1 (car lst)) (add-one-to-each-elem (cdr lst)))]))

(add-one-to-each-elem '(1 2 3 4))


(define (add-x-to-each-elem x lst)
  (cond [(empty? lst) '()]
        [else (cons (+ x (car lst)) (add-x-to-each-elem x (cdr lst)))]))

(add-x-to-each-elem 2 '(1 2 3 4))



(define (subtract-one-from-each-elem x lst)
  (cond [(empty? lst) '()]
        [else (cons (- (car lst) 1) (subtract-one-from-each-elem (cdr lst)))]))

(add-x-to-each-elem 2 '(1 2 3 4))

(define (do-to-each-elem fn lst)
  (cond [(empty? lst) '()]
        [else (cons (fn (car lst)) (do-to-each-elem fn (cdr lst)))]))

(do-to-each-elem (lambda (x) (- x 1)) '(1 2 3 4))


(do-to-each-elem (lambda (x) (* x x)) '(1 2 3 4))

(map (lambda (x) (* x x)) '(1 2 3 4))
#+end_src

- Tail recursion: possible test question

class code
#+begin_src scheme
#lang racket
(define (combine-lst fn acc lst)
  (cond [(empty? lst) acc]
        [else (combine-lst fn
                           (fn acc (car lst))
                           (cdr lst))]))

(combine-lst + 0 '(1 2 3 4))
(foldl + 0 '(1 2 3 4))
(foldl * 1 '(1 2 3 4))

(define (add-one-to-each-elem lst)
  (cond [(empty? lst) '()]
        [else (cons (+ 1 (car lst)) (add-one-to-each-elem (cdr lst)))]))

(add-one-to-each-elem '(1 2 3 4))


(define (add-x-to-each-elem x lst)
  (cond [(empty? lst) '()]
        [else (cons (+ x (car lst)) (add-x-to-each-elem x (cdr lst)))]))

(add-x-to-each-elem 2 '(1 2 3 4))



(define (subtract-one-from-each-elem x lst)
  (cond [(empty? lst) '()]
        [else (cons (- (car lst) 1) (subtract-one-from-each-elem (cdr lst)))]))

(add-x-to-each-elem 2 '(1 2 3 4))

(define (do-to-each-elem fn lst)
  (cond [(empty? lst) '()]
        [else (cons (fn (car lst)) (do-to-each-elem fn (cdr lst)))]))

(do-to-each-elem (lambda (x) (- x 1)) '(1 2 3 4))


(do-to-each-elem (lambda (x) (* x x)) '(1 2 3 4))

(map (lambda (x) (* x x)) '(1 2 3 4))
#+end_src
** Lab 3
[[/Users/Daniel/Dropbox/School/fall2015/cs152/labs/lab3/lab3.rkt][lab3.rkt]]
* <2015-09-09 Wed> Modules, Structs, Hashes, & Operational Semantics
[[file:~/Documents/School/cs152/slides/CS152-Day05_OperationalSemantics.pdf][Slides]]

** Problems from the labs

[[file:labs/lab3/lab3.rkt::#lang%20racket][lab3.rkt]]
Solution without higher order functions
#+begin_src scheme
(define (reverse lst)
  (if (empty? lst)
      lst
      (append (reverse (cdr lst))
              (cons (car lst) null))))
#+end_src

Solution with higher order functions
#+begin_src scheme
(define (reverse lst)
  (foldl cons '() lst))
#+end_src

[[file:labs/lab2/lab2.rkt::#lang%20racket][lab2.rkt]]
#+begin_src scheme
(define (add-two-lists lst1 lst2)
  (cond [(empty? lst1) lst2]
        [(empty? lst2) lst1]
        [else (cons (+ (car lst1) (car lst2))
                    (add-two-lists (cdr lst1) (cdr lst2)))]))
#+end_src

** HW 1

[[/Users/Daniel/Documents/School/cs152/hw/hw1/big-num.rkt][big-num.rkt]]

At the top of the =big-num.rkt=, there was this line:
#+begin_src scheme
(provide big-add big-subtract big-multiply ...)
#+end_src

At the top of the =bc.rkt=, there were lines like
#+begin_src scheme
(require "big-num.rkt")

; ...

(big-add arg1 arg2)
(big-subtract arg1 arg2)

; ...
#+end_src

** Lecture

*** Structs

#+begin_src scheme
(struct employee (fname lname ssin id salary dept position))
(struct manager (fname lname ssin id salary dept position bonus))

(define (calc-wages emp)
  (match emp
    [(struct employee (first last social id sal department pos)) sal]
    [(struct manager (_ _ _ _ sal _ _ extra)) (+ sal extra)]))

(let ([dilbert (employee "Dilbert" "Adams" 123 456 8000 "eng." "engineer")]
      [phb     (manager "Pointy-Haired" "Boss" 999 55 105000 "eng." "manager" 150000)])
  (displayln (calc-wages dilbert))
  (displayln (calc-wages phb)))
;; this is called 'pattern matching' or 'destructuring'
#+end_src

*** Hashes
- Like =Hashtable= or =HashMap= in Java, hashes are maps of key/value pairs.
- Unlike Java, hashes are immutable (or at least as far as your homework is concerned).

#+begin_src scheme
(define ht (hash 'a 1 'b 2))
(hash-ref ht 'a) ;1
(hash-ref ht 'c) ; error
(hash-ref ht 'c 0) ; 0, the default value
(hash-ref ht 'b 0) ; 2

(define ht2 (hash-set ht 'c 42))
(hash-ref ht2 'c 0) ; 42
(hash-ref ht2 'a 0) ; 1
(hash-ref ht 'c 0) ; 0
#+end_src

*** Formal semantics

Formal semantics define how our language works /concisely and with minimal ambiguity/. To demonstrate, let's make a small language.

#+BEGIN_SRC text
e ::=                         expressions
     true                     constant true
   | false                    constant false
   | if e then e else e       conditional

v ::= true | false
#+END_SRC

*** Writing our interpreter

#+begin_src scheme
; exp
(struct b-val (val))
(struct b-if (c thn els)) ; condition then-branch else-branch

(define (evaluate prog)
  (match prog
    [(struct b-val (v)) v]
    [(struct b-if (c thn els)) (if (evaluate c)
                                   (evaluate thn)
                                   (evaluate els))]))

(evaluate (b-val #t))
(evaluate (b-if (b-val #f)
                (b-if (b-val #f)
                      (b-val #t)
                      (b-val #f))
                (b-val #t)))
#+end_src

Users are demanding a new feature to be added to the language: numbers! Here is our new formal specification:

#+begin_src text
e ::=                         expressions
     true                     constant true
   | false                    constant false
   | if e then e else e       conditional
   | n | succ e | pred e

v ::= true | false | n

e.g.,
succ 7 ⇓ 8
prd 4 ⇓ 3
#+end_src

* <2015-09-14 Mon> SpartanLang
** Lab 4 review
#+begin_src scheme
(struct b-val (val))
(struct b-if (c thn els))
(struct b-succ (exp))
(strict b-pred (exp))

;; Subexpressions
;; -> succ (if true 0 else 1)
;; -> succ 0
;; -> 1

(define evaluate prog
  (match prog
    [(struct b-val (v)) v]
    [(struct b-if (c thn els)) (if (evaluate c)
                                   (evaluate then)
                                   (evaluate els))]
    [(evaluate b-succ (exp)) (let ([v (evaluate exp)])
                               (if (number? v)
                                   (+ v 1)
                                   (error "expected a number")))]
    [(evaluate b-pred (exp)) (let ([v (evaluate exp)])
                               (if (number? v)
                                   (- v 1)
                                   (error "expected a number")))]))
#+end_src

#+begin_src spartanlang
i := 5;
x := 1;
while !i > 0 do
  x := !x * 2;
  i := !i - 1;
end;
!x
#+end_src
** Bool* vs SpartanLang evaluation
See notebook
** Free variables, scoping, and how to resolve names

#+begin_src sh
x = 42

function foo {
    echo $x
}

function bar {
    local x=666
    foo
}

bar # prints 666
foo # prints 42
echo $x # prints 42
#+end_src

This shows the example of name resolution.

Here's another example in Java:

#+begin_src java
public class Test {
    public static int x = 42;
    public static void foo() {
        System.out.println(x);
    }

    public static void bar() {
        int x = 666;
        foo();
    }

    public static void main(String[] args) {
        bar(); // prints 42
    }
}
#+end_src

Dynamic scope (e.g., Bash): Name resolution depends on the execution path of the code (calling context).)
Lexical or static scoping (e.g., Java): Name resolution depends on where the named variable is defined.

#+begin_src scheme
#lang racket

(define (make-adder x)
  (lambda (y) (+ x y)))

(define inc (make-adder 1))
(inc 4)
(inc 42)

(define (make-counter)
  (let ([count 0])
    (lambda ()
      ;; Do as I say, not as I do
      (set! count (+ count 1))
      count)))

(define my-count (make-counter))
(my-count)
(my-count)
(my-count)

(define ctr2 (make-counter))
(my-count)
(ctr2)
#+end_src

- Closure :: A pair of a function and its environment.
- Environment :: A mapping of free variables to their values in an outer scope.

* <2015-09-16 Wed> Macros

** Here's our custom definition of =if=.
#+begin_src scheme
(define my-if c thn els
  (cond [(and (list? c) (empty? c)) els]
        [(and (number? c) (= c 0))  els]
        [(and (boolean? c) (not c)) els]
        [else                       thn]))
#+end_src

#+begin_src scheme
(my-if 0 1 0)
(my-if '(1 2 3) 1 0)
(my-if '() 1 0)
(define x 3)
(my-if #t
       (set! x (+ x 1))
       (set! x (* x 2)))
(displayln x)
;; outputs:
;; 8

(my-if #f
       (displayln "true")
       (displayln "false"))
;; outputs:
;; true
;; false
#+end_src

** Why didn't this work?

- Scheme evaluates function args eagerly

- macro :: short for macroinstruction. Scheme has macros to change the behavior of evaulation.
- text-substitution :: C

** Text substitutions

*** inadvertant variable capture
#+begin_src C
#include <stdio.h>

#define SWAP(a,b) { int tmp=a; a=b; b=tmp; }

void badHygieneExample() {
  int a = 5;
  int tmp = 0;
  printf("a=%d, tmp=%d\n", a, tmp);
  SWAP(a,tmp);
  // this macro actually looks like
  // { int tmp = a; a = tmp; tmp = tmp;};
  printf("a=%d, tmp=%d\n", a, tmp);
}

int main(int argc, const char* argv[]) {
  badHygieneExample();
}
#+end_src

#+RESULTS:
| a=5 | tmp=0 |
| a=5 | tmp=0 |

*** Macros in Racket

- define-syntax-rule :: use this to define macros

#+begin_src scheme
(define-syntax-rule (swap x y) ;; pattern
  
  (let ([tmp x])       ; |
    (set! x y)         ; | template
    (set! y tmp))      ; |
  
  )

(define a 3)
(define b 4)
(swap a b)
(displayln a)
#+end_src

#+begin_src scheme
(define-syntax rotate
  (syntax-rules ()
    [(rotate a b) (swap a b)]
    [(rotate a b c)
     (begin
       (swap a b)
       (swap b c))]))
#+end_src

* <2015-09-21 Mon> Contracts

- Contracts are enforced at run-time
- Types are enforced at compile-time
- We can write more sophisticated contracts

** Quick sort
*** Without contract (buggy)
#+begin_src scheme
#lang racket

(define (quicksort lst)
  (cond [(empty? lst) '()]
        [else (let* ([pivot (car lst)]
                     [p (partition (cdr lst) pivot '() '())]
                     [left (quicksort (car p))]
                     [right (quicksort (cdr p))])
                (cons pivot (append left right)))]))

(define (partition lst pivot left right)
  (if (empty? lst)
      (cons left right)
      (let ([x (car lst)]
            [rest (cdr lst)])
        (if (> x pivot)
            (partition rest pivot (cons x left) right)
            (partition rest pivot left (cons x right))))))

(quicksort '(9 33 0 1 45 16 8 33))
#+end_src

*** With contracts

#+begin_src scheme
#lang racket

(define/contract (quicksort lst)
  (-> list? (and/c list? ordered?))
  (cond [(null? lst) '()]
        [else
         (let* ([pivot (car lst)]
                [p (partition (cdr lst) pivot '() '())]
                [left  (quicksort (car p))]
                [right (quicksort (cdr p))])
           (cons pivot (append left right)))])) ;; BROKEN

(define ordered? lst
  (if (< (length lst) 2)
      #t
      (and (<= (car lst) (cadr lst))
           (ordered? (cdr lst)))))

(define (partition lst pivot left right)
  (if (null? lst)
    (cons left right)
    (let ([x (car lst)]
          [rest (cdr lst)])
      (if (> x pivot) ;; BROKEN
        (partition rest pivot (cons x left) right)
        (partition rest pivot left (cons x right))))))
#+end_src

* <2015-09-23 Wed>

** Lab 5 review

Removing repetition:
#+begin_src scheme
(define (Employee n p s)
  (list (box n)
        (box p)
        (box s)))
#+end_src

** Javascript

- Using node.js
- We are going to care about the language, not the web usage of JS.

*** Object-Oriented JavaScript

#+begin_src javascript
function Adder (amount) {
    this.amount = amount;
}

Adder.prototype.add = function(x) {
    return this.amount + x;
}

var myAdder = new Adder(1);
var y = myAdder.add(7);
#+end_src

#+begin_src js
var x = 42, y = 7;
function add(a,b) { return a + b; }
var square = function(a) { return a + a; }
console.log("x + y = " + add(x,y));
var print = console.log;
#+end_src

#+RESULTS:
: x + y = 49
: undefined

#+begin_src js
var getNextInt = function () {
    var nextInt = 0;
    return function () {
        return nextInt++;
    }
}();

console.log(getNextInt());
console.log(getNextInt());
console.log(getNextInt());
console.log(getNextInt());
#+end_src

#+RESULTS:
: 0
: 1
: 2
: 3
: undefined


#+begin_src js
var getNextInt = function () {
    var nextInt = 0;
    return function () {
        return nextInt++;
    }();
}

console.log(getNextInt());
console.log(getNextInt());
console.log(getNextInt());
console.log(getNextInt());
#+end_src

#+RESULTS:
: 0
: 0
: 0
: 0
: undefined

#+begin_src js
var complex = {real: 3, imaginary: 1};
console.log(complex.real);
complex.real = 4;
var s = 'imaginary';
complex[s] = 0; // complex.imaginary = 0
#+end_src

#+RESULTS:
**** Prototypes

#+begin_src js
var Dog = {
    speak: function() {print('bark!');}
}
rex = {name: 'Rex'}, __proto__: Dog};
#+end_src

#+RESULTS:

Dog:
|-----------+------------------|
| __proto__ | Object.prototype |
| speak     | fun(bark)        |
|-----------+------------------|

Rex:
|-----------+-----|
| name      | Rex |
| __proto__ | Dog |
|-----------+-----|

If Rex calls a function that isn't defined in its box, then it will delegate that method call to its =__proto__=.

Let's add a function to rex:
#+begin_src js
rex['favoriteToy'] = "squeaky ball";
rex.speak = function () { print('grr...');}
var fifi = { __proto__ : Dog};
fifi.speak(); // bark
rex.speak(); // grr
delete rex.speak
rex.speak(); // bark

delete rex.__proto__.speak; // deletes speak in Dog
fifi.speak(); // error
#+end_src

*** Constructors

#+begin_src js
function Cat(name, breed) {
    this.name = name; this.breed = breed;
    // this is not optional. You always have to use this.
    this.speak = function () { print('mean');}
}

var garfield = new Cat('Garfield', 'Orange tabby', 48);

// When we use the `new` keyword, we effectively all this function, with the following changes:
// it starts with
//     this = {};
//     this.__proto__ = Cat.prototype;
// it ends with
//     return this;
function Cat(name, breed) {
    this = {}; this.__proto__ = Cat.prototype;
    this.name = name; this.breed = breed;
    // this is not optional. You always have to use this.
    this.speak = function () { print('mean');}
    return this;
}
#+end_src

* <2015-09-28 Mon> Introduction to Node.js
  CLOCK: [2015-09-28 Mon 12:01]--[2015-09-28 Mon 13:13] =>  1:12

** Node.js

- A JavaScript runtime and library designed for use outside the
  browser, based off of Google's V8 engine.
- npm: package manager for Node.js
- http://nodejs.org

** myFile.txt

#+BEGIN_SRC text
This is my file
There are many like it,
but this one is mine.
#+END_SRC

** File I/O in Node.js

#+begin_src js
var fs = require('fs');
fs.readFile('myFile.txt',
            function(err, data) { // callback function
                if (err) throw err;
                console.log("" + data);
            })
console.log('all done');
#+end_src

#+RESULTS:

** Warm up exercise

Create a =makeListOfAdders= function.
input: a list of numbers
returns: a list of adders

Correct version:
#+begin_src js :results output
function makeListOfAdders(addrs) {
    var result = [];
    for (var i = 0; i < addrs.length; i++) {
        result.push(function (addr) {
            return function (x) {
                return x + addr;
            }
        }(addrs[i]));
    }
    return result;
}
var addrs = makeListOfAdders([3,5,9]);
console.log(addrs[0](4));
console.log(addrs[1](10));
#+end_src

#+RESULTS:
: 7
: 15


Wrong version:
#+BEGIN_SRC js
function makeListOfAdders(addrs) {
    var result = [];
    for (var i = 0; i < addrs.length; i++) {
        result.push(function (x) {
            return x + addrs[i];
        });
    }
    return result;
}
var addrs = makeListOfAdders([3,5,9]);
console.log(addrs[0](4));
console.log(addrs[1](10));
#+END_SRC

#+RESULTS:
: 13
: 19
: undefined

** Global object

=this= can refer to the global object

** Execution Contexts

Scoping is determined by /execution contexts/, each of which is
comprised of

- a variable object
  - Container of data for the execution context
  - Container for variables, function declarations, etc.
- a scope chain
  - The variable object plus the parent scopes.
- a context object (this)

*** Global context

- Top level context
- Variable object is known as a the /global object/.
- =this= refers to global object.

*** Function context

- Variable objects are known as /activation objects/.
  An activation object includes:
  - Arguments passed to the function
  - A special arguments objet
  - Local variables
- What is =this=? It's complicated.

*** What does this refer to?                 :exam-question:

- Normal function calls: this global object
- Object methods: The object
- Constructors (functions called with new): The new object being created.
- Special cases
  - Can be changed with =call= and =apply= functions
  - Can be changed with bind method (Ecmascript 5)
  - For an in-line event handler (on the web), refers to the relevant
    DOM element.

*** =apply=, =call=, and =bind=

#+BEGIN_SRC js :results output
x = 3;

function foo(y) {
  console.log(this.x + y);
}
foo(100);

foo.apply(null, [100]); // array passed for args
foo.apply({x:4}, [100]);
foo.call({x:4}, 100); // no array needed

var bf = foo.bind({x:5}); // create a new function
bf(100);
#+END_SRC

#+RESULTS:
: 103
: 103
: 104
: 104
: 105

* <2015-09-30 Wed> Makefiles and event handling

#+begin_src html
<html>
  <input type='button' onclick='alert("Hello!");' value='Say hi'/>
</html>
#+end_src

#+BEGIN_SRC html :tangle ./button.html
<html>
  <input id='thebutton' type='button' value='Say hi' onclick='alert(42);'/>
  <script>
    var button = document.getElementById('thebutton');
    function sayGroovy() {
        alert('groovy');
    }
    button.addEventListener('click', sayGroovy);
  </script>
</html>
#+END_SRC

** No concurrency                            :examquestion:

JavaScript runs everything top-down, and added events and setTimeout are just added on the todo list of what to execute.

* <2015-10-05 Mon>

** First JavaScript lab

#+BEGIN_SRC js
var foldl = function(f, acc, array) {
    if (array.length === 0) return acc;
    else foldl(f, f(acc, array[0]), array.slice(1));
}

var map = function(f, array) {
    if (array.length === 0) return [];
    else return [f(array[0])].concat(map(f, array.slice(1)));
}

function mult(x,y) { return x * y; }

function curryMult(x) {
    return function (y) { return x * y; }
}

mult(3,4);
curryMult(3)(4);
#+END_SRC

#+BEGIN_SRC js :results output
function Student(firstName, lastName, studentID) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.studentID = studentID;
    this.display = function () {
        console.log(this.firstName + " (this)");
    }
}

Student.prototype.display = function() {
    console.log(this.firstName + " (proto)");
}

var stu = new Student("Stu", "Disco", 1234);
stu.display();
delete stu.display;
stu.display();
delete stu.prototype;
console.log(Student.prototype);
#+END_SRC

#+RESULTS:
: Stu (this)
: Stu (proto)
: Student { display: [Function] }

#+BEGIN_SRC js
var harry = {
    firstName: 'Harry',
    lastName = 'Potter',
    studentID = 888,
    __proto__: Student.prototype
}
#+END_SRC

** Macros lab

#+BEGIN_SRC java
switch (x) {
    case 4: System.out.prinln("x is 4");
        break;
    case 5: return x * 2;
    default: System.out.println("not 4 nor 5");
}
#+END_SRC

#+BEGIN_SRC scheme
(swtch (x)
       [4 (displayln "x is 4")]
       [5 (* x 2)]
       ['default (displayln "not 4 nor 5")])
#+END_SRC

#+BEGIN_SRC scheme
(define-syntax switch
  (syntax-rules ()
    {(switch v) (void)}
    {(switch v ['default body]) body}
    {(switch v [val1 body1] rest-cases ...)
     (if (eqv? v val1)
         body1
         (swictch v rest-cases ...))}))
#+END_SRC

** JavaScript Object Proxies

*** What is metaprogramming?

- metaprogramming :: writing programs that manipulate other programs.

*** JavaScipt Proxies

Special metaprogramming features are proposed for ECMAScript 6,
code-named ECMAScript Harmony.

- Harmony :: getting into agreement from all the ECMAScript committee
             members.

Proposed by Tom Van Cutsem and Mark Miller (security guy)

* <2015-10-07 Wed> Midterm Review
  CLOCK: [2015-10-07 Wed 12:00]--[2015-10-07 Wed 13:13] =>  1:13

** Multiple Choice questions of the exam

1. AB
2. ABE
3. AC
4. BC
5. AE

** Material on this exam

*** Racket/Scheme
- primitive types
  - numbers
  - booleans
- compound types
  - strings (list of characters)
- lists
- functional language
- =lambda=
- dynamically type
  - type errors happen at runtime
  - there might be errors that never happen because the code path with
    the error does not get executed.

*** compilation process
- Lexer/Tokenizer
- Parser (takes in tokens and returns an AST)
- Compiler/Interpreter
  - Compiler :: transforms the language into a different source
                (machine code, source-to-source, etc.)
  - Interpreter :: executes the code, not necessarily with
                   machine-code optimization.
- Louden & Lambert design criteria
  - efficiency
  - regularity (uniformity)
    - e.g., PHP's inconsistent naming conventions. Pascal's return-value convention.
  - security
    - run-time checks.
  - extensibility

*** Functional programming

- clearly distinguish input/output
  - contrast with C/C++ with parametric return values (pointers).
- *functions are first-class values*
- higher-order functions
  - e.g., map, foldl, foldr, call, apply
- no assignments (no loops) (purely functional [PF])
- recursion
- value of a function call depends only on its arguments (PF)
- no side effects
- tail recursion
  - accumulator pattern (question #9 from [[/Users/Daniel/Documents/School/cs152/practice-exam.pdf][practice exam]])

*** Operational Semantics

- big-step
- store (or map, or env)
- closures + scoping
- lexical (static) scoping vs dynamic scoping
- free variables

*** Macros

- text substitution (C preprocessor)
  - inadvertent variable capture
  - bad hygiene
- Syntactic macros
  - working on ASTs
  - hygienic

*** Contracts

- run-time enforcement
- pre-conditions
- post-conditions
- blame
- ->
- ->i
- can be put on module and/or functions

*** JavaScript

- mult-paradigm
  - functional
  - object-oriented
    - prototypes. no classes.
- dynamically typed
- no concurrency
- event-based programming
  - browser
  - Node.js (events.EventEmitter)
    - on
    - emit
- meaning of 'this'


*** metaprogramming
- reflection: Introspection & Self Modification
- Intercession
  - metaobject protocol (MOP)

- Object proxies
  - MOP
  - proxy objects, behavior determined by handler

* <2015-10-12 Mon> Taming the Dark, Scary Corners of JavaScript

** Lack block scoping

** Forgetting =new= causes some strange errors

** Forgettting =var= makes variables global

** Forgetting semicolons
** parseInt won't warn you of problems
** JSLint
made by Douglas Crockford
** TypeScript
- Made by Microsoft
- One of the many JavaScript compilers
** What do type systems give us?

- Tips for compilers to make code more efficient
- most importantly, type systems prevent code from running with errors.

* <2015-10-19 Mon> Prolog

Facts:
1) All men are mortal.
2) Aristotle is a man.

Conclusion:
3) Aristotle is mortal.

** Prolog
- Declarative programming language
  - you specify what you want
  - Computer sorts out the details
- Logical
  - specify facts and rules
  - deduce conclusions

#+BEGIN_SRC prolog :tangle lecture_code/first.prolog
% Facts
king(robert).
wife(cersei, robert).

brother(jamie,cersei).
brother(jamie,tyrion).
brother(tyrion,jamie).

friend(robert,ned).
friend(robert,jon_arryn).
friend(tyrion,bronn).
friend(tyion,jamie).

enemy(tyrion,littlefinger).
enemy(cersei,robert).
enemy(cersei,tyrion).

% rules
enemy(cersei,X) :- friend(robert,X). % variables are Capitalized. if robert is a friend of X, then cersei is the enemy of X

% if tyrion is a friend of X, then cersei is the enemy of X
enemy(cersei,X) := friend(tyrion,X),
                   X \= jamie.
#+END_SRC

swi-prolog interpreter

?- enemy(cersei,bronn).
false.
?- enemy(cersei,jamie).
false.
?- enemy(cersei,Enemy).
Enemy = robert
?- enemy(Hater,tyrion).
?- enemy(Hater,Enemy).

#+BEGIN_SRC prolog :tangle lecture_code/first.prolog
queen(Q) :- wife(Q,K),
            king(K).

enemy(jamie,X) :- not (brother(jamie,X)),
                  X \= jamie
#+END_SRC

* <2015-10-21 Wed> Midterm rundown, Prolog

** Midterm scores

Approximate:
A: 88+
B: 77-87
C: 64-76

** Prolog

- "Learn Prolog Now", http://www.learnprolognow.org
- SWI-Prolog website (contains manual and tutorials), http://www.swi-prolog.org
- "NLP with Prolog in the IBM Watson System", http://www.cs.nmsu.edu/ALP/2011/03/natural-language-processing-with-prolog-in-the-ibm-watson-system/

*** Review: Facts

Batman likes gotham
#+BEGIN_SRC prolog
likes(batman, gotham).
likes(batman, justice).
likes(ras_al_ghul, justice).
likes(ras_al_ghul, revenge).
#+END_SRC

Everybody likes Raymond? That should probably be a rule, where X is everyone

*** Review: Queries & Variables

What do Batman and Ra's al Ghul both like?

#+BEGIN_SRC prolog
likes(batman,X), likes(ras_al_ghul,X)
#+END_SRC
- X :: variable
- , :: and

*** How does the Prolog engine resolve these queries?

Through 2 processes:
1. Resolution
2. Unification

*** Resolution and Unification

- Resolution :: The process of matching facts and rules to perform /inferencing/.

*** Rules

#+BEGIN_SRC prolog
scary(V) :- villain(V),
            kills_people(V).

scary(V) :- villian(V),
            power(V,_).
#+END_SRC
* <2015-10-26 Mon>

** Lab 12 review: clue.prolog

#+begin_src prolog
motive(S, mr_boddy) :- suspect(S), S \= wadsworth.
motive(mrs_peacock, cook).
motive(colonel_mustard, motorist).
...

#+end_src

Final result:
#+begin_src prolog
accuse(S,V) :- motive(S,V), murder(V,W,R), suspect(S), visited(S,R), can_use(S,W), not(alibi(S,V)).
#+end_src

#+begin_src prolog
visited(S,R) :- not(never_visited(S,R)).

never_visited(miss_scarlet, billiard_room).
never_visited(professor_plum, kitchen).
never_visited(colonel_mustard, R) :- murder(mr_boddy, _, R).

can_use(S,W) :- not(cant_use(S,W)).

cant_use(colonel_mustard, rope).
cant_use(professor_plum, revolver).
cant_use(mrs_peacock, candlestick).

alibi(mr_white, mr_boddy).
alibi(mr_green, _).

%% handle cases where a room can have more than one victim.
alibi(miss_scarlet, V) :- murder(V,_,R), murder(_, revolver, R).
#+end_src

** Lecture

#+begin_src prolog
edge(a,b,2).
edge(b,a,2).
edge(a,c,3).
edge(c,a,3).
edge(a,f,4).
edge(f,a,4).
edge(b,c,2).
edge(c,b,2).
edge(c,d,3).
edge(d,c,3).
edge(c,e,1).
edge(e,c,1).
edge(d,f,5).
edge(f,d,5).
#+end_src


Lists in Prolog:
[Head|Tail]

like car and cdr in Scheme

#+begin_src prolog
find_path(Start,End,Cost,Path) :- edge(Start,End,Cost),
                                  Path = [Start, End].
find_path(Start,End,Cost,Path) :- edge(Start,X, InitCost),
                                  find_path(X,End,RestCost,TailPath),
                                  TotalCost is InitCost + RestCost,
                                  Path = [Start|TailPath].
#+end_src

#+begin_src prolog
% myappend(List1,List2,Result).
myappend([],List2,List2).
myappend([H|T1],List2, [H|TResult]) :- myappend(T1,List2,TResult).

%myreverse(List,Reversed).
myreverse([],[]).
myreverse([H|T1],Rev) :-
    myreverse(T,RT),
    append(RT, [H], Rev).

%in_list(X,List).

% redundant
in_list(X, [Y|_]) :- X = Y.

in_list(X, [X,|_]).
in_list(X,[Y|Rest]) :- X \= Y, in_list(X,Rest).

% built-in prolog function called member

% add_nums(ListNums, Sum).
add_nums([], 0).
add_nums([H|T], Sum) :-
    add_nums(T, SubSum),
    Sum is H + SubSum.

% qsort(List,Sorted)
qsort([],[]).
qsort([Pivot|Tail], Sorted) :-
    divide(Pivot,Tail,Left,Right),
    qsort(Left,LSort),
    qsort(Right,RSort),
    append(LSort,[pivot|RSort]).

% divide(P,List,Left,Right)
divide(_,[],[],[]).
divide(P,[H|T],[H|L],R) :-
    P > H,
    divide(P,T,L,R).

divide(P,[H|T],L,[H|R]) :-
    P =< H,
    divide(P,T,L,R).

% note that less-than-equals is =<, not <=. "<=" looks like an arrow, which is probably something different in Prolog.
#+end_src

<<batman_example>>
#+begin_src prolog
villain(joker).
villain(penguin).
villain(catwoman).
villain(scarecrow).
villain(bane).

kills_people(joker).
kills_people(penguin).
kills_people(bane).
power(scarecrow,fear).
power(bane,venom).

scary(V) :- villain(V), kills_people(V).
scary(V) :- villain(V), power(V,_).

%% ?- scary(V).
%% V = joker ;
%% V = penguin ;
%% V = bane ;
%% V = scarecrow ;
%% V = bane ;
%% false.

%% ?- findall(V,scary(V),R).
%% R = [joker,penguin,bane,scarecrow,bane].

find_scary(scarySet) :-
    find_all(V,scary(V), ListOfScaries),
    get_unique(ListOfScaries, ScarySet), !. % green cut

get_unique([],[]).
get_unique([H|T], Set) :-
    get_unique(Tail, TailSet),
    \+ member(H, TailSet),
    Set = [H|TailSet].

% \+ is the same thing as "not"

get_unique([H|Tail], Set) :-
    get_unique(Tail,TailSet),
    member(H,TailSet),
    Set = TailSet.
#+end_src

* <2015-10-28 Wed> Syntax and ANTLR

** Syntax vs Semantics

- Semantics
  - What does a program mean?
  - Defined by an interpreter or compiler?
- Syntax
  - How is a program structured?
  - Defined by a lexer and parser.

** Tokenization

- Process of converting characters to the /words/ of the language.
- Generally handled through regular expressions.
- A variety of lexers exist
  - Lex/Flex are old and well-established
  - ANTLR & JavaCC both handle lexing and parsing
- Sample lexing rule for integers (Int Antlr)
  - =INT : [0-9]+ ;=

#+BEGIN_EXAMPLE
a       exactly one a
a?      0 or 1 a
a*      0 or more
a+      1 or more
#+END_EXAMPLE

#+BEGIN_SRC 
INT : [1-9][0-9]* | 0
#+END_SRC

** Categories of Tokens

- Reserved words or keywords
  - e.g., =if=, =while=
- Literals or constants
  - e.g., =123=, ="hello"=
- Special symbols
  - e.g., =";"=, ="<="=, ="+"=
- Identifiers
  - e.g., =balance=, =tryionLannister=


Example ANTLR file:

#+NAME: Expr.g4
#+BEGIN_SRC java
// Expr.g4
grammar Expr;

// Lexing rules (capital letters)
INT : [0-9]+ ;
NEWLINE: '\r'? '\n' ;
WS: [ \t] -> skip ;
MUL: '*' ;
DIV: '/' ;
ADD: '+' ;
SUB: '-' ;

#+END_SRC

** Lexing in ANTLR (v. 4)

(in class)

** Parsing

- Parsers take the tokens of the language and combines them into /abstract syntax trees/ (ASTs).
- The rules for parsers are defined by /context free grammars/ (CFGs).
- Parsers can be divided into
  - bottom-up/shift-reduce parsers
  - top-down parsers

** Context Free Grammars

- Grammars specify a language
- Backus-Naur form is a common format

#+BEGIN_SRC text
Expr -> Number
      | Number + Expr
#+END_SRC

- Terminals cannot be broken down further.
- Non-terminals can be broken down into further phrases.

Help our code read the parser by adding =op=:

#+NAME: Expr.g4
#+BEGIN_SRC 
// ***Paring rules ***

/** The start rule */
prog: stat+ ;

stat: expr NEWLINE              # printExpr
    | NEWLINE                   # blank
    ;

expr: expr op=( '*' | '/' ) expr   # MulDiv
    | expr op=( '+' | '-' ) expr   # AddSub
    | INT                       # int
    | '(' expr ')'              # parens
    ;
#+END_SRC


The # names are *not comments*. They are tips to ANTLR about how to name functions in the generated code.

* <2015-11-02 Mon> Lab review and Ruby

** Lab 13 review

graph.prolog

This still does backtracking.
#+BEGIN_SRC prolog
find_path(Start, End, _, TotalCost, Path) :-
  edge(Start, End, Cost),
  Path = [Start, End].

find_path(Start, End, Visited, TotalCost, Path) :-
  edge(Start, X, InitCost),
  \+ member(Start, Visited),
  find_path(X, End, [Start|Visited], RestCost, TailPath),
  TotalCost is InitCost + RestCost,
  Path = [Start|TailPath].
#+END_SRC

#+BEGIN_SRC prolog
find_path(Start, End, _, TotalCost, Path) :-
  edge(Start, End, Cost),
  Path = [Start, End].

find_path(Start, End, Visited, TotalCost, Path) :-
  edge(Start, X, InitCost),
  \+ member(Start, Visited),
  find_path(X, End, [X|[Start|Visited]], RestCost, TailPath),
  TotalCost is InitCost + RestCost,
  Path = [Start|TailPath].
#+END_SRC

** Ruby

- Created by Matz

** Ruby Influences

- Smalltalk
  - everything is an object
  - blocks
  - sophisticated metaprogramming
- Perl
  - Strong support for regular expressions.
  - Many functions names borrowed

** Ruby on Rails

- Ruby's "killper app": lightweight web framework.
  - "convention over configuration" philosophy.
- Initially, David Heinemeier Hansson (DHH) used PHP.
  - abandoned it for Ruby when it created Rails
- We will focus on /Ruby/ --- we don't care about Rails.

** Hello World in Ruby

#+BEGIN_SRC ruby :results output
puts 'Hello World!'
#+END_SRC

#+RESULTS:
: Hello World!

** Ruby is object-oriented

#+BEGIN_QUOTE
I was talking with my collegue about the possibility of an
object-oriented scripting language. [...] I knew Python then. But I
didn't like it.

- Matz 1999
#+END_QUOTE

#+BEGIN_SRC ruby
class Person
  def initialize name # Constructor
    @name = name
  end 

  def name
    return @name
  end

  def name= newName
    @name = newName
  end

  def say_hi
    puts "Hello, my name is #{@name}"
  end
end
#+END_SRC

#+BEGIN_SRC ruby
class Person
  attr_accessor :name

  def initialize name
    @name = name
  end

  def say_hi
    puts "Hello, my name is #{@name}"
  end
end
#+END_SRC

#+BEGIN_SRC ruby
p = Person.new "Joe"
puts "Hello, #{p.name}"
#+END_SRC

#+RESULTS:

** Inheritance in Ruby (in class)

#+BEGIN_SRC ruby :results output :session Dog
class Dog
  def initialize(name)
    @name = name
  end

  def speak
    puts "#{@name} says bark"
  end
end

rex = Dog.new('Rex')
rex.speak
#+END_SRC

#+RESULTS:

#+BEGIN_SRC ruby GuardDog :session Dog :results output
class GuardDog < Dog
  attr_reader :breed

  def initialize(name, breed)
    super(name)
    @breed = breed
  end

  def attack
    puts "growl"
  end

end

satan = GuardDog.new('Satan', 'Doberman')
puts "Satan is a #{satan.breed}"
satan.attack
satan.speak
#+END_SRC

#+RESULTS:

** Mixins

- Allow user to add features to a class
- Similar to interfaces in Java, but programmer can specify functionality

#+BEGIN_SRC ruby
class Person
  include Comparable
end
#+END_SRC

#+BEGIN_SRC ruby
module RevString
  def to_rev_s
    to_s.reverse
  end
end

class Person # Re-opening class
  include RevString
  def to_s
    @name
  end
end

p.to_rev_s # p defined previously
#+END_SRC

** Blocks in Ruby

- Borrowed from Smalltalk

- Supeficially similar to blocks in other languages.
- Allows developer to create custom control structures.
- (We'll discuss in depth another day).

*** File I/O Example (in-class)

#+BEGIN_SRC ruby
file = File.open('temp.txt', 'r')
file.each_line do |line|
  puts line
end
file.close
#+END_SRC

Version 2:
#+BEGIN_SRC ruby
File.open('temp.txt', 'r') do |file|
  file.each_line { |line| puts line }
end
#+END_SRC

** String Processing

** Regular Expressions in Ruby

#+BEGIN_SRC ruby :results output
s = "Hi, I'm Larry; this is my" +
    " brother Darryl, and this" +
    " is my other brother Darryl."
s.sub(/Larry/, 'Laurent')
puts s
s.sub!(/Larry/, 'Laurent')
puts s
puts s.sub(/brother/, 'frere')
puts s.gsub(/brother/, 'frere')
#+END_SRC

#+RESULTS:
: Hi, I'm Larry; this is my brother Darryl, and this is my other brother Darryl.
: Hi, I'm Laurent; this is my brother Darryl, and this is my other brother Darryl.
: Hi, I'm Laurent; this is my frere Darryl, and this is my other brother Darryl.
: Hi, I'm Laurent; this is my frere Darryl, and this is my other frere Darryl.

** Regular Expression Symbols

| /./  | Any character except a newline |
| /\w/ | A word character ([a-zA-Z0-9_]) |
* <2015-11-04 Wed> Blocks and Message Passing
** Smalltalk

- Ruby borrows heavily from Smalltalk. Some key Smalltalk concepts:
  - Everything is an object
  - Chunks of computation can be passed as /blocks/
  - Objects commuicate by /passing messages/

** Blocks

Opening/closing file example

** Creating Custom Blocks

With Ruby, we can write methods that accept blocks, which is useful for

- creating our own *control structures*
- eliminating *boilerplate* code

*** Example: Execute code with some probability
    :PROPERTIES:
    :results:  output
    :END:

#+BEGIN_SRC ruby :results output
def with_prob (prob)
  yield if (Random.rand < prob)
end

with_prob 0.42 do
  puts "There is a 42% chance "
    + "that this code will print"
end
#+END_SRC

=yield= is the special keyword to make use of custom blocks

Another way to use custom blocks is with named blocks using =&=.

#+BEGIN_SRC ruby
def with_prob (prob, &blk)
  blk.call if (Random.rand < prob)
end

with_prob 0.42 do
  puts "There is a 42% chance "
    + "that this code will print"
end

def half_the_time (&block)
  with_prob(0.5, &block)
end
#+END_SRC

#+BEGIN_SRC ruby :results output
def do_noisy
  puts "About to call block"
  yield
  puts "Just called block"
end

do_noisy do
  puts 3 + 4
end
#+END_SRC

#+RESULTS:
: About to call block
: 7
: Just called block

#+BEGIN_SRC ruby
class Array # re-opening array
  def each_downcase
    self.each do |word|
      yield word.downcase
    end
  end
end

["Alpha", "Beta", "AndSoOn"].each_downcase do |word|
  puts word
end
#+END_SRC

#+RESULTS:
: alpha
: beta
: andsoon

#+BEGIN_SRC ruby :results output
def conversion_chart(from_units, to_units, values)
  puts "#{from_units}\t#{to_units}"
  left_line = right_line = ""
  from_units.length.times { left_line += '-' }
  to_units.length.times { right_line += '-' }
  puts "#{left_line}\t#{right_line}"
  for val in values
    converted = yield val
    puts "#{val}\t#{converted}"
  end
  puts
end

celsius_temps = [0,10,20,30,40,50,60,70,80,90,100]
conversion_chart("C", "F", celsius_temps) { |cel| cel * 9 / 5 + 32 }

fahrenheit_temps = [0,10,20,30,40,50,60,70,80,90,100,110,120,130,140,150,160,170,180,190,200 ]
conversion_chart("Fahr.", "Celsius", fahrenheit_temps) {|fahr| (fahr-32) * 5 / 9 }
conversion_chart("Km", "Miles", (1..10)) do |km|
  mile = 0.621371 * km
end
#+END_SRC

#+RESULTS:
#+begin_example
C	F
-	-
0	32
10	50
20	68
30	86
40	104
50	122
60	140
70	158
80	176
90	194
100	212

Fahr.	Celsius
-----	-------
0	-18
10	-13
20	-7
30	-2
40	4
50	10
60	15
70	21
80	26
90	32
100	37
110	43
120	48
130	54
140	60
150	65
160	71
170	76
180	82
190	87
200	93

Km	Miles
--	-----
1	0.621371
2	1.242742
3	1.8641130000000001
4	2.485484
5	3.106855
6	3.7282260000000003
7	4.349597
8	4.970968
9	5.592339
10	6.21371

#+end_example



#+BEGIN_SRC ruby
3.times do
  puts 'hi'
end
#+END_SRC

#+RESULTS:
: hi
: hi
: hi

neil gaftner wanted to have Ruby-style closures in Java 8, but he lost and JS-style closures are now in Java.


#+BEGIN_SRC ruby
def with_prob (prob)
  yield if (Random.rand < prob)
end

def foo x
  with_prob 0.5
  do
    return 0
  end
  return x
end
#+END_SRC

#+RESULTS:

#+BEGIN_SRC ruby
class Employee
  attr_accessor :name, :ssid, :salary

  def initialize(name, sid, salary)
    @name = name; @sid = ssid; @salary = salary
  end

  def to_s
    @name
  end
end

alice = Employee.new("Alice Alley", 1234, 75000)
bob = Employee.new("Robert Tobles", 5678, 50000)
class << bob
  def signing_bonus
    2000
  end
end
puts(bob.signing_bonus)
puts(alice.signing_bonus)
#+END_SRC

#+RESULTS:
: 2000

#+BEGIN_SRC ruby
class Employee
  attr_accessor :name, :ssid, :salary

  class << self
    def add(emp)
      puts "Adding #{emp}"
      @employees = Hash.new unless @employees
      @employees[emp.name] = emp
    end

    def get_emp_by_name(name)
      @employees[name}
    end
  end

  def initialize(name, sid, salary)
    @name = name; @sid = ssid; @salary = salary
    Employee.add(self)
  end

  def to_s
    @name
  end
end

alice = Employee.new("Alice Alley", 1234, 75000)
bob = Employee.new("Robert Tables", 5678, 50000)
b = Employee.get_emp_by_name('Robert Tables')
puts b.salary
#+END_SRC

* <2015-11-09 Mon> Dynamic Code Evaluation and Taint Analysis

** =eval=

- Allows for code to be executed dynamically.
- In most languages, eval only takes a string

#+BEGIN_SRC ruby
eval "puts 2+3"
#+END_SRC

- While this feature is widely popular (especially in JavaScript), it is also a source of security problems.
  - See richards et al. /The Eval that Men Do/, 2011 for more details

#+BEGIN_SRC js
var jsonStr = "[{name: 'Philp Fry', age: 1000, job: 'delivery boy'," + 
              " {name: 'Bender Rodriquez', age: 42, job: 'bending unit'}]";
var employeeRecords = eval(jsonStr);
#+END_SRC

** Additional Ruby =eval= methods

- instance_eval
- class_eval

#+BEGIN_SRC ruby
class Musician
  attr_accessor :name, :genre, :instrument
end

m = Musician.new
m.name = 'Norah Jones'
puts m.name
#+END_SRC

#+BEGIN_SRC ruby :results output
class Class
  def my_attr_accessor(*args)
    args.each do |prop|
      self.class_eval("def #{prop}; @#{prop}; end")
      self.class_eval("def #{prop}=(v); @#{prop} = v; end")
    end
  end
end
class Musician
  # my_attr_accessor not built into Ruby. We're going to define this
  my_attr_accessor :name, :genre, :instrument
end

m = Musician.new
m.name = 'Norah Jones'
puts m.name
#+END_SRC

#+RESULTS:

** Termination-Insensitive Non-Interference  :test:

- Termination-Insensitive means there could be some data loss that it's OK to lose.

** Explicit and Implicit Flows

** Denning-style Static Analysis

- Done first by a woman named Dorothy Denning

* <2015-11-16 Mon> Virtual Machine Lab
** Review of ANTLR

#+begin_src antlr
grammar Expr;

ID:      [a-zA-Z]+;
INT:     [0-9]+;
NEWLINE: '\r'? '\n';
WS:      [ \t]+ -> skip;
MUL:     '*';
DIV:     '/';
ADD:     '+';
SUB:     '-';

prog: stat+;        
stat: exp NEWLINE   #printExpr
    | NEWLINE       #blank
    | ID '=' expr NEWLINE #assign
    ;

expr: expr op=('*'|'/') expr  #MulDiv
    | expr op:('+'|'-') expr  #AddSub
    | INT #int
    | '(' expr ')' #parens 
    | ID #id
    ;
#+end_src

#+begin_src java
public class EvalVisitor extends ExprBaseVisitor<Integer> {

    Map<String, Integer> memory = new HashMap<String, Integer>();
    
    public Integer visitPrintExpr(ExprParser.PrintExprContext ctx) {
        int value = visit(ctx.expr());
        System.out.println(value);
        return 0;
    }

    public Integer visitAssign(ExprParser.AssignContext ctx) {
        String id = ctx.ID().getText();
        int value = visit(ctx.expr());
        memory.put(id, value);
        return value;
    }

    public Integer visitId(ExprParser.IdContext ctx) {
        String id = ctx.ID().getText();
        if (memory.contains(id)) return memory.get(id);
        return 0;
    }
}
#+end_src

Example run of the 
#+BEGIN_SRC sh
java -cp build:lib/antlr-4.4-complete.jar org.antlr.v4.runtime.misc.TestRig edu.sjsu.fwjs.parser.FeatherweightJavaScript prog -gui fwjsScripts/controlStructs.fwjs
#+END_SRC
** A review of compilers

1. source code
2. lexer/tokenizer -> tokens/lexemes
3. parsers -> ASTs
4. Compiler or interpreter
** Virtual Machines (VM)

- More typically, languagse are compiled to /bytecode/ for a virtual machine
- The MV is responsible for executing these instructions.
- In today's lab, we will implement a compiler and a stack-based VM for Scheme.
** Input program

#+begin_src scheme
(println (+ 2 3 4))
(println (- 13 (* 2 4)))
(println (- 10 4 3))
#+end_src

** Supported VM Operations

- PUSH :: takes one arg & adds it to the stack.
- PRINT :: pops top of the stack and prints it.
- ADD :: pops top two elements of the stack, adds them together, placing the result on the stack.
- SUB :: performs subtraction.
- MUL :: performs multiplicatqion.

* <2015-11-18 Wed> \LaTeX

** Lab review: method_missing

#+begin_src ruby
class Tree
  attr_accessor :value, :left, :right

  def initialize(value, left=nil, right=nil)
    @value = value
    @left = left
    @right = right
  end

  # TBD
end
my_tree = Tree.new(...)
my_tree.each_node do |v|
  puts v
end
arr = []
my_tree.each_node |v|
  arr.push(v)
end
#+end_src

#+begin_src ruby
class Tree
  def each_node(&block)
    @left.each_node(&block) if @left
    @yield @value
    @right.each_node(&block) if @right
  end

  def method_missing(m, *args)
    path = m.to_s.scan(/left|right/)
    get_node(path)
  end

  private
  def get_node(path)
    next_step = path.shift
    if !next_step then
      @value
    elsif next_step == 'left' then
      @left.get_node(path)
    else
      @right.get_node(path)
    end
  end
end
#+end_src

** LaTeX

** The Birth of TeX

- Knuth created TeX to precsely control the interface of his content.
- His work on TeX was also the inspiration for
- Leslie Lamport built-upon

** Interesting aspects of LaTeX

- LaTeX is a domain specific language (DSL)
  - That is, it is designed for a very specific use case.
- Provides separation of concerns
  - Details about formatting are (for the most part) kept separately from your actual content.
- It is the inspiration of literate programming.

** A minimal example

#+BEGIN_SRC latex
\documentclass{article}         % specific document type

\title{Hello World}

\begin{document}
\maketitle
\end{document}
#+END_SRC

** Sections & Labels

#+BEGIN_SRC latex
\documentclass{article}
\title{Hello, \LaTeX}
\author{Me, myself, and I}
\begin{document}
\maketitle

\section{This is Section 1} \label{sec:one}
This is my section, there are many like it, but this one is mine.
\subsection{Sub}
Some more text
\section{New Stuff}
As discussed in Section~\ref{sec:one}
\end{document}
#+END_SRC

* <2015-11-23 Mon> Rust

** Sample C code

#+BEGIN_SRC C 
#include <stdlib.h>
#include <stdio.h>

int* zero_out_negatives(int a[], int len) {
  int *res = malloc(sizeof(int) * len);
  //int res[len];
  for (int i=0; i<len; i++) {
    if (a[i] < 0) res[i] = 0;
    else res[i] = a[i];
  }
  return res;
}

int main(int argc, char** argv) {
  int nums[] = {0, 12, 5, -42, 9, 7, -18, 0};
  int len = 8;
  int *no_negs = zero_out_negatives(nums, len);
  for (int i=0; i<len; i++) {
    printf("%d ", no_negs[i]);
  }
  printf("\n");
  // Forgot free(no_negs);
}
#+END_SRC

** Rust

#+BEGIN_SRC rust
fn main() -> () {}
#+END_SRC

#+BEGIN_SRC rust
fn foo(x: i32) -> i32 {
    x + 3
}

fn main() {
    println!("Hello World");
    println!("{}", foo(4));
    let a = 1;
    let b = 2;
    let c = 3;

    println!("a:{} b:{} c:{}", a, b, c);
    
}
#+END_SRC

** Ownership and Borrowing

- Creating a variable grants ownership
- Asignment transfers ownership
- "Borrowing" allows a section of code to use a variable without taking ownership. At one time, you can have either
  - 1 mutable borrow, or
  - Limitless immutable borrows

** Complex Numbers

#+BEGIN_SRC rust
struct Complex { real: i32, imaginary: i32 }

fn main() {
    let cmplx1 = Complex { real: 7, imaginary: 2 };
    let cmplx2 = Complex { real: 3, imaginary: 1 };

    let mut ans = add_complex(&complx1, &complx2);

    println!("The answer is {}+{}i", ans.real, ans.imaginary);
}
#+END_SRC
* <2015-11-30 Mon> Sweet.js

** Invoking Sweet.js

- From a Unix/Dos command line:

#+BEGIN_SRC sh
sjs <sweet.js file> -o <js file>
#+END_SRC

Then you may run the output file normally:

#+BEGIN_SRC sh
node <js file>
#+END_SRC

** Rule macros

#+BEGIN_SRC js
macro <name> {
  rule {
    <pattern>
  } => {
    <template>
  }
}
#+END_SRC

Example:

#+BEGIN_SRC js
macro swap {
    rule { ($a, $b) } => {
        var tmp=$a; $a=$b; $b=tmp;
    }
}

var a = 10;
var b = 20;
swap (a,b)
#+END_SRC

** Translated Sweet.js code

#+BEGIN_SRC js
var a$1 = 10;
var b$2 = 20;

var tmp$3 = a$1;
a$1 = b$2;
b$2 = tmp$3;
#+END_SRC

** In-class example (rotate left)

#+BEGIN_SRC js
macro rotateLeft {
    rule { ($a, $b, $c) } => {
        var tmp = $a;
        $a = $b;
        $b = $c;
        $c = tmp;
    }
}

var a = 10;
var b = 20;
var c = 40

// prints a:10 b:20 c:40
console.log("a:" + a + " b:" + b + " c:" + c);

rotateLeft(a, b, c);

// prints a:20 b:40 c:10
console.log("a:" + a + " b:" + b + " c:" + c);
#+END_SRC

Macros can specify multiple rules.
They may also be defined recusively.

#+BEGIN_SRC js
macro m {
  rule { ($base) } => { [$base] }
  rule { ($head $tail ...) } => {
    [$head, m ($tail ...)]
  }
}
m (1 2 3 4 5)
#+END_SRC

#+BEGIN_SRC js
macro addNums {
  rule { $x ... } => {
    $x (+) ...
  }
}
console.log(addNums 1 2 3 4);
#+END_SRC

** Case macros

can be used to create anaphoric macros

** In-class macro example (Class macro)

#+BEGIN_SRC js
class Person {
    constructor(name) {
        this.name = name;
    }

    say(msg) {
        console.log(this.name + " says: " + msg);
    }
}

var bob = new Person("Bob");
bob.say("Macros are sweet!");
#+END_SRC

class macro
#+BEGIN_SRC js
macro class {
    rule {
        $className {
            constructor $cparams $cbody
            $($mname $mparams $mbody) ...
        }
    } => {
        function $className $cparams $cbody

        $($className.prototype.$mname
          = function $mname $mparams $mbody; ) ...
    }
}
#+END_SRC
* <2015-12-02 Wed> Java

Sorting a list of numbers.

#+BEGIN_SRC java
public static void sortNums (List lst) {
    for (int i = 0; i < lst.size() - 1; i++) {
        for (int j = 0; i < lst.size() - 1; j++) {
            if (((Integer) lst.get(j)).intValue() >
                ((Integer) lst.get(j + 1)).intValue()) {
                Integer tmp = (Integer) lst.get(j);
                lst.set(j, lst.get(j + 1));;
                lst.set(j + 1, tmp);
            }
        }
    }
}
#+END_SRC
** In-class example

#+BEGIN_SRC java
sort(lint, (Integer x, Integer y) -> {
        if (x % 2 == 0 && y % 2 == 0)
            return 0;
        else if (x % 2 == 0)
            return -1;
        else if (y % 2 == 0)
            return 1;
        else
            return 0;
    });
#+END_SRC


#+BEGIN_SRC java
Consumer<Integer> c = ((Integer x, Integer y) -> x - y);
Consumer<String> s = ((String s) -> System.out.println(s));
#+END_SRC

#+BEGIN_SRC java
public static void applyFunOnVar(Consumer<String> f, String s) {
    f.accept(s);
}
#+END_SRC

#+BEGIN_SRC java
applyFunOnVar((String s) -> System.out.println(s), "Hello");
applyFunOnVar(System.out::println); // method reference
#+END_SRC
* <2015-12-07 Mon> Final Review

** JSLint
- looks for common JS errors
** TypeScript
  - source-to-source compiler aka transpiler
  - Types :: catches errors at compile time.
    - tips for your IDE or other tools.
    - tips for compiler
    - enforced documentation
      - comments can lie, but code can't.
** Prolog
  - logic with facts, engine deduces results
  - declarative programming language. State what you want, not how you get it.
  - facts, variables, and rules
  - resolution: if subgoal matches the head of another rule, we replace it with body of the matching rule.
  - unification: instantiation of variables in using pattern matching.
  - =is= operator
  - cut operator (!)
    - green cut: optimization
    - red cut: changes results
    - Example: see [[batman_example]]

#+BEGIN_EXAMPLE
goal :- a, b, c, !, d, e, f.
       <-------- || <-------
#+END_EXAMPLE

#+BEGIN_SRC prolog
% examples

teacher(thomas_austin). % fact
runs_class(X) :- teacher(X). % rule
loves(Everybody, raymond). % rule

% rules have variables, facts do not.
#+END_SRC

#+BEGIN_SRC prolog
% Get a set out of a list
get_unique([],[]).
get_unique([H|Tail],Set) :-
    get_unique(Tail,TailSet),
    \+ member(H, TailSet),
    Set = [H|TailSet].
get_unique([H|Tail], Set) :-
    get_unique(Tail, TailSet),
    member(H, TailSet),
    Set = TailSet.
#+END_SRC

** ANTLR
- ANTLR :: compiler-compiler; parser generator
  - syntax (structure), not semantics (meaning)
  - context-free grammars (CFGs)
  - lexing: source -> tokens
  - parsing: tokens -> AST
** Parsers

- bottom-up :: LR parsers (L to R, Rmost deriv)
  - left-recursive grammars OK (e.g., E -> E + T
  - LALR (lookahead, LR)
    - aka shift-reduce parsers
- top-down :: LL parsers (L to R, Lmost deriv)
  - LL(k) grammars (lookahead k tokens)
  - LL(1) grammars - recursive descent parsers
    - they're important because they're fast and easy to write
- ANTLR is an LL parser
  - LL(*) arbitrary lookahead (ANTLR v1 to v3)
  - ALL(*) - adaptive LL(*)
    - support left recursive (LR) grammars
** Ruby

- influences
  - Perl
    - regex, taint analysis, naming
  - Smalltalk
    - everything is an object
    - blocks
    - method_missing
- Rails
  - web framework, following convention over configuration.
  - Ruby's "killer app"
- eval
  - runs a string as code
  - instance_eval/class_eval
- taint analysis (integrity)
  - $SAFE
    - 0 (default)
    - 1 (no eval on tainted data)
    - 3 new stuff is tainted
  - info flow analysis (confidentiality)
    - noninteference :: prior inputs don't affect prior outputs
    - secure multi-execution :: running a prog mult times w/ different security levels.

#+BEGIN_SRC ruby
def with_prob(prob)
  yield if (Random.rand < prob)
end

def foo
  with_prob 0.5 do
    puts 'made it'
    return 0
  end
  return 1
end
#+END_SRC

#+BEGIN_SRC js
function withProb(p, f) {
  if (Math.random() < p)
    return f();
}

function foo() {
  withProb(0.5, function() {
    console.log("made it!");
    return 0;
  });
  return 1;
}
#+END_SRC
** Virtual Machines
- portability
** LaTeX
- DSL for typesetting
- \section
- \label
- \ref
- \cite (for BibTeX)
*** BibTeX

- takes care of formatting for references
** Inform 7

- DSL for text adv/interactive storytelling
- declarative
- logical
- natural language (it looks like English)
** Rust

- low-level system lang (kill C++)
- memory safe (no gc or runtime)
- reasonable build times
- concurrency
** Sweet.js

- macros for JS
- rule macros
- case macros
- allow you to break hygiene
** Java 8 lambdas/closures
** Nashorn

- JS engine in Java
- put: add a val to JS env.
- eval(jsCode) executes JS code and returns result.
  - we have to understand what that return value is and cast appropriately.

#+begin_src java
// lambda
File myFile = new File(dirName);
File[] dirs = myFile.listFiles((File f) -> f.isDirectory());

// method reference
File[] dirs = myFile.listFiles(f::isDirectory);
#+end_src
