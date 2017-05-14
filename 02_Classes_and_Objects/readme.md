# Classes and Objects

## Overview

- Create classes and instantiate objects
- Add instance variables and methods to your classes
- Control the visibility of these variables and methods
- Set initail state of the objects
- Create class variables and methods
- Leverage inheritance to re-use functionality between classes
- self, current context, executable class bodies and object equality


## Creating Classes and Objects

```
class Spaceship
end
```
- Classes names start with a capital letter and use CamelCase
- Capitalize abbreviations: XMLParser, JSONRequest

```
ship = Spaceship.new
```

use new to instantiate an object of class

### objects vs variables

```
$ irb
2.3.0 :001 > a = "abc"
 => "abc" 
2.3.0 :002 > b = a # will refer to same object
 => "abc" 
2.3.0 :003 > a.upcase!
 => "ABC" 
2.3.0 :004 > a
 => "ABC" 
2.3.0 :005 > b
 => "ABC" 
2.3.0 :006 > a.object_id
 => 16007600 
2.3.0 :007 > b.object_id
 => 16007600 
2.3.0 :008 > b = a.clone # create a copy object
 => "ABC" 
2.3.0 :009 > b.downcase!
 => "abc" 
2.3.0 :010 > a
 => "ABC" 
```

## Instance Variables and Methods

```
class spaceship
  def launch(destination)
    @destination = destination
    # go towards destination
  end
end
```

- def;end to define a method
- instance variables start with @

```
class Spaceship
  def launch(destination)
    @destination = destination
  end
end

ship = Spaceship.new
ship.launch("Earth")
# puts ship.inspect
p ship

#<Spaceship:0x00000002293630 @destination="Earth">
```

```
puts ship.destination
# undefined method `destination' for #<Spaceship:0x000000017e7470 @destination="Earth"> (NoMethodError)
```

[sample file](./instance_vars.rb)

variables in ruby are not visible outside the class. But instance methods are public bt default

```
def destination
  @destination
end
# makes variables visible
```

### Summary

- inspect and p methods allow you to take a look inside objects
- instance variables are private while methods are public by default

## Accessors and Virtual Attributes

### Accessors

```
def Spaceship
  attr_accessor :destination
  attr_reader :name
  attr_writer :name
end

ship = Spaceship.new 
ship.name = "Dreadnought"
puts ship.name
```

```
def Spaceship
  attr_accessor :destination, :name
  
  def cancel_launch
    destination = "" # creates local variable
    self.destination = ""
  end
end
```

```
def Spaceship
  # attr_accessor :destination
  def destination
    @destination
  end
  
  def destination=(new_destination)
    @destination = new_destination
  end
end
```

### Virtual Attributes

```
def Spaceship
  # attr_accessor :destination
  def destination
    @autopilot.destination
  end
  
  def destination=(new_destination)
    @autopilot.destination = new_destination
  end
end

ship = Spaceship.new
ship.destination = "Earth"
puts ship.destination # outputs Earth
```

參考： [Ruby on Rails - 虛擬屬性Virtual Attribute](https://mgleon08.github.io/blog/2015/12/14/ruby-on-rails-virtual-attribute/)

## Initialization

```
def Spaceship
  def initialize(name, cargo_module_count)
    @name = name
    @cargo_hold = CargoHold.new(cargo_module_count)
    @power_level = 100
  end
end

ship = Spaceship.new("Dreadnought", 4)
# will execute initialize("Dreadnought", 4)
```

## Inheritance

```
class Probe
  def deploy
    ...
  end
  
  def take_sample
    # do generiv sampling
  end
end

class MinerProbe < Probe
  def take_sample
    # take a mineral sample
  end
end

class AtomsphericProbe < Probe
  def take_sample
    # take a sample of the atmospere
  end
end
```

### super

```
class Probe
  def deploy(deploy_time, return_time)
    puts "Deploying"
  end
end

class MinerProbe < Probe
  def deploy(deploy_time)
    puts "Preparing sample chamber"
    super
  end
end

MinerProbe.new.deploy(Time.now)
# super.rb:2:in `deploy': wrong number of arguments (given 1, expected 2) (ArgumentError)
```

super 會執行superClass的method

[sample file](./super.rb)

*Inheritance is for reusing functionality, not enforcing interface*

```
class Probe
  def dock
    # probe specific docking actions
  end
end

class Lander
  def dock
    # lander specific docking actions
  end
end
```

假設spaceship可以launch both Probe 和 lander，回程時可dock the ship under the ships control。

do not do this:

```
class Dockable
  def dock
    # implemented by subclass
  end
end
```

do this:

```
class Spaceship
  def capture(unit)
    unit.dock # works on anything with dock method
    transport_to_storage(unit)
  end
end

ship.capture(probe)
ship.capture(lander)
```

## Class Methods and Variables

### Class Methods

```
class Spaceship
  def self.thruster_count
    2
  end
end

Spaceship.thruster_count

ship = Spaceship.new
ship.thruster_count # does not work
```

### Class Variables

```
class Spaceship
  @@thuruster_count = 2 # invisible outside
  
  def self.thuruster_count
    @@thuruster_count
  end
end

class SpritelySpaceship < Spaceship
  @@thruster_count = 4
end

class EconolineSpaceship < Spaceship
  @@ thruster_count = 1
end

puts SpritelySpaceship.thruster_count
# 1
```

### Class Instance Variables

```
class Spaceship
  @thruster_count = 2
  
  def self.thruster_count
    @thruster_count
  end
end

class SpritelySpaceship < Spaceship
  @thruster_count = 4
end

class EconolineSpaceship < Spaceship
  @ thruster_count = 1
end

puts SpritelySpaceship.thruster_count # 4
puts EconolineSpaceship.thruster_count # 1
puts Spaceship.thruster_count # 2
```

## Method Visibility

```
class Spaceship
  def launch
    batten_hatches
  end
  
  private
  
  def batten_hatches
    puts "Batten the hatches!"
  end
  
  # private :batten_hatches
end

ship = Spaceship.new
ship.batten_hatches 
# throw error
# NoMethodError: private method `batten_hatches' called for #<Spaceship:0x00000002522388>

ship.send :batten_hatches
# Batten the hatches!
```

### for class methods

```
class Spaceship
  def self.disable_engine_contaonment
    # dangerous - shuold be private!
  end
  
  # no error but does not nothing
  private :disable_engine_contaonment
  
  # corrent way:
  private_class_method :disable_engine_contaonment
end
```

### protected methods

```
class Spaceship
  def launch
    batten_hatches
  end
  
  attr_reader :call_sign
  protected :call_sign
  
  def initialize
    @call_sign = "Dreadnought"
  end
  
  def call_sign_matches? other
    call_sign == other.call_sign
  end
end

class SpritelySpaceship < Spaceship
  def initialize
    @call_sign = "Fast cruiser"
  end
end

ship = Spaceship.new
fast_ship = SpritelySpaceship.new
puts fast_ship.call_sign_matches? ship
puts ship.call_sign
# false
# 02_Classes_and_Objects/protected.rb:27:in `<main>': protected method `call_sign' called for #<Spaceship:0x00000001d52270 @call_sign="Dreadnought"> (NoMethodError)
```

[sample file](./protected.rb)

### Sumary

- public is the default
- private means "can't be called with an explicit receiver"
- private_class_method si private for class methods
- protected means "allow access for other objects of the same class"
- private and protected not used a whole lot in ruby


## Executable Class Bodies and self

### Executable Class Bodies

```
def greet(greeting)
  puts greeting + ", captain!"
end

result = class Spaceship
  answer = 7 * 6
  puts "Calculating in class context: " + answer.to_s
  greet "Good morning"
  answer
end

puts "The class calculated: " + result.to_s
puts Spaceship.superclass
```

### self

```
class Spaceship

  def self.thruster_count
    # self == Spaceship, hence we're adding this method to the class
    2
  end
  
  def cancel_launch
    self.destination = ""
    # self == ship inside method
    # ship.cancel_launch
    seatbelt_sign(:off)
    # No explicit object reference, so seatbelt_sign also called on ship
  end
  
end
```

## Open Classes and Monkey Patching

### Open Classes

```
class Spaceship
  private
  def batten_hatches
    puts "Batten the hatches!"
  end
end

ship = Spaceship.new

class Spaceship
  def batten_hatches
    puts "Avast!"
  end
end

ship.batten_hatches # Avast!
```


### Monkey Patch
程序運行時給代碼加補丁

```
class String
  def space_out
    chars.join(" ")
  end

  #def size
  # "Won't tell you!"
  #end
end

puts "Firefly".space_out
puts "abc".size
```

## Equality

```
a=b=1
a.equal?(b) # true
b=2
a.equal?(b) # false
```

```
class Spaceship
  attr_reader :name
  
  def initialize(name)
    @name = name
  end
end

ship1 = Spaceship.new("Serenity")
ship2 = Spaceship.new("Serenity")

puts ship1.equal?(ship2) # false
puts ship1.name == ship2.name # true
```

```
a = "abc"
b = "abc"
a == b # true
a.equal?(b) # false, different objects
```

overwrite the equal method

```
class Spaceship
  attr_reader :name
  
  def initialize(name)
    @name = name
  end
  
  def ==(other)
    name == other.name
  end
end

puts ship1 == ship2 # true
```