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

func ombyt(_ tal1 : inout Int, med tal2 : inout Int) {
	let ombytning = tal2
	
	tal2 = tal1
	tal1 = ombytning
}

print (tal1,tal2)
ombyt(&tal1, med &tal2)
print (tal1,tal2)
```

Funktionen ovenfor tager 2 reference variabler og bytter værdierne rundt.

**inout** keywordet der står foran typen betyder at der overføres en reference og ikke en kopi så hvis værdierne rettes i funktionen så rettes der altså i den kaldende værdi.

Derfor skal argumenterne i kald til funktionen også forindstilles med **&** tegnet.

```Swift
ombyt(&tal1, med &tal2)
```

Det virker fint fordi begge tal er heltal, men hvad nu hvis man vil have den samme funktion for en anden type.

Det er oplagt at hvis vi kan bytte rundt på 2 heltal i en funktion så kunne vi lige så godt få behov for at bytte rundt på 2 kommatal.

I et type sikkert sprog som Swift, betyder det at vi er nød til at lave stort set den samme funktion hvis vi ønsker at den skal kunne betjene en anden type som eksempelvis Double.

```Swift

func ombytHeltal(_ tal1 : inout Int, med tal2 : inout Int) {
	let ombytning = tal2
	
	tal2 = tal1
	tal1 = ombytning
}

func ombytKommatal(_ tal1 : inout Double, med tal2 : inout Double) {
	let ombytning = tal2

 	tal2 = tal1
	tal1 = ombytning
}
```

Og der er rigtig mange typer i et programmeringssprog, ja ud kan jo lave dine egne typer, så forestil dig hvor mange gange du bliver nød til at skrive ovenstående kode hvis det du har brug for at kunne ombytte alle slags værdier. 

Så hvordan sikrer man sig i et type sikkert sprog mod at skulle skrive det samme kode igen og igen. 

## Den dovne mands løsning.

> Jeg vil altid vælge en doven person til en besværlig opgave, fordi han/hun vil finde en let måde at løse det på. - Bill Gates.

Løsningen på at undgå at skulle skrive den samme kode for alle typer, er at anvende generiske (eller Genrics) kode.

```Swift
func ombytHeltal(_ tal1 : inout Int, med tal2 : inout Int) {
	let ombytning = tal2
	
	tal2 = tal1
	tal1 = ombytning
}

// kan skrives mere generisk således
func ombyt<T>(_ tal1 : inout T, med tal2 : inout T) {
	let ombytning = tal2
	
	tal2 = tal1
	tal1 = ombytning
}
```

Bemærk den sære syntax **<T>** skrevet efter ombyt funktions navnet.

**<T>** er en liste over alias for typer som funktionen kan modtage, og navnet T bliver så alias i resten af funktionen.

Derfor kan du se at argumenterne til funktionen ikke mere er af typen Int, men er nu af typen T.

T er altså ikke en type men et alias vi angiver. Vi kunne have valgt at kalde den andet end T.

```Swift

func ombyt<EtAliasForEnType>(_ tal1 : inout EtAliasForEnType, med tal2 : inout EtAliasForEnType) {
	let ombytning = tal2
	
	tal2 = tal1
	tal1 = ombytning
}

```

Ovenfor er **T** skiftet ud med **EtAliasForEnType** for at illustrere at T ikke er en type, men bare et alias vi kan bruge alle steder i funktionen.

Og som du kan se er argument typerne også skiftet fra **T** til **EtAliasForEnType**

Så <T> er bare en erstatning for en type uden at angive typen, men kompileren kontrollere stadigvæk, at typerne der anvendes til funktionen er af samme type, og på den måde kan vi skrive kode som er type sikkert uden at vi har angivet typen. 

## Kræv en bestemt slags typer
Det kan være nødvendigt at begrænse hvilke typer der kan anvendes trods alt.

Det kan være at funktionen der tager imod en generic type, anvender en operator eller kalder en funktion som forudsætter at de typer der anvendes er af en bestemt slags.

Det kaldes at man laver en constraint (begrænset) type.

Tag eksempelvis følgende funktion som sammenligner 2 heltal

```Swift
func er(_ heltal1 : Int, ensMed heltal2: Int) -> Bool {
	return heltal1 == heltal2
}
```

Denne funktion vil man nok aldrig programmere, da du ikke behøver den for at sammenligne 2 heltal. Men hvis du nu skulle skrive funktionen om til at være en generic funktion så man i stedet kunne bruge den til at sammenligne flere typer, så ville du måske være fristet til at skrive følgende:

```Swift
func er<T>(_ værdi1 : T, ensMed værdi2:T) --> Bool {
	return værdi1 == værdi2
}
```

Men det vil give dig en fejl, fordi kompileren ikke kan garantere at den type der anvendes, faktisk kan sammenlignes med == operatoren. 

For at sikre sig mod dette er du nød til at begrænse hvilke typer funktionen kan anvende, og det gør du ved at angive en protokol som argument typen skal understøtte.

Da vores funktion sammenligner 2 værdier af samme type, altså undersøger om de er ens, kan vi fjerne fejlen ved at skrive:

```Swift
func er<T: Equatable>(_ værdi1 : T, ensMed værdi2:T) --> Bool {
	return værdi1 == værdi2
}
```

Her angiver vi en protokol som typen skal overholde og for at vi kan anvende == operatoren, så skal en type overholde Equatable protokollen.

Når vi angiver denne begrænsning kan kompileren sikre sig mod at vi kommer til at anvende funktionen med en type der vil få programmet til at gå ned. 