# Blocks
## En handledning för Python och Pygame Zero 1.2

![image](https://user-images.githubusercontent.com/4598641/226001268-afea64f1-d51c-48e0-b4b8-0bff27a3e893.png)

# Regler

Det finns sju typer av bitar. Varje bit innehåller fyra block.

![image](https://user-images.githubusercontent.com/4598641/226001342-33230a9a-d8a3-4218-9a37-3cc579827ad0.png)

Bitar faller från toppen av spelområdet. Spelaren kan flytta pjäserna åt vänster och höger och rotera dem. När en bit kommer till vila, faller nästa bit.

Typen av nästa pjäs som kommer att falla visas ovanför spelområdet.

![image](https://user-images.githubusercontent.com/4598641/226001405-e8e90545-4b84-4dc6-87a5-374f584ade98.png)

När en obruten rad av block bildas, försvinner raden och alla block ovanför flyttas ner en rad.

Spelet slutar när en pjäs har hamnat i vila och nästa pjäs skulle omedelbart överlappa ett tidigare fallet block.

## Kontroller

**Vänsterpil**	Flytta vänster ⬅️<br>
**Högerpil**	Flytta höger ➡️<br>
**z**	Rotera moturs 🔄<br>
**x**	Rotera medurs 🔃<br>
**c**	Släpp ⏬


# Översikt
Ett rutnät lagrar de orörliga blocken som redan har fallit.

Tillståndet för ett block kan antingen vara tomt eller fyllt med ett block av en viss färg.

Strängen ' ' (ett mellanslag) betyder ett tomt block, och strängarna 'i' , 'j' , 'l' , 'o' , 's' , 't' och 'z' representerar block med olika färger.

![image](https://user-images.githubusercontent.com/4598641/226003821-3a435de3-4843-421e-ab20-477e93bf3fe8.png)

Alla olika typer av bitar lagras med sina roterade varianter.

![image](https://user-images.githubusercontent.com/4598641/226003959-15932dfd-3435-47dd-b1f2-78b050e562fb.png)

Den fallande pjäsen lagras som ett nummer som representerar vilken typ av pjäs det är, ett nummer som representerar vilken rotationsvariation den befinner sig vid, och siffror som representerar dess X- och Y-position i spelområdet.

En ny pjäs skapas längst upp på skärmen, såvida den inte skulle överlappa ett inert block, i vilket fall spelet är över.

Spelaren kan flytta pjäsen åt vänster och höger, såvida inte denna nya position skulle överlappa ett inert block eller vara utanför spelområdet.

Efter att en tid har gått flyttas pjäsen nedåt, såvida inte denna nya position skulle överlappa ett inert block eller vara utanför spelområdet, i vilket fall den har kommit till vila.

När en av rotationsknapparna trycks in ändrar pjäsen sin rotationsvariation, såvida inte denna variation skulle överlappa ett inert block eller vara utanför spelområdet.

När släppknappen trycks in, flyttas pjäsen ner tills nästa position skulle överlappa ett inert block eller vara utanför spelområdet, vid vilken punkt den har kommit till vila.

När en bit kommer till vila, läggs bitarnas block till de inerta blocken, och nästa bit skapas.

En sekvens av en av var och en av de sju bitarna i en slumpmässig ordning skapas, och nästa bit tas från denna sekvens. När alla bitar har tagits skapas en ny slumpmässig sekvens.

# Kodning

## Rita rutnätet med block
En ruta ritas för varje block i spelområdet.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226004861-bb0676e3-8ace-444f-af72-a4c3859483b0.png)

## Lagring av orörliga block

Rutnätet för de orörliga blocken skapas och varje block sätts till ' ' (ett mellanslag), vilket representerar ett tomt block.

Bredden och höjden på rutnätet i block återanvänds från ritning av blocken, så de görs till variabler.

Kod:XXXX

## Sätt färgen på blocken

När block ritas ställs färgen in baserat på vilken typ av block det är.

För att testa detta är vissa block i det orörliga nätet inställda på olika typer.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226006718-62e1013b-99f3-427b-b095-4cda85184e19.png)

## Lagring av bitarnas utseende
Varje rotation av en pjäs är 4x4-rutnät av strängar.

```python
[
    [' ', ' ', ' ', ' '],
    ['i', 'i', 'i', 'i'],
    [' ', ' ', ' ', ' '],
    [' ', ' ', ' ', ' '],
]
```

Varje pjäs lagras som en lista av de olika rotationerna.

```python
[
    [
        [' ', ' ', ' ', ' '],
        ['i', 'i', 'i', 'i'],
        [' ', ' ', ' ', ' '],
        [' ', ' ', ' ', ' '],
    ],
    [
        [' ', 'i', ' ', ' '],
        [' ', 'i', ' ', ' '],
        [' ', 'i', ' ', ' '],
        [' ', 'i', ' ', ' '],
    ],
]
```

Alla pjäserna och deras rotationer sparas som en lista.

```python
piece_structures = [
    [
        [
            [' ', ' ', ' ', ' '],
            ['i', 'i', 'i', 'i'],
            [' ', ' ', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            [' ', 'i', ' ', ' '],
            [' ', 'i', ' ', ' '],
            [' ', 'i', ' ', ' '],
            [' ', 'i', ' ', ' '],
        ],
    ],
    [
        [
            [' ', ' ', ' ', ' '],
            [' ', 'o', 'o', ' '],
            [' ', 'o', 'o', ' '],
            [' ', ' ', ' ', ' '],
        ],
    ],
    [
        [
            [' ', ' ', ' ', ' '],
            ['j', 'j', 'j', ' '],
            [' ', ' ', 'j', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            [' ', 'j', ' ', ' '],
            [' ', 'j', ' ', ' '],
            ['j', 'j', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            ['j', ' ', ' ', ' '],
            ['j', 'j', 'j', ' '],
            [' ', ' ', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            [' ', 'j', 'j', ' '],
            [' ', 'j', ' ', ' '],
            [' ', 'j', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
    ],
    [
        [
            [' ', ' ', ' ', ' '],
            ['l', 'l', 'l', ' '],
            ['l', ' ', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            [' ', 'l', ' ', ' '],
            [' ', 'l', ' ', ' '],
            [' ', 'l', 'l', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            [' ', ' ', 'l', ' '],
            ['l', 'l', 'l', ' '],
            [' ', ' ', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            ['l', 'l', ' ', ' '],
            [' ', 'l', ' ', ' '],
            [' ', 'l', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
    ],
    [
        [
            [' ', ' ', ' ', ' '],
            ['t', 't', 't', ' '],
            [' ', 't', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            [' ', 't', ' ', ' '],
            [' ', 't', 't', ' '],
            [' ', 't', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            [' ', 't', ' ', ' '],
            ['t', 't', 't', ' '],
            [' ', ' ', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            [' ', 't', ' ', ' '],
            ['t', 't', ' ', ' '],
            [' ', 't', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
    ],
    [
        [
            [' ', ' ', ' ', ' '],
            [' ', 's', 's', ' '],
            ['s', 's', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            ['s', ' ', ' ', ' '],
            ['s', 's', ' ', ' '],
            [' ', 's', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
    ],
    [
        [
            [' ', ' ', ' ', ' '],
            ['z', 'z', ' ', ' '],
            [' ', 'z', 'z', ' '],
            [' ', ' ', ' ', ' '],
        ],
        [
            [' ', 'z', ' ', ' '],
            ['z', 'z', ' ', ' '],
            ['z', ' ', ' ', ' '],
            [' ', ' ', ' ', ' '],
        ],
    ],
]
```


## Lagra det aktuella blocket

Det för närvarande fallande blocket representeras av ett nummer som anger vilken typ det är (som kommer att användas för att indexera listan över pjässtrukturer), och ett nummer som anger vilken rotation den har (som kommer att användas för att indexera listan över rotationer).

Kod:XXXX

## Rita blocket

Pjäsen ritas genom att slinga genom dess struktur och, om inte blocket är tomt, ritar du en fyrkant med en färg som bestäms av blocktypen.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226010899-049e0b7e-591d-4d0b-b296-7fb35778e094.png)


## Förenkla koden

Koden för att rita ett orörligt block och rita ett block av den fallande biten är liknande, så en funktion skapas.

Kod:XXXX

## Rotation
När x -tangenten trycks in, ökas pjäsens rotationsnummer med 1, genom att rotera pjäsen medurs.

Om rotationstalet är större än antalet rotationspositioner minus 1 sätts rotationstalet till 0 (dvs första rotationspositionen).

På samma sätt, när z -tangenten trycks ned, minskas talet för styckerotation med 1, varvid stycket roteras moturs.

Om rotationstalet är mindre än 0, sätts rotationstalet till antalet rotationspositioner minus 1 (dvs. den sista rotationspositionen).

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226011415-59b9b18c-2496-4af0-a39c-f854ef940d2e.png)


## Testa block

För teständamål växlar upp- och nedpilarna genom stycketyperna.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226011550-d53162ca-1eaf-4674-b2dc-71eefe2fed7d.png)


## Placera nästa block

Pjäsens position i spelområdet lagras och pjäsen dras på den positionen.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226011946-ae6035af-d12b-4390-bfe8-2a1a3019655b.png)



## Flytta blocket
Vänster- och högerpilarna subtraherar eller adderar 1 till pjäsens X-koordinat.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226012210-eff3bfe9-dcb6-4579-be14-4eb21ec43338.png)


## Timer

Bitar kommer att falla var 0,5 sekund.

En timervariabel börjar vid 0 och ökar med dt för varje bildruta.

När timern är på eller över 0,5 återställs den till 0.

För närvarande skrivs 'tick' ut varje gång pjäsen faller.

Kod:XXXX

## Fallande block
Timern används för att öka pjäsens Y-position var 0,5 sekund.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226012579-0a5bce97-00a6-4f54-ba96-95cc123f9a4c.png)


## Begränsa rörelsefriheten

För att förhindra att pjäsen flyttas från vänster eller höger om skärmen när den flyttas eller roteras, kontrolleras vart och ett av dess block för att se om de är inom spelområdet innan pjäsen flyttas eller roteras.

Eftersom denna kontroll kommer att göras på flera ställen kommer den att skrivas som en funktion. Denna funktion får position och rotation att kontrollera, och returnerar Sant eller Falskt beroende på om pjäsen kan röra sig eller rotera.

Till att börja med kommer denna funktion alltid att returnera True , så att flytta och rotera är fortfarande alltid möjligt.

Koden ändras från att omedelbart ställa in positioner/rotationer, till att skapa variabler för de ändrade värdena, och om kontrollfunktionen returnerar True ställs den faktiska positionen/rotationen till de ändrade värdena.

Kod:XXXX

## Kontrollera vänsterkanten
Om något block inte är tomt och dess X-position är mindre än 0 (dvs. utanför spelområdets vänstra sida), returnerar funktionen False .

Kod:XXXX

## Förenkla koden
Antalet block som varje bit har på X- och Y-axlarna återanvänds från att rita bitarna, så variabler görs för dem.

## Kontrollera högerkanten

Om något blocks X-position är större än eller lika med spelområdets bredd (dvs. utanför spelområdets högra sida), returnerar funktionen också False .


Kod:XXXX

## Kontrollera underkanten
Om något blocks Y-position är större än eller lika med höjden på spelområdet (dvs. utanför botten av spelområdet), returnerar funktionen också False .


Kod:XXXX

## Kontrollera orörliga block
Om det finns ett inert block vid något blocks position, returnerar funktionen också False .

För att testa detta sätts ett inert block manuellt.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226013942-ae181f75-53b1-4b7c-8156-ba22cf2ecc9c.png)

## Förenkla koden
De beräknade blockpositionerna som ska testas återanvänds, så de lagras i variabler.

Kod:XXXX

## Släppa ner ett block

När c- tangenten trycks in, ökas pjäsens Y-position med 1 så länge som blocket får plats.

Kod:XXXX

## Återställa blocket

Om timern tickar och pjäsen inte kan röra sig nedåt, återställs pjäsen till sin ursprungliga position och rotation, och (för nu) sin ursprungliga typ.

Kod:XXXX

## Förenkla koden

Pjäsen sätts till sitt ursprungliga tillstånd på två ställen, så en funktion skapas.

Kod:XXXX

## Håll reda på kommande block
Sekvensen av nästa bitar lagras som en lista som innehåller numren som representerar bittyper i slumpmässig ordning.

En lista skapas från ett intervall från 0 till en mindre än längden på piece_structures och blandas sedan.

För att testa detta skapas en ny sekvens när s- tangenten trycks ned och sekvensen skrivs ut.

Slumpmodulen importeras så att random.shuffle kan användas.

Kod:XXXX

```python
[3, 2, 4, 1, 0, 5, 6]
```

## Nästa block från sekvensen
När en ny pjäs skapas tar den bort det sista objektet från sekvensen och använder det för pjästypen.

När sekvensen är tom skapas en ny sekvens.

Kod:XXXX

## Lägg till orörliga block
När en pjäs har kommit till vila, läggs pjäsens block till de inerta blocken.

Pjäsens block slingras igenom, och om ett block inte är tomt, är det inerta blocket i denna position inställt på typen av pjäsens block.

Kod:XXXX

## Ny pjäs direkt efter släpp
När en pjäs tappas ställs timern omedelbart till gränsen så att lägga till pjäsen till de inerta pjäserna och skapa den nya pjäsen sker direkt istället för att vänta på timern.

Timergränsen återanvänds, så den görs till en variabel.

Kod:XXXX

## Hitta fyllda rader
Varje rad av de inerta blocken loops igenom, och om ingen av kolumnerna i raden innehåller ett tomt block, är raden fylld.

För närvarande skrivs de fullständiga radnumren ut.

Kod:XXXX

## Ta bort fyllda rader
Om raden är komplett, slingras raderna från hela raden till raden tvåa uppifrån.

Varje block i raden loopas igenom och ställs in på värdet för blocket ovanför det. Eftersom det inte finns något ovanför den översta raden behöver den inte slingras igenom.

Den översta raden är då inställd på alla tomma block.


Kod:XXXX

## Slut på spelet

Om en nyskapad pjäs är i en orörlig position är spelet över.

En funktion skapas som ställer in spelets initiala tillstånd.

Denna funktion anropas innan spelet börjar och när spelet är över.

Kod:XXXX

## Förskjutning av spelområdet
Spelområdet ritas 2 block från vänster på skärmen och 5 block från toppen av skärmen.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226016663-cb1d5333-1bd0-4943-91e7-8d22d195f2ef.png)

## Rita nästa block

Den sista biten i sekvensen (dvs nästa bit som faller) dras vid sin första rotationsposition. Den är förskjuten 5 kvarter från vänster och 1 kvarter från toppen.

Kod:XXXX

![image](https://user-images.githubusercontent.com/4598641/226016912-b2e1d0a6-fbf5-41b8-b808-9fdacaea6fb0.png)



# Källor

Översatt och bearbetat för repl.it baserat på originalet: https://simplegametutorials.github.io/pygamezero/blocks/
