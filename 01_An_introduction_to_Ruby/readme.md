# 01 An introduction to Ruby

## Overview

- setting up 
- IDE and tools
- Language features
- standard libraries

## 10,000 foot View of Ruby

- Focused on helping developers be productive and happy
- Thoroughly object oriented 全面物件導向
- Dynamic typing and [duck typing](https://zh.wikipedia.org/wiki/%E9%B8%AD%E5%AD%90%E7%B1%BB%E5%9E%8B)
  可以指派不同類型的值給一個變數
- Multi paradigm (擁有支援非常多樣程式語言的特性，這種形式我們稱之多範式)
- Reflection
- Metaprogramming
  開發者本身可以在原有語言上輕鬆構造、擴充專屬功能，
  甚至可發展出一套具備特有語法的框架、程式庫或工具，
  就擴充的語法本身已成為特定領域語言（Domain specific language, DSL）；
  有些語言則提供有限的動態能力，讓語言使用者維持一致的交流方式，避免產生溝通與維護上的問題。
- Bytecode interpreted


## Installing Ruby

rvm.io -> install RVM and lastest version of ruby

check ruby version
```
$ ruby -v
```

## Interactive Shell Demo

IRB(interactive Ruby Shell)

- Great for experimentation and exploration
- Allows arbitrary Ruby code
- Use up arrow to go through history
- _ stores the result of the last evaluated expression

```
$ irb
2.3.0 :001 > count = 10
 => 10 
2.3.0 :002 > count.class
 => Fixnum 
2.3.0 :003 > String.public_methods.sort
 => [:!, :!=, :!~, :<, :<=, :<=>, :==, :===, :=~, :>, :>=, :__id__, :__send__, :allocate, :ancestors, :autoload, :autoload?, :class, :class_eval, :class_exec, :class_variable_defined?, :class_variable_get, :class_variable_set, :class_variables, :clone, :const_defined?, :const_get, :const_missing, :const_set, :constants, :define_singleton_method, :deprecate_constant, :display, :dup, :enum_for, :eql?, :equal?, :extend, :freeze, :frozen?, :hash, :include, :include?, :included_modules, :inspect, :instance_eval, :instance_exec, :instance_method, :instance_methods, :instance_of?, :instance_variable_defined?, :instance_variable_get, :instance_variable_set, :instance_variables, :is_a?, :itself, :kind_of?, :method, :method_defined?, :methods, :module_eval, :module_exec, :name, :new, :nil?, :object_id, :prepend, :private_class_method, :private_constant, :private_instance_methods, :private_method_defined?, :private_methods, :protected_instance_methods, :protected_method_defined?, :protected_methods, :public_class_method, :public_constant, :public_instance_method, :public_instance_methods, :public_method, :public_method_defined?, :public_methods, :public_send, :remove_class_variable, :remove_instance_variable, :respond_to?, :send, :singleton_class, :singleton_class?, :singleton_method, :singleton_methods, :superclass, :taint, :tainted?, :tap, :to_enum, :to_s, :trust, :try_convert, :untaint, :untrust, :untrusted?] 
2.3.0 :004 > String.superclass
 => Object
2.3.0 :005 > String.superclass.superclass
 => BasicObject
2.3.0 :006 > def double(val); val * 2; end
 => :double 
2.3.0 :007 > double(10)
 => 20 
2.3.0 :008 > double("abc")
 => "abcabc" 
2.3.0 :009 > double([1, 2, 3])
 => [1, 2, 3, 1, 2, 3]
2.3.0 :010 > double(_)
 => [1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3]
2.3.0 :011 > exit
```

## IDE Options and RubyMine Demo

IDE Options:
- RubyMine
- Aptana Studio
- No IDE - text editor

## Variables,nil,Methods and Scope

### Variables, nil

定義多字變數使用下底線： my_var

```
$ irb
2.3.0 :001 > result = nil
 => nil 
2.3.0 :002 > nil.class
 => NilClass 
2.3.0 :003 > result.nil?
 => true 
2.3.0 :004 > a = " abc "
 => " abc " 
2.3.0 :005 > a.strip
 => "abc" 
2.3.0 :006 > a
 => " abc " 
2.3.0 :007 > a.strip!
 => "abc" 
2.3.0 :008 > a
 => "abc" 
```

### Methods

```
def double(val)
  val * 2
end

#def double(val); val * 2; end

double("abc")
```

### Scope

```
lander_count = 10

def report
  lander_count = 5 
  puts lander_count
end

report

puts lander_count

# 5
# 10
```

define global variable with $ (not recommended)

```
$log_level = "debug"
```

## Flow Control,Operators,Comments

```
lander_count = 11

message = if lander_count > 10
  "Launching probe"
else
  "waiting for probes to return"
end

puts message
```

### Operators

Comparison operators:

```
>
>=
<
<=
==
!=
```

Mathematical operators:

```
+
-
*
/
%
**(exponentiation指數函數)
```

Logical operators:

```
!
&&
||
not
and
or
```

Bitwise integer operators:

```
&
|
^
~
<<
>>
```

Assignment operators:
```
+=
-=
*=
/=
%=
**=
&=
|=
^=
>>=
<<=
```

## Some Useful Methods

```
print "Enter your name:"
name = gets # input
puts "Your name is " + name
```

```
puts "Using backticks:"
res = `time /t`
puts res

puts "Using system:"
res = system "time /t"
puts res
```

ruby use 2 spaces indent