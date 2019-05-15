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

Så en fejl kan altså være et objekt af alle typer der implementerer Error protokollen, men hvad gør man når man vil udløse en sådan fejl.


