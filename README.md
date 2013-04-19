Metaprogramming Spells
===========
All the spells from <a href="https://twitter.com/nusco">Paolo Perrotta's</a> <i><a href="http://pragprog.com/book/ppmetr/metaprogramming-ruby">Metaprogramming Ruby</a></i>.

### Argument Array
Collapse a list of arguments into an array.
```ruby
def my_method(*args)
  args.map { |arg| arg.reverse }
end

my_method('abc', 'xyz', '123') #=> ["cba", "zyx", "321"]
```

### Around Alias
Call the previous, aliased version of a method from a redeﬁned method.
```ruby
class String
  alias :old_reverse :reverse

  def reverse
    "x#{old_reverse}x"
  end
end

"abc".reverse #=> "xcbax"
