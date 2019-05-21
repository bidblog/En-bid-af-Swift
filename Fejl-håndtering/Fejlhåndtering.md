# Fejlhåndtering (error handling)

> Den der intet laver, laver ingen fejl

Det er muligt at skrive fejlfri kode, men det er ikke særligt sandsynligt. Og selv om du skriver fejlfri kode, så vil koden, når den bliver afviklet skulle håndtere undtagelser, eller situationer der ender i en fejl, og så gør at dit program enten skal bede brugeren om hjælp eller afvikle koden afhængigt af den fejl (undtagelse) som er opstået.

Og fejlhåndtering handler om at at strukturere sin kode så koden er i stand til at tage hånd omkring de undtagelser og fejl der kan opstå.

## Undtagelser eller fejl?

I andre programmeringssprog kaldes fejlhåndtering ofte for "Exception handling" eller på dansk ville vi kalde det for håndtering af undtagelser.

Men i Swift har man valgt at kalde det for Error handling, altså fejlhåndtering. 

Og spørger du mig, vil jeg også sige at det er en bedre betegnelse, for når der opstår en "undtagelse" i et program, så vil du betragte programmet som kommet i en fejl tilstand. En tilstand der kræver handling. 

Og den måde fejlhåndteringen er bygget på i Swift, kræver at du tager handling når fejlen kan opstå, modsat den undtagelses håndtering som ofte findes i andre programmeringssprog, hvor undtagelsen ikke behøver at blive håndteret der hvor fejlen opstår, og det kan give dårligere oplevelser for brugeren, fordi fejlen opstår ude af kontekst.

Men før vi ser på hvad jeg mener med håndtering af en fejl, skal vi først se på hvad en fejl overhovedet er for noget.

## Hvad er en fejl

> handling som har uønskede konsekvenser eller er i modstrid med gældende regler.. sproget.dk

Sådan er en fejl defineret på hjemmesiden sproget.dk, og det identificerer sådan set godt hvad en fejl er i et program.

Eksempel kunne være at du forsøger at skrive til en fil der ikke eksisterer. Det vil være i modstrid med gældende regler (defineret af operativsystemet)

Eller hvis du skal bruge et objekt, og dette objekt ikke har en instans, dvs der er afsat et område i computerens hukommelse til objektet.

Så forsøger du at tilgå noget hukommelse som ikke indeholder det data du forventer, og det kan få uønskede konsekvenser for programmet, så altså en anden type fejl.

Fejl kan altså skyldes mange forskellige ting, men hvordan er det så implementeret i Swift.

### En fejl i Swift forstand.

I Swift er en fejl repræsenteret med et objekt der implementerer protokollen **Error**.

Og **Error** protokollen er meget nem at implementere. Det er nemlig en tom protokol, så der er ikke noget krav om at du skal implementere en funktion eller en variabel på dit objekt. 

Alt du skal gøre for at have en **Error** er altså at sige at du implementere protokollen. 

En fejl kan altså være enten en **struct** en **class** eller en **enum**

Typisk vil man implementere det med en enum, fordi du har ofte behov for at have en specifikation af en fejl. 

Her er eksempelvis en fejl som kunne anendes for fejl ved filer

```swift
enum FilFejl: Error {
    case filFindesIkke
    case mappeFindesIkke
}
```

Der er masser af andre fejl der kan opstå når man arbejder med filer, men ovenstående viser 2 slags fejl man kan forestille sig der opstår når man arbejder med en fil.

Eksempelvis at koden forsøger at læse en fil der ikke findes, eller at koden forsøger at gemme en fil i en mappe der ikke findes. 

Så en fejl kan altså være et objekt af alle typer der implementerer Error protokollen, men hvad gør du når du bliver nød til at udløse en fejl, fordi koden i dit program ellers kommer i en uønsket tilstand.

## Man smider en fejl

Når dit program kommer i en tilstand hvor du bliver nød til at sige "STOP der er sket en fejl" ja så smider du bogstavligt fejlen. 

Kommandoen du skal bruge hedder **throw** og det du smider er en fejl.

```Swift
enum MatematikFejl : Error {
	case DivisionMedNul
)

var tæller = 10
var nævner = 0

//Man må ikke dividere med nul, så hvis nævner er 0 smider vi en fejl

if nævner == 0 {
	throw MatematikFejl.DivisionMedNul
}
```

Ovenstående kode er ikke komplet og vil ikke kompilerer. Men det tjener til det formål at du kan se det simple i at en fejl opstår og vi bliver nød til at smide fejlen. 

I eksemplet har vi oven i købet defineret en slags beskrivende fejl ved at lave en enum der implementerer **Error** protokollen og som gør det nemt at signalere hvilken type fejl der er tale om.

Vi kunne undlade at lave vores egen **MatematikFejl** og i stedet bare smide en fejl ved at udvide en af de eksiterende typer til at håndtere en fejl således

```Swift
var tæller = 10
var nævner = 0

extension String : Error {
	// Vi gør det muligt at en fejl bare kan være en streng ved at lade String implementere Error protokollen
}
	
//Man må ikke dividere med nul, så hvis nævner er 0 smider vi en fejl

if nævner == 0 {
	// Vi kan smide en fejl som en streng.
	throw "Fejl streng"
}
```

Men som du snart vil se, er fordelen ved at lave vores egen fejl, den at vi kan fange fejlen og reagerer på den afhængigt af hvilken type fejl der er tale om. 

## Hvordan reagerer vi på en fejl.

Se svaret er faktisk logisk. Når nogen kaster noget til dig kan du vælge at gribe det eller ej.

Og sådan er det også ved fejl, for vi kaster en fejl og så kan vi vælge at gribe den. Kommandoen til at smide/kaste en fejl hedder **throw** og kommandoen vi bruger til at gribe en fejl hedder **catch**

Men det er ikke nødvendigt at gribe en fejl hver gang, det er dog strengt nødvendigt at håndtere en fejl, og Swift kompileren vil hjælpe os, så hvis du kalder kode som kan smide en fejl, så vil kompileren tvinge dig til at tage stilling til hvad du vil gøre med denne fejl.

Du får simpelthen fejl når du kompilere koden hvis ikke du har taget stilling til hvorledes en fejl håndteres. 

## Håndtering af kode der smider en fejl

Når kode smider en fejl er der flere måder vi kan håndtere det på.

Kommandoen (keywordet) vi skal bruge når vi kalder kode der kan smide en fejl hedder **try**.

Altså det engelske ord for "prøv", og du kan huske det på at du i koden prøver at kalde koden og håber det går godt. 

Men hvis ikke det går godt skal du tage stilling til hvordan du vil reagere på fejlen.

Du kan vælge at gribe fejlen der bliver smidt og det kan se sådan her ud.

```Swift
var tæller = 10
var nævner = 0

//Man må ikke dividere med nul, så hvis nævner er 0 smider vi en fejl

//Fordi vi smider en fejl indkapsler vi kaldet der smider en fejl med try, og fordi vi gerne vil gribe fejlen og reagerer på den så kaplser vi try ind i en do/catch syntax

do {
	try divider(tæller med nævner)
}
catch {
	print("Hov der skete en fejl")
}
```


### do og catch keywords

Når vi vil gribe en fejl der smides så gør vi det med en catch blok.

Syntaxen for do/catch er følgende:

```Swift
do {
	// her kaldes koden der kan fejle, og kald af fejlende kode skal foranstilles med et try
	try funktionDerSmiderEnFejl()
}
catch <fejltype a> {
	// Her skriver du kode der skal reagere på fejl type a
}
catch <fejltype b> {
	// Her skriver du kode der skal reagere på fejl type b
}
catch {
	// Hvis ikke du har angivet en fejltype er der en catch-all funktion som vil fange fejlen uanset hvilken type den er.
}
```

Catch sektionen af en do/catch viser med tydelighed hvorfor en **Error** ofte laves som en enum. 

```Swift
enum LagerFejl : Error {
	case ingenBeholdning
	case aktuelBeholdningLavereEnd(aktuelBeholdning : Int)
}

do {
	try fjernFraLager(antalVarer 5)
}
catch LagerFejl.ingenBeholdning {
	print ("Ingen varer på lageret")
}
catch LagerFejl.aktuelBeholdningLavereEnd(let aktuelBeholdning) {
	print ("Der er kun \(aktuelBeholdning) på lageret")
}
catch {
	print ("Ukendt Fejl")
}
```

Ovenstående viser et eksempel hvor vi har defineret en enum med de 2 fejl der kan opstå i en funktion vi kalder hvor vi forsøger at fjerne et antal varer fra et lager.

En af udfaldene i fejl enum typen har en associeret værdi, og den kan vi bruge når vi skal fange og behandle fejlen.

## Funktioner der smider fejlen

**try** kan kun skrives foran et kald af en funktion som kan smide en fejl.

Kompileren vil brokke sig hvis ikke funktionen er markeret til at kunne smide en fejl, så det skal angives.

Man angiver at en funktion kan smide en fejl ved skrive **throws** efter argument listen og før en eventuel retur type.

```Swift
func divider(tæller : Double, med nævner : Double) throws -> Double { ... }

//Funktionen kaldes herefter således
let resultat = try divider(20.0 med 5)
```

Kompileren hjælper os med at skrive **try** og **throws**.

Hvis funktionen smider en fejl som ikke er håndteret inde i funktionen, så kræver kompileren at vi skriver **throws** i funktions erklæringen, og kompileren vil også tvinge os til at skrive **try** foran kaldet til funktionen, når nu vi ved at den kan smide en fejl.



