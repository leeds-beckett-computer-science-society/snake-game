# snake-game-ai
REF: https://pythonspot.com/snake-with-pygame/
code along session 16/10/2019

Tutorial:

First we need to import the correct libraries, for this tutorial we'll use pygame (https://www.pygame.org/news)
This library contains a bunch of modules we can utilise for making games. Also its important to remember with
python that format aka. indentation is very important. 

Step 1) import the correct libraries

from pygame.locals import *
from random import randint
import pygame
import time 
-----------------------------------------------------------------------------------------------------------------
Then we want to define a player class which can define certain attributes such as movement.

Step 2) define player class and attributes

// you can play with the speed and size to change the game
// a class is an object orientated concept whiccencapsulates data
// and object attributes within that class so it can be reused elsewhere without

class Player:
    x = 10
    y = 10
    speed = 1

    def moveRight(self):
        self.x = self.x + self.speed

    def moveLeft(self):
        self.x = self.x - self.speed

    def moveUp(self):
        self.y = self.y - self.speed

    def moveDown(self):
        self.y = self.y + self.speed
-------------------------------------------------------------------------------------------------------------------

// Now we can start implementing the actual game with another class
// First we should initialise the window the game can be played in

Step 3) Define window dimensions

class App:

    windowWidth = 800
    windowHeight = 600
    player = 0
    
   
-------------------------------------------------------------------------------------------------------------------
// Then we want to start initialising some methods which define the basic logic of the game to control the player
// a def is a self contained function which contains some sort of logical statement which
// can readibly be used by other functions. We pass 'self' into the function

Step 4) intialise game logic

def __init__(self):
        self._running = True
        self._display_surf = None
        self._image_surf = None
        self.player = Player() 

    def on_init(self):
        pygame.init()
        self._display_surf = pygame.display.set_mode((self.windowWidth,self.windowHeight), pygame.HWSURFACE)
        
-------------------------------------------------------------------------------------------------------------------

// Here we need to enter in some graphics, so here quickly make a simple block in paint with whatever texture or
// image you want. Point for creativity. Then when done save the image somewhere you remember then drag it into
// the project/venv folder. Also make sure to edit the code to have the correct image file name.
        
        pygame.display.set_caption('Ssssnake game')
        self._running = True
        self._image_surf = pygame.image.load("[your texture].png").convert()
 
    def on_event(self, event):
        if event.type == QUIT:
            self._running = False

    def on_loop(self):
        pass
    
    def on_render(self):
        self._display_surf.fill((0,0,0))
        self._display_surf.blit(self._image_surf,(self.player.x,self.player.y))
        pygame.display.flip()
 
    def on_cleanup(self):
        pygame.quit()
 
    def on_execute(self):
        if self.on_init() == False:
            self._running = False
            
-------------------------------------------------------------------------------------------------------------------

// We can then implement some code which will allow the keyboard arrows be used to control the player

Step 5) Initialise keyboard actions
 
   def on_execute(self):
        if self.on_init() == False:
              self._running = False
              
        while( self._running ):
            pygame.event.pump()
            keys = pygame.key.get_pressed() 
            
            if (keys[K_RIGHT]):
                self.player.moveRight()

            if (keys[K_LEFT]):
                self.player.moveLeft()

            if (keys[K_UP]):
                self.player.moveUp()

            if (keys[K_DOWN]):
                self.player.moveDown()

            if (keys[K_ESCAPE]):
                self._running = False

            self.on_loop()
            self.on_render()
        self.on_cleanup()

-------------------------------------------------------------------------------------------------------------------

// Finally to run it all together we have to execute in main:

Step 6) Execute the program

if __name__ == "__main__" :
    theApp = App()
    theApp.on_execute()

// We will now have a running game where you are able to move around the player, well done! But now we're going to 
// add some more advanced features.

-------------------------------------------------------------------------------------------------------------------

// Now we'll go back to the player class and flesh out the player a bit more:

Step 7) add update function which tracks where the player is #def update'


class Player:
    x = 0
    y = 0
    speed = 32
    direction = 0
    
    // this funtion gets the position of the player

    def update(self):
        if self.direction == 0:
            self.x = self.x + self.speed
        if self.direction == 1:
            self.x = self.x - self.speed
        if self.direction == 2:
            self.y = self.y - self.speed
        if self.direction == 3:
            self.y = self.y + self.speed

    def moveRight(self):
        self.direction = 0

    def moveLeft(self):
        self.direction = 1

    def moveUp(self):
        self.direction = 2

    def moveDown(self):
        self.direction = 3
        
-------------------------------------------------------------------------------------------------------------------

// Also to make sure the game loops we need to enter this code in the 
// def on_execute function (just after self.on_render()):

Step 8) Add game loop

import time
...
time.sleep (100.0 / 1000.0);

-------------------------------------------------------------------------------------------------------------------

Now we need to define the length of the snake, instead of statically defining the player size we 
put the variables as arrays:

Step 7) Make the game 'snakey'

class Player:
    
    # define player size in an empty array
    x = []
    y = []
    step = 44
    direction = 0
    
    # initial length '3'
    length = 3
    updateCountMax = 2
    updateCount = 0
    
    # define function which uses loop to append length on
    
    def __init__(self, length):
        self.length = length
        for i in range(0, length):
            self.x.append(0)
            self.y.append(0)
            
  -------------------------------------------------------------------------------------------------------------------

Then we must add the advanced update functions:

Step 8) 

def update(self):
        # increment updateCount 
        self.updateCount = self.updateCount + 1
        if self.updateCount &gt; self.updateCountMax:

            # update previous positions
            for i in range(self.length-1,0,-1):
                print "self.x[" + str(i) + "] = self.x[" + str(i-1) + "]"
                self.x[i] = self.x[i-1]
                self.y[i] = self.y[i-1]

            # update position of head of snake
            if self.direction == 0:
                self.x[0] = self.x[0] + self.step
            if self.direction == 1:
                self.x[0] = self.x[0] - self.step
            if self.direction == 2:
                self.y[0] = self.y[0] - self.step
            if self.direction == 3:
                self.y[0] = self.y[0] + self.step

            self.updateCount = 0


    def moveRight(self):
        self.direction = 0

    def moveLeft(self):
        self.direction = 1

    def moveUp(self):
        self.direction = 2

    def moveDown(self):
        self.direction = 3 

    def draw(self, surface, image):
        for i in range(0,self.length):
            surface.blit(image,(self.x[i],self.y[i]))
            
   It looks confusing but it is just a lot of updating arrays aka. memory management:D
            
-------------------------------------------------------------------------------------------------------------------

Add the final touches to the app class

Step 9) Copy and paste this and replace class app: in your program (make sure to rename the texture.png file)

class App:

    windowWidth = 800
    windowHeight = 600
    player = 0

    def __init__(self):
        self._running = True
        self._display_surf = None
        self._image_surf = None
        self.player = Player(10) 

    def on_init(self):
        pygame.init()
        self._display_surf = pygame.display.set_mode((self.windowWidth,self.windowHeight), pygame.HWSURFACE)
        
        pygame.display.set_caption(''Ssssnake game'')
        self._running = True
        self._image_surf = pygame.image.load("[your texture].png").convert()
 
    def on_event(self, event):
        if event.type == QUIT:
            self._running = False

    def on_loop(self):
        self.player.update()
        pass
    
    def on_render(self):
        self._display_surf.fill((0,0,0))
        self.player.draw(self._display_surf, self._image_surf)
        pygame.display.flip()
 
    def on_cleanup(self):
        pygame.quit()
 
    def on_execute(self):
        if self.on_init() == False:
            self._running = False
 
        while( self._running ):
            pygame.event.pump()
            keys = pygame.key.get_pressed() 
            
            if (keys[K_RIGHT]):
                self.player.moveRight()

            if (keys[K_LEFT]):
                self.player.moveLeft()

            if (keys[K_UP]):
                self.player.moveUp()

            if (keys[K_DOWN]):
                self.player.moveDown()

            if (keys[K_ESCAPE]):
                self._running = False
            self.on_loop()
            self.on_render()

            time.sleep (50.0 / 1000.0);
        self.on_cleanup()
 
if __name__ == "__main__" :
    theApp = App()
    theApp.on_execute()
-------------------------------------------------------------------------------------------------------------------

FINALLY we have to add a game objective:

If the snake eats an apple, the apple moves to a new position.

If the snake eats an apple, the snakes length grows.

If a snake collapses with itself, game over.


So first we have to define what the apple is:

Step 10) define apple class

class Apple:
    x = 0
    y = 0
    step = 44

    def __init__(self,x,y):
        self.x = x * self.step
        self.y = y * self.step

    def draw(self, surface, image):
        surface.blit(image,(self.x, self.y))
-------------------------------------------------------------------------------------------------------------------

Step 11) define apple in game logic

class App:

    windowWidth = 800
    windowHeight = 600
    player = 0
    apple = 0

    def __init__(self):
        self._running = True
        self._display_surf = None
        self._image_surf = None
        self._apple_surf = None
        self.player = Player(10) 
        self.apple = Apple(5,5)

    def on_init(self):
        pygame.init()
        self._display_surf = pygame.display.set_mode((self.windowWidth,self.windowHeight), pygame.HWSURFACE)
        
        pygame.display.set_caption('Pygame pythonspot.com example')
        self._running = True
        self._image_surf = pygame.image.load("pygame.png").convert()
        self._apple_surf = pygame.image.load("apple.png").convert()

    def on_event(self, event):
        if event.type == QUIT:
            self._running = False

    def on_loop(self):
        self.player.update()
        pass
    
    def on_render(self):
        self._display_surf.fill((0,0,0))
        self.player.draw(self._display_surf, self._image_surf)
        self.apple.draw(self._display_surf, self._apple_surf)
        pygame.display.flip()
 
    def on_cleanup(self):
        pygame.quit()
 
    def on_execute(self):
        if self.on_init() == False:
            self._running = False
 
        while( self._running ):
            pygame.event.pump()
            keys = pygame.key.get_pressed() 
            
            if (keys[K_RIGHT]):
                self.player.moveRight()

            if (keys[K_LEFT]):
                self.player.moveLeft()

            if (keys[K_UP]):
                self.player.moveUp()

            if (keys[K_DOWN]):
                self.player.moveDown()

            if (keys[K_ESCAPE]):
                self._running = False

            self.on_loop()
            self.on_render()

            time.sleep (50.0 / 1000.0);
        self.on_cleanup()
 
if __name__ == "__main__" :
    theApp = App()
    theApp.on_execute()
-------------------------------------------------------------------------------------------------------------------
Step 12) add collison detection
A function which detects when the snake hits the wall or itself:
class Game:
  def isCollision(self,x1,y1,x2,y2,bsize):
      if x1 >= x2 and x1 <= x2 + bsize:
          if y1 >= y2 and y1 <= y2 + bsize:
              return True
      return False

-------------------------------------------------------------------------------------------------------------------
Final basic working snake code:

We'll add a second AI snake next


from pygame.locals import *
from random import randint
import pygame
import time


class Apple:
    x = 0
    y = 0
    step = 44

    def __init__(self, x, y):
        self.x = x * self.step
        self.y = y * self.step

    def draw(self, surface, image):
        surface.blit(image, (self.x, self.y))


class Player:
    x = [0]
    y = [0]
    step = 44
    direction = 0
    length = 3

    updateCountMax = 2
    updateCount = 0
    
    def __init__(self, length):
        self.length = length
        for i in range(0, 2000):
            self.x.append(-100)
            self.y.append(-100)

        # initial positions, no collision.
        self.x[1] = 1 * 44
        self.x[2] = 2 * 44

    def update(self):

        self.updateCount = self.updateCount + 1
        if self.updateCount > self.updateCountMax:

            # update previous positions
            for i in range(self.length - 1, 0, -1):
                self.x[i] = self.x[i - 1]
                self.y[i] = self.y[i - 1]

            # update position of head of snake
            if self.direction == 0:
                self.x[0] = self.x[0] + self.step
            if self.direction == 1:
                self.x[0] = self.x[0] - self.step
            if self.direction == 2:
                self.y[0] = self.y[0] - self.step
            if self.direction == 3:
                self.y[0] = self.y[0] + self.step

            self.updateCount = 0

    def moveRight(self):
        self.direction = 0

    def moveLeft(self):
        self.direction = 1

    def moveUp(self):
        self.direction = 2

    def moveDown(self):
        self.direction = 3

    def draw(self, surface, image):
        for i in range(0, self.length):
            surface.blit(image, (self.x[i], self.y[i]))


class Game:
    def isCollision(self, x1, y1, x2, y2, bsize):
        if x1 >= x2 and x1 <= x2 + bsize:
            if y1 >= y2 and y1 <= y2 + bsize:
                return True
        return False


class App:
    windowWidth = 800
    windowHeight = 600
    player = 0
    apple = 0

    def __init__(self):
        self._running = True
        self._display_surf = None
        self._image_surf = None
        self._apple_surf = None
        self.game = Game()
        self.player = Player(3)
        self.apple = Apple(5, 5)

    def on_init(self):
        pygame.init()
        self._display_surf = pygame.display.set_mode((self.windowWidth, self.windowHeight), pygame.HWSURFACE)

        pygame.display.set_caption('Sssnake')
        self._running = True
        self._image_surf = pygame.image.load("green.png").convert()
        self._apple_surf = pygame.image.load("block.png").convert()

    def on_event(self, event):
        if event.type == QUIT:
            self._running = False

    def on_loop(self):
        self.player.update()

        # does snake eat apple?
        for i in range(0, self.player.length):
            if self.game.isCollision(self.apple.x, self.apple.y, self.player.x[i], self.player.y[i], 44):
                self.apple.x = randint(2, 9) * 44
                self.apple.y = randint(2, 9) * 44
                self.player.length = self.player.length + 1

        # does snake collide with itself?
        for i in range(2, self.player.length):
            if self.game.isCollision(self.player.x[0], self.player.y[0], self.player.x[i], self.player.y[i], 40):
                print("You lose! Collision: ")
                print("x[0] (" + str(self.player.x[0]) + "," + str(self.player.y[0]) + ")")
                print("x[" + str(i) + "] (" + str(self.player.x[i]) + "," + str(self.player.y[i]) + ")")
                exit(0)

        pass

    def on_render(self):
        self._display_surf.fill((0, 0, 0))
        self.player.draw(self._display_surf, self._image_surf)
        self.apple.draw(self._display_surf, self._apple_surf)
        pygame.display.flip()

    def on_cleanup(self):
        pygame.quit()

    def on_execute(self):
        if self.on_init() == False:
            self._running = False

        while (self._running):
            pygame.event.pump()
            keys = pygame.key.get_pressed()

            if (keys[K_RIGHT]):
                self.player.moveRight()

            if (keys[K_LEFT]):
                self.player.moveLeft()

            if (keys[K_UP]):
                self.player.moveUp()

            if (keys[K_DOWN]):
                self.player.moveDown()

            if (keys[K_ESCAPE]):
                self._running = False

            self.on_loop()
            self.on_render()

            time.sleep(50.0 / 1000.0);
        self.on_cleanup()


if __name__ == "__main__":
    theApp = App()
    theApp.on_execute()
    
 -------------------------------------------------------------------------------------------------------------------

To implement an AI into the game we have to create a computer class:

It can use the same basic functions we used to define our player class.

Step 13) copy and paste this below the player class

class Computer:
    x = [0]
    y = [0]
    step = 44
    direction = 0
    length = 3
 
    updateCountMax = 2
    updateCount = 0

    def __init__(self, length):
       self.length = length
       for i in range(0,2000):
           self.x.append(-100)
           self.y.append(-100)

       # initial positions, no collision.
       self.x[0] = 1*44
       self.y[0] = 4*44

    def update(self):

        self.updateCount = self.updateCount + 1
        if self.updateCount &gt; self.updateCountMax:

            # update previous positions
            for i in range(self.length-1,0,-1):
                self.x[i] = self.x[i-1]
                self.y[i] = self.y[i-1]

            # update position of head of snake
            if self.direction == 0:
                self.x[0] = self.x[0] + self.step
            if self.direction == 1:
                self.x[0] = self.x[0] - self.step
            if self.direction == 2:
                self.y[0] = self.y[0] - self.step
            if self.direction == 3:
                self.y[0] = self.y[0] + self.step

            self.updateCount = 0


    def moveRight(self):
        self.direction = 0

    def moveLeft(self):
        self.direction = 1

    def moveUp(self):
        self.direction = 2

    def moveDown(self):
        self.direction = 3 

    def draw(self, surface, image):
        for i in range(0,self.length):
            surface.blit(image,(self.x[i],self.y[i]))
            
-------------------------------------------------------------------------------------------------------------------

We need to call the conputer updates in the App class:

Step 14) Edit these functions in App class to implement the updates


    def on_loop(self):
        self.player.update()
        self.computer.update()   <<

....
    
    def on_render(self):
        self._display_surf.fill((0,0,0))
        self.player.draw(self._display_surf, self._image_surf)
        self.apple.draw(self._display_surf, self._apple_surf)
        self.computer.draw(self._display_surf, self._image_surf)  <<
        pygame.display.flip()
-------------------------------------------------------------------------------------------------------------------

We now need to impletement the intelligence of the AI, obviously since this is just a snake game 
it doesn't need to be very intelligent. It only needs to know the position of the player and move towards
those coordinates.

Step 15) Implement ai algorithm in Computer class

def target(self,dx,dy):
    if self.x[0] &gt; dx:
        self.moveLeft()

    if self.x[0] &lt; dx:
        self.moveRight()

    if self.x[0] == dx:
        if self.y[0] &lt; dy:
            self.moveDown()

        if self.y[0] &gt; dy:
            self.moveUp()
            
-------------------------------------------------------------------------------------------------------------------

THE FINISHED CODE:

from pygame.locals import *
from random import randint
import pygame
import time


class Apple:
    x = 0
    y = 0
    step = 44

    def __init__(self, x, y):
        self.x = x * self.step
        self.y = y * self.step

    def draw(self, surface, image):
        surface.blit(image, (self.x, self.y))


class Player:
    x = [0]
    y = [0]
    step = 44
    direction = 0
    length = 3

    updateCountMax = 2
    updateCount = 0

    def __init__(self, length):
        self.length = length
        for i in range(0, 2000):
            self.x.append(-100)
            self.y.append(-100)

        # initial positions, no collision.
        self.x[0] = 1 * 44
        self.x[0] = 2 * 44

    def update(self):

        self.updateCount = self.updateCount + 1
        if self.updateCount > self.updateCountMax:

            # update previous positions
            for i in range(self.length - 1, 0, -1):
                self.x[i] = self.x[i - 1]
                self.y[i] = self.y[i - 1]

            # update position of head of snake
            if self.direction == 0:
                self.x[0] = self.x[0] + self.step
            if self.direction == 1:
                self.x[0] = self.x[0] - self.step
            if self.direction == 2:
                self.y[0] = self.y[0] - self.step
            if self.direction == 3:
                self.y[0] = self.y[0] + self.step

            self.updateCount = 0

    def moveRight(self):
        self.direction = 0

    def moveLeft(self):
        self.direction = 1

    def moveUp(self):
        self.direction = 2

    def moveDown(self):
        self.direction = 3

    def draw(self, surface, image):
        for i in range(0, self.length):
            surface.blit(image, (self.x[i], self.y[i]))


class Computer:
    x = [0]
    y = [0]
    step = 44
    direction = 0
    length = 3

    updateCountMax = 2
    updateCount = 0

    def __init__(self, length):
        self.length = length
        for i in range(0, 2000):
            self.x.append(-100)
            self.y.append(-100)

        # initial positions, no collision.
        self.x[0] = 1 * 44
        self.y[0] = 4 * 44

    def update(self):

        self.updateCount = self.updateCount + 1
        if self.updateCount > self.updateCountMax:

            # update previous positions
            for i in range(self.length - 1, 0, -1):
                self.x[i] = self.x[i - 1]
                self.y[i] = self.y[i - 1]

            # update position of head of snake
            if self.direction == 0:
                self.x[0] = self.x[0] + self.step
            if self.direction == 1:
                self.x[0] = self.x[0] - self.step
            if self.direction == 2:
                self.y[0] = self.y[0] - self.step
            if self.direction == 3:
                self.y[0] = self.y[0] + self.step

            self.updateCount = 0

    def moveRight(self):
        self.direction = 0

    def moveLeft(self):
        self.direction = 1

    def moveUp(self):
        self.direction = 2

    def moveDown(self):
        self.direction = 3

    def target(self, dx, dy):
        if self.x[0] > dx:
            self.moveLeft()

        if self.x[0] < dx:
            self.moveRight()

        if self.x[0] == dx:
            if self.y[0] < dy:
                self.moveDown()

            if self.y[0] > dy:
                self.moveUp()

    def draw(self, surface, image):
        for i in range(0, self.length):
            surface.blit(image, (self.x[i], self.y[i]))


class Game:
    def isCollision(self, x1, y1, x2, y2, bsize):
        if x1 >= x2 and x1 <= x2 + bsize:
            if y1 >= y2 and y1 <= y2 + bsize:
                return True
        return False


class App:
    windowWidth = 800
    windowHeight = 600
    player = 0
    apple = 0

    def __init__(self):
        self._running = True
        self._display_surf = None
        self._image_surf = None
        self._apple_surf = None
        self.game = Game()
        self.player = Player(5)
        self.apple = Apple(8, 5)
        self.computer = Computer(5)

    def on_init(self):
        pygame.init()
        self._display_surf = pygame.display.set_mode((self.windowWidth, self.windowHeight), pygame.HWSURFACE)

        pygame.display.set_caption('Ssssnake')
        self._running = True
        self._image_surf = pygame.image.load("green.png").convert()
        self._apple_surf = pygame.image.load("block.png").convert()

    def on_event(self, event):
        if event.type == QUIT:
            self._running = False

    def on_loop(self):
        self.computer.target(self.apple.x, self.apple.y)
        self.player.update()
        self.computer.update()

        # does snake eat apple?
        for i in range(0, self.player.length):
            if self.game.isCollision(self.apple.x, self.apple.y, self.player.x[i], self.player.y[i], 44):
                self.apple.x = randint(2, 9) * 44
                self.apple.y = randint(2, 9) * 44
                self.player.length = self.player.length + 1

        # does computer eat apple?
        for i in range(0, self.player.length):
            if self.game.isCollision(self.apple.x, self.apple.y, self.computer.x[i], self.computer.y[i], 44):
                self.apple.x = randint(2, 9) * 44
                self.apple.y = randint(2, 9) * 44
                # self.computer.length = self.computer.length + 1

        # does snake collide with itself?
        for i in range(2, self.player.length):
            if self.game.isCollision(self.player.x[0], self.player.y[0], self.player.x[i], self.player.y[i], 40):
                print("You lose! Collision: ")
                print("x[0] (" + str(self.player.x[0]) + "," + str(self.player.y[0]) + ")")
                print("x[" + str(i) + "] (" + str(self.player.x[i]) + "," + str(self.player.y[i]) + ")")
                exit(0)

        pass

    def on_render(self):
        self._display_surf.fill((0, 0, 0))
        self.player.draw(self._display_surf, self._image_surf)
        self.apple.draw(self._display_surf, self._apple_surf)
        self.computer.draw(self._display_surf, self._image_surf)
        pygame.display.flip()

    def on_cleanup(self):
        pygame.quit()

    def on_execute(self):
        if self.on_init() == False:
            self._running = False

        while (self._running):
            pygame.event.pump()
            keys = pygame.key.get_pressed()

            if (keys[K_RIGHT]):
                self.player.moveRight()

            if (keys[K_LEFT]):
                self.player.moveLeft()

            if (keys[K_UP]):
                self.player.moveUp()

            if (keys[K_DOWN]):
                self.player.moveDown()

            if (keys[K_ESCAPE]):
                self._running = False

            self.on_loop()
            self.on_render()

            time.sleep(50.0 / 1000.0);
        self.on_cleanup()


if __name__ == "__main__":
    theApp = App()
    theApp.on_execute()
    
    
 CHALLENGE: ADD A SCOREBOARD
