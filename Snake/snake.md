# Snake ⭐⭐⭐
## En handledning för Python och Pygame Zero 1.2

**[Regler](#regler)**

**[Översikt](#översikt)**

**[Kodning](#kodning)**
  [Rita bakgrunden](#rita-bakgrunden)
  &bull; [Rita ormen](#rita-ormen)
  &bull; [Timer](#timer)
  &bull; [Flytta ormen åt höger](#flytta-ormen-åt-höger)
  &bull; [Flytta ormen i alla fyra riktningar](#flytta-ormen-i-alla-fyra-riktningar)
  &bull; [Hindra att ormen rör sig rakt bakåt](#hindra-att-ormen-rör-sig-rakt-bakåt)
  &bull; [Använd en kö för riktningarna](#använd-en-kö-för-riktningarna)
  &bull; [Hindra att lägga till samma riktning två gånger](#hindra-att-lägga-till-samma-riktning-två-gånger)
  &bull; [Slå över vid skärmkanten](#slå-över-vid-skärmkanten)
  &bull; [Rita maten](#rita-maten)
  &bull; [Förenkla koden](#förenkla-koden)
  &bull; [Äta maten](#äta-maten)
  &bull; [Förenkla koden](#förenkla-koden-1)
  &bull; [Flytta maten till en ledig ruta](#flytta-maten-till-en-ledig-ruta)
  &bull; [Game over](#game-over)
  &bull; [Återställa spelet](#återställa-spelet)
  &bull; [Pausa efter att ormen har kraschat](#pausa-efter-att-ormen-har-kraschat)
  &bull; [Ändra ormens färg när den är död](#ändra-ormens-färg-när-den-är-död)

**[Uppgifter](#uppgifter)**

**[Källor](#källor)**

![image](https://user-images.githubusercontent.com/4598641/226439115-c9800ff9-c916-406c-9efb-39407658988a.png)

## Jobba så här
Vägen till ett färdigt projekt är en pusselbit i taget 🧩. Gör därför ett avsnitt i taget uppifrån och ner.
- Få det att fungera innan ni går vidare till nästa avsnitt. 
- Testkör ofta, efter varje avsnitt eller ännu oftare.

Var det något i beskrivningen som var svårt att förstå? Ta med det i redovisningen. 📝

✏️ Pennan visar något du ska göra. Mata in koden och testkör innan du går vidare. 
**Fungerar det inte? Rätta koden innan du fortsätter till nästa ändring.**

# Regler
När ormen äter mat, växer den. När en matbit är uppäten, dyker det upp en ny på en slumpmässig position.

Ormen kommer att slå över till andra sidan skärmen när den går utanför kanten.

Spelet är över när ormen slingrar in i sig själv.

## Kontroller
**Piltangenter**	Byt riktning

# Översikt
Ormen är en variabel som har en lista av X- och Y-koordinater.

Maten representeras av en X- och Y-koordinat.

Ormen rör sig i två steg:
1. Vi tar bort det sista elementet i listan &ndash; det är den gamla svanspositionen.
2. Sen lägger vi till ett nytt element först i listan. Det är den nya positionen för huvudet i den riktning som ormen ringlar.

Så här kan det se ut Pygames koordinatsystem. Y-axeln är upp och ner jämfört med vanliga koordinataxlar. Det är vanligt i datorgrafik. [Scratch](https://scratch.mit.edu) är ett undantag.
<br>
<img src="https://github.com/user-attachments/assets/a771cf43-c8a9-4f59-82ae-16955d1fbd44" width="50%">

Jämför med hur det ser ut i koden. Verkar det stämma?<br>
![image](https://user-images.githubusercontent.com/4598641/226439258-020b4582-5409-448b-99e6-55cf6a4bbcdc.png)

![image](https://user-images.githubusercontent.com/4598641/226439284-599e5e4f-6987-4eea-8b79-f39d5a2d850a.png)

När ormen når maten är huvudpositionen samma som matens position.
Då rör vi inte svansen utan flyttar maten till en slumpmässig ruta där ormen inte är.

![image](https://user-images.githubusercontent.com/4598641/226439323-b54bc813-62f2-49d5-ac3b-1002ba0de713.png)

Ormen har krockat med sig själv om den nya huvudpositionen är samma som något av ormens andra segment. Då är spelet över.

# Kodning
## Rita bakgrunden
Spelområdet är 20 celler brett och 15 celler högt, och varje cell har en sidolängd på 15 pixlar.

En rektangel ritas för bakgrunden.

✏️ Se till att du är inloggad i repl.it. Öppna startprojektet https://replit.com/@RobertStorlind/snake-starter och spara en egen kopia med knappen "Fork".

✏️ Kör koden med Run i repl.it. Ser det ut så här?

![image](https://user-images.githubusercontent.com/4598641/235364441-4af78ea9-4545-40d5-af52-ed10b6de87da.png)

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
from pygame import Rect

# Globala variabler här nedanför

# Funktioner här nedanför
def draw():
    screen.fill((0, 0, 0))
    
    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15
    
    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70)
    )
  
# Kod för att starta appen här nedanför

pgzrun.go() # måste vara sista raden
```
</details>

## Rita ormen
Ormens segment lagras som X- och Y-koordinater och ritas som rutor.

✏️ Uppdatera funktionen `draw` och testkör! Ritas ormen rätt?
>Raderna som ska ändras är markerade med 🐍

```python
import pgzrun

# Globala variabler här nedanför

# Funktioner här nedanför
def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    snake_segments = [ #nytt 🐍
        {'x': 2, 'y': 0}, #nytt 🐍
        {'x': 1, 'y': 0}, #nytt 🐍 
        {'x': 0, 'y': 0}, #nytt 🐍
    ] #nytt 🐍

    for segment in snake_segments: #nytt 🐍
        screen.draw.filled_rect( #nytt 🐍
            Rect( #nytt 🐍
                segment['x'] * cell_size, segment['y'] * cell_size, #nytt 🐍
                cell_size - 1, cell_size - 1 #nytt 🐍
            ), #nytt 🐍
            color=(165, 255, 81) #nytt 🐍
        ) #nytt 🐍

# Kod för att starta appen här nedanför

pgzrun.go()  # måste vara sista raden
```

✏️ Testkör koden. Så här ska det se ut.
>Fungerar det inte? Kolla dina ändringar noga.

![image](https://user-images.githubusercontent.com/4598641/226439469-a0bf9621-d2ff-4b38-810e-9a1be63b3324.png)


## Timer
Ormen kommer att röra sig var 0,15:e sekund.

En timervariabel börjar på 0 och ökar med `dt` för varje bildruta.

När timern är större eller lika med 0,15 återställs den till 0.

För närvarande skrivs 'tick' ut varje gång ormen ska röra sig.

✏️ Uppdatera koden. Testkör och kolla utskriften i terminalfönstret.

```python
import pgzrun

# Globala variabler här nedanför
timer = 0 #nytt 🐍

# Funktioner här nedanför
def update(dt): #nytt 🐍
    global timer #nytt 🐍
    
    timer += dt #nytt 🐍
    if timer >= 0.15: #nytt 🐍
        timer = 0 #nytt 🐍
        # Tillfälligt #nytt 🐍
        print('tick') #nytt 🐍

# etc.
```
✏️ Testkör och kolla utskriften i terminalfönstret. Ser det ut så här?

![image](https://user-images.githubusercontent.com/4598641/235365117-32e6a3e4-1427-450b-8f0b-8fddb4fb64ee.png)


<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun

# Globala variabler här nedanför
timer = 0

# Funktioner här nedanför


def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        # Temporary
        print("tick")


def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    snake_segments = [
        {"x": 2, "y": 0},
        {"x": 1, "y": 0},
        {"x": 0, "y": 0},
    ]

    for segment in snake_segments:
        screen.draw.filled_rect(
            Rect(
                segment["x"] * cell_size,
                segment["y"] * cell_size,
                cell_size - 1,
                cell_size - 1,
            ),
            color=(165, 255, 81),
        )


# Kod för att starta appen här nedanför

pgzrun.go()  # måste vara sista raden
```

</details>

## Flytta ormen åt höger
Nästa position för ormens huvud beräknas genom att öka  den nuvarande X-koordinaten för ormens huvud med 1. Det är första elementet i listan med ormens segment.
Detta nya segment läggs till i början av segmentlistan.

Det sista elementet i segmentlistan, ormens svans, tas bort.

Segmentlistan ändras i funktionen `update()`, så den flyttas till att vara global.

✏️ Uppdatera koden och testkör.

```python
import pgzrun

# Globala variabler här nedanför
snake_segments = [ #flyttad från funktionen draw() 🐍
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

# Funktioner här nedanför
def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        next_x_position = snake_segments[0]['x'] + 1  #nytt 🐍
        next_y_position = snake_segments[0]['y']  #nytt 🐍

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position}) #nytt 🐍
        snake_segments.pop() #nytt 🐍


def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    # snake_segments har flyttat 🐍

    for segment in snake_segments:
    # etc.
```
✏️ Testkör! Flyttar ormen i X-led?

![image](https://user-images.githubusercontent.com/4598641/226439549-4395b5df-c7f0-4a1f-9a91-921994eb1365.png)

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

# Funktioner här nedanför

def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        next_x_position = snake_segments[0]['x'] + 1
        next_y_position = snake_segments[0]['y']

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()


def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )
  
    for segment in snake_segments:
        screen.draw.filled_rect(
            Rect(
                segment['x'] * cell_size, segment['y'] * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=(165, 255, 81)
        )

# Kod för att starta appen här nedanför

pgzrun.go()  # måste vara sista raden
```
</details>

## Flytta ormen i alla fyra riktningar
Ormens nuvarande riktning lagras i en variabel och ändras med hjälp av piltangenterna.

Ormens nästa huvudposition ska ställas in beroende på denna riktning.

✏️ Uppdatera koden så här. Testkör med piltangenterna.
>Kom ihåg att först klicka i spelfönstret så kan din kod fånga upp tangenterna.

```python
import pgzrun

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction = 'right' #nytt 🐍

# Funktioner här nedanför

def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        next_x_position = snake_segments[0]['x'] #nytt 🐍
        next_y_position = snake_segments[0]['y']

        if direction == 'right': #nytt 🐍
            next_x_position += 1 #nytt 🐍
        elif direction == 'left': #nytt 🐍
            next_x_position -= 1 #nytt 🐍
        elif direction == 'down': #nytt 🐍
            next_y_position += 1 #nytt 🐍
        elif direction == 'up': #nytt 🐍
            next_y_position -= 1 #nytt 🐍

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()

def on_key_down(key): #nytt 🐍
    global direction #nytt 🐍

    if key == keys.RIGHT: #nytt 🐍
        direction = 'right' #nytt 🐍
    elif key == keys.LEFT: #nytt 🐍
        direction = 'left' #nytt 🐍
    elif key == keys.DOWN: #nytt 🐍
        direction = 'down' #nytt 🐍
    elif key == keys.UP: #nytt 🐍
        direction = 'up' #nytt 🐍

# etc.
```

✏️ Testkör med piltangenterna.

![image](https://user-images.githubusercontent.com/4598641/226439597-2d0fded6-4174-4bbb-8dc1-9f3499761701.png)

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction = 'right'

# Funktioner här nedanför

def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction == 'right':
            next_x_position += 1
        elif direction == 'left':
            next_x_position -= 1
        elif direction == 'down':
            next_y_position += 1
        elif direction == 'up':
            next_y_position -= 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()


def on_key_down(key):
    global direction

    if key == keys.RIGHT:
        direction = 'right'
    elif key == keys.LEFT:
        direction = 'left'
    elif key == keys.DOWN:
        direction = 'down'
    elif key == keys.UP:
        direction = 'up'


def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    for segment in snake_segments:
        screen.draw.filled_rect(
            Rect(
                segment['x'] * cell_size, segment['y'] * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=(165, 255, 81)
        )

# Kod för att starta appen här nedanför

pgzrun.go()  # måste vara sista raden
```
    
</details>

## Hindra att ormen rör sig rakt bakåt
Ormen ska inte kunna röra sig i motsatt riktning som den för närvarande går. När den t.ex. går åt höger ska den inte direkt gå åt vänster. Detta kontrolleras innan riktningen ställs in.

✏️ Uppdatera funktionen `on_key_down()`. 

```python
def on_key_down(key):
    global direction

    if key == keys.RIGHT and direction != 'left': #ändrat 🐍
        direction = 'right'
    elif key == keys.LEFT and direction != 'right': #ändrat 🐍
        direction = 'left'
    elif key == keys.DOWN and direction != 'up': #ändrat 🐍
        direction = 'down'
    elif key == keys.UP and direction != 'down': #ändrat 🐍
        direction = 'up'
```

✏️ Testkör svängar med piltangenterna och kolla om det blev rätt!

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction = 'right'

# Funktioner här nedanför


def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction == 'right':
            next_x_position += 1
        elif direction == 'left':
            next_x_position -= 1
        elif direction == 'down':
            next_y_position += 1
        elif direction == 'up':
            next_y_position -= 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()


def on_key_down(key):
    global direction

    if key == keys.RIGHT and direction != 'left':
        direction = 'right'
    elif key == keys.LEFT and direction != 'right':
        direction = 'left'
    elif key == keys.DOWN and direction != 'up':
        direction = 'down'
    elif key == keys.UP and direction != 'down':
        direction = 'up'


def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )
  
    for segment in snake_segments:
        screen.draw.filled_rect(
            Rect(
                segment['x'] * cell_size, segment['y'] * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=(165, 255, 81)
        )

# Kod för att starta appen här nedanför


pgzrun.go()  # måste vara sista raden
```
    
</details>

## Använd en kö för riktningarna
Just nu kan ormen fortfarande gå bakåt om en annan riktning och sedan den motsatta riktningen trycks inom ett enda tick på timern.
Om ormen t.ex. slingrade åt höger på den sista ticket och du trycker neråtpil och sedan vänsterpil före nästa tick, kommer ormen att flytta åt vänster på nästa tick.

Dessutom kan spelaren vilja ge flera anvisningar inom ett enda tick. I exemplet ovan kan spelaren ha velat att ormen skulle flytta neråt ett tick och sen vänster nästa tick.

Därför behöver vi en riktningskö. Det första elementet i kön är riktningen som ormen kommer att röra sig vid nästa tick.

Om  riktningskön har mer än ett element, tas det första elementet bort vid varje tick.

När en knapp trycks ned läggs riktningen till i slutet av riktningskön.

Det sista elementet i riktningskön, alltså den senast tryckta riktningen, kontrolleras för att se om att inte är i motsatt riktning mot den nya riktningen innan den nya riktningen läggs till i riktningskön.

✏️ Uppdatera koden.

```python
import pgzrun

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right'] #ändrat 🐍

# Funktioner här nedanför
def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1: #nytt 🐍
            direction_queue.pop(0) #nytt 🐍

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right': #ändrat 🐍
            next_x_position += 1
        elif direction_queue[0] == 'left': #ändrat 🐍
            next_x_position -= 1
        elif direction_queue[0] == 'down': #ändrat 🐍
            next_y_position += 1
        elif direction_queue[0] == 'up': #ändrat 🐍
            next_y_position -= 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()


def on_key_down(key):
    if (key == keys.RIGHT and direction_queue[-1] != 'left'): #ändrat 🐍
        direction_queue.append('right') #nytt 🐍

    elif (key == keys.LEFT and direction_queue[-1] != 'right'): #ändrat 🐍
        direction_queue.append('left') #nytt 🐍

    elif (key == keys.DOWN and direction_queue[-1] != 'up'): #ändrat 🐍
        direction_queue.append('down') #nytt 🐍

    elif (key == keys.UP #ändrat 🐍
          and direction_queue[-1] != 'down'): #ändrat 🐍
        direction_queue.append('up') #nytt 🐍


def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )
    
    for segment in snake_segments:
        screen.draw.filled_rect(
            Rect(
                segment['x'] * cell_size, segment['y'] * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=(165, 255, 81)
        )

    # Tillfälligt #nytt 🐍
    for direction_index, direction in enumerate(direction_queue): #nytt 🐍
        screen.draw.text( #nytt 🐍
            f"direction_queue[{direction_index}]: {direction}", #nytt 🐍
            (15, 15 + 15 * direction_index)) #nytt 🐍

# Kod för att starta appen här nedanför
pgzrun.go()  # måste vara sista raden

```
✏️ Testkör &ndash; vad händer när du trycker snabbt på de olika piltangenterna?

![image](https://user-images.githubusercontent.com/4598641/226439688-1765d719-ee76-4b94-be2f-d8760ced80d7.png)

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right']

# Funktioner här nedanför


def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
        elif direction_queue[0] == 'left':
            next_x_position -= 1
        elif direction_queue[0] == 'down':
            next_y_position += 1
        elif direction_queue[0] == 'up':
            next_y_position -= 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()


def on_key_down(key):
    if (key == keys.RIGHT
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    for segment in snake_segments:
        screen.draw.filled_rect(
            Rect(
                segment['x'] * cell_size, segment['y'] * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=(165, 255, 81)
        )

    # Tillfälligt
    for direction_index, direction in enumerate(direction_queue):
        screen.draw.text(
            f"direction_queue[{direction_index}]: {direction}",
            (15, 15 + 15 * direction_index))

# Kod för att starta appen här nedanför


pgzrun.go()  # måste vara sista raden
```
</details>

## Hindra att lägga till samma riktning två gånger
Om den senaste riktningen är samma som den nya riktningen, ska den nya riktningen inte läggas till i riktningskön.

✏️ Uppdaterad 🐍-raderna i `on_key_down()`. 

```python
def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right' #nytt 🐍
        and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left' #nytt 🐍
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down' #nytt 🐍
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up' #nytt 🐍
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')
```

Testkör &ndash; vad händer när du  snabbt trycker på olika piltangenter, eller samma piltangent?

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right']

# Funktioner här nedanför

def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
        elif direction_queue[0] == 'left':
            next_x_position -= 1
        elif direction_queue[0] == 'down':
            next_y_position += 1
        elif direction_queue[0] == 'up':
            next_y_position -= 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
        and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    for segment in snake_segments:
        screen.draw.filled_rect(
            Rect(
                segment['x'] * cell_size, segment['y'] * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=(165, 255, 81)
        )

    # Tillfälligt
    for direction_index, direction in enumerate(direction_queue):
        screen.draw.text(
            f"direction_queue[{direction_index}]: {direction}",
            (15, 15 + 15 * direction_index))

# Kod för att starta appen här nedanför


pgzrun.go()  # måste vara sista raden    
```
</details>

## Slå över vid skärmkanten
Om nästa position skulle vara utanför rutnätet, slår vi över till motsatta sidan på skärmen.

Rutnätets X/Y-storlek återanvänds från att rita bakgrunden, så de variablerna flyttas till att vara globala.

✏️ Uppdatera/flytta raderna med 🐍 så att det blir så här:

```python
# etc.

grid_x_count = 20 #flyttad 🐍
grid_y_count = 15 #flyttad 🐍

def update(dt):
    # etc.

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count: #nytt 🐍
                next_x_position = 0 #nytt 🐍

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0: #nytt 🐍
                next_x_position = grid_x_count - 1 #nytt 🐍

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count: #nytt 🐍
                next_y_position = 0 #nytt 🐍

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0: #nytt 🐍
                next_y_position = grid_y_count - 1 #nytt 🐍

    # etc.

def draw():
    # Flyttat: grid_x_count = 20
    # Flyttat: grid_y_count = 15
```
✏️ Testkör &ndash; vad händer när ormen når kanten?

![image](https://user-images.githubusercontent.com/4598641/226439789-ce8299ae-1e6c-449b-9dc0-6c64b6124c6f.png)

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15

# Funktioner här nedanför


def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count:
                next_x_position = 0

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0:
                next_x_position = grid_x_count - 1

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count:
                next_y_position = 0

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    for segment in snake_segments:
        screen.draw.filled_rect(
            Rect(
                segment['x'] * cell_size, segment['y'] * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=(165, 255, 81)
        )

    # Tillfälligt
    for direction_index, direction in enumerate(direction_queue):
        screen.draw.text(
            f"direction_queue[{direction_index}]: {direction}",
            (15, 15 + 15 * direction_index))

# Kod för att starta appen här nedanför


pgzrun.go()  # måste vara sista raden
```

</details>


## Rita maten
Maten lagras som ett talpar med X- och Y-koordinater. Maten ritas som en kvadrat.

Slumpmodulen importeras så att `random.randint` kan användas.

✏️ Uppdatera koden.

```python
import pgzrun
import random #nytt 🐍

# etc.

grid_x_count = 20
grid_y_count = 15

food_position = { #nytt 🐍
    'x': random.randint(0, grid_x_count - 1), #nytt 🐍
    'y': random.randint(0, grid_y_count - 1), #nytt 🐍
} #nytt 🐍

# etc.

def draw():
    # etc.

    screen.draw.filled_rect( #nytt 🐍
        Rect( #nytt 🐍
            food_position['x'] * cell_size, food_position['y'] * cell_size, #nytt 🐍
            cell_size - 1, cell_size - 1 #nytt 🐍
        ), #nytt 🐍
        color=(255, 76, 76) #nytt 🐍
    ) #nytt 🐍 
```
✏️ Testkör &ndash; ritas maten?

![image](https://user-images.githubusercontent.com/4598641/226439842-6fae488e-e72d-494c-bad4-9204c860144a.png)
<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
import random

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15

food_position = {
    'x': random.randint(0, grid_x_count - 1),
    'y': random.randint(0, grid_y_count - 1),
}

# Funktioner här nedanför


def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count:
                next_x_position = 0

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0:
                next_x_position = grid_x_count - 1

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count:
                next_y_position = 0

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(
            0, 0,
            grid_x_count * cell_size, grid_y_count * cell_size
        ),
        color=(70, 70, 70)
    )

    for segment in snake_segments:
        screen.draw.filled_rect(
            Rect(
                segment['x'] * cell_size, segment['y'] * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=(165, 255, 81)
        )

    screen.draw.filled_rect(
        Rect(
            food_position['x'] * cell_size, food_position['y'] * cell_size,
            cell_size - 1, cell_size - 1
        ),
        color=(255, 76, 76)
    )

# Kod för att starta appen här nedanför

pgzrun.go()  # måste vara sista raden
```
</details>

## Förenkla koden

Koden för att rita en orms segment och rita maten är samma, förutom färgen. 
Vi gör det till en funktion med färgen som parameter.

✏️ Uppdatera de markerade raderna i `draw`.

```python
def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    def draw_cell(x, y, color): #nytt 🐍
        screen.draw.filled_rect( #nytt 🐍
            Rect( #nytt 🐍
                x * cell_size, y * cell_size, #nytt 🐍
                cell_size - 1, cell_size - 1 #nytt 🐍
            ), #nytt 🐍
            color=color #nytt 🐍
        ) #nytt 🐍

    for segment in snake_segments:
        draw_cell(segment['x'], segment['y'], color=(165, 255, 81)) #nytt 🐍

    draw_cell(food_position['x'], food_position['y'], (255, 76, 76)) #nytt 🐍
```
✏️ Testkör &ndash; fungerar koden som innan vi förenklade den?

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
import random

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15

food_position = {
    'x': random.randint(0, grid_x_count - 1),
    'y': random.randint(0, grid_y_count - 1),
}

# Funktioner här nedanför


def update(dt):
    global timer

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count:
                next_x_position = 0

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0:
                next_x_position = grid_x_count - 1

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count:
                next_y_position = 0

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})
        snake_segments.pop()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    def draw_cell(x, y, color):
        screen.draw.filled_rect(
            Rect(
                x * cell_size, y * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=color
        )

    for segment in snake_segments:
        draw_cell(segment['x'], segment['y'], color=(165, 255, 81))

    draw_cell(food_position['x'], food_position['y'], (255, 76, 76))

# Kod för att starta appen här nedanför

pgzrun.go()  # måste vara sista raden

```
</details>


## Äta maten
    
Om ormens nya huvudposition är samma som matens position, tas inte ormens svans bort och maten får en ny slumpmässig position. 
    
På så vis blir ormen en ruta längre.

✏️ Uppdatera raderna i `update()`.

  ```python
def update(dt):
    global timer, food_position #ändrat 🐍

    timer += dt
    if timer >= 0.15:
        timer = 0

        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count:
                next_x_position = 0

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0:
                next_x_position = grid_x_count - 1

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count:
                next_y_position = 0

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})

        if (snake_segments[0]['x'] == food_position['x'] #nytt 🐍
        and snake_segments[0]['y'] == food_position['y']): #nytt 🐍
            food_position = { #nytt 🐍
                'x': random.randint(0, grid_x_count - 1), #nytt 🐍
                'y': random.randint(0, grid_y_count - 1), #nytt 🐍
            } #nytt 🐍
        else: #nytt 🐍
            snake_segments.pop() #dra in raden 🐍
```
                                   
✏️ Testkör &ndash; vad händer när ormen äter?

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
import random

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15

food_position = {
    'x': random.randint(0, grid_x_count - 1),
    'y': random.randint(0, grid_y_count - 1),
}

# Funktioner här nedanför


def update(dt):
    global timer, food_position

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count:
                next_x_position = 0

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0:
                next_x_position = grid_x_count - 1

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count:
                next_y_position = 0

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})

        if (snake_segments[0]['x'] == food_position['x']
                and snake_segments[0]['y'] == food_position['y']):
            food_position = {
                'x': random.randint(0, grid_x_count - 1),
                'y': random.randint(0, grid_y_count - 1),
            }
        else:
            snake_segments.pop()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    def draw_cell(x, y, color):
        screen.draw.filled_rect(
            Rect(
                x * cell_size, y * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=color
        )

    for segment in snake_segments:
        draw_cell(segment['x'], segment['y'], color=(165, 255, 81))

    draw_cell(food_position['x'], food_position['y'], (255, 76, 76))

# Kod för att starta appen här nedanför

pgzrun.go()  # måste vara sista raden
```

</details>

## Förenkla koden
Koden för att lägga maten på en slumpmässig position behövs på fler ställen. Vi gör en funktion för det.
    
✏️ Uppdatera koden.

```python
def move_food(): #nytt 🐍
    global food_position #nytt 🐍

    food_position = { #flyttat 🐍
        'x': random.randint(0, grid_x_count - 1), #flyttat 🐍
        'y': random.randint(0, grid_y_count - 1) #flyttat 🐍
    } #flyttat 🐍

def update(dt):
    # etc.

        if (snake_segments[0]['x'] == food_position['x']
        and snake_segments[0]['y'] == food_position['y']):
            move_food() #nytt 🐍
        else:
            snake_segments.pop()

    # etc.
    
# Kod för att starta appen här nedanför
move_food() #nytt 🐍

pgzrun.go()  # måste vara sista raden
```
✏️ Testkör &ndash; fungerar koden lika bra som innan?

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
import random

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15

food_position = {
    'x': random.randint(0, grid_x_count - 1),
    'y': random.randint(0, grid_y_count - 1),
}

# Funktioner här nedanför
def move_food():
    global food_position

    food_position = {
        'x': random.randint(0, grid_x_count - 1),
        'y': random.randint(0, grid_y_count - 1)
    }

def update(dt):
    global timer, food_position

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count:
                next_x_position = 0

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0:
                next_x_position = grid_x_count - 1

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count:
                next_y_position = 0

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})

        if (snake_segments[0]['x'] == food_position['x']
                and snake_segments[0]['y'] == food_position['y']):
            move_food()
        else:
            snake_segments.pop()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    def draw_cell(x, y, color):
        screen.draw.filled_rect(
            Rect(
                x * cell_size, y * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=color
        )

    for segment in snake_segments:
        draw_cell(segment['x'], segment['y'], color=(165, 255, 81))

    draw_cell(food_position['x'], food_position['y'], (255, 76, 76))

# Kod för att starta appen här nedanför
move_food()

pgzrun.go()  # måste vara sista raden

```
</details>

## Flytta maten till en ledig ruta
    
Istället för att flytta maten till en slumpmässig plats, flyttar den till en plats där ormen inte är just nu.
    
Alla positioner i rutnätet loopas igenom. För varje rutnätsposition loopar vi igenom alla ormens segment.
Om inga ormsegment är på en viss rutnätsposition, läggs den positionen till i en lista över möjliga matpositioner.
Nästa matposition väljs sen slumpmässigt från den listan.

✏️ Uppdatera kodraderna i `move_food`. 

```python
def move_food():
    global food_position

    possible_food_positions = [] #nytt 🐍

    for food_x in range(grid_x_count): #nytt 🐍
        for food_y in range(grid_y_count): #nytt 🐍
            possible = True #nytt 🐍

            for segment in snake_segments: #nytt 🐍
                if food_x == segment['x'] and food_y == segment['y']: #nytt 🐍
                    possible = False #nytt 🐍

            if possible: #nytt 🐍
                possible_food_positions.append({'x': food_x, 'y': food_y}) #nytt 🐍

    food_position = random.choice(possible_food_positions) #nytt 🐍
```

✏️ Testkör &ndash; vad händer när ormen äter?

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
import random

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15

# Funktioner här nedanför


def move_food():
    global food_position

    possible_food_positions = []

    for food_x in range(grid_x_count):
        for food_y in range(grid_y_count):
            possible = True

            for segment in snake_segments:
                if food_x == segment['x'] and food_y == segment['y']:
                    possible = False

            if possible:
                possible_food_positions.append({'x': food_x, 'y': food_y})

    food_position = random.choice(possible_food_positions)


def update(dt):
    global timer, food_position

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count:
                next_x_position = 0

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0:
                next_x_position = grid_x_count - 1

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count:
                next_y_position = 0

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1

        snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})

        if (snake_segments[0]['x'] == food_position['x']
                and snake_segments[0]['y'] == food_position['y']):
            move_food()
        else:
            snake_segments.pop()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    def draw_cell(x, y, color):
        screen.draw.filled_rect(
            Rect(
                x * cell_size, y * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=color
        )

    for segment in snake_segments:
        draw_cell(segment['x'], segment['y'], color=(165, 255, 81))

    draw_cell(food_position['x'], food_position['y'], (255, 76, 76))


# Kod för att starta appen här nedanför
move_food()

pgzrun.go()  # måste vara sista raden
```
</details>

## Game over
    
Ormens segment loopas igenom. Om något av dem &ndash; förutom det sista &ndash; är i samma position som ormens nya huvudposition, så har ormen kraschat in i sig själv.
>Det sista segmentet på ormen ska inte kollas eftersom det kommer att tas bort inom samma tick.

För närvarande skrivs `collision` ut när ormen kraschar in i sig själv. 

✏️ Uppdatera i funktionen `update`.
  
```python
def update(dt):
    # etc.
        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1    
    
        can_move = True #nytt 🐍

        for segment in snake_segments[:-1]: #nytt 🐍
            if (next_x_position == segment['x'] #nytt 🐍
            and next_y_position == segment['y']): #nytt 🐍
                can_move = False #nytt 🐍

        if can_move: #nytt 🐍
            snake_segments.insert(0, {'x': next_x_position, 'y': next_y_position})

            if (snake_segments[0]['x'] == food_position['x']
            and snake_segments[0]['y'] == food_position['y']):
                move_food()
            else:
                snake_segments.pop()
        else: #nytt 🐍
            print('collision') #nytt 🐍    
```
✏️ Testkör &ndash; vad händer när ormen kraschar in sig själv? Kolla i det svarta terminalfönstret (Console) nere till höger.
                                   
<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
import random

# Globala variabler här nedanför
snake_segments = [
    {'x': 2, 'y': 0},
    {'x': 1, 'y': 0},
    {'x': 0, 'y': 0},
]

timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15

# Funktioner här nedanför


def move_food():
    global food_position

    possible_food_positions = []

    for food_x in range(grid_x_count):
        for food_y in range(grid_y_count):
            possible = True

            for segment in snake_segments:
                if food_x == segment['x'] and food_y == segment['y']:
                    possible = False

            if possible:
                possible_food_positions.append({'x': food_x, 'y': food_y})

    food_position = random.choice(possible_food_positions)


def update(dt):
    global timer, food_position

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count:
                next_x_position = 0

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0:
                next_x_position = grid_x_count - 1

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count:
                next_y_position = 0

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1

        can_move = True

        for segment in snake_segments[:-1]:
            if (next_x_position == segment['x']
                    and next_y_position == segment['y']):
                can_move = False

        if can_move:
            snake_segments.insert(
                0, {'x': next_x_position, 'y': next_y_position})

            if (snake_segments[0]['x'] == food_position['x']
                    and snake_segments[0]['y'] == food_position['y']):
                move_food()
            else:
                snake_segments.pop()
        else:
            print('collision')


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    def draw_cell(x, y, color):
        screen.draw.filled_rect(
            Rect(
                x * cell_size, y * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=color
        )

    for segment in snake_segments:
        draw_cell(segment['x'], segment['y'], color=(165, 255, 81))

    draw_cell(food_position['x'], food_position['y'], (255, 76, 76))


# Kod för att starta appen här nedanför
move_food()

pgzrun.go()  # måste vara sista raden
```
</details>

## Återställa spelet
Vi gör en funktion som ställer in spelets startläge.

Funktionen anropas innan spelet börjar och när ormen kraschar.

✏️ Lägg till funktionen `reset()` och gör de andra småändringarna på markerade rader.

```python
grid_x_count = 20
grid_y_count = 15

def move_food():
    # etc.

def reset():
    global timer, direction_queue, snake_segments #nytt 🐍

    timer = 0
    direction_queue = ['right']
    snake_segments = [
        {'x': 2, 'y': 0},
        {'x': 1, 'y': 0},
        {'x': 0, 'y': 0},
    ]
    move_food()

def update(dt):
    # etc.

        if can_move:
            # etc.
        else:
            reset() #nytt 🐍

# etc.

# Kod för att starta appen här nedanför
reset() #ändrat 🐍

pgzrun.go()  # måste vara sista raden
    
```

✏️ Testkör &ndash; vad händer när ormen kraschar?

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
import random

# Globala variabler här nedanför
timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15


# Funktioner här nedanför


def move_food():
    global food_position

    possible_food_positions = []

    for food_x in range(grid_x_count):
        for food_y in range(grid_y_count):
            possible = True

            for segment in snake_segments:
                if food_x == segment['x'] and food_y == segment['y']:
                    possible = False

            if possible:
                possible_food_positions.append({'x': food_x, 'y': food_y})

    food_position = random.choice(possible_food_positions)


def reset():
    global timer, direction_queue, snake_segments  # nytt 🐍

    timer = 0
    direction_queue = ['right']
    snake_segments = [
        {'x': 2, 'y': 0},
        {'x': 1, 'y': 0},
        {'x': 0, 'y': 0},
    ]
    move_food()


def update(dt):
    global timer, food_position

    timer += dt
    if timer >= 0.15:
        timer = 0
        if len(direction_queue) > 1:
            direction_queue.pop(0)

        next_x_position = snake_segments[0]['x']
        next_y_position = snake_segments[0]['y']

        if direction_queue[0] == 'right':
            next_x_position += 1
            if next_x_position >= grid_x_count:
                next_x_position = 0

        elif direction_queue[0] == 'left':
            next_x_position -= 1
            if next_x_position < 0:
                next_x_position = grid_x_count - 1

        elif direction_queue[0] == 'down':
            next_y_position += 1
            if next_y_position >= grid_y_count:
                next_y_position = 0

        elif direction_queue[0] == 'up':
            next_y_position -= 1
            if next_y_position < 0:
                next_y_position = grid_y_count - 1

        can_move = True

        for segment in snake_segments[:-1]:
            if (next_x_position == segment['x']
                    and next_y_position == segment['y']):
                can_move = False

        if can_move:
            snake_segments.insert(
                0, {'x': next_x_position, 'y': next_y_position})

            if (snake_segments[0]['x'] == food_position['x']
                    and snake_segments[0]['y'] == food_position['y']):
                move_food()
            else:
                snake_segments.pop()
        else:
            reset()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    def draw_cell(x, y, color):
        screen.draw.filled_rect(
            Rect(
                x * cell_size, y * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=color
        )

    for segment in snake_segments:
        draw_cell(segment['x'], segment['y'], color=(165, 255, 81))

    draw_cell(food_position['x'], food_position['y'], (255, 76, 76))


# Kod för att starta appen här nedanför
reset()

pgzrun.go()  # måste vara sista raden

```
</details>

## Pausa efter att ormen har kraschat
    
En variabel används för att lagra om ormen är vid liv eller inte. Om den är `False` (falskt) så har ormen kraschat.

Om ormen är död, väntar timern i 2 sekunder innan den anropar `reset()` för att starta om spelet.
    
✏️ Uppdatera koden på de markerade raderna.

```python
def reset():
    # etc.
    global snake_alive #nytt 🐍

    # etc.
    snake_alive = True #nytt 🐍

def update(dt):
    global timer, food_position, snake_alive #ändrat 🐍

    timer += dt

    if snake_alive: #nytt
        if timer >= 0.15:
            timer = 0

            # etc.

            if can_move:
                # etc.
            else:
                snake_alive = False #nytt 🐍

    elif timer >= 2: #nytt 🐍
        reset() #nytt 🐍
```
✏️ Testkör &ndash; vad händer när ormen kraschar?

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
import random

# Globala variabler här nedanför
timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15

    
# Funktioner här nedanför

def move_food():
    global food_position

    possible_food_positions = []

    for food_x in range(grid_x_count):
        for food_y in range(grid_y_count):
            possible = True

            for segment in snake_segments:
                if food_x == segment['x'] and food_y == segment['y']:
                    possible = False

            if possible:
                possible_food_positions.append({'x': food_x, 'y': food_y})

    food_position = random.choice(possible_food_positions)


def reset():
    global timer, direction_queue, snake_segments, snake_alive  # nytt 🐍

    timer = 0
    direction_queue = ['right']
    snake_segments = [
        {'x': 2, 'y': 0},
        {'x': 1, 'y': 0},
        {'x': 0, 'y': 0},
    ]
    move_food()
    snake_alive = True


def update(dt):
    global timer, food_position, snake_alive

    timer += dt
    if snake_alive:
        if timer >= 0.15:
            timer = 0
            if len(direction_queue) > 1:
                direction_queue.pop(0)

            next_x_position = snake_segments[0]['x']
            next_y_position = snake_segments[0]['y']

            if direction_queue[0] == 'right':
                next_x_position += 1
                if next_x_position >= grid_x_count:
                    next_x_position = 0

            elif direction_queue[0] == 'left':
                next_x_position -= 1
                if next_x_position < 0:
                    next_x_position = grid_x_count - 1

            elif direction_queue[0] == 'down':
                next_y_position += 1
                if next_y_position >= grid_y_count:
                    next_y_position = 0

            elif direction_queue[0] == 'up':
                next_y_position -= 1
                if next_y_position < 0:
                    next_y_position = grid_y_count - 1

            can_move = True

            for segment in snake_segments[:-1]:
                if (next_x_position == segment['x']
                        and next_y_position == segment['y']):
                    can_move = False

            if can_move:
                snake_segments.insert(
                    0, {'x': next_x_position, 'y': next_y_position})

                if (snake_segments[0]['x'] == food_position['x']
                        and snake_segments[0]['y'] == food_position['y']):
                    move_food()
                else:
                    snake_segments.pop()
            else:
                snake_alive = False
    elif timer >= 2:
        reset()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    def draw_cell(x, y, color):
        screen.draw.filled_rect(
            Rect(
                x * cell_size, y * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=color
        )

    for segment in snake_segments:
        draw_cell(segment['x'], segment['y'], color=(165, 255, 81))

    draw_cell(food_position['x'], food_position['y'], (255, 76, 76))


# Kod för att starta appen här nedanför
reset()

pgzrun.go()  # måste vara sista raden
```

</details>

## Ändra ormens färg när den är död

Ormens färg ändras beroende på om den är vid liv eller inte.

✏️ Uppdatera kodraderna i `draw()`.

```python
def draw():
    # etc.

    for segment in snake_segments:
        color = (165, 255, 81)
        if not snake_alive:
            color = (140, 140, 140)
        draw_cell(segment['x'], segment['y'], color)

    # etc.    
```
✏️ Testkör &ndash; vad händer när ormen dör?


![image](https://user-images.githubusercontent.com/4598641/226440133-a580b309-3b49-400d-ab5b-97c545c75ecd.png)
<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
import pgzrun
import random

# Globala variabler här nedanför
timer = 0

direction_queue = ['right']

grid_x_count = 20
grid_y_count = 15

# Funktioner här nedanför

def move_food():
    global food_position

    possible_food_positions = []

    for food_x in range(grid_x_count):
        for food_y in range(grid_y_count):
            possible = True

            for segment in snake_segments:
                if food_x == segment['x'] and food_y == segment['y']:
                    possible = False

            if possible:
                possible_food_positions.append({'x': food_x, 'y': food_y})

    food_position = random.choice(possible_food_positions)


def reset():
    global timer, direction_queue, snake_segments, snake_alive  # nytt 🐍

    timer = 0
    direction_queue = ['right']
    snake_segments = [
        {'x': 2, 'y': 0},
        {'x': 1, 'y': 0},
        {'x': 0, 'y': 0},
    ]
    move_food()
    snake_alive = True


def update(dt):
    global timer, food_position, snake_alive

    timer += dt
    if snake_alive:
        if timer >= 0.15:
            timer = 0
            if len(direction_queue) > 1:
                direction_queue.pop(0)

            next_x_position = snake_segments[0]['x']
            next_y_position = snake_segments[0]['y']

            if direction_queue[0] == 'right':
                next_x_position += 1
                if next_x_position >= grid_x_count:
                    next_x_position = 0

            elif direction_queue[0] == 'left':
                next_x_position -= 1
                if next_x_position < 0:
                    next_x_position = grid_x_count - 1

            elif direction_queue[0] == 'down':
                next_y_position += 1
                if next_y_position >= grid_y_count:
                    next_y_position = 0

            elif direction_queue[0] == 'up':
                next_y_position -= 1
                if next_y_position < 0:
                    next_y_position = grid_y_count - 1

            can_move = True

            for segment in snake_segments[:-1]:
                if (next_x_position == segment['x']
                        and next_y_position == segment['y']):
                    can_move = False

            if can_move:
                snake_segments.insert(
                    0, {'x': next_x_position, 'y': next_y_position})

                if (snake_segments[0]['x'] == food_position['x']
                        and snake_segments[0]['y'] == food_position['y']):
                    move_food()
                else:
                    snake_segments.pop()
            else:
                snake_alive = False
    elif timer >= 2:
        reset()


def on_key_down(key):
    if (key == keys.RIGHT
        and direction_queue[-1] != 'right'
            and direction_queue[-1] != 'left'):
        direction_queue.append('right')

    elif (key == keys.LEFT
          and direction_queue[-1] != 'left'
          and direction_queue[-1] != 'right'):
        direction_queue.append('left')

    elif (key == keys.DOWN
          and direction_queue[-1] != 'down'
          and direction_queue[-1] != 'up'):
        direction_queue.append('down')

    elif (key == keys.UP
          and direction_queue[-1] != 'up'
          and direction_queue[-1] != 'down'):
        direction_queue.append('up')


def draw():
    screen.fill((0, 0, 0))

    cell_size = 15

    screen.draw.filled_rect(
        Rect(0, 0, grid_x_count * cell_size, grid_y_count * cell_size),
        color=(70, 70, 70),
    )

    def draw_cell(x, y, color):
        screen.draw.filled_rect(
            Rect(
                x * cell_size, y * cell_size,
                cell_size - 1, cell_size - 1
            ),
            color=color
        )

    for segment in snake_segments:
        color = (165, 255, 81)
        if not snake_alive:
            color = (140, 140, 140)
        draw_cell(segment['x'], segment['y'], color)

    draw_cell(food_position['x'], food_position['y'], (255, 76, 76))

# Kod för att starta appen här nedanför
reset()

pgzrun.go()  # måste vara sista raden
```
</details>

# Uppgifter
## 1. Utvärdera ert eget arbete!
När ni svarar på detta, tänk er att *ni har tillgång till uppgiften* &ndash; ni behöver alltså inte kunna koden utantill.

**1A.** De här delarna av uppgiften har vi gjort. Vi förstår dem och kan förklara koden för Susanne eller inför klassen.

**1B.** De här delarna av uppgiften har vi gjort *men vi förstår dem inte till 100%*. Ge exempel på något ni inte förstår.

## 2. Gör spelet ännu bättre
Pröva att göra några ändringar eller tillägg. Det kan vara en poängräknare för ormens längd, high score, roligare game over, en introskärm eller något annat som är standard i den här typen av spel.
- Beskriv kort vad förändringen är och hur den ska fungera.
- Beskriv hur du fick ändra koden för att göra ändringen. Glöm inte kodexempel.
- Om det inte gick att genomföra, förklara med några meningar vad du försökte och vad som hände. Glöm inte kodexempel.

## 3. Be någon annan att testa ditt projekt
- Be om konstruktiv kritik på spelet. Skriv ner
- Be om konstruktiv kritik på koden. Skriv ner
- Har du något tips att för någon som inte har gjort det här projektet innan? Skriv ner
  
# Källor

https://simplegametutorials.github.io/pygamezero/snake/

https://web.archive.org/web/20140820192218/http://www.realtid.se/ArticlePages/200603/01/20060301132710_Realtid437/20060301132710_Realtid437.dbp.asp?Action=Print
