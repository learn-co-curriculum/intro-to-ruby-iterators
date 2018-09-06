# Intro To Ruby Iterators

## Objectives

1. Understand the difference between looping and iterating.
2. Learn how to pass a block to an iterator.
3. `do;end` block syntax.
4. `{ }` block syntax.
5. Capture a value yielded to the block by the iterator in `| |` (pipes).
6. Use a captured yield value within the iterator's block.

## Looping vs. Iteration

Looping is a programming construct that allows you to tell your program to do something a certain number of times, or until a certain condition is met. It is a mechanism to repeat a selection of code.

Iteration, on the other hand, is a way to operate on a collection object, like an array, and do something with each element in that collection.

Let's say that we are writing a program to annoy our little brother. We don't want to annoy him *too much* though, or else we might get grounded. So, our program, when it runs, will `#puts` "Stop hitting yourself!" seven times, and then stop. For a task like this, in which we need to perform a task a certain, discrete number of times, we would use a loop.

Let's take a look:

```ruby
7.times do
  puts "Stop hitting yourself!"
end
```

What if we want to output the phrase only *until* our little brother calls out "Mommmm!!"? We can stick with a loop construct like `while`:

```ruby
input = ""
while input != "Mommmm!!"
  puts "Stop hitting yourself!"
  input = gets.chomp
end
```

However, what if we have three little brothers: Tom, Tim and Jim, and we want to output "Stop hitting yourself, #{little brother's name}!" once for each brother? Let's try that out using a loop with the `while` construct:

```ruby
brothers = ["Tom", "Tim", "Jim"]

count = 0
while count <= brothers.length-1
  puts "Stop hitting yourself #{brothers[count]}!"
  count += 1
end
```

In order to output a simple phrase using each brother's name from our collection with a `while` loop we need to:

1. Establish a counter
2. Set the condition for the `while` loop
3. Read data out of the array by index using the counter
4. Increment the counter at the bottom of the loop

That's a lot of code to accomplish such a simple task. In fact, a loop isn't a good tool for this job. Since we are now operating on a collection of data and seeking to *do something* with each element of that collection, we want to use an **iterator**.

Iterators are methods that you can call on a collection, like an array, to loop over each member of that collection and do something to or with that member of the collection. Let's take a look in the next section.

## Using `#each`

The `#each` method is a prime example of an iterator. Here's a boilerplate example of its usage:

```ruby
primary_colors = ["Red", "Yellow", "Blue"]
primary_colors.each do |color|
  puts "Primary Color #{color} is #{color.length} letters long."
end
```

`#each` is called on the collection `primary_colors`, which is an array containing 3 individual strings.

A block is passed to `#each`, opened by the code that starts with `do` and closed by the `end`. Every `do` needs a closing `end`.

```ruby
primary_colors = ["Red", "Yellow", "Blue"]
primary_colors.each do |color| # do begins a block
  # the lines between the do/end are the block's body
  puts "Primary Color #{color} is #{color.length} letters long."
end # end terminates the block
```

The output from this code is:

```
Primary Color Red is 3 letters long.
Primary Color Yellow is 6 letters long.
Primary Color Blue is 4 letters long.
```

We can see that the block passed to `each` is executed once for each element in the original collection. If there were 5 colors in `primary_colors`, the block would have run 5 times. We call each run, each execution, of the block passed to the iterator (`#each` in this case), an iteration. It's a word used to refer to each 'step', or each 'execution', of a block. An iteration is the singular execution of a sequence of code (that we call a block) within a loop.

When we iterate over a collection of elements using `#each` (and also in other iterators and enumerables we'll soon learn about), the iterator `#each` yields each element one at a time to every iteration via a variable declared with the opening of the block.

After the opening `do` of our code above, we see `|color|`. `|` is called a pipe. After `do`, we declare a local variable `color` by enclosing it in `| |` pipes. This variable's value is automatically assigned the element from the array for the current iteration. So on the first iteration of the `each` above, the variable `color` would be equal to `"Red"`. But on the next iteration of the block, `color` will be reassigned the value of the next element in the `primary_colors` array, `"Yellow"`.

Let's take a closer look at some of these concepts.

### What is a block?

A block is a chunk of code between braces, `{ }` or between `do`/`end` keywords that you can pass to a method almost exactly like you can pass an argument to a method. There are some methods, like iterator methods, that can be called *with a block*, i.e. accompanied by a block denoted with `{ }` or `do`/`end`. Such a method would run and pass, or yield, data to the code in the block for that code to operate on or do something with.

Blocks are part of what make the Ruby language special, powerful, and loved.

### What are the `| |`?

Those are called "pipes". When invoking an iterator like `#each`, the variable name inside the pipes acts as an argument that is being passed into the block. The iterator will pass, or yield, each element of the collection on which it is called to the block. Each element, as it gets passed into the block, will be equal to the variable name inside the pipes. Think of it like this:

* Call, or run, the code in the block once for each element of the collection.
* Pass a single element of the collection into the block every time the code in the block is called, or run. Start with the first element in the collection, and then move on to the second element, then the third, etc.
* Every time you call the code in the block and pass in an element from the collection, set the variable name from between the pipes equal to that element.

This is exactly what happens when you define a method to accept an argument and then call that method with a real argument:

```ruby
def hi_there(name)
  puts "Hi, #{name}"
end

hi_there("Sophie") # > "Hi, Sophie"
# => nil 
```

Think of the variable between the pipes like the `name` variable we are using to define our argument.

The variable name inside the pipes is more or less arbitrary. For example:

```ruby
brothers = ["Tim", "Tom", "Jim"]
brothers.each do |brother|
  puts "Stop hitting yourself #{brother}!"
end
```

Will output the same thing as:

```ruby
brothers = ["Tim", "Tom", "Jim"]
brothers.each do |hippo|
  puts "Stop hitting yourself #{hippo}!"
end
```

Which is:

```ruby
Stop hitting yourself Tim!
Stop hitting yourself Tom!
Stop hitting yourself Jim!
```

We should, however, be reasonable and sensical when we name our variables. If your collection is called `brothers`, name the variable between the pipes `brother`. If your collection is called `apples`, name your variable `apple`.

### A Closer Look

Let's revisit our example from above and break it down, step by step:

```ruby
brothers = ["Tim", "Tom", "Jim"]
brothers.each do |brother|
  puts "Stop hitting yourself #{brother}!"
end
```

Here, the `#each` method takes each element of the `brothers` array, one at a time, and passes, or **yields**, it into the block of code between the `do`/`end` keywords. It makes each element of the array available to the block by assigning it to the variable `brother`. It does so by placing that variable name in between the pipes `| |`.

In summary, `#each` yields each item of the collection on which it is called to the block with which it is called. It keeps track of which element of the collection it is manipulating as it moves through the collection. During the first step of the iteration, `#each` will yield the first array element to the block. At that point in time, inside the block, `brother` will equal "Tim". During the second step of the iteration, `brother` will equal "Tom" and so on.

Iterators like `#each` are smart – they don't need a separate counter variable and manual incrementation of that variable to know how many times to do something. They use the number of items in the collection on which they are called to determine how many times they will do something.

Let's set a `counter` variable and manually increment it in order to see the `#each` method in action:

```ruby
brothers = ["Tim", "Tom", "Jim"]
counter = 1
brothers.each do |brother|
  puts "This is loop number #{counter}"
  puts "Stop hitting yourself #{brother}!"
  counter += 1
end
```

Copy and paste the above code into IRB. You should see this output:

```bash
This is loop number 1
Stop hitting yourself Tim!
This is loop number 2
Stop hitting yourself Tom!
This is loop number 3
Stop hitting yourself Jim!
#=> ["Tim", "Tom", "Jim"]
```

See that, during loop number 1, the string "Tim" was yielded to the block and the variable name `brother`, when interpolated into the string we `#puts`ed out, was set equal to "Tim". During loop number 2, the same thing happened with "Tom", and during loop number 3, the same thing happened with "Jim". There was no loop number four because the `#each` iterator operated on each member of the array on which it was called and then stopped.

### A Note on Return Values

Different iterators have different return values. Notice that the return value of the call to `#each` above returned `["Tim", "Tom", "Jim"]` – the original array. The `#each` method will always return the original collection on which it was called.

### The `{ }` Syntax

Another way of establishing a code block that you may encounter is to use curly brackets, `{ }`, instead of the `do`/`end` keywords. Let's take a look:

```ruby
brothers = ["Tim", "Tom", "Jim"]
brothers.each{|brother| puts "Stop hitting yourself #{brother}!"}
```

It is appropriate to use the `{ }` syntax when the code in the block is short and can fit on one line.

## Conclusion

Both loops and iterators are powerful tools in Ruby, but they're not right for every job. Loops are useful when you need to tell your program to do something a certain number of times or to do something based on a certain condition. Iterators are useful for operating on a collection of objects, and even performing complex operations on the members of that collection. Because iterators are called with blocks, it's easy to carry out complex logic or tasks using each individual member of a collection of objects.



<p class='util--hide'>View <a href='https://learn.co/lessons/intro-to-ruby-iterators'>Intro to Ruby Iterators</a> on Learn.co and start learning to code for free.</p>
