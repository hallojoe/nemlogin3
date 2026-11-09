# Fra tomt DevTest4-miljø til første testlogin

## 1. Opret en tjenesteudbyder (TU) eller en brugerorganisation (BO)

Hvem udbyder it-systemet, og hvem skal logge ind i det? I DevTest4 kan de to organisationer have hver sit fiktive CVR-nummer. Ved projektets oprettelse af en BO og en TU oprettede portalen automatisk en testbruger for hver organisation. Brugeren fra hver kvittering kunne både logge ind som privatperson og repræsentere sin egen organisation. Tabellen viser, hvor du begynder.

| Det vil du teste | Vælg | Hvad gør du bagefter? |
| --- | --- | --- |
| Dit eget it-system skal bruge NemLog-in til login. | [TU – tjenesteudbyder](https://testportal.test-devtest4-nemlog-in.dk/TU) | Registrér it-systemet i NemLog-in Administration. TU giver også adgang til MitID Erhverv, så du kan oprette testmedarbejdere i udbyderens egen organisation. |
| En medarbejder skal logge ind på vegne af sin organisation i et allerede tilsluttet it-system. | [BO – brugerorganisation](https://testportal.test-devtest4-nemlog-in.dk/BO) | Start med brugeren fra BO-kvitteringen. Opret en ekstra medarbejder i MitID Erhverv, hvis du vil teste andre rettigheder. |
| En kunde skal bruge din løsning, fx en kommune, der logger ind hos en privat udbyder. | TU til udbyderen og BO til kunden | Registrér løsningen under TU. Log ind med brugeren fra kundens BO-kvittering. Ved erhvervslogin skal løsningen vise kundens CVR-nummer. |

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
3. **Kontrollér organisationens CVR-nummer.** Åbn MitID Erhverv, vælg **Test login**, og log ind som administrator. Kontrollér, at organisationens CVR-nummer matcher kvitteringen. Guidens loginprøver bruger [MitID Erhverv på DevTest4-integrationstestbenet](https://erhvervsadministration.test-devtest4-nemlog-in.dk/).

[NemLog-ins BO-vejledning](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/fiktivt-cvr-nummer-opret-en-brugerorganisation-til-test-devtest4/)

### Sådan læser du TU-kvitteringen

Eksemplet nedenfor bygger på projektets kvittering for en offentlig TU. Feltnavnene og de valgte organisationstyper er gengivet fra kvitteringen. Identifikatorer og loginoplysninger er erstattet af pladsholdere.

| Felt på kvitteringen | Eksempel eller anvendelse |
| --- | --- |
| Organisationsnavn og CVR-nummer | `Testorganisation nr. <test-CVR>`. Brug kvitteringens CVR-nummer til at kontrollere, hvilken organisation brugeren repræsenterer. |
| Brugernavn og Password | Brug `<testbruger>` og din valgte adgangskode under **Test login**. |
| Fornavn og Efternavn | Den automatisk oprettede testpersons navn. |
| RID | Brugerens identifikator i organisationen. Feltet **Brugernavn** er det, du indtaster ved login. |
| API-adgangsnøgle | Feltet var tomt i eksemplet. |
| Organisationstype og CSS Organisationstype(r) | På kvitteringen stod **Offentlig virksomhed** og **Tjenesteudbyder**. Det første er typen af organisation, det andet er dens rolle. |
| EIA og EIA Integrationtest | Links til MitID Erhverv. Brug **EIA Integrationtest** til erhvervsadministrationen på guidens integrationstestben. |
| MitID simulator | Link til simulatoren, fx hvis du senere vil oprette en ekstra testperson. |
| Administrationen | Åbn NemLog-in Administration herfra for at registrere it-systemet under TU’en i trin 2. |

## 2. Log ind i NemLog-in Administration, og opret it-systemet

Når du har oprettet både TU og BO, skal it-systemet registreres under **TU’en**. BO-brugeren kan du senere bruge til at teste en medarbejder hos kunden. Har du allerede et tilsluttet it-system og vil du kun teste login, kan du gå direkte til trin 3.

1. Åbn [NemLog-in Administration i DevTest4](https://administration.devtest4-nemlog-in.dk/). Vælg **Test login**, og brug brugernavnet fra TU-kvitteringen og den adgangskode, du valgte ved oprettelsen.
2. Når du skal vælge, hvem du vil logge ind som, så vælg **TU’ens organisation**. Sammenhold navn og CVR-nummer med TU-kvitteringen.
3. Åbn **Løs opgaver → Opret nyt it-system**. Angiv, om *det konkrete it-system* udfører en myndighedsopgave, og giv systemet et navn. Når du har besvaret spørgsmålet om myndighedsopgaven, kan du vælge de NemLog-in-komponenter, systemet skal bruge. Vælg login til guidens loginprøve.
4. Vælg **Tilføj teknisk administrator**. Vælg brugeren fra TU-kvitteringen i medarbejderlisten, tryk **Næste**, kontrollér oplysningerne, og tryk **Tilføj**.
5. Log ud og ind igen som TU-brugeren på vegne af organisationen. Find systemets navn under **It-systemer**, og åbn det. Når du kan se systemets metadataside, har du bekræftet oprettelsen.

For at prøve login i det nye system skal du derefter indlæse systemets metadata, tilføje en medarbejder-testbruger og anmode om integrationstest. [NemLog-ins vejledning fra registrering til integrationstest](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/nyt-it-system-oprettes-i-devtest4-pre-produktionsmiljoet/)

## 3. Afprøv den automatisk oprettede bruger

Start fra et tilsluttet it-system på DevTest4-integrationstestbenet, der understøtter både privat- og erhvervslogin. Brug **samme brugernavn og adgangskode fra én kvittering** i de to prøver.

| Prøve | Handling | Bestået når |
| --- | --- | --- |
| Privatlogin | Vælg **Test login**, log ind med kvitteringens bruger, og fortsæt som privatperson. | It-systemet viser testpersonen i privat kontekst. |
| Organisationslogin | Start en ny browsersession, log ind med samme bruger, og fortsæt på vegne af organisationen. | It-systemet viser organisationen med CVR-nummeret fra den samme kvittering. |

Har du oprettet både BO og TU, kan du gentage de to prøver med den anden kvittering. Når du tester en kunde i din TU’s løsning, bruger du BO-brugeren og sammenholder resultatet med **BO’ens CVR-nummer**. Hvis en loginmulighed mangler, kontrollér først, hvilke loginformer it-systemet understøtter, og at du er på det rigtige DevTest4-ben. [Tekniske oplysninger om DevTest4](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/)

## 4. Opret en ekstra testperson efter behov

Brug denne fremgangsmåde, hvis du vil teste en anden medarbejder, særskilte rettigheder eller samme person i flere organisationer.

1. Åbn [MitID Simulator](https://mitidsimulator.test-devtest4-nemlog-in.dk/). Vælg **Create identity**, brug fiktive personoplysninger med test-CPR, og markér **Private MitID**. Gem testpersonens bruger-ID og adgangskode.
2. Log ind som administrator i MitID Erhverv på integrationstestbenet. Under **Indstillinger → Identifikationsmidler** skal organisationen tillade brug af privat MitID.
3. Vælg **Brugere → Opret bruger**. Brug navn og test-CPR fra simulatoren samt en mailadresse, du kan læse. Vælg **Privat MitID** som identifikationsmiddel, og opret brugeren.
4. Åbn aktiveringslinket fra MitID Erhverv i en ny browsersession. Under **Test login** bruger du nu testpersonens bruger-ID og adgangskode fra simulatoren. Bekræft brug af privat MitID, og kontrollér, at kvitteringen viser, at personen kan handle for organisationen.

Skal samme person teste begge organisationstyper, gentager du erhvervsoprettelsen og aktiveringen for den anden organisation med samme testperson. Erhvervsbrugeren skal oprettes på det DevTest4-ben, hvor it-systemet testes.

[Kilde: NemLog-ins vejledning til oprettelse og aktivering af erhvervsbrugere](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/opret-mitid-erhverv-brugere-til-test-i-devtest4-miljoet/)
