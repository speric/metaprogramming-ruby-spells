Metaprogramming Spells
===========
All the spells from <a href="https://twitter.com/nusco">Paolo Perrotta's</a> <i><a href="http://pragprog.com/book/ppmetr/metaprogramming-ruby">Metaprogramming Ruby</a></i>.
  * <a href="#argument-array">Argument Array</a>
  * <a href="#around-alias">Around Alias</a>
  * <a href="#blank-slate">Blank Slate</a>
  * <a href="#class-extension">Class Extension</a>
  * <a href="#class-extension-mixin">Class Extension Mixin</a>
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
<a name="class-extension"></a>
### Class Extension
Define class methods by mixing a module into a class's eigenclass (a special case of <a href="#object-extension">Object Extension</a>).
```ruby
class C; end

module M
  def my_method
    'a class method'
  end
end

class << C
  include M
end

C.my_method #=> "a class method"
```

<a name="class-extension-mixin"></a>
### Class Extension Mixin
Enable a module to extend it's includer through a <a href="#hook-method">Hook Method</a>.
```ruby
module M
  def self.included(base)
    base.extend(ClassMethods)
  end
  
  module ClassMethods
    def my_method
      'a class method'
    end
  end
end

class C
  include M
end

C.my_method #=> "a class method"
```
