# Snake ⭐⭐⭐
## En handledning för Python och Pygame Zero 1.2

![image](https://user-images.githubusercontent.com/4598641/226439115-c9800ff9-c916-406c-9efb-39407658988a.png)

# Regler
Att äta mat gör att ormen växer. När maten är uppäten flyttas den till en annan slumpmässig position.

Ormen kommer att svepa runt till andra sidan av skärmen när den går av kanten.

Spelet är över när ormen kraschar in i sig själv.

## Kontroller
**Piltangenter**	Byt riktning

# Översikt
Ormen representeras av en sekvens av X- och Y-positioner.

Maten representeras av en enda X- och Y-position.

När ormen rör sig, tas det sista föremålet i sekvensen (dvs. dess gamla svansposition) bort, och ett föremål läggs till framtill (dvs. dess nya huvudposition) i den riktning som ormen går.

![image](https://user-images.githubusercontent.com/4598641/226439258-020b4582-5409-448b-99e6-55cf6a4bbcdc.png)

![image](https://user-images.githubusercontent.com/4598641/226439284-599e5e4f-6987-4eea-8b79-f39d5a2d850a.png)

Om den nya huvudpositionen är i samma position som matens position tas inte ormens svans bort, och maten flyttas till en slumpmässig position som inte upptas av ormen.

![image](https://user-images.githubusercontent.com/4598641/226439323-b54bc813-62f2-49d5-ac3b-1002ba0de713.png)

Om den nya huvudpositionen är i samma position som någon av ormens andra segment, är spelet över.

# Kodning
## Rita bakgrunden
Spelområdet är 20 celler brett och 15 celler högt, och varje cell har en sidolängd på 15 pixlar.

En rektangel ritas för bakgrunden.

✏️ Se till att du är inloggad i repl.it. Öppna startprojektet https://replit.com/@RobertStorlind/snake-starter och spara en egen kopia med knappen "Fork".

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
        Rect(
            0, 0,
            grid_x_count * cell_size, grid_y_count * cell_size
        ),
        color=(70, 70, 70)
    )
  
# Kod för att starta appen här nedanför

pgzrun.go() # måste vara sista raden
```

![image](https://user-images.githubusercontent.com/4598641/226439410-a04eb468-d4cf-4b10-9916-02534cea3a3d.png)

## Rita ormen
Ormens segment lagras som X- och Y-positioner och ritas som rutor.

Uppdatera funktionen `draw` och testkör!

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
        Rect(
            0, 0,
            grid_x_count * cell_size, grid_y_count * cell_size
        ),
        color=(70, 70, 70)
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

![image](https://user-images.githubusercontent.com/4598641/226439469-a0bf9621-d2ff-4b38-810e-9a1be63b3324.png)

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>


## Timer
Ormen kommer att röra sig var 0,15:e sekund.

En timervariabel börjar på 0 och ökar med dt för varje bildruta.

När timern är större eller lika med 0,15 återställs den till 0.

För närvarande skrivs 'tick' ut varje gång ormen ska röra sig.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

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
        print('tick')


def draw():
    screen.fill((0, 0, 0))

    grid_x_count = 20
    grid_y_count = 15
    cell_size = 15

    screen.draw.filled_rect(
        Rect(
            0, 0,
            grid_x_count * cell_size, grid_y_count * cell_size
        ),
        color=(70, 70, 70)
    )

    snake_segments = [
        {'x': 2, 'y': 0},
        {'x': 1, 'y': 0},
        {'x': 0, 'y': 0},
    ]

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

## Flytta ormen åt höger
Nästa position för ormens huvud beräknas genom att lägga till 1 till den nuvarande X-positionen för ormens huvud (dvs. det första elementet i segmentlistan). Detta nya segment läggs till i början av segmentlistan.

Det sista elementet i segmentlistan (ormens svans) tas bort.

Segmentlistan ändras i funktionen `update()`, så den flyttas till att vara global.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

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
        Rect(
            0, 0,
            grid_x_count * cell_size, grid_y_count * cell_size
        ),
        color=(70, 70, 70)
    )

    # snake_segments har flyttat 🐍

    for segment in snake_segments:
    # etc.
```

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

# Kod för att starta appen här nedanför

pgzrun.go()  # måste vara sista raden
```
</details>

## Flytta ormen i alla fyra riktningar
Ormens nuvarande riktning lagras i en variabel och ändras med hjälp av piltangenterna.

Ormens nästa huvudposition ställs in beroende på denna riktning.

✏️ Uppdatera koden så här. Testkör &ndash; vad händer när ...?

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

# Kod för att starta appen här nedanför

pgzrun.go()  # måste vara sista raden
```
    
</details>

## Hindra att ormen rör sig rakt bakåt
Ormen ska inte kunna röra sig i motsatt riktning som den för närvarande går i (t.ex. när den går åt höger ska den inte direkt gå åt vänster), så detta kontrolleras innan riktningen ställs in.

✏️ Uppdatera funktionen `on_key_down()`. Testkör &ndash; vad händer när ...?

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

# Kod för att starta appen här nedanför


pgzrun.go()  # måste vara sista raden
```
    
</details>

## Använd en kö för riktningarna
För närvarande kan ormen fortfarande gå bakåt om en annan riktning och sedan den motsatta riktningen trycks in inom ett enda tick på timern. Till exempel, om ormen flyttade höger på den sista ticket, och sedan spelaren trycker ner och sedan vänster före nästa tick, kommer ormen att flytta åt vänster på nästa tick.

Dessutom kan spelaren vilja ge flera anvisningar inom ett enda tick. I exemplet ovan kan spelaren ha velat att ormen skulle flytta ner för nästa tick, och sedan lämnat på ticket efter.

En riktningskö skapas. Det första objektet i kön är riktningen som ormen kommer att röra sig vid nästa tick.

Om vägbeskrivningskön har mer än en post tas den första posten bort från den vid varje tick.

När en knapp trycks ned läggs riktningen till i slutet av vägbeskrivningskön.

Den sista posten i riktningskön (dvs. den senast tryckta riktningen) kontrolleras för att se om den inte är i motsatt riktning mot den nya riktningen innan den nya riktningen läggs till i riktningskön.

✏️ Uppdatera koden. Testkör &ndash; vad händer när du trycker snabbt på de olika piltangenterna?

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
    if (key == keys.RIGHT #ändrat 🐍
            and direction_queue[-1] != 'left'): #ändrat 🐍
        direction_queue.append('right') #nytt 🐍

    elif (key == keys.LEFT #ändrat 🐍
          and direction_queue[-1] != 'right'): #ändrat 🐍
        direction_queue.append('left') #nytt 🐍

    elif (key == keys.DOWN #ändrat 🐍
          and direction_queue[-1] != 'up'): #ändrat 🐍
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

    # Tillfälligt #nytt 🐍
    for direction_index, direction in enumerate(direction_queue): #nytt 🐍
        screen.draw.text( #nytt 🐍
            f"direction_queue[{direction_index}]: {direction}", #nytt 🐍
            (15, 15 + 15 * direction_index)) #nytt 🐍

# Kod för att starta appen här nedanför
pgzrun.go()  # måste vara sista raden

```

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
Om den sista riktningen är i samma riktning som den nya riktningen läggs den nya riktningen inte till i riktningskön.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>

## Slår sig runt skärmen!!!
Om nästa position skulle vara utanför nätet, lindas!!! den runt till positionen på andra sidan.

Rutnätets X/Y-antal återanvänds från att rita bakgrunden, så de flyttas till att vara globala.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```

![image](https://user-images.githubusercontent.com/4598641/226439789-ce8299ae-1e6c-449b-9dc0-6c64b6124c6f.png)

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>


## Rita mat
Maten lagras som ett par av X- och Y-värden och ritas som en kvadrat.

Slumpmodulen importeras så att random.randint kan användas .

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```

![image](https://user-images.githubusercontent.com/4598641/226439842-6fae488e-e72d-494c-bad4-9204c860144a.png)
<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>

## Förenkla kod
Koden för att rita en orms segment och rita maten är densamma förutom färgen, så en funktion görs med färgen som parameter.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>


## Äter mat
Om ormens nya huvudposition är samma som matens position tas inte ormens svans bort, och maten får en ny slumpmässig position.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```


<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>

## Förenkla koden
Koden för att ställa in maten till en slumpmässig position återanvänds, så en funktion görs.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>

## Flytta mat till lediga positioner
Istället för att flytta maten till valfri slumpmässig position, flyttas den till en position som ormen inte upptar.

Alla positioner i rutnätet loopas igenom, och för varje rutnätsposition slingras alla segment av ormen, och om inga segment av ormen är i samma position som rutnätspositionen läggs rutnätspositionen till till en lista över möjliga matpositioner. Nästa matposition väljs slumpmässigt från denna lista.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>

## Spelet slut
Ormens segment slingras igenom, och om någon av dem förutom det sista är i samma position som ormens nya huvudposition, så har ormen kraschat in i sig själv.

Det sista segmentet är inte markerat eftersom det kommer att tas bort inom samma bock.

För närvarande skrivs 'kollision' ut när ormen kraschar in i sig själv.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>

## Återställa spelet
En funktion skapas som ställer in spelets initiala tillstånd.

Denna funktion anropas innan spelet börjar och när ormen kraschar.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>

## Pausar efter att ormen har kraschat
En variabel används för att lagra om ormen är vid liv eller inte, och den är inställd på False när ormen har kraschat.

Om ormen är död, väntar timern i 2 sekunder innan den ringer återställning .

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```

<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>

## Ändra ormens färg när den är död
Ormens färg ändras beroende på om den är vid liv eller inte.

✏️ Uppdatera koden. Testkör &ndash; vad händer när ...?

```python
```


![image](https://user-images.githubusercontent.com/4598641/226440133-a580b309-3b49-400d-ab5b-97c545c75ecd.png)
<details>
    <summary>📝 Så här kan koden se ut nu</summary>

```python
###
```
</details>


# Källor

https://simplegametutorials.github.io/pygamezero/snake/

https://web.archive.org/web/20140820192218/http://www.realtid.se/ArticlePages/200603/01/20060301132710_Realtid437/20060301132710_Realtid437.dbp.asp?Action=Print

