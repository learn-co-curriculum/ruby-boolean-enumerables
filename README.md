# Ruby Boolean Enumerables

## Objectives

1. Understand return values for enumerators.
2. Use true/false expressions within a block.
3. Use the `#all?` enumerator to create a true/false return value.
4. Use the `#none?` enumerator to create a true/false return value.
5. Use the `#any?` enumerator to create a true/false return value.
6. Use the `#include?` enumerator to create a true/false return value.

## Overview

When we are iterating over objects in a collection like with `#each` we generally don't care about the return values.

```ruby
["Red", "Yellow", "Blue"].each do |color|
  puts "There are #{color.length} letters in #{color}"
end #=> ["Red", "Yellow", "Blue"]
```

If you run this code in IRB, you'll see:

```
001:0 > ["Red", "Yellow", "Blue"].each do |color|
?>        puts "There are #{color.length} letters in #{color}"
003:1 > end

There are 3 letters in Red
There are 6 letters in Yellow
There are 4 letters in Blue
=> ["Red", "Yellow", "Blue"]
```

You can see the block did what we intended it to do, it printed our output. You'll notice the last line also indicates that the `#each` method also returned a value. All expressions in ruby must return a value. When you use `#each` on a collection, the return value is always the original collection. Nothing you do inside the block you pass `#each` will ever change the return value. But that's not always the case. With other enumerator methods, the return value of the method is very much dependent on the block.

## `#all?`

Imagine wanting to know if all the numbers in an array are odd. You could use each with something like:

```ruby
all_odd = true
[1,2,3].each do |number|
  if number.even? # Will evaluate to false for 1, true for 2, false for 3
    all_odd = false
  end
end
all_odd #=> false
```

That works, the end value of `all_odd` will be false because `2`  flipped the `all_odd` variable to false. However, something so simple - checking if all the elements in this array are odd - isn't being expressed clearly. Worse than our code not expressing our intention is that our code requires us to maintain variable state, `all_odd`, which can easily lead to errors (say if some other piece of code accidentally changes that variable value).

Consider the following example using `#all?`:

```ruby
all_odd = [1,3].all? do |number|
  number.odd? # Will evaluate to true for 1, true for 3
end #=> true
all_odd #=> true
```

The rule for the `#all?` enumerator is that the block passed to it must return `true` for every iteration for the entire `#all?` expression or method to return `true`. If we introduce an even number to the collection, the return value will change.

```ruby
all_odd = [1,2,3].all? do |number|
  number.odd? # Will evaluate to true for 1, false for 2, true for 3
end #=> false
all_odd #=> false
```

That's the rule for `#all?` - every iteration, every loop of the block must return `true`. When the block encounters the value `2` for `number`, it will run the expression `2.odd?` which will return `false`. Because there was at least one iteration of the block that had a `false` return value, the entire `#all?` expression returns `false`.

## `#none?`

Imagine the opposite of `#all?`, a method `#none?`, where we are interested in none of the elements in a collection producing a true expression within the block passed to `#none?`.

```ruby
[1,3].none?{|i| i.even?} #=> true
```

The entire expression `#none?` returns true because none of those numbers will produce a `true` expression when asked if they are even within the block. Compare the code above to the code required to test that condition using `#each`.

```ruby
none_even = true
[1,3].each do |i|
  if i.even?
    none_even = false
  end
end #=> [1,3] because `#each` always returns the original collection
none_even #=> true
```

These high-level boolean enumerators like `#all?` and `#none?` are way cleaner for evaluating elements in a collection for `true`/`false` conditions.

The way `#none?` works is that no iteration of the block passed to `#none?` can create a `true` expression.

## `#any?`

Sometimes you want to be a bit more forgiving than `#all?` or `#none?` and just ensure that at least one element in a collection will create a `true` expression within the block passed. `#any?` is perfect for this. The `#any?` enumerator will return true if at least one iteration of the block evaluates to true, but false if none of them do.

```ruby
[1,2,100].any?{|i| i > 99} #=> true
```

The `#any?` expression above will return `true` because at least one element, `100`, will produce a `true` evaluation in the block.

## `#include?`

Whereas `#any?` is useful for evaluating the truthiness of the logic of a block, `#include?` is helpful if you'd like to merely compare actual contents of a known value.

`#include?` will return `true` if the given object exists in the element. If it doesn't find a match, it will return `false`.

```ruby
the_numbers = [4,8,15,16,23,42]
the_numbers.include?(42)   #=> true
the_numbers.include?(6)   #=> false
```

The `#include?` expression first returns `true` because `the_numbers[5] == 42`. When it is run with `6`, it will evaluate to `false` since that item is not present in the array.

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/ruby-boolean-enumerables' title='Ruby Boolean Enumerables'>Ruby Boolean Enumerables</a> on Learn.co and start learning to code for free.</p>
