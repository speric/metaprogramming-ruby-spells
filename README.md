Metaprogramming Spells
===========
All the spells from <a href="https://twitter.com/nusco">Paolo Perrotta's</a> <i><a href="http://pragprog.com/book/ppmetr/metaprogramming-ruby">Metaprogramming Ruby</a></i>.
  * <a href="#argument-array">Argument Array</a>
  * <a href="#around-alias">Around Alias</a>
  * <a href="#blank-slate">Blank Slate</a>
  * <a href="#class-extension">Class Extension</a>
  * <a href="#class-extension-mixin">Class Extension Mixin</a>
  * <a href="#class-instance-variable">Class Instance Variable</a>

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
```
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
### Class Instance Variable
Store class-level state in an instance variable of the `Class` object.
```ruby
Class C
  @my_class_instance_variable = "some value"
  
  def self.class_attribute
    @my_class_instance_variable
  end
end

C.class_attribute #=> "some value"
```
