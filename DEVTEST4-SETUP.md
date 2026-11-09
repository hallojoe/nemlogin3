# Fra tomt DevTest4-miljø til første testlogin

## 1. Vælg testorganisation efter opgaven

Hvem udbyder it-systemet, og hvem skal logge ind i det? I DevTest4 kan de to organisationer have hver sit fiktive CVR-nummer. Begge formularer opretter en organisation og et administratorlogin. Tabellen viser, hvor du begynder.

| Det vil du teste | Vælg | Hvad gør du bagefter? |
| --- | --- | --- |
| Dit eget it-system skal bruge NemLog-in til login. | [TU – tjenesteudbyder](https://testportal.test-devtest4-nemlog-in.dk/TU) | Registrér it-systemet i NemLog-in Administration. TU giver også adgang til MitID Erhverv, så du kan oprette testmedarbejdere i udbyderens egen organisation. |
| En medarbejder skal logge ind på vegne af sin organisation i et allerede tilsluttet it-system. | [BO – brugerorganisation](https://testportal.test-devtest4-nemlog-in.dk/BO) | Opret og aktivér medarbejderen i MitID Erhverv. Du kan også afprøve brugeradministration og certifikater. |
| En kunde skal bruge din løsning, fx en kommune, der logger ind hos en privat udbyder. | TU til udbyderen og BO til kunden | Registrér løsningen under TU. Opret medarbejderen under BO. Ved erhvervslogin skal løsningen vise kundens CVR-nummer. |

**Privat eller offentlig** beskriver den organisation, du opretter. På TU vælger du typen for udbyderen, på BO for den organisation, medarbejderen repræsenterer. Samme person kan derfor godt teste både privatlogin som borger og erhvervslogin for en offentlig organisation.

### Felter på både TU og BO

| Felt | Hvad skal du vælge? |
| --- | --- |
| Administratorens e-mailadresse | Brug en postkasse, du kan åbne. Her modtager du administratorens bruger-ID og linket til miljøet. |
| Password | Vælg en adgangskode til administratorens testlogin, og gem den ved oprettelsen. |
| API-adgangsnøgle | Valgfri. Angiv en separat nøgle for at beskytte organisationen mod ændringer via API'et. Ved efterfølgende API-kald sender du nøglen i HTTP-headeren `X-API-HEADER`. |
| Kvalificerede certifikater | Sæt kryds, hvis testen omfatter kvalificerede bruger- eller organisationscertifikater i MitID Erhverv. Til en almindelig loginprøve kan feltet stå tomt. |
| Organisationstype | **Privat virksomhed** findes på begge formularer. Den offentlige type hedder **Offentlig organisation** på TU og **Offentlig virksomhed** på BO. |
| Sikringsniveau for identifikationsproces | Gælder organisationens identifikation af erhvervsbrugere ved oprettelse. Vælg efter den proces, du vil afprøve, som beskrevet nedenfor. |

### Hvornår skal du vælge kvalificerede certifikater?

Lad feltet stå tomt til guidens loginprøve. Her skal løsningen vise, hvem der er logget ind, og hvilken organisation personen repræsenterer. Sæt kryds, hvis en del af testen er, at en medarbejder skal underskrive et dokument med en kvalificeret signatur, eller at en organisation skal sætte et kvalificeret elektronisk segl. Sæt krydset på den testorganisation, hvis medarbejder skal underskrive, eller hvis segl skal fremgå af dokumentet.

Et certifikat forbinder en identitet med en offentlig nøgle. Modtageren kan bruge nøglen til at kontrollere en signatur eller et segl. **Kvalificeret** betyder, at en kvalificeret tillidstjenesteudbyder har udstedt certifikatet efter eIDAS-kravene. En kvalificeret signatur eller et kvalificeret segl kræver også, at selve signeringen eller forseglingen sker med et kvalificeret signatur- eller seglgenereringssystem. [eIDAS, artikel 3, 25 og 35](https://eur-lex.europa.eu/eli/reg/2014/910/2024-10-18/dan)

| Situation | Hvad bruger du? | Hvad kan modtageren kontrollere? |
| --- | --- | --- |
| En medarbejder underskriver en aftale på vegne af virksomheden. | Et kvalificeret brugercertifikat til elektronisk signatur. | Hvilken person der underskrev, og om det underskrevne indhold er ændret. En kvalificeret elektronisk signatur har efter eIDAS samme retsvirkning som personens håndskrevne underskrift. |
| En kommune udsteder en kvittering med kommunen som afsender. | Et kvalificeret certifikat til organisationens elektroniske segl. | At kvitteringen stammer fra kommunen, og at indholdet er intakt. Det er kommunen, der står som afsender. |

**Prøv det i DevTest4.** Brug en løsning med signering eller segldannelse på testbenet. Underskriv eller forsegl et testdokument, og validér det i testmiljøet. Kontrollér, at resultatet viser den forventede person eller organisation, den forventede signatur- eller segltype og intakt indhold. Afkrydsningen på TU eller BO er et tilvalg ved oprettelsen. NSIS-feltet nedenfor styrer særskilt, hvordan organisationen identificerer erhvervsbrugere. [MitID Erhvervs vilkår, bilag 4 og 5](https://www.mitid-erhverv.dk/om-mitid-erhverv/vilkar/) · [Funktioner i DevTest4](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/)

### Sikringsniveau for identifikationsprocessen

Til guidens første loginprøve anbefaler NemLog-in **Betydelig** som udgangspunkt. It-systemets krav til et login fastlægges separat.

| Valg i formularen | Betydning for testen |
| --- | --- |
| Intet | Organisationen angiver intet NSIS-niveau for identifikationsprocessen. |
| Betydelig | Organisationsadministratoren kan give en identitetsadministrator lov til at oprette erhvervsbrugere direkte på niveau Betydelig. |
| Høj | Brug dette til at teste oprettelse direkte på niveau Høj. Også her skal organisationsadministratoren give identitetsadministratoren den relevante tilladelse. |

### Ekstra valg på TU

**Organisationsrolle(r)** angiver, hvem der har ansvaret for it-systemet. Vælg **Tjenesteudbyder**, når organisationen udbyder sit eget system. Vælg **IT leverandør**, når den udfører den tekniske integration for en anden udbyder. Vælg **Tjenesteudbyder og IT leverandør**, når du skal teste begge roller.

**Tillad myndighedsudøvelse** vises, når du vælger **Privat virksomhed** og en rolle, der omfatter **Tjenesteudbyder**. Vælg den situation, som den private udbyder skal repræsentere i testen.

| Valg i formularen | Testscenarie |
| --- | --- |
| Nej | En privat udbyder tilbyder sin egen tjeneste uden myndighedsudøvelse. |
| Ja, offentligtretligt organ | Organisationen testes i egenskab af offentligretligt organ. |
| Ja, leverandør til det offentlige | Organisationen testes som leverandør, der udfører en myndighedsopgave for det offentlige. |
| Ja, broker til det offentlige | Organisationen testes som broker for det offentlige. |

En privat it-leverandør, som integrerer **kommunens** system, kan fx afprøve leverandørrollen. Hvis **leverandøren selv** er tjenesteudbyder for en løsning med en myndighedsopgave, er myndighedsvalget relevant. For en privat tjenesteudbyder beskriver NemLog-in adgang til login, STS og signering. [NemLog-ins TU-vejledning](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/fiktivt-cvr-nummer-opret-en-it-systemudbyder-til-test-devtest4/)

### Opret og kontrollér organisationen

1. Udfyld den valgte formular, og tryk **Opret**. Gem det tildelte CVR-nummer og administratorens bruger-ID fra kvitteringssiden. Opbevar adgangskoden og en eventuel API-nøgle i en adgangskodeadministrator.
2. Kontrollér kvitteringsmailen. Den indeholder bruger-ID og miljølink. Brug den adgangskode, du selv valgte.
3. **TU:** Hvis du valgte tjenesteudbyderrollen, skal du kunne logge ind i NemLog-in Administration og påbegynde registrering af dit it-system. Registreringen er næste trin, hvis du vil afprøve login i din egen løsning. [Opret it-systemet i DevTest4](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/nyt-it-system-oprettes-i-devtest4-pre-produktionsmiljoet/)
4. **BO eller TU med egne medarbejdere:** Åbn MitID Erhverv, vælg **Test login**, og log ind som administrator. Kontrollér, at organisationens CVR-nummer matcher kvitteringen. Fortsæt med trin 2 for at oprette testpersonen. Guidens loginprøver bruger [MitID Erhverv på DevTest4-integrationstestbenet](https://erhvervsadministration.test-devtest4-nemlog-in.dk/).

[NemLog-ins BO-vejledning](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/fiktivt-cvr-nummer-opret-en-brugerorganisation-til-test-devtest4/)

## 2. Giv en testperson adgang som både privatperson og medarbejder

Administratorens login fra trin 1 bruges til opsætningen. Opret en testperson til selve loginprøven.

1. Åbn [MitID Simulator](https://mitidsimulator.test-devtest4-nemlog-in.dk/). Vælg **Create identity**, brug fiktive personoplysninger med test-CPR, og markér **Private MitID**. Gem testpersonens bruger-ID og adgangskode.
2. Log ind som administrator i MitID Erhverv på integrationstestbenet. Under **Indstillinger → Identifikationsmidler** skal organisationen tillade brug af privat MitID.
3. Vælg **Brugere → Opret bruger**. Brug navn og test-CPR fra simulatoren samt en mailadresse, du kan læse. Vælg **Privat MitID** som identifikationsmiddel, og opret brugeren.
4. Åbn aktiveringslinket fra MitID Erhverv i en ny browsersession. Under **Test login** bruger du nu testpersonens bruger-ID og adgangskode fra simulatoren. Bekræft brug af privat MitID, og kontrollér, at kvitteringen viser, at personen kan handle for organisationen.

Skal samme person teste begge organisationstyper, gentager du erhvervsoprettelsen og aktiveringen for den anden organisation med samme testperson. Erhvervsbrugeren skal oprettes på det DevTest4-ben, hvor it-systemet testes.

[Kilde: NemLog-ins vejledning til oprettelse og aktivering af erhvervsbrugere](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/opret-mitid-erhverv-brugere-til-test-i-devtest4-miljoet/)

## 3. Afprøv login i et tilsluttet it-system

Disse prøver kræver et it-system på DevTest4-integrationstestbenet, der understøtter de relevante loginformer. Start hver prøve fra systemets login i en ny browsersession, og brug testpersonens identitet.

| Prøve | Handling | Bestået når |
| --- | --- | --- |
| Privatperson | Log ind som dig selv. | Systemet viser testpersonen i privat kontekst. |
| Privat virksomhed | Log ind på vegne af den private testorganisation. | Systemet viser organisationen med det fiktive CVR-nummer fra trin 1. |
| Offentlig virksomhed | Log ind på vegne af den offentlige testorganisation. | Systemet viser den offentlige organisations eget fiktive CVR-nummer. |

Hvis den forventede organisation mangler, kontrollér først brugerens aktiveringskvittering og derefter, at it-systemet og erhvervsbrugeren er på samme ben. Notér dato, organisationstype og udfald for hver prøve, når den er gennemført.

[Kilde: Tekniske oplysninger om DevTest4 og miljøets to ben](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/)
