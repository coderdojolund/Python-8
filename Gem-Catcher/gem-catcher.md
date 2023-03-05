[Första koden](#första-koden) &bull;
[Funktionen update()](#funktionen-update) &bull;
[Lägga till en ädelsten](#lägga-till-en-ädelsten) &bull;
[Kollision och random()](#kollision-och-random) &bull;
[Räkna poäng](#räkna-poäng) &bull;
[Mus och händelser](#mus-och-händelser) &bull;
[Game Over](#game-over) &bull;
[Utmaningar](#utmaningar)

# Första koden

Öppna startprojektet här i repl.it: XXXLÄNK

This is the structure of a Pygame Zero program ...
```python
import pgzrun

WIDTH = 800
HEIGHT = 600

pgzrun.go() # Must be last line
```

The first line, import pgzrun, loads the Pygame Zero module, and the last line, pgzrun.go(), is a function that starts Pygame Zero. 
In the middle we have WIDTH = 800 and HEIGHT = 600 which sets the width and height of the game window.

# Skådespelaren
Now that wasn't very interesting, so let us add in a spaceship. 
To do so, we need to first provide an image for the spaceship. 
You can use any images you want, but to make it easy for you, I have prepared a zip file containing a variety of images.
[Download it here.](https://www.aposteriori.com.sg/wp-content/uploads/2020/02/image_pack.zip)

Remember the images folder I told you to create inside your project folder? 
That's where your spaceship image need to go. Open the zip file, choose a suitable image, and copy it into the images folder. 
I'm using this image...

![image](https://user-images.githubusercontent.com/4598641/222978313-4661f08c-e673-40be-87be-5fa1d8d06de8.png)

IMPORTANT: Your image filename must only contain lowercase letters, numbers and underscores.

Once that is done, you can add the spaceship to your Python program...

```python
import pgzrun

WIDTH = 800
HEIGHT = 600

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

def draw():
    ship.draw()

pgzrun.go() # Must be last line
```

This is what each line does:

`ship = Actor('playership1_blue')` : Create a new Actor using the playership1_blue image file. If you are using a different file, you'll need to change this.

`ship.x = WIDTH / 2 - 30` : Set the x position of the ship to 370. Try changing this!

`ship.y = HEIGHT - 50` : Set the y position of the ship to 550. Try changing this!

`def draw():` : This is a special function. We don't need to run it ourselves; Pygame Zero will run it for us regularly.

`ship.draw()` : This tells the ship Actor to draw itself on the screen. It needs to be indented under def draw():, so that it will run whenever Pygame Zero run the draw() function.

![image](https://user-images.githubusercontent.com/4598641/222978372-85be8781-e6fe-414e-9fb1-b7847dfc8b5f.png)

# Funktionen update()

Earlier, we created the `draw()` function. This is a special function that Pygame Zero runs regularly to draw what you see on the screen. Another special function is `update()`. Pygame Zero will regularly run our `update()` function to update the position of the various actors in the game.

Let's add in the `update()` function and program it to react to keypresses.

```python
import pgzrun

WIDTH = 800
HEIGHT = 600

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

def draw():
    ship.draw()

pgzrun.go() # Must be last line
```

This is what we added ...
```python
def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5
```
Whenever Pygame Zero runs our `update()` function, we will check if the left key is pressed. 
If it is, we'll reduce the x position by 5.
If the right key is pressed, we'll increase the x position by 5.

Try out the program! Write it in IDLE and run it using F5 or Run -> Run Module. Did that work the way you expected?

## Clearing Screen
If you tried the previous program, you should have gotten something funky like this ...

![image](https://user-images.githubusercontent.com/4598641/222978681-7aa59c8e-4b41-49ae-8415-e5df0db6b51d.png)

That's because we told Pygame Zero to update the ship's position and draw it to screen, but we didn't tell it to erase what was drawn before!

To erase the screen, we'll fill the entire screen with a single color. We'll do this in the `draw()` function, using

```python
screen.fill((80, 0, 70))
```

The `(80, 0, 70)` is a tuple (a tuple is like a list, but it cannot be changed) representing the color. The first number (80) is for red, the second number (0) is for green, and the last number (70) is for blue. The largest allowable value for each color is 255 and the smallest is 0. **Try different numbers and see what color you get!**

Your progam should now look like this: 
```
import pgzrun

WIDTH = 800
HEIGHT = 600

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

def draw():
    screen.fill((80, 0, 70))
    ship.draw()

pgzrun.go() # Must be last line
```

# Lägga till en ädelsten

The game won't be called "Gem Catcher" without a gem! We can add in a gem the same way we added the spaceship.

First, add a gem image to the **images** folder. I'm using this one:

![image](https://user-images.githubusercontent.com/4598641/222978860-59fdc056-ee3c-4d15-bca8-09b80051d3c9.png)

Next, add a new gem **Actor**
```python
gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0
```
Setting x this way will place it roughly in the middle horizontally, while setting y to 0 will place it at the top. Don't forget to also draw the gem in the draw() function.

```python
gem.draw()
```
Your program should now look like this:

```python
import pgzrun

WIDTH = 800
HEIGHT = 600

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

def draw():
    screen.fill((80, 0, 70))
    gem.draw()
    ship.draw()

pgzrun.go() # Must be last line
```

## Flytta ädelstenen
Previously, we have written code in the `update()` function to make the ship move by changing its x position when the left or right key is pressed. For the gem, we'll make it move continuously downwards by changing the y position.

```python
gem.y += 4
```

We also want the gem to return to the top when it reaches the bottom. To do that, we'll set the y position to 0 (top), when it exceeds HEIGHT (bottom most position).

```python
if gem.y > HEIGHT:
    gem.y = 0
```

Add that into your program!

```python
import pgzrun

WIDTH = 800
HEIGHT = 600

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

    gem.y += 4
    if gem.y > HEIGHT:
        gem.y = 0

def draw():
    screen.fill((80, 0, 70))
    gem.draw()
    ship.draw()

pgzrun.go() # Must be last line
```

# Kollision och random()

## Känna av en kollision

We also want the gem to go back to the top if it touches the spaceship. 
If you had done Scratch before, you might remember the if touching sprite command. 
In Pygame Zero, we'll use `if gem.colliderect(ship):` or `if ship.colliderect(gem):` &ndash; both works the same.

```python
if gem.colliderect(ship):
    gem.y = 0
```
eller

```python
if ship.colliderect(gem):
    gem.y = 0
```
**Testa det i din kod!**

## Slumptal med `random()`
It's not vey interesting to have the gem fall from the same spot everytime. 
In Introduction to Python, you learned to use the random module.
We'll use it here to randomize the x position of the gem.

Before we can use the random module, we'll need to import it.

```python
import random
```

To assign a random x position, we use

```python
gem.x = random.randint(20, WIDTH - 20)
```
The `random.randint(20, WIDTH - 20)` function will provide a random number between 20 to WIDTH - 20. This should be added whenever we return the gem to the top of the screen.

Your program should look like this now.


```python
import pgzrun

WIDTH = 800
HEIGHT = 600

ship = Actor('playership1_blue')
ship.x = WIDTH / 2 - 30
ship.y = HEIGHT - 50

gem = Actor('gemgreen')
gem.x = WIDTH / 2 - 50
gem.y = 0

def update():
    if keyboard.left:
        ship.x -= 5
    if keyboard.right:
        ship.x += 5

    gem.y += 4
    if gem.y > HEIGHT:
        gem.y = 0
    if gem.colliderect(ship):
        gem.x = random.randint(20, WIDTH - 20)
        gem.y = 0

def draw():
    screen.fill((80, 0, 70))
    gem.draw()
    ship.draw()

pgzrun.go() # Must be last line
```


# Räkna poäng
# Mus och händelser
# Game Over
# Utmaningar
___

# Källor/Sources

Det här projektet är översatt och anpassat till repl.it baserat på https://aposteriori.trinket.io/game-development-with-pygame-zero#/gem-catcher/first-program

This project is a translated and adapted to repl.it from https://aposteriori.trinket.io/game-development-with-pygame-zero#/gem-catcher/first-program