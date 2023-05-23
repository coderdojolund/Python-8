# Eyes ⭐⭐

![image](https://user-images.githubusercontent.com/4598641/223815678-2c97174e-578c-4df4-9f8a-d8764f0b2424.png)

We want to make an animation where the eyes follow the mouse pointer.

## Introduction
The road to a finished project is one puzzle piece at a time 🧩. Therefore, do one section at a time from top to bottom.
- Get it working before moving on to the next section.
- Test often, after every episode or even more frequently.

Was there anything in the description that was difficult to understand? Please let us know!

## Contents
[Draw an eye](#draw-an-eye)
&bull; [Calculate the distance in X- and Y-direction between the mouse pointer and the eye](#calculate-the-distance-in-x--and-y-direction-between-the-mouse-pointer-and-the-eye)
&bull; [Calculate the distance between the mouse pointer and the eye](#calculate-the-distance-between-the-mouse-pointer-and-the-eye)
&bull; [Moving pupil](#moving-pupil)
&bull; [The pupil should not dilate when the mouse cursor is outside the eye](#the-pupil-should-not-move-when-the-mouse-cursor-is-outside-the-eye)
&bull; [Two eyes](#two-eyes)
&bull; [Tasks](#tasks)
&bull; [Sources](#sources)

# Draw an eye
The eye is drawn with a white circle and a slightly smaller, dark blue circle for the pupil.
>We use `screen` from Pygame Zero to draw.

![image](https://user-images.githubusercontent.com/4598641/223816876-1da49223-c7af-46d5-836c-9b1216eb52d4.png)

The code so far:
```python
import pgzrun
WIDTH, HEIGHT = 530, 400

def draw():
     screen.fill((0, 0, 0))

     screen.draw.filled_circle((200, 200), 50, color=(255, 255, 255))
     screen.draw.filled_circle((200, 200), 15, color=(0, 0, 100))

pgzrun.go() # must be last
```

✏️ Make sure you are logged in to repl.it. Make a copy of the starter project https://replit.com/@RobertStorlind/eyes-starter
with the "Fork" button. Try running it!

# Calculate the distance in X and Y directions between the mouse pointer and the eye
We add code so that the distance between the center of the eye and the mouse pointer is displayed, counted in pixels.

- We reuse the eye's x and y coordinates from when we draw the eye and save the coordinates from there in variables.
- We import the `pygame` module to be able to use the `pygame.mouse.get_pos` function.
- The function `update()` is empty for now. The `draw()` function redraws the screen on each update.

✏️ **Enter** and run the code!

The code so far &ndash; new lines are highlighted:

```python
import pgzrun
import pygame #new line 👀
WIDTH, HEIGHT = 530, 400

def update(): #new line 👀
     pass #new line 👀

def draw():
     screen.fill((0, 0, 0))

     mouse_x, mouse_y = pygame.mouse.get_pos() #new line 👀
     eye_x = 200 #new line 👀
     eye_y = 200 #new line 👀

     distance_x = mouse_x - eye_x #new line 👀
     distance_y = mouse_y - eye_y #new line 👀

     screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255)) #slightly changed 👀
     screen.draw.filled_circle((200, 200), 15, color=(0, 0, 100))

     screen.draw.text(f"distance x: {distance_x}\ndistance y: {distance_y}", (0, 0)) #new line 👀

pgzrun.go() # must be last
```

This is what it looks like when I test drive. If it is not working, check the code again.

![image](https://user-images.githubusercontent.com/4598641/223817639-1363643f-481d-44e3-979b-d0b48eb0c9da.png)


# Calculate the distance between the mouse pointer and the eye
We can calculate the distance, `distance`, using the Pythagorean theorem. See the figure.

![image](https://user-images.githubusercontent.com/4598641/224125785-ee2eedc9-2155-4508-9fc2-d8518bfdfe32.png)

The distance in pixels is the square root of (the number of pixels in the X direction squared + the number of pixels in the Y direction squared).
- The operator `**2` means raised to 2, i.e. the square
- We import the math module `math` to calculate the square root with `math.sqrt()`.

:pencil2: **Update the code** and run it!

```python
import pgzrun
import pygame
import math # new line 👀

WIDTH, HEIGHT = 530, 400

def update():
     pass

def draw():
     screen.fill((0, 0, 0))

     mouse_x, mouse_y = pygame.mouse.get_pos()
     eye_x = 200
     eye_y = 200

     distance_x = mouse_x - eye_x
     distance_y = mouse_y - eye_y
     distance = math.sqrt(distance_x**2 + distance_y**2) #new line 👀

     screen.draw.filled_circle(
         (eye_x, eye_y), 50, color=(255, 255, 255))
     screen.draw.filled_circle((200, 200), 15, color=(0, 0, 100))

     screen.draw.text(
         f"distance x: {distance_x}\ndistance y: {distance_y}\ndistance: {distance}", (0, 0)) # changed 👀

pgzrun.go() # must be last
```

Is the distance calculated correctly? Move the mouse pointer slowly and check.

# Moving pupil

We want the pupil to follow the mouse pointer.

If the mouse cursor is inside the eye, it's easy. Then we can set the pupil to the coordinates of the mouse pointer.

Then the code for `draw()` could look like this. The rest of the code is the same as before.

```python
# keep the rest of the code

def draw(): #function is updated 👀
     screen.fill((0, 0, 0))

     mouse_x, mouse_y = pygame.mouse.get_pos()
     eye_x = 200
     eye_y = 200

     distance_x = mouse_x - eye_x
     distance_y = mouse_y - eye_y
     distance = math.sqrt(distance_x**2 + distance_y**2) # Pythagoras
     pupil_x = eye_x + distance_x #new row
     pupil_y = eye_y + distance_y #new row

     screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255))
     screen.draw.filled_circle((pupil_x, pupil_y), 15, color=(0, 0, 100))

# keep the rest of the code
```
:pencil2: **Update and test** your code. Does it work well when the mouse pointer is inside the eye circle?

<details>
   <summary>📝 This is what the entire code looks like now</summary>
  
```python
import pgzrun
import pygame
import math

WIDTH, HEIGHT = 530, 400


def update():
     pass


def draw(): #function is updated 👀
     screen.fill((0, 0, 0))

     mouse_x, mouse_y = pygame.mouse.get_pos()
     eye_x = 200
     eye_y = 200

     distance_x = mouse_x - eye_x
     distance_y = mouse_y - eye_y
     distance = math.sqrt(distance_x**2 + distance_y**2) # Pythagoras
     pupil_x = eye_x + distance_x #new row 👀
     pupil_y = eye_y + distance_y #new row 👀

     screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255))
     screen.draw.filled_circle((pupil_x, pupil_y), 15, color=(0, 0, 100))


pgzrun.go() # must be last
```
    
</details>

# The pupil should not move when the mouse pointer is outside the eye

🤔 How do we know the mouse pointer is in the eye? We have calculated the distance from the center of the eye to the mouse pointer in the `distance` variable, so we use the radius of the eye as a limit, for example 30 pixels. There must be some space to draw the pupil as well.

If the mouse pointer is more than 30 pixels from the center of the eye, we place the pupil at the edge of the eye. See the picture.

![image](https://user-images.githubusercontent.com/4598641/224125785-ee2eedc9-2155-4508-9fc2-d8518bfdfe32.png)

- The dashed triangle has a hypotenuse of 30 pixels and is congruent with the larger triangle with the mouse pointer in one corner.
- The proportion between the dashed triangle and the large triangle is `30 / distance` when the mouse pointer is outside the eye. If e.g. `distance` is 60 then the sides of the small dashed triangle are 30/60 = 1/2 of the large triangle.
- We must therefore scale `distance_x` and `distance_y` with that scale factor when calculating the x and y coordinates of the pupil.
- Then the pupil will be drawn inside the circle of the eye, as we want.

This is what `draw()` might look like now:

```python
# keep the rest of the code

def draw():
     screen.fill((0, 0, 0))

     mouse_x, mouse_y = pygame.mouse.get_pos()
     radius = 30 #new line
     eye_x = 200
     eye_y = 200

     distance_x = mouse_x - eye_x
     distance_y = mouse_y - eye_y
     distance = math.sqrt(distance_x**2 + distance_y**2) # Pythagoras
    
     if distance < radius: # inside the eye # changed 👀
         pupil_x = eye_x + distance_x # changed 👀
         pupil_y = eye_y + distance_y # changed 👀
     else: # outside the eye # changed 👀
         scale = radius / distance # see image # changed 👀
         pupil_x = eye_x + distance_x * scale # changed 👀
         pupil_y = eye_y + distance_y * scale # changed 👀

     screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255))
     screen.draw.filled_circle((pupil_x, pupil_y), 15, color=(0, 0, 100))

# keep the rest of the code
```
:pencil2: **Update the highlighted lines and test the code.** Does it work fine even when the mouse pointer is outside the eye circle?

<details>
   <summary>📝 This is what the entire code looks like now</summary>
  
```python
import pgzrun
import pygame
import math

WIDTH, HEIGHT = 530, 400

def update():
     pass

def draw(): # updated 👀
     screen.fill((0, 0, 0))

     mouse_x, mouse_y = pygame.mouse.get_pos()
     radius = 30 # newline
     eye_x = 200
     eye_y = 200

     distance_x = mouse_x - eye_x
     distance_y = mouse_y - eye_y
     distance = math.sqrt(distance_x**2 + distance_y**2) # Pythagoras

     if distance < radius: # inside the eye
         pupil_x = eye_x + distance_x
         pupil_y = eye_y + distance_y
     else: # outside the eye
         scale = radius / distance # see image
         pupil_x = eye_x + distance_x * scale
         pupil_y = eye_y + distance_y * scale

     screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255))
     screen.draw.filled_circle((pupil_x, pupil_y), 15, color=(0, 0, 100))


pgzrun.go() # must be last
```
    
</details>

# Two eyes
We want *two* eyes that follow the mouse pointer. We can reuse the same code.
Therefore, we put the code of the eye
in the `draw_eye` function which has the eye's x and y coordinate as input/parameters.

This is what the code might look like now:

```python
import pgzrun
import pygame
import math

WIDTH, HEIGHT = 530, 400

def update():
     pass

def draw():
     screen.fill((0, 0, 0))

     def draw_eye(eye_x, eye_y): # changed 👀
         radius = 30 # changed 👀 -- remember to put four extra spaces before
         mouse_x, mouse_y = pygame.mouse.get_pos() # changed 👀

         distance_x = mouse_x - eye_x # changed 👀
         distance_y = mouse_y - eye_y # changed 👀
         distance = math.sqrt(distance_x**2 + distance_y**2) # changed 👀
         if distance < radius: # changed 👀
             pupil_x = eye_x + distance_x # changed 👀
             pupil_y = eye_y + distance_y # changed 👀
         else: # changed 👀
             scale = radius / distance # see image # changed 👀
             pupil_x = eye_x + distance_x * scale # changed 👀
             pupil_y = eye_y + distance_y * scale # changed 👀

         screen.draw.filled_circle((eye_x, eye_y), 50, color=(255, 255, 255)) # changed 👀
         screen.draw.filled_circle((pupil_x, pupil_y), 15, color=(0, 0, 100)) # changed 👀

     draw_eye(200, 200) # first eye 👀
     draw_eye(330, 200) # the other eye 👀

pgzrun.go()
```
:pencil2: **Update the changed lines and test the code.** Do both eyes follow the mouse pointer correctly?
>Remember to adjust the indentation in the code when adding the `draw_eye()` function.
    
# Tasks

## Task 1: Evaluate your own work!
When you answer this, imagine that *you have access to the task* &ndash; so you don't need to know the code by heart.

**1A.** Show what you did to the rest of your group.

**1B.** Are there some parts of the exercise that you do not understand yet? Give examples of something you don't understand.

## Task 2
The calculation of scale can be redone so that we get even fewer lines of code. Right now we have this code, where there is very little difference between the two code blocks under `if` and `else` respectively:
```python
     # inside the draw() function:
         if distance < radius: # case 1: inside the eye
             pupil_x = eye_x + distance_x
             pupil_y = eye_y + distance_y
         else: # case 2: outside the eye
             scale = radius / distance # see image
             pupil_x = eye_x + distance_x * scale
             pupil_y = eye_y + distance_y * scale
```
Can you simplify the code so that we don't need two different calculations of `pupil_x` or `pupil_y`?[^1]
- Don't forget to save code examples before you move on
    
## Extra task 3
Can you make your eyes change color, disappear or move to a different position on the screen sometimes?
- Don't forget to save code examples before you move on
- If you do `from random import randint`, you can use the function `randint(min, max)` to get a random integer between min and max.
    
## Extra task 4
Can you insert some other kind of shape and make the eyes follow that shape instead of the mouse pointer?
- Don't forget to save code examples before you move on

# More about the code in Pygame Zero and Pygame, explained in English

**[draw(), update()](https://pygame-zero.readthedocs.io/en/stable/hooks.html#game-loop-hooks)**: https://pygame-zero.readthedocs .io/en/stable/hooks.html#game-loop-hooks
    
**[pygame.mouse.get_pos()](https://www.pygame.org/docs/ref/mouse.html#pygame.mouse.get_pos)**: https://www.pygame.org/docs /ref/mouse.html#pygame.mouse.get_pos

**[screen.draw(), screen.fill()](https://pygame-zero.readthedocs.io/en/stable/builtins.html#screen)**: https://pygame-zero.readthedocs .io/en/stable/builtins.html#screen
    
# Sources

This is an adaptation to repl.it of the original project https://simplegametutorials.github.io/pygamezero/eyes/

[^1]:What can you set `scale` to when `distance < radius`? How can you use it to calculate `pupil_x` and `pupil_y` using the same code?
