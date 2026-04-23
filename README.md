# Devops Release Pipeline - Tronic Construction Software


## Sammanfattning

Tronic Construction Software utvecklar digitala lösningar för byggbranchen, där systemen används direkt ute på byggarbetsplatser. Det gör att stabilitet och tillgänglighet är väldigt viktigt 
om något går fel påverkar det snabbt användarnas arbete. I dagsläget har företaget problem med instabila releaser där buggar ibland når produktionen. Det leder till störningar i systemet och i värsta fall avbrott i arbetsflödet. En stor del av problemet är att testning och kvalitetssäkring inte är tillräckligt strukturerad, samtidigt som releaser sker ganska manuellt.
I denna rapport föreslås en DevOps-baserad release-pipline med automatiserade tester, tydliga quality gates och en mer kontrollerad deployment process. Målet är att minska risken för fel i produktionen och skapa en stabilare leveransprocess utan att bromsa ner utvecklingen.

---

## Företagsbeskrivning
Detta är ett fiktivt men realistikt företag baserat på typiska system inom byggbranchen.

Tronic Construction Software utvecklar system för byggindustrin där användare hanterar projektdata, ritningar och rapporter via webbaserade lösningar. Systemen används i kritiska miljöer där driftstopp kan leda till förseningar i byggprojekt och ekonomiska förluster. Därför är det viktigt att nya releaser inte introducerar buggar eller instabilitet. 

---

## Problem
Företaget har vuxit snabbt och fokus har legat mest på att få ut nya funktioner. Det har gjort att kvalitetssäkring inte riktigt hängt med. 

Detta märks bland annat genom att:
- Releaser sker utan tillräcklig testning
- Ingen tydlig staging-miljö används -- kolla 
- QA-processen är otydlig
- Ansvar mellan utveklare och drift är oklart
- Buggar upptäcks först av slutanvändare

Detta leder till:
- Driftstopp i produktion
- Missnöjda användare
- Ökad stress i teamet
- Många akuta hotfixar

---

## Lösningsförslag (översiktligt)
För att lösa detta föreslås en CI/CD-pipline som automatiskt kontrollerar koden innan den når produktion.

Tanken är att inget ska kunna deployas utan att ha passerat flera steg av testning och granskning. Flödet ser ut ungefär så här:

1. Kod pushas till Git
2. Pull request kräver code review
3. CI kör build och tester
4. Godkänd kod deployas till staging 
5. E2E-tester körs
6. En manuell godkännande-gate innan produktionen
7. Deployment sker stegvis med Canary Release
8. Systemet övervakas efter release

---

## Qualtiy Gates 
För att undvika att dålig kod når produktion införs flera tydliga stopp i pipelinen:
- Build måste lyckas
- Alla tester måste passera
- Kod måste granskas via pull request
- Inga kritiska säkerhetsproblem får finnas
- Deployment till produktion kräver godkännande

Om något av dessa misslyckas så stoppas releasen direkt.

---

## Lösningsförslag (tekniskt)
CI/CD
- Github Actions används för automation
- Pipeline definieras i YAML

Teststrategi
- Enhetstester (snabba och många)
- Integrationstester (API och databas)
- E2E-tester 

Piplinen körs automatiskt vid varje push till main branchen

---

## Deployment
Canary Release används:
- Ny version rullas ut till en liten del av användarna först
- Om inga fel upptäcks då blir det full release
- Vid fel så blir det snabb rollback

---

## Pipeline
Code => Build => Test => Stagning => Approval => Production => Monitoring => Feedback

---

## G-nivå

- Beskriver en fungerande release-Pipline
- Förklarar CI/CD processen
- Visar deployment metod (Canary Release)
- Innehåller quality gates och teststrategier 

## VG-nivå - Jämförelse av release-strategier
Två vanliga strategier är Canary Release och Blue/Green Deployment.

**Canary Release**
- Mindre risk eftersom bara en del av användarna påverkas först
- Gör det möjligt att upptäcka problem tidigt
- Kräver bra övervakning

**Blue/Green Deployment**
- Gör det enkelt att rulla tillbaka snabbt
- Ger en stabil release
- Kräver dubbla miljöer, vilket kan bli dyrt

---


### Val
Canary Release passar bäst för Tronic eftersom systemen är kritiska och det är viktigt att minimera risk vid varje release. Det är bättre att upptäcka problem tidigt än att riskera att alla användare påverkas direkt.

---

## Karriär
Genom denna uppgift har jag fått en bättre förståelse för hur DevOps inte bara handlar om teknik, utan också om processer och ansvar. Jag har lärt mig hur man kan bygga en pipeline som minskar risk och samtidigt gör det enklare att jobba strukturerat. Detta är något jag kan ta med mig i framtida roller, särskilt inom backend eller DevOps, där leveransprocessen är en viktig del av systemets kvalitet.

---

## Exempel på CI Pipline
name: CI PipeLine

on: 
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - name: Install depenndencies
        run: npm install
      - name: Run tests
        run: npm test