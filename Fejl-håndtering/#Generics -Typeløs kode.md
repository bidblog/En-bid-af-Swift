# Generics <Typeløs kode>

>Kend reglerne vel så du kan bryde dem effektivt. --Dalai Lama

Med ovenstående citat, tager vi hul på Genereics eller kode der anvender typer uden typer. 

Swift er type sikker. Du kan ikke sammenligne 2 uens typer, og der er typer på argumenter du kalder funktioner med. 

Men når vi skal angive typer, så ender vi også med at skulle skrive den samme kode igen og igen.

Så ironisk nok er reglerne om type sikkerhed, med til at sikre sig at Swift er nød til at have kode der kan kaldes uden fast typer. 

Tag nedenstående eksempel som bytter rundt på 2 heltal

```Swift
var tal1 = 5
var tal2 = 10

func ombyt(inout tal1 : Int, inout med tal2 : Int) {
	var førsteTal = tal1
	
	tal1 = tal2
	tal2 = førsteTal
}

print (tal1,tal2)
ombyt(tal1, med tal2)
print (tal1,tal2)
```

Funktionen ovenfor tager 2 reference variabler og bytter værdierne rundt.

Det virker fint fordi begge tal er heltal, men hvad nu hvis man vil have den samme funktion for en anden type.

Det er oplagt at hvis vi kan bytte rundt på 2 heltal i en funktion så kunne vi lige så godt få behov for at bytte rundt på 2 kommatal.

I et type sikkert sprog som Swift, betyder det at vi er nød til at lave stort set den samme funktion hvis vi ønsker at den skal kunne betjene en anden type som eksempelvis Double.

```Swift

func ombytHeltal(inout tal1 : Int, inout med tal2 : Int) {
	var førsteTal = tal1
	
	tal1 = tal2
	tal2 = førsteTal
}

func ombytKommatal(inout tal1 : Double, inout med tal2 : Double) {
	var førsteTal = tal1
	
	tal1 = tal2
	tal2 = førsteTal
}
```

Og der er rigtig mange typer i et programmeringssprog, ja ud kan jo lave dine egne typer, så forestil dig hvor mange gange du bliver nød til at skrive ovenstående kode hvis det du har brug for at kunne ombytte alle slags værdier. 

Så hvordan sikrer man sig i et type sikkert sprog mod at skulle skrive det samme kode igen og igen. 

## Den dovne mands løsning.

> Jeg vil altid vælge en doven person til en besværlig opgave, fordi han/hun vil finde en let måde at løse det på. - Bill Gates.

Løsningen på at undgå at skulle skrive den samme kode for alle typer, er at anvende generiske (eller Genrics) kode.

```Swift
func ombytHeltal(inout tal1 : Int, inout med tal2 : Int) {
	var førsteTal = tal1
	
	tal1 = tal2
	tal2 = førsteTal
}

// kan skrives mere generisk således
func ombyt<T>(inout tal1 : T, inout med tal2 : T) {
	var førsteTal = tal1
	
	tal1 = tal2
	tal2 = førsteTal
}
```

Bemærk den sære syntax **<T>** skrevet efter ombyt funktions navnet.

**<T>** er en liste over alias for typer som funktionen kan modtage, og navnet T bliver så alias i resten af funktionen.

Derfor kan du se at argumenterne til funktionen ikke mere er af typen Int, men er nu af typen T.

T er altså ikke en type men et alias vi angiver. Vi kunne have valgt at kalde den andet end T.

```Swift

func ombyt<EtAliasForEnType>(inout tal1 : EtAliasForEnType, inout med tal2 : EtAliasForEnType) {
	var førsteTal = tal1
	
	tal1 = tal2
	tal2 = førsteTal
}

```

Ovenfor er **T** skiftet ud med **EtAliasForEnType** for at illustrere at T ikke er en type, men bare et alias vi kan bruge alle steder i funktionen.

Og som du kan se er argument typerne også skiftet fra **T** til **EtAliasForEnType**

Så <T> er bare en guide for en type uden at angive typen, men kompileren kontrollere stadigvæk, at typerne der anvendes til funktionen er af samme type, og på den måde kan vi skrive kode som er type sikkert uden at vi har angivet typen. 

