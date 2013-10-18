Note: these are instructions to follow for the instructor.

# 1. Setup

## Windows

- Install Ruby by running `installers/windows/rubyinstaller-1.9.3-p448.exe` 
- Install Sublime by running `installers/windows/Sublime Text 2.0.2 Setup.exe`

## Mac

- Ruby is already installed! Yaay!
- Install Sublime by running `installers/mac/Sublime Text 2.0.2.dmg`
  - And drag Sublime into Applications

# 2. Let's Begin!

We're going to be reading and writing a lot of code. It may seem like a lot at first. You don't have to understand everything all at once, I know I never do. But we can play around with the code, we'll change it and see what happens. We'll learn by doing. So type along with me and we'll be fine.

- Open Sublime
- Drag the `code` folder into Sublime, this is the starting code we'll use to build upon.

The `code` folder has two files: `main.rb` and `window.rb`

**main.rb:**

```ruby
require 'rubygems'
require 'gosu'

require './window'

window = Window.new
window.show
```

This file will be used to run the program. It includes the other files we need.

Remember that computers really don't know much and we have to define things for it. This is what is happening here. We are telling the program to require libraries that already have definitions. And then we're telling it to require a definition we created. We'll take a look at that in a moment.

We are requiring the Gosu library, this is a library that makes it easier for us to make games in Ruby.

The last two lines are used to create a new window, and then we're sending the `show` message to the window, and this tells the window to show itself!

**window.rb**

```ruby
class Window < Gosu::Window
  def initialize
    super(300, 300, false)
    self.caption = "Adnan's Game!"
  end

  def update
  end

  def draw
  end
end
```

So this is how we define objects in Ruby. `Gosu::Window` is a prefined object from the Gosu library. We define object by using the term `class`. And inside the object we define methods. These are the messages we can send to the objects. The `show` method isn't defined here because the `Gosu::Window` library does that for us.

# 3. Let's run the code!

Click on the `main.rb` file
Click on the `Tools` menu and select `Build`

A window pops up! NEAT!

Let's close the window and play around with the code a little.

Click on the `window.rb` file. Let's change around the numbers here. Run the program.

Okay, let's settle on `500` and `500`.

Oh look, it says "Adnan's Game!", put in your own name here. Run the program.

It shows your name! How amazing is that? Let's get more amazing by adding images that we can control!

# 4. Creating the player.

Right click on the code folder, and select `New file`

Save the file and call it `player.rb`

Let's define the Class with the most basic code:

```ruby
class Player
end
```

And then let's add some methods to it.

```ruby
class Player
  def initialize(window)
    @icon = Gosu::Image.new(window, "images/ghost.png", true)
  end

  def draw
    @icon.draw(0, 0, 1)
  end
end
```

# 4. Let's add the player to the window.

Click on `window.rb`

Add the following line to the `initialize` method:

```ruby
def initialize
  super(500, 500, false)
  self.caption = "Adnan's Game!"
  @player = Player.new(self)
end
```

Remember that the initialize method needs a window, well, we're in the window, so we'll pass it self.

And then in the `draw` method:

```ruby
def draw
  @player.draw
end
```

Okay, let's see what happens when we run the program.

Whoa! The ghost is in our window!

Let's make the ghost start lower in the window.

For that we'll go to the `player.rb` file.

And we'll modify the co-ordinates:

```ruby
def draw
  @icon.draw(50, 100, 1)
end
```

Let's setting on 200 and 200. About the centre of the screen.

# 5. Using the keyboard to move the ghost.

So you know how we keep changing the co-ordinates to move the ghost? Wouldn't it be cool if we could use the keyboard to control the ghost!? Let's learn how to do that.

We will need to detect when the keyboard is pressed, and then we'll have to tell the player to move. But since the player doesn't know how to move, we'll have to define movement in the player class!

First, let's modify the code in the window class to detect the keyboard.

**window.rb**
```ruby
def update
  if button_down? Gosu::Button::KbRight
    @player.move_right
  end
end
```

`button_down?` is a method that `Gosu::Window` gives us to we can see if which button is currently pressed down on the keyboard.

`Gosu::Button::KbRight` is how we reference the right button. We don't have to understand the whole line, but as we can see, it's used to reference the right button.

We're using the if statement here, so we're telling our program, if the right button is pressed down, then send the `move_right` message to the player. Makes sense.

But the player doesn't have a `move_right` method!

Let's write it!

**player.rb**
```ruby
class Player
  def initialize(window)
    @icon = Gosu::Image.new(window, "images/ghost.png", true)
    @right = 200
  end

  def draw
    @icon.draw(@right, 200, 1)
  end

  def move_right
    @right = @right + 3
  end
end
```

Let's define a `@right` variable and let's start it at 200.

Now in our `move_right` method, we'll just add 3 to `@right`.

Okay, let's run our program and see what happens.

Right on!

Let's detect the left key and make our ghost go left.

**window.rb**
```ruby
def update
  if button_down? Gosu::Button::KbRight
    @player.move_right
  end

  if button_down? Gosu::Button::KbLeft
    @player.move_left
  end
end
```

**player.rb**
```ruby
  def move_right
    @right = @right - 3
  end
```

We're going to subtract 3 from the right in order to move left.

Let's do the same thing for up and down!

**window.rb**
```ruby
def update
  if button_down? Gosu::Button::KbRight
    @player.move_right
  end

  if button_down? Gosu::Button::KbLeft
    @player.move_left
  end

  if button_down? Gosu::Button::KbUp
    @player.move_up
  end

  if button_down? Gosu::Button::KbDown
    @player.move_down
  end
end
```

**player.rb**
```ruby
  def move_up
    @top = @top - 3
  end

  def move_down
    @top = @top + 3
  end
```

Note that we will have to define a `@top` variable.

**player.rb**
```ruby
  def initialize(window)
    @icon = Gosu::Image.new(window, "images/ghost.png", true)
    @right = 200
    @top = 200
  end

  def draw
    @icon.draw(@right, @top, 1)
  end
```

Don't worry if we get the plusses and the minusses mixed up, we can change and try again.

Let's run the program now...

WHOA! You can even move diagonally by pressing right and up at the same time!

# 6. Let's add pacman!

Our program doesn't know what pacman is, so we'll have to define pacman!

Let's start by right clicking on the code directory, add a new file.

Save it and call it `pacman.rb`

**pacman.rb**
```ruby
class Pacman
  def initialize(window)
    @icon = Gosu::Image.new(window, "images/pacman.png", true)
  end

  def draw
    @icon.draw(10, 10, 1)
  end
end
```

Let's get our window to draw the pacman.

**window.rb**

Up top:
```ruby
require './pacman'
```

```ruby
  def initialize
    super(500, 500, false)
    self.caption = "Adnan's Game!"
    @player = Player.new(self)
    @pacman = Pacman.new(self)
  end

  def draw
    @player.draw
    @pacman.draw
  end
```

Okay, so now let's run our program...

Cool, we have pacman.

# 7. Making pacman move.

Since pacman is controlled by the computer, we don't need keyboard input to make it move.

Let's start by making it start in a random position each time we start the game.

**pacman.rb**
```ruby
class Pacman
  def initialize(window)
    @icon = Gosu::Image.new(window, "images/pacman.png", true)
    @top = rand(window.height)
  end

  def draw
    @icon.draw(10, @top, 1)
  end
end
```

Now let's make it move to the right automatically.

We'll have to do that in the window code

**window.rb**
```ruby
  def update
    if button_down? Gosu::Button::KbRight
      @player.move_right
    end

    if button_down? Gosu::Button::KbLeft
      @player.move_left
    end

    if button_down? Gosu::Button::KbDown
      @player.move_down
    end

    if button_down? Gosu::Button::KbUp
      @player.move_up
    end

    @pacman.move_right
  end
```

We don't have to check for the keyboard now. We just want to move pacman to the right everytime update is called.

Let's define `move_right` for pacman. Just like we did for our ghost a while ago.

**pacman.rb**
```ruby
class Pacman
  def initialize(window)
    @icon = Gosu::Image.new(window, "images/pacman.png", true)
    @top = rand(window.height)
    @right = 10
  end

  def draw
    @icon.draw(@right, @top, 1)
  end

  def move_right
    @right = @right + 3
  end
end
```

# 8. There goes pacman!

Let's make it comeback...

Everytime it gets to the end of the screen, let's make pacman start at the beginning.

**pacman.rb**
```ruby
class Pacman
  def initialize(window)
    @window = window
    @icon = Gosu::Image.new(window, "images/pacman.png", true)
    @top = rand(window.height)
    @right = 10
  end

  def draw
    @icon.draw(@right, @top, 1)
  end

  def move_right
    @right = @right + 3

    if @right > @window.width
      self.reset
    end
  end

  def reset
    @right = 10
  end
end
```

Okay, so we're doing a few things here.

We're adding a reset method. This will reset the right position to 10.

And we're calling reset when the pacman's right position goes over the width of the window. And note that for this, we're storing the window variable in a `@window` variable so that we can use it in our other methods.

Let's run the program.

That's interesting, pacman is starting at the same position everytime. Let's randomize it when we reset!

**pacman.rb**
```ruby
  def reset
    @right = 10
    @top = rand(window.height)
  end
```

Okay, cool. But nothing happens when pacman gets to the ghost.

# 9. Catching the ghost.