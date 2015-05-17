;; gorilla-repl.fileformat = 1

;; **
;;; 
;; **

;; **
;;; # Clojure as a functional language
;;; 
;;; Clojure is a functional language. Functions are first class citizens just like
;;; any other value type you have already encountered (ints/floats/objects etc.).
;;; This means that we can pass functions as arguments to other functions, returning
;;; functions from functions, and even manipulating functions.
;;; 
;;; # Syntax
;;; 
;;; Compared to other languages, Clojure does function calls slightly differently than one
;;; might expect. To give a quick example:
;;; 
;;; ```java
;;; myFunction("foo", "bar")
;;; ```
;;; 
;;; In Clojure this might look like the following:
;;; 
;;; ```clojure
;;; (my-function "foo" "bar")
;;; ```
;;; 
;;; There are a couple differences here, first thing to note is that we are using a different
;;; case style. In Clojure we don't use camel case but instead lisp case which is dash seperated.
;;; 
;;; Another thing is that we've placed the function inside the parenthesis. In Clojure a function
;;; call is always an open paren followed by the function we want to call, followed by any arguments
;;; we are passing to the function, followed by a closing paren.
;;; 
;;; Lastly another thing is that we aren't separating the arguments with a comma anymore. In Clojure
;;; commas are simply treated as whitespace so we don't need them.
;;; 
;;; ## Common types
;;; 
;;; Clojure mostly just uses Java classes under the hood. Lets examine some common types using
;;; the `type` function.
;; **

;; @@
(type 2015)
;; @@

;; @@
(type 3.14)
;; @@

;; @@
(type "clojure is cool")
;; @@

;; @@
(type true)
;; @@

;; **
;;; ## Common data structures
;;; 
;;; When working in Clojure you'll encounter 3 common types of data structures.
;;; 
;;; ### Lists
;;; 
;;; A list in Clojure is just a linked list. There are a couple of ways we can create them, the
;;; first one being the `list` function:
;; **

;; @@
(list)
;; @@

;; **
;;; 
;;; This gave us an empty list, but we can also supply it with a variable number of arguments to
;;; initialize the list with:
;; **

;; @@
(list 1 2 3)
;; @@

;; **
;;; We can efficiently add to the beginning of a linked list using the `conj` function:
;; **

;; @@
(conj (list 1 2 3) 0)
;; @@

;; **
;;; Accessing the head or tail of the list is possible using the `first` and `rest` functions.
;;; We can also access a specific element using the `nth` function.
;; **

;; @@
(first (list 1 2 3))
;; @@

;; @@
(rest (list 1 2 3))
;; @@

;; @@
(nth (list 1 2 3) 2)
;; @@

;; **
;;; Finally Clojure has a shorthand for defining lists, instead of calling the list function we can
;;; use the following:
;; **

;; @@
'(1 2 3)
;; @@

;; @@
'()
;; @@

;; **
;;; ### Vectors
;;; 
;;; A vector in Clojure is a lot like an array in other languages. That means we can efficiently access
;;; the index of a vector compared to a list, but adding elements might be a bit more expensive than on
;;; a list.
;;; 
;;; To create a vector we can use the vector function:
;; **

;; @@
(vector 1 2 3)
;; @@

;; @@
(vector)
;; @@

;; **
;;; We can perform a lot of the same operations on vectors as we can on lists:
;; **

;; @@
(first (vector 1 2 3))
;; @@

;; @@
(rest (vector 1 2 3))
;; @@

;; @@
(nth (vector 1 2 3) 0)
;; @@

;; **
;;; Lets try using the `conj` function from earlier:
;; **

;; @@
(conj (vector 1 2 3) 0)
;; @@

;; **
;;; 
;;; Wait, this isn't what we expect. With a list using `conj` added the new element to the front,
;;; not to the back. This is because the `conj` function adds elements where it is most efficient
;;; for the datastructure.
;;; 
;;; In a list it is cheapest to add an element to the front, we don't
;;; need to traverse the whole list to get to the end to add the element.
;;; 
;;; In a vector it is cheapest to add an element to the end. Because vectors are arrays if we were to
;;; add to the front, we would need to move every element that was already there down a slot to make
;;; room for the new element.
;;; 
;;; Finally like lists vectors have a shorthand way to create them quickly:
;; **

;; @@
[1 2 3]
;; @@

;; @@
[]
;; @@

;; **
;;; 
;;; ### Hash Maps
;;; 
;;; Hashmaps are just like maps or dictionaries in other languages. They store key/value pairs
;;; and we can efficiently access values in the hash-map by their keys.
;;; 
;;; We can create hash-maps with the `hash-map` function. This function takes an even number
;;; of arguments, which is of the format key followed by pair.
;;; 
;; **

;; @@
(hash-map "blue" 30 "red" 100)
;; @@

;; **
;;; So this will create a map, with keys "blue" and "red" each with an integer as its value.
;;; 
;;; To access values from the keys we can use the `get` function which takes a map as its first
;;; argument and the key for the value we want to retrieve as the second.
;; **

;; @@
(get (hash-map "blue" 30 "red" 100) "blue")
;; @@

;; **
;;; We can also build a new map from an old one using the `assoc` function:
;; **

;; @@
(assoc (hash-map "blue" 30 "red" 100) "green" 20)
;; @@

;; **
;;; Finally we have a shorthand to build maps, we don't need to use the hash-map function.
;; **

;; @@
{"red" 100 "blue" 30}
;; @@

;; **
;;; 
;;; # Saving values into names
;;; 
;;; So far we've just been nesting our function calls, what if we wanted to be able to store
;;; the result of a call into a name so we can easily access it? We can use the `def` function
;;; for this.
;; **

;; @@
(def PI 3.14)
PI
;; @@

;; @@
(def some-list (list 1 2 3 4))
some-list
;; @@

;; **
;;; 
;;; # Defining your own functions
;;; 
;;; So far we've seen how we can use builtin functions, but how do we define our own?
;;; 
;;; We can use the `fn` function to create new functions, we can use the `def` function from the
;;; previous section to associate the new function with a name. Lets write a simple square function.
;;; 
;;; `fn` takes two arguments, the first argument is a vector of the arguments and the second is the
;;; operation we want to perform.
;;; 
;; **

;; @@
(def square (fn [number] (* number number)))
(square 5)
;; @@

;; **
;;; Notice that we didn't need to specify a return statement like we often have to do in other languages.
;;; In Clojure a function call will return the last expression it evaluates.
;;; 
;;; We can also use a shorthand to combine `def` and `fn` which is `defn`.
;; **

;; @@
(defn square [number] (* number number))
;; @@

;; **
;;; 
;;; # Thinking functionally
;;; 
;;; TODO
;;; 
;;; ## Recursion
;;; 
;;; TODO
;;; 
;;; ## Higher-order functions
;;; 
;;; A higher-order function is one that either takes a function as input or returns a function as
;;; output. In Clojure we can treat functions as values just like any other type. This is one
;;; of the really powerful things of functional programming in general.
;;; 
;;; To explore this further we'll go over 3 built-in higher order functions that are integral
;;; to a functional programmers toolbox.
;;; 
;;; ### Map
;;; 
;;; `map` is a function that takes a function as its first argument and a sequence as its second
;;; and applies the function to each element building a new list from that.
;; **

;; @@
(map square [1 2 3])
;; @@

;; @@
(map square '(1 2 3))
;; @@

;; **
;;; 
;;; So we passed square as a value, and either a list or a vector and got a new list built.
;;; 
;;; ### Filter
;;; 
;;; `filter` works like `map` in the sense that it also takes a function as its first argument
;;; and a sequence as its second. The function passed to `filter` must return a boolean.
;;; Like `map` we will walk through each element of the sequence and apply the function to it,
;;; if the function we passed in evaluates to `true` on the element we will keep it, if it's `false`
;;; we will drop the element from the sequence.
;;; 
;;; Lets use a builtin function `even?` which returns `true` if the number we pass it is even, otherwise
;;; `false`.
;;; 
;; **

;; @@
(even? 3)
;; @@

;; @@
(even? 2)
;; @@

;; **
;;; Now lets pass `even?` to `filter`.
;; **

;; @@
(filter even? [1 2 3 4 5 6])
;; @@

;; **
;;; 
;;; ### Reduce
;;; 
;;; `reduce` also takes two arguments, the first one is a function and the second is a sequence just
;;; like `map` and `filter`. The function we pass in must take two arguments instead of one.
;;; 
;;; `reduce` applies the function to the first element of the sequence and the second. It then
;;; applies the result of the first call and applies it to the third element of the sequence.
;;; It then walks through the rest of the sequence.
;;; 
;;; A quick example is we can reduce `+` over a sequence of numbers to add them all together.
;;; 
;; **

;; @@
(reduce + [1 2 3 4])
;; @@
