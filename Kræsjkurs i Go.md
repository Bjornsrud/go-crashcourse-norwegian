# Go – Kræsjkurs for erfarne utviklere

Denne guiden er for deg som allerede kan programmere, for eksempel i Java eller C#, og som ønsker å lære deg Go-syntaks raskt og forstå **hvordan Go er ment å brukes**.

Guiden har sitt utspring i at jeg selv ønsket å lære Go etter mange år som Java-utvikler. Underveis opplevde jeg at mye av friksjonen ikke handlet primært om syntaks, men om *mentale modeller, idiomer og bevisste designvalg*. Denne teksten er en syntese av notater, eksperimentering og spørsmål jeg har gjort meg underveis, og deles i håp om at også andre kan ha nytte av den samme "oversettelsen" fra Java/OOP-tankegang til Go-måten å gjøre ting på.

Målet med guiden er derfor ikke først og fremst å lære deg syntaks i isolasjon, men å hjelpe deg med å **oversette etablerte mentale modeller til Go**, særlig fra Java og objektorientert programmering.

Guiden fokuserer på:
- hvordan kjente programmeringskonsepter uttrykkes i Go på en annen og ofte mer eksplisitt måte
- hvilke mekanismer og abstraksjoner Go bevisst mangler, og hva det betyr i praksis
- hvordan du raskt kan skrive kode som føles naturlig og idiomatisk i Go, uten å kjempe mot språket

Guiden forutsetter at du allerede kjenner grunnleggende programmeringskonsepter. Fokus ligger derfor på **forskjeller i modell og tankesett**, ikke på elementær programmering.

Eksemplene er bevisst små, direkte og uten rammeverk, i tråd med Go-kulturen. Guiden er ikke ment som en fullstendig referanse, men som en praktisk og konseptuell inngang til språket.


---

## Innholdsfortegnelse

- [0. Omfang og mål med guiden](#0-omfang-og-mål-med-guiden)
- [1. Grunntanke og filosofi](#1-grunntanke-og-filosofi)
- [2. Typer og variabler](#2-typer-og-variabler)
- [3. Kontrollflyt: if / else og switch](#3-kontrollflyt-if--else-og-switch)
- [4. Løkker og kontroll (for, break, continue)](#4-løkker-og-kontroll-for-break-continue)
- [5. Funksjoner](#5-funksjoner)
- [6. Structs, metoder og peker-receivere](#6-structs-metoder-og-peker-receivere)
- [7. Komposisjon](#7-komposisjon)
- [8. Interfaces og implisitt implementasjon](#8-interfaces-og-implisitt-implementasjon)
- [9. Verdier, pekere og instanser](#9-verdier-pekere-og-instanser)
- [10. Collections: arrays, slices og maps](#10-collections-arrays-slices-og-maps)
- [11. Filtrering av slices og maps](#11-filtrering-av-slices-og-maps)
- [12. Strenger: trimming, splitting, formatering og escape](#12-strenger-trimming-splitting-formatering-og-escape)
- [13. Input og filer](#13-input-og-filer)
- [14. Blank identifier _](#14-blank-identifier-_)
- [15. Feilhåndtering (vil feile fordi missing.txt ikke finnes)](#15-feilhåndtering-vil-feile-fordi-missingtxt-ikke-finnes)
- [16. Imports, pakker, moduler og startpunkt](#16-imports-pakker-moduler-og-startpunkt)
- [17. Kjøring, bygging og testing](#17-kjøring-bygging-og-testing)
- [18. Testing i Go](#18-testing-i-go)
- [19. Språkidiomer og runtime-atferd](#19-språkidiomer-og-runtime-atferd)
- [20. API-design og navngiving i Go](#20-api-design-og-navngiving-i-go)
- [21. Strukturelle idiomer og Go-kultur](#21-strukturelle-idiomer-og-go-kultur)
- [22. Concurrency: parallell behandling av uavhengige jobber](#22-concurrency-parallell-behandling-av-uavhengige-jobber)
- [23. Mentalt skifte fra språk som Java, C# og andre](#23-mentalt-skifte-fra-språk-som-java-c-og-andre)

---

## 0. Omfang og mål med guiden

Denne guiden er ment å fungere som et **oppslagskart mellom kjente programmeringskonsepter og Go-syntaks**.

Den forklarer:
- hvordan velkjente ideer som variabler, kontrollflyt, funksjoner, datastrukturer, feilhåndtering, testing og concurrency uttrykkes i Go
- hvordan Go løser en del av de samme problemene som andre språk, men ofte med andre og mer eksplisitte virkemidler

Den er **ikke** ment å:
- lære bort grunnleggende programmering fra bunnen av
- dekke alle detaljer i språket
- fungere som en form for referansedokumentasjon for Go

Fokuset er å gi deg rask mental oversettelse fra det du allerede kan, til idiomatisk Go.

**Om kodeeksemplene:** 
Mange av eksemplene i denne guiden er bevisst skrevet som små utdrag, ikke som komplette, kjørbare programmer. Dette er gjort fordi guiden er rettet mot erfarne utviklere, og fokuset er på syntaks, idiomer og mentale modeller, ikke på å levere ferdige "copy/paste-programmer". Hvis du vil kjøre et eksempel, er det som regel nok å legge til riktig package-linje, relevante imports, og eventuelt en main() eller en liten run() error-wrapper.

---

## 1. Grunntanke og filosofi

Go er designet for **store kodebaser, lange levetider og team med mange utviklere**.

Språket er ikke optimalisert for maksimal uttrykkskraft per linje kode, men for:
- forutsigbarhet
- lesbarhet
- enkelt samarbeid over tid

### Grunnleggende designprinsipper

- **Lesbarhet trumfer korthet**  
  Kode skal være enkel å lese, også for noen som ikke har skrevet den selv.

- **Lite magi**  
  Go foretrekker eksplisitt kontrollflyt fremfor skjult oppførsel.
  Få språkkonstruksjoner betyr færre overraskelser.

- **Konvensjoner fremfor konfigurasjon**  
  Verktøy som `go fmt`, `go test` og `go build` er standardisert og forventes brukt likt i alle prosjekter.

- **Komposisjon fremfor arv**  
  Funksjonalitet bygges ved å sette sammen små, enkle deler.

### Konsekvensene av disse valgene

Som følge av dette:
- har Go færre språkkonstruksjoner enn mange andre språk
- mangler funksjoner som arv, exceptions og operator-overloading
- oppleves språket ofte som "enkelt" eller "begrenset" ved første møte

Dette er bevisste valg. Ideen er at:
- kompleksitet skal ligge i **løsningen**, ikke i språket
- kode skal være lett å forstå, også om fem år
- samarbeid og vedlikehold skal være billigere enn "cleverness"
- språket skal være mindre omfattende enn andre språk, og dermed også lettere å lære

### Et minimumseksempel

For ordens skyld, her er det klassiske "Hello World"-programmet i Go:

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello Go!")
}
```

Dette viser flere sentrale trekk allerede:
- `package main` markerer et kjørbart program
- `main()` er alltid inngangspunktet
- standardbiblioteket brukes eksplisitt
- ingen skjult konfigurasjon eller rammeverk

**Kjøre eksemplet:**  
For å kunne kjøre dette eksemplet må du ha Go installert (se https://go.dev/dl/).  
Lagre koden i en fil, for eksempel `hello.go`, og kjør den fra kommandolinjen med:

```bash
go run hello.go
```

Hvis du i stedet vil bygge en kjørbar binær, kan du bruke:

```bash
go build hello.go
```

Dette vil produsere en kjørbar fil i gjeldende mappe, tilpasset operativsystemet du bygger på.

---

## 2. Typer og variabler

Go er statisk typet, men med type inference.

### Verdier (sammenligning med Java)
I Go er de fleste ting **verdier** som kopieres ved tilordning.
- Dette ligner Java sine primitive typer (`int`, `double`, `boolean`).
- Kontrast: Java-objekter håndteres via **referanser**.

Eksempel:
```go
x := 10
y := x
y = 20
fmt.Println(x) // 10
fmt.Println(y) // 20
```
Her kopieres verdien `10` inn i `y`. Endring av `y` påvirker ikke `x`.

### Deklarasjon
```go
var a int = 10 // Med eksplisitt satt type
var b = 10
c := 10        // kort deklarasjon (kun inni funksjoner)
const pi = 3.14
```

### Viktige basis-typer

| Type | Bits | Beskrivelse |
|------|------|------------|
| bool | 1 (konseptuelt) | Boolsk type, kan være `true` eller `false` |
| string | variabel | UTF-8-kodet tekst, immutable |
| int | plattform | Signert heltall, størrelse avhenger av arkitektur (32/64-bit) |
| uint | plattform | Usignert heltall, størrelse avhenger av arkitektur (32/64-bit) |
| int8 | 8 | Signert heltall |
| int16 | 16 | Signert heltall |
| int32 | 32 | Signert heltall |
| int64 | 64 | Signert heltall |
| uint8 | 8 | Usignert heltall |
| uint16 | 16 | Usignert heltall |
| uint32 | 32 | Usignert heltall |
| uint64 | 64 | Usignert heltall |
| byte | 8 | Alias for `uint8` (ofte brukt ved byte-/binærdata) |
| rune | 32 | Alias for `int32` (ofte brukt for Unicode code points, ≈ `char`-aktig) |
| float32 | 32 | Flyttall med enkel presisjon |
| float64 | 64 | Flyttall med dobbel presisjon |

Merk: `int`/`uint` er ofte det du bruker til "vanlige" heltall, med mindre du trenger en spesifikk bitstørrelse (f.eks. filformat/protokoll).

---

## 3. Kontrollflyt: if / else og switch

### if / else
```go
x := 7

if x > 10 {
    fmt.Println("x er større enn 10")
} else if x > 5 {    
    fmt.Println("x er større enn fem og mindre enn elleve")
} else {
    fmt.Println("x er 5 eller mindre")
}
```

### Init-statement i if
Init-statement lar deg kjøre en kort deklarasjon før betingelsen, og variabelen lever kun i if/else.

```go
s := "hei"

if n := len(s); n > 0 {
    fmt.Println("strengen er ikke tom, lengde:", n)
} else {
    fmt.Println("strengen er tom")
}
```
Hensikt:
- unngå at midlertidige variabler "lekker" ut i ytre scope
- holde data og beslutning tett sammen

### switch
`switch` er ofte en mer lesbar erstatning for lange `else if`-kjeder.

```go
day := 3

switch day {
case 1:
    fmt.Println("Mandag")
case 2:
    fmt.Println("Tirsdag")
case 3:
    fmt.Println("Onsdag")
default:
    fmt.Println("Ukjent dag")
}
```

Go sitt `switch` "bryter" automatisk etter en `case` (du trenger ikke `break`).

Du kan også ha "switch uten uttrykk" (som en if-kjede):
```go
x := -1
switch {
case x < 0:
    fmt.Println("negativ")
case x == 0:
    fmt.Println("null")
default:
    fmt.Println("positiv")
}
```

---

## 4. Løkker og kontroll (for, break, continue)

Go har kun `for`, men den dekker klassisk `for`, `while` og evig løkke.

### Klassisk for
```go
for i := 0; i < 3; i++ {
    fmt.Println("i:", i)
}
```

### while-stil
```go
x := 0
for x < 3 {
    fmt.Println("x:", x)
    x++
}
```

### Evig løkke + bryte ut
```go
count := 0
for {
    count++

    if count == 2 {
        continue // hopper over resten av iterasjonen
    }

    fmt.Println("count:", count)

    if count == 4 {
        break // bryter ut av løkken
    }
}
```

---

## 5. Funksjoner

### Grunnform
```go
func isLongerThan(a int, b string) bool {
    return len(b) > a
}
```

### Flere parametre med samme type
```go
func add(a, b int) int {
    return a + b
}

func sum(a, b, c int) int {
    return a + b + c
}
```

### Flere returverdier (svært vanlig)
Go bruker ofte flere returverdier for å returnere både resultat og feil.

```go
func div(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}

q, err := div(10, 0)
if err != nil {
    fmt.Println("Feil:", err)
} else {
    fmt.Println("Kvotient:", q)
}
```

---

## 6. Structs, metoder og peker-receivere

I Go finnes det ikke klasser, men du får samme praktiske effekt ved å kombinere:
- **struct** (data)
- **metoder** med receiver (oppførsel knyttet til typen)

Det som "kobler" metoder og structs sammen er **receiver-typen** i metode-signaturen.
Hvis receiveren er `User` eller `*User`, så er metoden en del av API-et til typen `User`.

Dette kan du tenke på som at Go gjør det Java gjør med:
```java
class User { void birthday() { ... } }
```
men i Go er det ikke en syntaktisk "klasseblokk". Koblingen er via receiveren.

### Struct = data
```go
type User struct {
    Name string
    Age  int
}
```

### Metoder = oppførsel
```go
func (u User) Label() string {
    return fmt.Sprintf("%s (%d)", u.Name, u.Age)
}
```

Du kan plassere denne metoden i samme fil som structen, eller i en annen fil, så lenge:
- den ligger i **samme package**
- receiver-typen matcher (`User` eller `*User`)

Dette er vanlig i større prosjekter: du kan ha `user.go` med structen og `user_methods.go` med metoder.

### Peker-receiver (`*T`) = muter samme instans
Verdireceiver får en kopi. Pekerreceiver får en peker til originalen.

```go
func (u User) RenameValue(name string) {
    u.Name = name // endrer bare kopien
}

func (u *User) RenamePtr(name string) {
    u.Name = name // endrer originalen
}

u := User{Name: "Ada", Age: 37}

u.RenameValue("Bea")
fmt.Println(u.Name) // Ada

u.RenamePtr("Bea")
fmt.Println(u.Name) // Bea
```

---

## 7. Komposisjon

Go har ikke arv (`extends`). I stedet bygger du større typer ved å sette sammen mindre typer.

Det vanligste mønsteret er **embedding**, hvor du legger en type som et felt uten navn.
Da blir metodene til den embedded typen tilgjengelige direkte på den ytre typen.

```go
type Logger struct{}

func (Logger) Log(msg string) {
    fmt.Println("LOG:", msg)
}

type Service struct {
    Logger // embedding: Service har en Logger inni seg
}

svc := Service{}
svc.Log("starter")           // kaller svc.Logger.Log(...)
svc.Logger.Log("også mulig") // eksplisitt tilgang
```

Hva du oppnår:
- `Service` **inneholder** en `Logger` (komposisjon)
- du kan gjenbruke funksjonalitet uten superklasser
- det er tydelig hva `Service` består av

Du kan også kombinere flere ting:

```go
type Metrics struct{ Count int }
func (m *Metrics) Inc() { m.Count++ }

type Service2 struct {
    Logger
    Metrics
}

s := Service2{}
s.Log("hei")
s.Inc()
fmt.Println(s.Count)
```

Dette gir "byggesteiner" fremfor hierarkier.

---

## 8. Interfaces og implisitt implementasjon

Et interface i Go er en kontrakt som beskriver *oppførsel* (metoder), ikke data.

```go
type Greeter interface {
    Greet() string
}
```

### Implisitt implementasjon
I f.eks. Java må du eksplisitt skrive `implements Greeter`. I Go skjer dette automatisk.

En type implementerer et interface hvis den har alle metodene interfacet krever, med riktig navn og signatur.

Dette gjelder **uavhengig av pakke eller mappe**. Typene trenger ikke ligge i samme package.
Det er heller ingen "registrering" av implementasjonen.

```go
type Person struct {
    Name string
}

func (p Person) Greet() string {
    return "Hei " + p.Name
}

type Robot struct {
    ID int
}

func (r Robot) Greet() string {
    return fmt.Sprintf("Beep %d", r.ID)
}
```

Implisitt implementasjon påvirker ikke typene i seg selv.
`Person` og `Robot` vet ikke at de "implementerer" noe.
Det er først når de brukes som `Greeter` at interfacet får betydning.

```go
func SayHello(g Greeter) {
    fmt.Println(g.Greet())
}

// Bruk:

p := Person{Name: "Ada"}
r := Robot{ID: 7}

// Begge typene brukes her som Greeter
SayHello(p)
SayHello(r)
```

---

## 9. Verdier, pekere og instanser

### Kopi vs peker
```go
x := 10
p := x
p = 20
fmt.Println(x) // 10
fmt.Println(p) // 20
```
I `p := x` kopieres verdien i x til p. Det opprettes ingen kobling mellom variablene. Her er `p` bare en ny kopi av x.

```go
x := 10
ptr := &x      // ptr peker på x
*ptr = 20      // endrer verdien der ptr peker
fmt.Println(x) // 20
```

### `ptr = 20` vs `*ptr = 20`
- `ptr = ...` endrer hva pekeren peker på (eller vil feile type-messig hvis du prøver å sette int inn i en pointer-variabel).
- `*ptr = ...` endrer verdien i minnet som pekeren peker på.

### Instans
En instans er en konkret verdi av en type.

```go
u := User{Name: "Ada"}   // instans som verdi
up := &User{Name: "Ada"} // instans via peker
```

---

## 10. Collections: arrays, slices og maps

### Array (fast lengde)
```go
arr := [3]int{10, 20, 30}
arr[1] = 99
fmt.Println(arr[0]) // 10
fmt.Println(arr[1]) // 99
```

### Slice (dynamisk, ≈ ArrayList)

Historisk har Go ikke hatt en innebygd ‘delete-from-slice’-operasjon; idiomet er slicing + append. Dette er bevisst: slices er lavnivå, fleksible datastrukturer. (Fra Go 1.21 finnes også slices.Delete i pakken slices i standardbiblioteket.) 

```go
s := []string{"eple", "banan", "pære"}

// legg til
s = append(s, "appelsin")

// fjern indeks i (bevarer rekkefølge)
// Dette lager en ny slice ved å "lime sammen":
// - alt før i (s[:i])
// - alt etter i (s[i+1:])
// Elementet på indeks i blir dermed utelatt.
i := 1
s = append(s[:i], s[i+1:]...)
fmt.Println(s) // [eple pære appelsin]
```

I nyere Go finnes det, som nevnt, hjelpefunksjoner i standardbiblioteket (`slices`-pakken), men idiomet bak er fortsatt slicing + `append`.

---

## 11. Filtrering av slices og maps

### Slice-filtrering (behold bare partall)
```go
nums := []int{1, 2, 3, 4, 5}

out := nums[:0]
for _, v := range nums {
    // _ ignorerer indeksen (0..), vi trenger bare verdien v
    // Se kapittel 14 for mer om _
    if v%2 == 0 {
        out = append(out, v)
    }
}

fmt.Println(out) // [2 4]
```

Hvorfor nyttig:
- du får et "rent" resultat basert på en regel
- du kan ofte gjenbruke backing array for effektivitet

**Obs:** Hvis du trenger å bevare nums, bruk out := make([]int, 0, len(nums)) i stedet.

### Map-filtrering (fjern oddetall)
```go
m := map[string]int{"a": 1, "b": 2, "c": 3}
for k, v := range m {
    if v%2 != 0 {
        delete(m, k)
    }
}
fmt.Println(m) // map[b:2]
```

---

## 12. Strenger: trimming, splitting, formatering og escape

### Trim og split
```go
text := " Go er gøy "
text = strings.TrimSpace(text) // fjerner ledende og etterfølgende whitespace
parts := strings.Fields(text) // returnerer []string, splitter på ett eller flere whitespace-tegn
fmt.Println(parts) // output: [Go er gøy]
```
(Husk `import "strings"` for å bruke disse)

### Printf og strengformatering

`fmt.Printf` skriver formattert output direkte til stdout.

```go
name := "Ada"
age := 37
pi := 3.14159

fmt.Printf("Navn: %s, alder: %d\n", name, age)
fmt.Printf("pi: %.2f\n", pi) // 2 desimaler
fmt.Printf("generisk: %v\n", []string{"Go", "er", "gøy"})
```

Hvis du vil formattere til en **string** (slik `String.format()` gjør i Java), bruker du `fmt.Sprintf`:

```go
msg := fmt.Sprintf("Navn: %s, alder: %d", name, age)
fmt.Println(msg)
```

Vanlige format-verb:
- `%s` string
- `%d` heltall
- `%f` flyttall
- `%.2f` flyttall med 2 desimaler
- `%v` generisk

### Escape-tegn
```go
fmt.Println("Linje 1\nLinje 2")   //    \n gir linjeskift
fmt.Println("\"Go\" er gøy")      //    \" brukes for å skrive anførselstegn i streng
fmt.Println("Backslash: \\")     //     \\ representerer én faktisk backslash
```

### strings.Builder
`strings.Builder` er et verktøy for å bygge strenger effektivt, spesielt i løkker.
Det reduserer antall midlertidige allokeringer som kan komme av mange `+`-konkateneringer.

```go
var b strings.Builder
b.WriteString("Hei")
b.WriteString(", ")
b.WriteString("Go")

fmt.Println(b.String()) // Hei, Go
```

Nyttige metoder:
- `WriteString(string)` legger til tekst
- `WriteRune(rune)` legger til ett Unicode-tegn
- `WriteByte(byte)` legger til én byte
- `Len()` nåværende lengde
- `Reset()` tømmer builderen

---

## 13. Input og filer

### Tastatur (stdin)

```go
scanner := bufio.NewScanner(os.Stdin)
fmt.Print("Skriv noe: ")

// Scan() blokkerer og venter på input.
// Den returnerer false hvis vi får EOF (f.eks. brukeren sender Ctrl+D/Ctrl+Z)
// eller hvis en feil oppstod.
if !scanner.Scan() { // her kalles Scan(); den blokkerer og forsøker å lese neste linje
    if err := scanner.Err(); err != nil {
        return fmt.Errorf("stdin scan: %w", err)
    }
    return fmt.Errorf("ingen input (EOF)")
}

line := scanner.Text() // returnerer teksten for linjen som nettopp ble lest
fmt.Println("Du skrev:", line)
```

**Hvorfor sjekke EOF/false her?**
Fordi `Scan()` er API-et som signaliserer om vi faktisk fikk en linje. Hvis brukeren avslutter input uten å skrive (EOF), må vi håndtere det i stedet for å late som vi fikk tekst.

**Merk**: Scanner har en default maks-linjelengde; for veldig lange linjer kan du bruke scanner.Buffer(...) eller bufio.Reader.

### Fil, linje for linje

```go
f, err := os.Open("x.txt")
if err != nil {
    return fmt.Errorf("open file: %w", err)
}
defer f.Close() // sørger for at filen alltid lukkes når funksjonen returnerer

scanner := bufio.NewScanner(f)
for scanner.Scan() { // leser neste linje; stopper ved EOF eller feil
    line := scanner.Text() // teksten for gjeldende linje
    fmt.Println("LINJE:", line)
}

// Etter løkka: sjekk om stopp skyldtes feil (ikke bare EOF)
if err := scanner.Err(); err != nil {
    return fmt.Errorf("scan file: %w", err)
}
```

Merk:
Mange IO-funksjoner i Go returnerer to ting: et resultat og en `error`.
Det betyr ikke at feil er normalen, men at API-ene gir deg en eksplisitt måte å oppdage og håndtere feil.
Se kapittel 15 for feilmønsteret.

---

## 14. Blank identifier _

`_` brukes når du bevisst vil ignorere en verdi. Dette brukes ofte sammen med range, slik vi så i kapittel 11.

Eksempel: `range` over slice gir to verdier: indeks og verdi. Her ignorerer vi indeksen.

```go
for _, v := range []int{1, 2, 3} {
    fmt.Println(v)
}
```

`_` mottar altså indeksen (0, 1, 2), men vi velger å ikke bruke den.

---

## 15. Feilhåndtering (vil feile fordi missing.txt ikke finnes)

Go bruker ikke exceptions. Feil returneres som verdier.

```go
func ReadFirstLine(path string) (string, error) {
    f, err := os.Open(path)
    if err != nil {
        return "", fmt.Errorf("open %s: %w", path, err)
    }
    defer f.Close()

    scanner := bufio.NewScanner(f)
    if !scanner.Scan() {
        if err := scanner.Err(); err != nil {
            return "", fmt.Errorf("scan %s: %w", path, err)
        }
        return "", fmt.Errorf("%s: empty file", path)
    }

    return scanner.Text(), nil
}

line, err := ReadFirstLine("missing.txt")
if err != nil {
    fmt.Println("Feil:", err)
    return
}
fmt.Println("Første linje:", line)
```

Dette er Go sin "try/catch"-analog: du sjekker `err` rett etter operasjoner som kan feile.

---


## 16. Imports, pakker, moduler og startpunkt

### Imports
`import` brukes for å ta i bruk kode fra andre packages.

```go
import (
    "fmt"                                   // standardbibliotek
    "example.com/fruitapp/internal/store"   // import via modulnavn + sti
)
```

- Standardbiblioteket (`fmt`, `os`, `strings`) importeres uten modul-prefiks
- Egen kode importeres via modulnavn + sti


### Moduler

En `Go-modul` er en versjonert samling av packages.
Den defineres av en go.mod-fil i roten av repoet og har et modulnavn.

Eksempel:
```text
module example.com/fruitapp
```

Modulnavnet brukes som prefiks i alle import-stier i prosjektet.

### Pakker
En *package* er en mappe med `.go`-filer som kompileres sammen.
- Alle `.go`-filer i samme mappe må ha samme `package`-navn.
- Du kan spre koden over flere filer for lesbarhet.

Eksempel: `internal/store/store.go` og `internal/store/store_test.go` tilhører `package store`.

### Hvordan Go vet hvor programmet starter
Et kjørbart Go-program er en package som heter `main` og som inneholder en `main()-funksjon`.
Et Go-program starter alltid i:

```go
func main()
```

Derfor ligger entrypoint ofte i `cmd/<appnavn>/main.go`.

### Hvilke filer er en del av programmet?
Når du kjører:

```bash
go run ./cmd/fruitapp
```

så skjer dette:
- kompilér alle `.go`-filer i mappen `cmd/fruitapp` (package `main`)
- følg `import`-setningene og kompilér alle avhengige packages

Go inkluderer altså ikke "tilfeldige filer". Bare:
- filer i pakken du bygger/kjører
- filer i pakker som importeres (direkte eller indirekte)

Struktur (eksempel):
```
fruitapp/
  go.mod
  cmd/
    fruitapp/
      main.go
  internal/
    cli/
      run.go
    store/
      store.go
```

`cmd/fruitapp/main.go` (programstart):
```go
package main

import "example.com/fruitapp/internal/cli"

func main() {
    cli.Run()
}
```

`internal/cli/run.go`:
```go
package cli

import (
    "fmt"
    "example.com/fruitapp/internal/store"
)

func Run() {
    fmt.Println("Frukt:", store.List())
}
```

---

## 17. Kjøring, bygging og testing

### go fmt

`fmt`-pakken brukes i kode (som vi har sett over) for formattert input/output (utskrift, strengformatering osv.). I tillegg finnes et viktig verktøy som kjøres fra kommandolinjen: **`go fmt`** Dette formatterer Go-kode i prosjektet til idiomatisk standard.
Dette fjerner dermed diskusjoner om stil, og gjør all Go-kode lettlest og gjenkjennelig.



```bash
go fmt ./...
```
Formatterer alle packages i modulen.


```bash
go fmt ./internal/store
```
Formatterer én spesifikk package.

### Kjøre
Kjører direkte uten å lage en varig binær (bra under utvikling):
```bash
go run ./cmd/fruitapp
```
Hvis du vil sende inn argumenter:
```bash
go run ./cmd/fruitapp --help
```
### Bygge

```bash
go build -o fruitapp ./cmd/fruitapp
```
`-o fruitapp` betyr at den ferdige binærfilen får navnet `fruitapp` i stedet for standardnavnet.

På Windows er det vanlig å bruke .exe:
```bash
go build -o fruitapp.exe ./cmd/fruitapp
```

### Release-build (mindre binær, uten debug-info)

Vanlig ved distribusjon:

```bash
go build -trimpath -ldflags="-s -w" -o fruitapp ./cmd/fruitapp
```

Forklaring:
- `-ldflags="-s -w"` fjerner symboltabell og debug-informasjon.
- `-trimpath` fjerner lokale filstier fra binæren.

---

### Cross-compiling (bygge for ulike plattformer)

Go støtter cross-compiling via miljøvariabler.

#### Windows (64-bit)

```bash
GOOS=windows GOARCH=amd64 go build -o fruitapp.exe ./cmd/fruitapp
```

#### Linux (64-bit)

```bash
GOOS=linux GOARCH=amd64 go build -o fruitapp-linux ./cmd/fruitapp
```

#### macOS (Intel, amd64)

```bash
GOOS=darwin GOARCH=amd64 go build -o fruitapp-macos-amd64 ./cmd/fruitapp
```

#### macOS (Apple Silicon, arm64)

```bash
GOOS=darwin GOARCH=arm64 go build -o fruitapp-macos-arm64 ./cmd/fruitapp
```

---

### CGO og statiske binærer

Cross-compiling fungerer "rett ut av boksen" for ren Go.

Hvis prosjektet bruker **CGO**, kan cross-compiling kreve en C-toolchain **for target-plattformen** (og i noen tilfeller er det enklest å bygge på target-maskinen).

Hvis prosjektet ikke bruker C-biblioteker, kan CGO slås av for enklere cross-compiling og mer portabel binær:


```bash
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o fruitapp-linux ./cmd/fruitapp
```

Dette gir ofte:
- enklere distribusjon
- færre runtime-avhengigheter
- én enkelt binærfil

---

### Installere binær i PATH

Hvis prosjektet er et CLI-verktøy:

```bash
go install ./cmd/fruitapp
```

Binæren installeres i:
- `$GOBIN` hvis satt
- ellers `$GOPATH/bin`

---

### Nyttige kommandoer relatert til bygging

Se aktivt Go-miljø:

```bash
go env
```

List alle packages i modulen:

```bash
go list ./...
```

Tøm build-cache (ved mistenkelige cache-problemer):

```bash
go clean -cache
```


### Kjøre tester

```bash
go test ./...
```

---

## 18. Testing i Go

Go sin test-runner er innebygget i verktøykjeden (`go test`).
Den finner og kjører tester basert på filnavn, pakkenavn og funksjonsnavn.

---

### Hvordan Go finner tester

Go ser etter:
- Filer som slutter på `_test.go`
- Funksjoner som heter `TestXxx(t *testing.T)` (må starte med `Test`)

Eksempel:
- `add.go` inneholder produksjonskode
- `add_test.go` inneholder tester for samme package

---

### Hvordan testen vet hvilke funksjoner den tester

Det finnes ingen eksplisitt kobling slik som annotasjoner eller konfigurasjon.
I stedet skjer koblingen automatisk:

- Testfilen kompileres sammen med produksjonskoden i samme package
- Tester kan kalle funksjoner de har tilgang til via vanlige Go-regler (package-scope)

Dette betyr:
- Ligger testfilen i **samme package**, kan den kalle både eksporterte og ueksporterte funksjoner.
- Ligger testen i en **annen package** (ofte `packagename_test`), kan den bare kalle eksporterte funksjoner.

---

### Enkel, rett-frem test (samme package)

**Filstruktur:**
```
internal/mathutil/
  add.go
  add_test.go
```

`internal/mathutil/add.go`:
```go
package mathutil

func Add(a, b int) int {
    return a + b
}
```

`internal/mathutil/add_test.go`:
```go
package mathutil

import "testing"

func TestAddSimple(t *testing.T) {
    got := Add(2, 3)
    want := 5
    if got != want {
        t.Fatalf("Add(2,3)=%d, want %d", got, want)
    }
}
```

Her vet testen om `Add` fordi `add_test.go` kompileres sammen med `add.go` i samme package.

---
### Testing på tvers av pakker (black-box testing)

Noen ganger vil du teste en package utenfra, slik andre packages ville brukt den.
Da brukes `packagename_test` som package-navn i testfilen.

Fordeler:
- tester kun eksportert API
- mer realistisk brukerperspektiv
- mindre kobling til interne detaljer

```go
package mathutil_test

import (
    "testing"
    "example.com/fruitapp/internal/mathutil"
)

func TestAddBlackBox(t *testing.T) {
    got := mathutil.Add(2, 3)
    want := 5
    if got != want {
        t.Fatalf("Add(2,3)=%d, want %d", got, want)
    }
}
```

---

### Table-driven testing

Table-driven tester samler mange testcases i én struktur og itererer over dem.
Dette gir mindre duplisert kode og gjør det lett å legge til nye cases.

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name string
        a, b int
        want int
    }{
        {"1+2", 1, 2, 3},
        {"2+3", 2, 3, 5},
        {"-1+1", -1, 1, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Add(tt.a, tt.b)
            if got != tt.want {
                t.Fatalf("Add(%d,%d)=%d, want %d", tt.a, tt.b, got, tt.want)
            }
        })
    }
}
```

---


### Nyttige test-idiomer

#### Kjøre utvalg av tester

```bash
go test -run TestAdd ./internal/mathutil
```

Kjører kun tester som matcher navnet.

---

#### TestMain - setup og teardown per package

```go
func TestMain(m *testing.M) {
    // setup
    code := m.Run()
    // teardown
    os.Exit(code)
}
```

Brukes når tester trenger felles oppsett (database, filer, env).

---

#### t.Helper - bedre feilmeldinger

```go
func assertEqual(t *testing.T, got, want int) {
    t.Helper()
    if got != want {
        t.Fatalf("got %d, want %d", got, want)
    }
}
```

---

#### Parallelle tester

```go
func TestSomething(t *testing.T) {
    t.Parallel()
    // testkode
}
```

Brukes når tester er uavhengige.

---

#### Race detector

```bash
go test -race ./...
```

Finner mange concurrency-relaterte feil.

---

### Kjøre tester

Kjør alle tester i modulen:
```bash
go test ./...
```

Kjør tester for én package:
```bash
go test ./internal/mathutil
```

Kjør med verbose output:
```bash
go test -v ./...
```


---

## 19. Språkidiomer og runtime-atferd

Her dekkes idiomer som handler om **hvordan Go oppfører seg i praksis under kjøring**.  
Fokuset er på livstid, ressurskontroll, fravær av verdier og eksplisitt kontrollflyt.

---

### `defer`

`defer` brukes til å utsette kjøring av en funksjon til den omsluttende funksjonen returnerer.  
Typisk brukt til ressursrydding som `Close()`, `Unlock()` eller logging.

```go
f, err := os.Open("x.txt")
if err != nil {
    return err
}
defer f.Close()
```

Egenskaper:
- **argumentene** til det du `defer`-er evalueres umiddelbart, men selve kallet skjer ved retur.
- kjøres også ved tidlig retur pga. feil
- flere `defer`-kall kjøres i LIFO-rekkefølge

Dette gir robust kode uten behov for `try/finally`.

---

### Zero values og `nil`

Alle typer i Go har en **definert nullverdi (zero value)**.

For utviklere som kommer fra Java er dette et viktig skille:
- I Java er `null` en spesiell verdi som ofte fører til `NullPointerException`
- I Go er `nil` en forventet del av språket, ment å håndteres eksplisitt

| Typekategori | Zero value |
|-------------|------------|
| `int`, `float` | `0` |
| `string` | `""` |
| `bool` | `false` |
| pointer | `nil` |
| slice | `nil` |
| map | `nil` |
| channel | `nil` |
| interface | `nil` |

```go
var count int
var name string
var items []string
var m map[string]int
var p *User

fmt.Println(count)          // 0
fmt.Println(name)           // ""
fmt.Println(items)          // []
fmt.Println(len(items))     // 0
fmt.Println(items == nil)   // true
fmt.Println(m)              // map[]
fmt.Println(m == nil)       // true
fmt.Println(p)              // nil
```

Merk at fmt printer nil slice/map som []/map[], så sjekk == nil hvis det betyr noe.
---

### `nil` vs `null` (kort sammenlikning med Java)

| Java | Go |
|-----|----|
| `null` kan dukke opp "overalt" | `nil` gjelder kun bestemte typer |
| `NullPointerException` | eksplisitte sjekker (`if x == nil`) |
| exceptions | verdier + kontrollflyt |

I Go er `nil` en synlig del av API-kontrakten, ikke en skjult felle.

---

### `nil` er ikke det samme som "ingen verdi"

```go
var a []int        // nil slice
b := []int{}       // tom slice

fmt.Println(a == nil) // true
fmt.Println(b == nil) // false
```

`nil` betyr at variabelen ikke refererer til noe i minnet, ikke at den er "tom".

---

### Bruk av `nil` per type

**Slice**
```go
var s []int
fmt.Println(len(s)) // 0
s = append(s, 1)    // OK
```

**Map**
```go
var m map[string]int
fmt.Println(m["x"]) // 0
m["x"] = 1          // PANIC
```

Riktig:
```go
m := make(map[string]int)
```

**Pointer**
```go
var p *User
if p != nil {
    fmt.Println(p.Name)
}
```

**Channel**
```go
var ch chan int
// send/receive blokkerer for alltid
```

---

### Designkonsekvenser

- strukturer kan brukes uten konstruktør
- slices kan returneres uten init
- tomme resultater er ofte `nil`
- API-er dokumenterer når `nil` er gyldig

---

### Viktig tommelfingerregel

> `nil` er vanlig og forventet i Go.  
> Det er ikke en feil i seg selv.

---

## 20. API-design og navngiving i Go

Hvordan du designer kode som **andre skal bruke**.

---

### Idiomatisk navngiving

- `camelCase`
- korte navn i lite scope
- beskrivende navn i større scope

```go
count := 0
userID := 42
parseConfig()
```

---

### Eksporterte navn (offentlig API)

Synlighet styres av første bokstav:

```go
type User struct {
    Name string // eksportert
    age  int    // ikke eksportert
}

func NewUser(name string) User { ... }
```

Bare det som er ment å brukes utenfra bør eksporteres.

---

### Konstanter

```go
const maxRetries = 3
const DefaultTimeout = time.Second
```

ALL_CAPS brukes sjelden.

---

### Fil- og pakkenavn

- filnavn: `snake_case.go`
- pakkenavn: korte, entall

Unngå `utils`, `common`.

---

## 21. Strukturelle idiomer og Go-kultur

Praksiser som preger hvordan Go-kode skrives i større prosjekter.

---

### Små funksjoner og flat struktur

```go
func readConfig(path string) (Config, error) {
    data, err := os.ReadFile(path)
    if err != nil {
        return Config{}, err
    }

    cfg, err := parse(data)
    if err != nil {
        return Config{}, err
    }

    return cfg, nil
}
```

---

### Ingen framework-first-mentalitet

I Go er det vanlig å:
- starte med standardbiblioteket
- legge til avhengigheter ved behov
- forstå hele stacken

Dette er like mye kultur som teknikk.


---

## 22. Concurrency: parallell behandling av uavhengige jobber

Concurrency i Go handler om å kunne gjøre flere ting samtidig på en kontrollert og trygg måte.
Det brukes når du har mange uavhengige oppgaver som ikke trenger å vente på hverandre.

Typiske eksempler:
- mange API-kall
- filbehandling
- CPU-tunge beregninger som kan deles opp

---

### Problemstilling

Anta at vi har mange uavhengige oppgaver ("jobber") som skal behandles.
Hver jobb kan ta litt tid, men jobbene påvirker ikke hverandre.

Hvis vi kjører dem sekvensielt, bruker vi kun én CPU-kjerne og total kjøretid blir høyere enn nødvendig.
Concurrency lar oss:
- starte flere jobber samtidig
- utnytte flere CPU-kjerner
- samle resultatene når de er ferdige

---

### Første møte med concurrency: én goroutine og én channel

Et helt minimalt eksempel:

```go
func main() {
    ch := make(chan string)

    go func() {
        ch <- "hei fra goroutine"
    }()

    msg := <-ch
    fmt.Println(msg)
}
```

Hva skjer her:
- `make(chan string)` lager en kanal
- `go func() {}` starter en goroutine
- goroutinen sender en verdi inn i kanalen
- hovedprogrammet blokkerer på `<-ch` til verdien er tilgjengelig

Channels brukes både til kommunikasjon og synkronisering.

---

### Goroutines

En goroutine er en lettvekts-tråd styrt av Go-runtime.

```go
go doWork()
```

Dette betyr at `doWork()` kjører parallelt med resten av programmet.
Du kan starte tusenvis av goroutines uten høy overhead.

---

### Channels og pil-operatoren `<-`

En channel kan sees som en trådsikker kø for verdier mellom goroutines.

- `ch <- v` sender verdien `v` inn i kanalen (kan blokkere)
- `v := <-ch` leser én verdi ut av kanalen (blokkerer til noe er tilgjengelig)

Dette gjør at goroutines ikke trenger å dele mutable variabler direkte.

---

### Unbuffered vs buffered channels

Unbuffered channel:

```go
ch := make(chan int)
```

- sender blokkerer til mottaker er klar
- gir sterk synkronisering

Buffered channel:

```go
ch := make(chan int, 2)
```

- kan holde to verdier uten å blokkere
- nyttig når produsent og konsument jobber i ulikt tempo

Eksempel:

```go
ch := make(chan int, 2)

ch <- 1
ch <- 2
// ch <- 3  // ville blokkert

fmt.Println(<-ch)
fmt.Println(<-ch)
```

---

### Retningsbegrensede channels

Du kan begrense hvordan en channel brukes i funksjonssignaturer:

```go
jobs <-chan Job       // kun les
results chan<- Result // kun skriv
```

Dette:
- dokumenterer intensjon
- forhindrer feilbruk
- gjør API-et tydeligere

---

### Sammenligning: sekvensiell vs concurrent behandling

Sekvensiell versjon:

```go
for i := 1; i <= 5; i++ {
    out := i * i
    fmt.Println(out)
}
```

Concurrent versjon:
- jobber sendes inn i en channel
- flere goroutines prosesserer dem parallelt
- resultater samles inn

Dette leder oss til worker-pool-mønsteret.

---

### Eksempel: worker-pool som prosesserer jobber


```go
package main

import (
	"fmt"
	"sync"
)

type Job struct {
	ID    int
	Input int
}

type Result struct {
	JobID  int
	Output int
}

// Eksempel 1: Sekvensiell behandling (én etter én)
func eksempelSekvensielt() {
	fmt.Println("Sekvensiell behandling:")
	for i := 1; i <= 5; i++ {
		out := i * i
		fmt.Println(out)
	}
}

// Worker som leser jobber fra jobs-kanalen og skriver resultater til results-kanalen.
// Når jobs lukkes, går løkka ut, og workeren avslutter.
func worker(jobs <-chan Job, results chan<- Result, wg *sync.WaitGroup) {
	defer wg.Done()

	for job := range jobs {
		out := job.Input * job.Input
		results <- Result{JobID: job.ID, Output: out}
	}
}

// Eksempel 2: Worker-pool (flere jobber behandles parallelt)
func eksempelWorkerPool() {
	fmt.Println("\nWorker-pool (parallell behandling):")

	const (
		antallWorkers = 3
		antallJobber  = 5
	)

	jobs := make(chan Job)

	// Buffer gjør eksempelet litt mer tilgivende og reduserer “tett kobling”
	// mellom workers og consumer. (Valgfritt, men ofte praktisk.)
	results := make(chan Result, antallJobber)

	// Start workers
	var wg sync.WaitGroup
	wg.Add(antallWorkers)
	for i := 0; i < antallWorkers; i++ {
		go worker(jobs, results, &wg)
	}

	// Lukk results når alle workers er ferdige
	go func() {
		wg.Wait()
		close(results)
	}()

	// Produser jobber
	go func() {
		for i := 1; i <= antallJobber; i++ {
			jobs <- Job{ID: i, Input: i}
		}
		close(jobs)
	}()

	// Konsumer resultater til results-kanalen lukkes
	for r := range results {
		fmt.Printf("Jobb %d -> %d\n", r.JobID, r.Output)
	}

	// Merk: Resultatene kan komme i vilkårlig rekkefølge fordi flere workers jobber parallelt.
	// Hvis du trenger stabil rekkefølge, sorter på JobID etterpå eller skriv ut i ønsket rekkefølge.
}

func main() {
	eksempelSekvensielt()
	eksempelWorkerPool()
}

```

---

### Hvorfor concurrency hjelper her

- flere workers jobber parallelt
- bedre utnyttelse av CPU og I/O
- channels gir trygg koordinering
- ingen eksplisitte låser eller mutexer

Dette mønsteret er svært vanlig i idiomatisk Go og dekker mange reelle behov.


## 23. Mentalt skifte fra språk som Java, C# og andre

Hva Go bevisst mangler:

- arv
- exceptions
- operator overloading
- macros

Dette er designvalg for å holde språket enkelt og forutsigbart.

Typiske skift:
- Du bygger med **komposisjon** fremfor arv.
- Du bruker **interfaces** som kontrakter, men ofte introdusert senere.
- Du håndterer feil eksplisitt (`if err != nil`).

Det at Go kan føles "for enkelt", er en konsekvens av at språket bevisst fjerner en del mekanismer og valg som finnes i andre språk.
Det kan oppleves uvant fordi du mister noen verktøy du er vant til å jobbe med (exceptions, avansert OO-hierarki), men gevinsten er:
- mer ensartet kode
- færre overraskelser
- enklere vedlikehold i team

Med andre ord: enkelhet er et bevisst virkemiddel for å skalere kode og samarbeid.

