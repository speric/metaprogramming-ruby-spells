Metaprogramming Spells
===========
All the spells from <a href="https://twitter.com/nusco">Paolo Perrotta's</a> <i><a href="http://pragprog.com/book/ppmetr/metaprogramming-ruby">Metaprogramming Ruby</a></i>.

<a name="argument-array"></a>
### Argument Array
Collapse a list of arguments into an array.
```ruby
def my_method(*args)
  args.map { |arg| arg.reverse }
end

my_method('abc', 'xyz', '123') #=> ["cba", "zyx", "321"]
```
<a name="around-alias"></a>
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
```
<a name="blank-slate"></a>
### Blank Slate
Remove methods from an object to turn them into <a href="#ghost-methods">Ghost Methods</a>.
```ruby
class C
  def method_missing(name, *args)
    "a Ghost Method"
  end
end

obj = C.new
obj.to_s #=> "#<C:0x357258>"

class C
  instance_methods.each do |m|
    undef_method m unless m.to_s =~ /method_missing|respond_to?|^__/
  end
end

obj.to_s #=> "a Ghost Method"
```
