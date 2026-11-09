# Fra tomt DevTest4-miljø til første testlogin

Målet er at teste samme fiktive person i to situationer. Først logger personen ind som privatperson. Derefter handler personen på vegne af en testorganisation. Guiden bruger **integrationstestbenet i DevTest4** hele vejen.

## 1. Opret en brugerorganisation

1. Åbn [DevTest4-testportalen for brugerorganisationer](https://testportal.test-devtest4-nemlog-in.dk/BO).
2. Angiv en administratormail, du kan læse, og vælg en testadgangskode. En særskilt API-adgangsnøgle kan beskytte organisationen mod ændringer via API'et.
3. Vælg **Privat virksomhed** eller **Offentlig virksomhed** efter det scenarie, du vil teste. Skal begge typer afprøves, opretter du én organisation af hver type.
4. Vælg sikringsniveau. NemLog-in anbefaler **Betydelig** som udgangspunkt. Tilvælg kvalificerede certifikater, hvis de indgår i testen.
5. Tryk **Opret**. Gem det fiktive CVR-nummer og administratorens bruger-ID fra kvitteringssiden. Opbevar koder og eventuelle API-nøgler i en adgangskodeadministrator.

**Sådan tester du oprettelsen.** Kontrollér, at kvitteringsmailen til administratoren indeholder bruger-ID og et link til miljøet. Åbn derefter [MitID Erhverv på integrationstestbenet](https://erhvervsadministration.test-devtest4-nemlog-in.dk/), vælg **Test login**, og brug ID'et fra kvitteringen sammen med den adgangskode, du selv valgte. Kontrollér, at du kan åbne den forventede organisation.

[Kilde: NemLog-ins vejledning til en testorganisation med fiktivt CVR-nummer](https://www.nemlog-in.dk/om-nemlog-in/miljoer-i-nemlog-in/devtest4-miljoet-tekniske-oplysninger/fiktivt-cvr-nummer-opret-en-brugerorganisation-til-test-devtest4/)

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
