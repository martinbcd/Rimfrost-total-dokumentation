---
titel: "Rimfrost — masterdokument för regelverk, ramverk, templates och exempel"
dokument-id: RIMFROST-MASTER
version: 1.1.0
datum: 2026-10-26
validerad: "2026-10-26 — se avsnitt [0.6]"
språk: sv
primär-läsare: AI-assistent (frågebesvarande och implementationsstöd)
sekundär-läsare: utvecklare
rotkatalog-för-källreferenser: "C:/Programmering/FKRimfrost/"
källprioritet: "kod > template > exempel > README/docs"
exkluderade-repon:
  - rimfrost-regel-katt-sjukpenning-maskinell   # enligt användarens instruktion: ingen originalinformation
ej-klonade-men-refererade-repon:
  - rimfrost-portal-handlaggare
  - rimfrost-service-*   # handläggning, oul, workflow, sid m.fl.
  - rimfrost-adapter-*   # folkbokford, arbetsgivare, referensdata m.fl.
  - rimfrost-kubernetes
  - rimfrost-ersattning-data
källrepon-med-commit:
  rimfrost: c21c5cb
  rimfrost-framework-bff: 4e15d8f
  rimfrost-framework-handlaggning-adapter: ca85895
  rimfrost-framework-oul: 55a2cfb
  rimfrost-framework-oul-adapter: b8aae81
  rimfrost-framework-process: 626ba7c
  rimfrost-framework-referensdata-interface: 55fdb82
  rimfrost-framework-regel: e4ffda1
  rimfrost-framework-regel-asyncapi: d78a00c
  rimfrost-framework-regel-error-codes: 0e3da27
  rimfrost-framework-regel-kommunicering: 3d91052
  rimfrost-framework-regel-komplettering: b9ed447
  rimfrost-framework-regel-manuell: 59de6f0
  rimfrost-framework-regel-manuell-openapi: 1ac7668
  rimfrost-framework-regel-maskinell: be01d8e
  rimfrost-framework-regel-oul: 6adf392
  rimfrost-framework-regel-oul-asyncapi: e2b31c1
  rimfrost-framework-regel-oul-openapi: cbaed81
  rimfrost-framework-sid-adapter: 4e0bac1
  rimfrost-guide: f1769d9
  rimfrost-portal-bff: 9f4fac9
  rimfrost-process-vah: f0016ec
  rimfrost-referensdata-erbjudande: ff03e53
  rimfrost-regel-bekraftabeslut: 016aff2
  rimfrost-regel-bekraftabeslut-bff: ae1adf0
  rimfrost-regel-bekraftabeslut-fe: 43f6e01
  rimfrost-regel-bekraftabeslut-subprocess: 5eb35ec
  rimfrost-regel-beraknaersattning: a79aaf7
  rimfrost-regel-kommunicering: 43c58eb
  rimfrost-regel-kommunicering-openapi: b9799f8
  rimfrost-regel-maskinell-subprocess: 99ae578
  rimfrost-regel-rtf-manuell: 4139265
  rimfrost-regel-rtf-manuell-asyncapi: 2b9315f
  rimfrost-regel-rtf-manuell-bff: e964828
  rimfrost-regel-rtf-manuell-fe: 71ba514
  rimfrost-regel-rtf-manuell-komplettering: 6685ce9
  rimfrost-regel-rtf-manuell-komplettering-bff: a64ec1c
  rimfrost-regel-rtf-manuell-komplettering-fe: 17a4363
  rimfrost-regel-rtf-manuell-komplettering-openapi: 6ffa81f
  rimfrost-regel-rtf-manuell-openapi: 4791617
  rimfrost-regel-rtf-manuell-subprocess: 7de1198
  rimfrost-regel-rtf-maskinell: bda2b44
  rimfrost-regel-rtf-maskinell-openapi: 070d593
  rimfrost-regel-rtf-maskinell-subprocess: 3097b4b
  rimfrost-regler: 9207651
  rimfrost-template-micro-fe: 0492833
  rimfrost-template-micro-fe-bff: df9b1bf
  rimfrost-template-process: 159f2ed
  rimfrost-template-referensdata-erbjudande: 9748c2a
  rimfrost-template-regel-kommunicering: 0f4aa66
  rimfrost-template-regel-kommunicering-openapi: 9503fde
  rimfrost-template-regel-komplettering: 7248ae1
  rimfrost-template-regel-komplettering-openapi: 4c9617f
  rimfrost-template-regel-manuell: 9ddc13d
  rimfrost-template-regel-manuell-openapi: e704869
  rimfrost-template-regel-maskinell: 481e093
  rimfrost-template-regel-subprocess: 0b36c83
---

# Rimfrost — masterdokument

> Detta dokument är skrivet för att läsas av en AI. Läs avsnitt `[0]` först. Det beskriver hur
> resten av dokumentet är uppbyggt, hur källreferenser ska tolkas och vad du ska göra när källor
> säger emot varandra.

## Innehållsförteckning (ID-baserad)

| ID | Rubrik | Använd när frågan gäller … |
|---|---|---|
| `[0]` | Läsinstruktioner för AI | hur dokumentet ska tolkas |
| `[1]` | Repokatalog | vilka repon som finns, vad de gör, versioner |
| `[2]` | Arkitektur och flöden | helheten: process → subprocess → regel → OUL → portal |
| `[3]` | Domänmodell och versionshantering | `Handlaggning`, `Yrkande`, `ProduceratResultat`, `RegelUtils`, `version` |
| `[4]` | Gemensamt regelramverk (`rimfrost-framework-regel`) | Kafka-kontrakt, `config.yaml`, utfall, felkoder, test-JAR |
| `[5]` | Maskinell regel | hur man bygger en automatisk regel |
| `[6]` | Manuell regel | regel med handläggarinteraktion via portal |
| `[7]` | Kompletteringsregel | regel som kompletterar saknad information i yrkandet |
| `[8]` | regel-oul och OUL-integration | OUL-uppgifter, korrelationstabeller, statusnotifieringar, cancelled-event |
| `[9]` | Regel-subprocess (BPMN) | Kogito-subprocess som anropar en regel via Kafka |
| `[10]` | Process (toppnivå) | Kogito-flödesprocess, `rimfrost-process-vah`, workflow |
| `[11]` | OpenAPI/AsyncAPI-repon | kontraktsrepon per regel och ramverk |
| `[12]` | Referensdata-erbjudande | `ErbjudandeReferensdataInterface` |
| `[13]` | Frontend: micro-fe, BFF och portal | Vue-remote, Quarkus-BFF, `remotes.json` |
| `[14]` | Felhantering och ansvarsfördelning | vem gör vad vid fel, HTTP-statusmappning |
| `[15]` | Konfigurationsreferens | alla properties per komponent |
| `[16]` | Kafka-topicreferens | namnkonventioner, vilka topics som krävs |
| `[17]` | Steg-för-steg-recept | ordning och checklistor för att skapa nya repon |
| `[18]` | Kända avvikelser och fallgropar | motsägelser, fel i docs/templates, risker |
| `[19]` | Ej täckt / öppna frågor | vad dokumentet inte kan svara på |
| `[20]` | Ordlista | svenska domäntermer |
| `[21]` | Källindex | alla refererade filer per repo |

---

## [0] Läsinstruktioner för AI

### [0.1] Källreferenser

- Varje faktapåstående följs av en eller flera källreferenser i formatet
  `repo/sökväg/fil:rad`, `repo/sökväg/fil:rad-rad` eller `repo/sökväg/fil`, omslutet av
  tecknen U+3014 och U+3015 (vinkelhakar). Exempel på en verklig referens:
  〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml:5-9〕.
  Mönster för maskinell extraktion: allt mellan U+3014 och närmast följande U+3015.
- Sökvägen är relativ till rotkatalogen `C:/Programmering/FKRimfrost/` (se frontmatter).
  Första sökvägssegmentet är alltid reponamnet.
- Radnummer gäller den commit som anges i frontmatter (`källrepon-med-commit`). Om repot har
  uppdaterats kan raderna ha flyttats; sök då på symbolnamnet.
- En referens utan radnummer betyder att hela filen (eller dess struktur) är källan.

### [0.2] Säkerhetsmärkning

| Märkning | Betydelse | Hur du ska använda påståendet |
|---|---|---|
| (ingen märkning) eller `[KÄLLA]` | Läst direkt i angiven fil | Behandla som fakta för angiven commit |
| `[HÄRLETT]` | Slutsats dragen från flera källor eller från hur koden fungerar | Använd, men nämn att det är en slutsats om frågeställaren behöver säkerhet |
| `[EJ VERIFIERAT]` | Trolig konsekvens som inte har testats (inget har byggts eller körts) | Presentera som risk/misstanke, inte som fakta |
| `[AVVIKELSE-nn]` | Hänvisning till en post i `[18]` | Slå upp posten innan du svarar |

### [0.3] Källprioritet vid motsägelser

1. **Kod** (`src/main/**`) i ramverksrepon är sanningen om beteende.
2. **Templates** (`rimfrost-template-*`) är sanningen om vilka filer och vilken konfiguration
   en ny regel förväntas ha — men kan innehålla fel (se `[18]`).
3. **Exempelregler** (`rimfrost-regel-*`) visar faktisk användning i drift — men har
   kopierade värden och lokala avvikelser.
4. **README, `docs/krav.md`, `docs/*.md` och `rimfrost/*.md`** beskriver avsikt och kan vara
   inaktuella.

Om två källor säger emot varandra: svara enligt högsta prioritet och nämn avvikelsen (finns den i
`[18]`, hänvisa dit).

### [0.4] Konventioner i dokumentet

- **Rubrik-ID:** Varje rubrik har ett stabilt ID inom hakparenteser, t.ex. `[6.4]`. Hänvisa till
  avsnitt med ID, inte rubriktext.
- **Kodblock:** Märkta med `// KÄLLA: …` (exakt eller nästan exakt utdrag) eller
  `// MALL` (förenklad mall som du kan anpassa). Plocka aldrig signaturer från ett `// MALL`-block
  utan att jämföra mot källan.
- **Platshållare:** `<regel>` = kebab-case-namn på regeln (t.ex. `rtf-manuell`), `<Regel>` =
  PascalCase (t.ex. `RtfManuell`), `<regel_snake>` = snake_case (t.ex. `rtf_manuell`).
- **Termer:** Svenska domäntermer används oförändrade (handläggning, yrkande, utfall). Se `[20]`.
- **"Regeltyp"** betyder en av: maskinell, manuell, komplettering (kommunicering är planerad
  men tom, se `[19]`).

### [0.5] Snabbguide: vilken sektion svarar på vad?

| Fråga (exempel) | Gå till |
|---|---|
| "Vilka filer behöver jag skapa för en ny manuell regel?" | `[6.3]`, `[17.2]` |
| "Vilka properties är obligatoriska för en manuell regel?" | `[6.7]`, `[15]` |
| "Varför startar inte min regel?" | `[18]`, `[4.6]`, `[8.6]` |
| "Hur hänger topic-namnen ihop mellan process, subprocess och regel?" | `[16]`, `[9.5]` |
| "Vad händer om handläggningstjänsten är nere?" | `[14]`, `[5.6]`, `[6.6]` |
| "Hur testar jag min regel?" | `[5.10]`, `[6.11]`, `[7.10]` |
| "Hur registrerar jag en ny micro-frontend?" | `[13.4]` |
| "Vad betyder `aktivitetId`/`replyTo`/`kogitoprocinstanceid`?" | `[4.2]`, `[20]` |
| "Vad ska version vara när jag skriver tillbaka handläggningen?" | `[3.3]` |

### [0.6] Valideringslogg (version 1.1.0)

Dokumentet har validerats mot källorna i två steg:

1. **Automatisk kontroll:** varje källreferens pekar på en fil som finns och, där radnummer
   anges, på rader som finns; alla avsnitts- och avvikelse-ID:n är unika och alla hänvisningar
   går att slå upp; alla repon i katalogen är nämnda.
2. **Manuell genomgång:** varje avsnitt lästes och påståendena jämfördes mot kod, pom-filer och
   BPMN. Rättat i 1.1.0: beroendeträdet `[1.7]`, felkodsanvändning `[4.3]`, omförsök vid
   `NOT_FOUND` `[5.5]`, SID bara vid GET `[6.6]`, exakt subprocessflöde `[9.3]`, VAH-värden och
   -flöde `[9.4]`/`[10.3]`, CORS i micro-fe-templaten `[13.5]`, allvarsgrad för `[AVVIKELSE-02]`.
   Tillagt: `[4.10]`, `[4.11]`, `[AVVIKELSE-23]`–`[AVVIKELSE-26]`.

**Inte verifierat genom körning:** inget repo har byggts (det finns ingen JDK 21 i miljön där
dokumentet togs fram), och ett isolerat test av YAML-parsningen stoppades av behörighetsregler.
Påståenden märkta `[EJ VERIFIERAT]` är därför fortfarande misstankar.

---

## [1] Repokatalog

### [1.1] Namnkonvention för repon

Alla repon har prefixet `rimfrost-` och delas in i nio kategorier 〔rimfrost/README.md:13-23〕:

| Prefix | Syfte |
|---|---|
| `rimfrost-framework-*` | Ramverkskod som implementationer bygger på |
| `rimfrost-template-*` | Startprojekt att kopiera när man skapar nytt |
| `rimfrost-regel-*` | Faktiska regelimplementationer |
| `rimfrost-process-*` | Faktiska processimplementationer |
| `rimfrost-service-*` | Tjänster som regler kommunicerar med |
| `rimfrost-adapter-*` | REST-klienter för integration mot tjänster |
| `rimfrost-portal-*` | Portal och micro-frontends för handläggare |
| `rimfrost-*-openapi` | OpenAPI-specifikationer (REST-kontrakt) |
| `rimfrost-*-asyncapi` | AsyncAPI-specifikationer (Kafka-kontrakt) |

Teknikstack: Java 21, Quarkus, Kogito för processorkestrering, Kafka mellan tjänster
〔rimfrost/README.md:25〕. Frontend: Vue 3 + Module Federation, FKUI, Pinia
〔rimfrost-template-micro-fe/README.md:42-77〕. BFF:er: Java/Quarkus
〔rimfrost-template-micro-fe-bff/docs/teknisk-spec.md:5〕 (se `[AVVIKELSE-14]` om "Node.js/Express").

Quarkus-projekten (ramverk, regler, BFF:er, processer) använder parent
`fk-maven-quarkus-parent:1.12.0`; rena bibliotek som referensdata-implementationerna använder
`fk-maven-parent:1.12.0` 〔rimfrost-template-regel-maskinell/pom.xml〕
〔rimfrost-template-referensdata-erbjudande/pom.xml〕. Byggen kräver miljövariablerna
`GITHUB_ACTOR` och `GITHUB_TOKEN` (med repo-access) för att hämta beroenden samt JDK 21
〔rimfrost-template-regel-maskinell/README.md:17-25〕. Bygg med `./mvnw -s settings.xml clean verify`
〔rimfrost-template-regel-maskinell/README.md:34-36〕.

### [1.2] Ramverksrepon (bibliotek)

| Repo | Maven-koordinat (version i BOM rimfrost-1_2) | Syfte | Källa |
|---|---|---|---|
| `rimfrost-framework-regel` | `se.fk.rimfrost.framework.regel:rimfrost-framework-regel` (1.4.4) + `tests` | Gemensam bas för alla regeltyper: Kafka-konsument/producent, `config.yaml`, `RegelRequestHandlerBase`, `RegelUtils`, test-JAR | 〔rimfrost-framework-regel/README.md〕 〔rimfrost/bom/rimfrost-1_2-bom/rimfrost.md:27〕 |
| `rimfrost-framework-regel-maskinell` | `se.fk.rimfrost.framework.regel.maskinell:rimfrost-framework-regel-maskinell` (1.1.8) + `tests` | Ramverk för maskinella regler: `RegelMaskinellServiceInterface`, `RetryUtil`, `DmnService` | 〔rimfrost-framework-regel-maskinell/README.md〕 |
| `rimfrost-framework-regel-manuell` | `se.fk.rimfrost.framework.regel.manuell:rimfrost-framework-regel-manuell` (1.4.5) + `tests` | Ramverk för manuella regler: service/middleware/controller, SID-kontroll | 〔rimfrost-framework-regel-manuell/README.md〕 |
| `rimfrost-framework-regel-komplettering` | `se.fk.rimfrost.framework.regel.komplettering:rimfrost-framework-regel-komplettering` (0.1.1) + `tests` | Ramverk för kompletteringsregler | 〔rimfrost-framework-regel-komplettering/README.md〕 |
| `rimfrost-framework-regel-oul` | `se.fk.rimfrost.framework.regel.oul:rimfrost-framework-regel-oul` (0.1.2) + `tests` | OUL-uppgifter, korrelationslagring (PostgreSQL), statusnotifieringar, cancelled-event | 〔rimfrost-framework-regel-oul/README.md〕 |
| `rimfrost-framework-oul` | `se.fk.rimfrost.framework.oul:rimfrost-framework-oul` (1.1.3) | Kafka-konsument för OUL-statusnotifieringar, `OulHandlerInterface` | 〔rimfrost-framework-oul/README.md〕 |
| `rimfrost-framework-oul-adapter` | `se.fk.rimfrost.framework.oul:rimfrost-framework-oul-adapter` (1.1.5) | REST-klient mot OUL (`OulAdapter`) | 〔rimfrost-framework-oul-adapter/src/main/java/se/fk/rimfrost/framework/oul/adapter/OulAdapter.java〕 |
| `rimfrost-framework-handlaggning-adapter` | `se.fk.rimfrost.framework.handlaggning:rimfrost-framework-handlaggning-adapter` (1.2.4 i BOM; 1.2.5 på main) | REST-klient mot handläggningstjänsten + domänmodellen | 〔rimfrost-framework-handlaggning-adapter/CHANGELOG.md:5〕 |
| `rimfrost-framework-sid-adapter` | `se.fk.rimfrost.framework.sid:rimfrost-framework-sid-adapter` (0.0.1) | REST-klient mot SID (skyddad identitet) | 〔rimfrost-framework-sid-adapter/src/main/java/se/fk/rimfrost/framework/sid/adapter/SidAdapter.java〕 |
| `rimfrost-framework-regel-error-codes` | `se.fk.rimfrost.framework.regel.error:rimfrost-framework-regel-error-codes` (1.1.1) | Konstanter `RegelFelkod` | 〔rimfrost-framework-regel-error-codes/src/main/java/se/fk/rimfrost/framework/regel/error/RegelFelkod.java〕 |
| `rimfrost-framework-regel-asyncapi` | `se.fk.rimfrost.framework.regel:rimfrost-framework-regel-asyncapi` (1.1.4) | Kafka-kontrakt för regelförfrågan/-svar, genererade Java-klasser (`Utfall`, `RegelErrorInformation`, …) | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml〕 |
| `rimfrost-framework-regel-oul-asyncapi` | `…regel.oul:rimfrost-framework-regel-oul-asyncapi` (0.0.2) | Kafka-kontrakt för cancelled-event (`regel.oul.cancelled`) | 〔rimfrost-framework-regel-oul-asyncapi/asyncapi.yaml〕 |
| `rimfrost-framework-regel-oul-openapi` | `…regel.oul:rimfrost-framework-regel-oul-openapi-jaxrs-spec` (0.0.3) | Gemensamt REST-kontrakt `GET /utokadUppgiftsbeskrivning`, `POST /{handlaggningId}/done` (`RegelOulControllerApi`) | 〔rimfrost-framework-regel-oul-openapi/openapi.yaml:13-41〕 |
| `rimfrost-framework-regel-manuell-openapi` | `…regel.manuell:rimfrost-framework-regel-manuell-openapi-*` (1.0.1) | **Ersatt** av `rimfrost-framework-regel-oul-openapi` | 〔rimfrost-framework-regel-manuell-openapi/README.md〕 |
| `rimfrost-framework-process` | `se.fk.rimfrost.framework.process:rimfrost-framework-process` (1.6.4) | `RegelService` (används från BPMN), `ProcessService`, `RegelProcessResult` | 〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelService.java〕 |
| `rimfrost-framework-referensdata-interface` | `se.fk.rimfrost.framework.referensdata:rimfrost-framework-referensdata-interface` (0.0.1) | `ErbjudandeReferensdataInterface` | 〔rimfrost-framework-referensdata-interface/src/main/java/se/fk/rimfrost/framework/referensdata/ErbjudandeReferensdataInterface.java〕 |
| `rimfrost-framework-bff` | `se.fk.rimfrost.framework.bff:rimfrost-framework-bff` (0.0.1) + `tests` | Gemensam BFF-infrastruktur: felmappning, hälsokontroll, Authorization-vidarebefordran | 〔rimfrost-framework-bff/README.md〕 |
| `rimfrost-framework-regel-kommunicering` | — | **Tom** (endast README från initial commit) | 〔rimfrost-framework-regel-kommunicering/README.md〕 |

### [1.3] Template-repon

| Repo | Typ | Ger | Källa |
|---|---|---|---|
| `rimfrost-template-regel-maskinell` | Maven, Quarkus-app | Maskinell regel | 〔rimfrost-template-regel-maskinell/README.md〕 |
| `rimfrost-template-regel-manuell` | Maven, Quarkus-app | Manuell regel inkl. middleware + Flyway | 〔rimfrost-template-regel-manuell/README.md〕 |
| `rimfrost-template-regel-komplettering` | Maven, Quarkus-app | Kompletteringsregel | 〔rimfrost-template-regel-komplettering/README.md〕 (se `[AVVIKELSE-04]`) |
| `rimfrost-template-regel-manuell-openapi` | Gradle | OpenAPI-repo för en manuell regel | 〔rimfrost-template-regel-manuell-openapi/README.md〕 |
| `rimfrost-template-regel-komplettering-openapi` | Gradle | OpenAPI-repo för en kompletteringsregel | 〔rimfrost-template-regel-komplettering-openapi/README.md〕 |
| `rimfrost-template-regel-subprocess` | Maven, bibliotek (BPMN) | Kogito-subprocess som anropar regel + komplettering | 〔rimfrost-template-regel-subprocess/README.md〕 |
| `rimfrost-template-process` | Maven, Quarkus/Kogito-app | Toppnivåprocess | 〔rimfrost-template-process/README.md〕 |
| `rimfrost-template-referensdata-erbjudande` | Maven, bibliotek | Stub-implementation av `ErbjudandeReferensdataInterface` | 〔rimfrost-template-referensdata-erbjudande/README.md〕 |
| `rimfrost-template-micro-fe` | npm, Vue/Vite | Micro-frontend (Module Federation-remote) | 〔rimfrost-template-micro-fe/README.md〕 |
| `rimfrost-template-micro-fe-bff` | Maven, Quarkus-app | BFF till en micro-frontend | 〔rimfrost-template-micro-fe-bff/docs/teknisk-spec.md〕 |
| `rimfrost-template-regel-kommunicering`, `rimfrost-template-regel-kommunicering-openapi` | — | **Tomma** (endast README) | 〔rimfrost-template-regel-kommunicering/README.md〕 |

### [1.4] Exempel-/produktionsrepon

| Repo | Regeltyp | Beskrivning | Källa |
|---|---|---|---|
| `rimfrost-regel-rtf-maskinell` | Maskinell | Rätt till försäkring: folkbokföring + arbetsgivare, DMN-beslut | 〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RtfService.java〕 |
| `rimfrost-regel-rtf-manuell` | Manuell | Handläggaren tar ställning per ersättningspost (JA/NEJ/FU) | 〔rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/logic/RtfService.java〕 |
| `rimfrost-regel-rtf-manuell-komplettering` | Komplettering | Kompletterar personnummer och avsikt | 〔rimfrost-regel-rtf-manuell-komplettering/src/main/java/se/fk/github/rtf/manuell/komplettering/logic/RtfKompletteringService.java〕 |
| `rimfrost-regel-bekraftabeslut` | Manuell | Bekräfta beslut; egna extra REST-endpoints för referensdata | 〔rimfrost-regel-bekraftabeslut/src/main/java/se/fk/github/bekraftabeslut/presentation/rest/BekraftaBeslutController.java〕 |
| `rimfrost-regel-beraknaersattning` | Maskinell (föråldrad) | Använder framework-regel 0.1.3 och gamla API:t — **följ inte** | 〔rimfrost-regel-beraknaersattning/src/main/java/se/fk/github/regelmaskinell/logic/RegelService.java:20〕 |
| `rimfrost-regel-*-subprocess` (rtf-maskinell, rtf-manuell, bekraftabeslut) | Subprocess | BPMN som anropar respektive regel | 〔rimfrost-regel-rtf-manuell-subprocess/src/main/resources/rtf_manuell.bpmn〕 |
| `rimfrost-regel-maskinell-subprocess` | — | **Tom** | 〔rimfrost-regel-maskinell-subprocess/README.md〕 |
| `rimfrost-regel-*-openapi` / `-asyncapi` | Kontrakt | Regelspecifika REST-/Kafka-kontrakt | 〔rimfrost-regel-rtf-manuell-openapi/openapi.yaml〕 |
| `rimfrost-regel-*-fe` / `-bff` (rtf-manuell, rtf-manuell-komplettering, bekraftabeslut) | Frontend | Micro-frontend + BFF per manuell regel | `[13]` |
| `rimfrost-regel-kommunicering` | — | **Tom** | 〔rimfrost-regel-kommunicering/README.md〕 |
| `rimfrost-regel-kommunicering-openapi` | Kontrakt | Har innehåll (GET/PATCH `/{handlaggningId}`) men ingen implementation finns | 〔rimfrost-regel-kommunicering-openapi/openapi.yaml:8-31〕 |
| `rimfrost-process-vah` | Process | "Vård av husdjur": anropar rtf-maskinell, rtf-manuell (med komplettering) och bekräfta beslut | 〔rimfrost-process-vah/src/main/resources/vah.bpmn〕 |
| `rimfrost-portal-bff` | Portal | Tjänar `remotes.json` (Module Federation-register) | 〔rimfrost-portal-bff/src/main/resources/remotes.json〕 |
| `rimfrost-referensdata-erbjudande` | Bibliotek | Riktig implementation av `ErbjudandeReferensdataInterface` mot referensdata-adaptern | 〔rimfrost-referensdata-erbjudande/src/main/java/se/fk/rimfrost/referensdata/erbjudande/ErbjudandeReferensdata.java〕 |

### [1.5] Dokumentations- och övriga repon

| Repo | Innehåll | Relevans |
|---|---|---|
| `rimfrost` | Översiktsdokumentation: `README.md`, `FRAMEWORK.md`, `ASYNC_ROUTING.md`, `FAULT_HANDLING_OVERVIEW.md`, `regler/*`, `processer/README.md`, `frontend/*`, `bom/*`, `sequences/*.mmd`, Claude-kommandon i `.claude/commands/` | Hög — men delvis inaktuell (se `[18]`) |
| `rimfrost-guide` | Vue-webbplats för icke-utvecklare (GitHub Pages) | Låg teknisk relevans 〔rimfrost-guide/README.md〕 |
| `rimfrost-regler` | Gammal generisk Quarkus-mall (Fullmakt-exempel) | Ingen — ignorera 〔rimfrost-regler/README.md〕 |

### [1.6] BOM `rimfrost-1_2`

`rimfrost/bom/rimfrost-1_2-bom/rimfrost.md` och `vah.md` listar varje repo med git-taggen
`rimfrost-1_2`, artefakter och versioner. Pom:arna i repona deklarerar `*-SNAPSHOT`; versionen är
release-taggen som `rimfrost-1_2` pekar på 〔rimfrost/bom/rimfrost-1_2-bom/rimfrost.md:3-5〕.
Använd BOM:en för att välja beroendeversioner som är kända att fungera tillsammans. Templates och
exempel på main använder samma versioner som BOM:en för regelramverken (framework-regel 1.4.4,
-maskinell 1.1.8, -manuell 1.4.5, -komplettering 0.1.1, -process 1.6.4)
〔rimfrost-template-regel-manuell/pom.xml〕 〔rimfrost-template-regel-maskinell/pom.xml〕
〔rimfrost-template-regel-komplettering/pom.xml〕 〔rimfrost-template-regel-subprocess/pom.xml〕.

### [1.7] Beroende-/arvsträd mellan ramverk

Direkta `se.fk`-beroenden enligt pom-filerna (compile-scope om inget annat anges):

```
rimfrost-framework-regel 1.4.4
    ├── rimfrost-framework-regel-asyncapi 1.1.4
    ├── rimfrost-framework-handlaggning-adapter 1.2.4
    ├── rimfrost-framework-regel-error-codes 1.1.1
    ├── rimfrost-framework-referensdata-interface 0.0.1   (används inte i main-koden)
    └── rimfrost-framework-sid-adapter 0.0.1              (används inte i main-koden)

rimfrost-framework-regel-maskinell 1.1.8
    ├── rimfrost-framework-regel 1.4.4
    ├── rimfrost-framework-referensdata-interface 0.0.1
    ├── rimfrost-referensdata-erbjudande 0.0.3            (riktig impl., används inte i main-koden)
    └── rimfrost-adapter-referensdata 1.1.2

rimfrost-framework-regel-oul 0.1.2
    ├── rimfrost-framework-regel 1.4.4
    ├── rimfrost-framework-oul 1.1.3  ──► rimfrost-service-oul-asyncapi 1.1.2
    ├── rimfrost-framework-oul-adapter 1.1.5
    ├── rimfrost-framework-regel-oul-asyncapi 0.0.2
    └── rimfrost-framework-referensdata-interface 0.0.1

rimfrost-framework-regel-manuell 1.4.5
    ├── rimfrost-framework-regel 1.4.4
    ├── rimfrost-framework-regel-oul 0.1.2
    ├── rimfrost-framework-regel-oul-openapi-jaxrs-spec 0.0.3
    ├── rimfrost-framework-referensdata-interface 0.0.1
    └── rimfrost-framework-sid-adapter 0.0.1

rimfrost-framework-regel-komplettering 0.1.1
    ├── rimfrost-framework-regel 1.4.4
    ├── rimfrost-framework-regel-oul 0.1.2
    ├── rimfrost-framework-regel-oul-openapi-jaxrs-spec 0.0.3
    └── rimfrost-framework-referensdata-interface 0.0.1   (sid-adapter kommer transitivt via framework-regel)

rimfrost-framework-process 1.6.4
    ├── rimfrost-framework-regel-asyncapi 1.1.4
    ├── rimfrost-process-asyncapi 1.2.3
    └── rimfrost-framework-regel-error-codes 0.0.3        (äldre version än regelramverken)
```

Källor: 〔rimfrost-framework-regel/pom.xml〕 〔rimfrost-framework-regel-maskinell/pom.xml〕
〔rimfrost-framework-regel-oul/pom.xml〕 〔rimfrost-framework-oul/pom.xml〕
〔rimfrost-framework-regel-manuell/pom.xml〕 〔rimfrost-framework-regel-komplettering/pom.xml〕
〔rimfrost-framework-process/pom.xml〕. Arvsträdet på hög nivå finns även i
〔rimfrost/README.md:55-82〕 och 〔rimfrost/FRAMEWORK.md〕.

Viktigt: Quarkus måste indexera beroendena för att hitta CDI-bönor. Därför har varje ramverk
och regel `quarkus.index-dependency.<namn>.group-id/artifact-id` i `application.properties`
〔rimfrost-template-regel-manuell/src/main/resources/application.properties:1-9〕. Saknas
indexering hittas inte bönor från beroendet `[HÄRLETT]`.

---

## [2] Arkitektur och flöden

### [2.1] Grundidé

En **process** (Kogito BPMN) orkestrerar ett flöde och anropar en eller flera **regler**. Varje
regel är en fristående mikrotjänst som kommunicerar asynkront via Kafka och returnerar ett
**utfall** 〔rimfrost/README.md:29-47〕. Mellan process och regel sitter en **regel-subprocess**
(återanvändbar BPMN i ett eget bibliotek) som bygger förfrågan, väntar på svar, hanterar
timeout och omförsök 〔rimfrost-template-regel-subprocess/README.md:5-6〕.

```
Extern anropare
   │ REST POST /yrkande (replyTo)
   ▼
rimfrost-service-workflow ──(lagrar replyTo per handlaggningId i DB)
   │ Kafka: <process>-handlaggning-requests  (CloudEvent, bara handlaggningId)
   ▼
Process (Kogito, t.ex. rimfrost-process-vah)
   │ callActivity → subprocess (calledElement = subprocessens process-id)
   ▼
Regel-subprocess (BPMN i bibliotek, t.ex. rimfrost-regel-rtf-manuell-subprocess)
   │ Kafka: <regel>-requests  (RegelRequestMessage med replyTo=${PROCESS_ID}-<regel>-responses)
   ▼
Regel (Quarkus, rimfrost-regel-<regel>)
   │ läser/skriver handläggning (REST), ev. OUL-uppgift (REST), SID (REST)
   │ Kafka: replyTo-topic (RegelResponseMessage med utfall)
   ▼
Subprocess → Process → Kafka: handlaggning-responses → workflow → extern anropares replyTo
```

Källor: 〔rimfrost/ASYNC_ROUTING.md:150-168〕 〔rimfrost/sequences/start_end_process.mmd〕
〔rimfrost/sequences/ratt_till_forsakring_flode.mmd〕 〔rimfrost-process-vah/src/main/resources/application.properties〕
〔rimfrost-template-regel-subprocess/src/main/resources/application.properties〕.

### [2.2] Regeltyper i korthet

| Typ | Beteende | Svarar processen | OUL-uppgift | Databas | REST mot portal |
|---|---|---|---|---|---|
| Maskinell | Läser handläggning → kör logik → skriver handläggning → svarar | Direkt | Nej | Nej | Nej |
| Manuell | Läser handläggning → skapar OUL-uppgift → väntar → handläggaren arbetar via micro-fe → `POST /done` → svarar | När handläggaren är klar | Ja | Ja (PostgreSQL, korrelation) | Ja |
| Komplettering | Läser handläggning → `isKompletteringRequired()`; om false svarar `JA` direkt, annars som manuell; `POST /done` kontrollerar igen (422 om fortfarande ofullständig) | Direkt eller när klar | Villkorad | Ja | Ja |

Källor: 〔rimfrost/README.md:86-92〕 〔rimfrost-framework-regel-komplettering/docs/krav.md:24-52〕
〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/RegelMaskinellRequestHandler.java:40-154〕.

### [2.3] Integrationer (externa tjänster)

| Tjänst | Används av | Klient | Property | Källa |
|---|---|---|---|---|
| Handläggning (`rimfrost-service-handlaggning`) | Alla regler | `HandlaggningAdapter` (`readHandlaggning`, `updateHandlaggning`) | `handlaggning.api.base-url` (default `http://rimfrost-k8s-handlaggning:8080`) | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/adapter/HandlaggningAdapter.java:28〕 〔rimfrost-framework-regel/src/main/resources/application.properties:22〕 |
| OUL (Operativt uppgiftslager) | Manuell, komplettering | `OulAdapter` via `OulUppgiftService` | `oul.api.base-url` (default `http://rimfrost-k8s-uppgiftslager:8080`) | 〔rimfrost-framework-oul-adapter/src/main/java/se/fk/rimfrost/framework/oul/adapter/OulAdapter.java:28〕 〔rimfrost-framework-regel-oul/src/main/resources/application.properties:22〕 |
| SID (skyddad identitet) | Manuell, komplettering (vid GET) | `SidAdapter.containsSid` | `sid.api.base-url` (default bara i komplettering-ramverket) | 〔rimfrost-framework-sid-adapter/src/main/java/se/fk/rimfrost/framework/sid/adapter/SidAdapter.java:36〕 〔rimfrost-framework-regel-komplettering/src/main/resources/application.properties:17〕 |
| Referensdata (erbjudandenamn) | Manuell, komplettering | `ErbjudandeReferensdataInterface` | `referensdata.api.base-url` (vid riktig impl.) | `[12]` |
| Erbjudande-topic | Workflow | `rimfrost-service-erbjudande-topic-openapi` | — | 〔rimfrost/README.md:103-109〕 |
| Team | Regler (vid behov) | `rimfrost-service-team-openapi` / `rimfrost-adapter-team` | — | 〔rimfrost/README.md:111-117〕 |
| Folkbokföring, arbetsgivare | rtf-maskinell, rtf-manuell | `rimfrost-adapter-folkbokford`, `rimfrost-adapter-arbetsgivare` | `folkbokford.api.base-url`, `arbetsgivare.api.base-url` | 〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:41-42〕 |

### [2.4] Sekvensdiagram (Mermaid) i `rimfrost/sequences/`

| Fil | Visar |
|---|---|
| `start_end_process.mmd` | Workflow skapar yrkande/handläggning, triggar process, processen svarar |
| `ratt_till_forsakring_flode.mmd` | Helt VAH-flöde: workflow → VAH → rtf-maskinell → rtf-manuell |
| `ratt_till_forsakring_flode_regel_maskinell_error.mmd` | Maskinell regel svarar med fel; workflow initierar ny begäran |
| `ratt_till_forsakring_maskinell.mmd` | Detaljsekvens maskinell regel (individ, folkbokford, arbetsgivare, handläggning) |
| `ratt_till_forsakring_manuell.mmd` | Detaljsekvens manuell regel inkl. OUL, SID, portal |
| `ratt_till_forsakring_komplettering.mmd` | Detaljsekvens kompletteringsregel |
| `ratt_till_forsakring_manuell_storage_read_failure.mmd` | Fel vid läsning av lagring i manuell regel |
| `restart_process_while_waiting_for_rtf_manuell_regel.mmd` | Omstart av processinstans medan manuell regel väntar |
| `portal_micro_fe.mmd` | Portal, portal-bff, OUL, micro-fe, micro-fe-bff, regel |
| `kommunicering/kommunicering_handlaggare_hanterar_kundkontakt.mmd` | **Planerad** kommuniceringsregel |
| `kommunicering/kommunicering_timeout.mmd` | **Planerad** timeout via `RegelOulCancelledMessage` |

Källa: 〔rimfrost/sequences/〕 (filerna). Diagrammen beskriver avsikt; verifiera mot kod.

### [2.5] CloudEvents och Kogito-korrelation

All asynkron kommunikation med Kogito-processer måste bära CloudEvent-data
〔rimfrost-framework-regel/README.md〕. Regelförfrågan och -svar är CloudEvents
(`kogito-cloudevent.yaml`) med payload `data` 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:126-155〕.
Ramverket kopierar Kogito-attributen (`kogitoprocinstanceid`, `kogitorootprociid`,
`kogitoparentprociid`, `kogitoprocid`, `kogitorootprocid`, `kogitoprocist`, `kogitoprocversion`)
och `id` från förfrågan till svaret, sätter `type` = värdet av
`mp.messaging.outgoing.regel-responses.topic` och `source` = `kafka.source`
〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:63-77〕.
Det är så Kogito korrelerar svaret med rätt väntande processinstans `[HÄRLETT]`.

Att `replyTo` ligger i payload och inte i Kafka-header beror på att förfrågningar kommer från
Kogito som bygger CloudEvents och inte sätter Kafka-headers 〔rimfrost/ASYNC_ROUTING.md:43〕.


---

## [3] Domänmodell och versionshantering

Domänmodellen ligger i `rimfrost-framework-handlaggning-adapter`, paket
`se.fk.rimfrost.framework.handlaggning.model`. Alla typer är Immutables-gränssnitt
(`@Value.Immutable`); bygg instanser med `Immutable<Typ>.builder()` och kopiera med `.from(x)`
〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Handlaggning.java:9-28〕.

### [3.1] Typer och fält

`?` = `@Nullable`.

| Typ | Fält | Källa |
|---|---|---|
| `Handlaggning` | `UUID id`, `int version`, `Yrkande yrkande`, `UUID processInstansId?`, `OffsetDateTime skapadTS`, `OffsetDateTime avslutadTS?`, `UUID handlaggningspecifikationId` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Handlaggning.java:10-28〕 |
| `HandlaggningUpdate` | samma som `Handlaggning` + `List<Underlag> underlag`, `Uppgift uppgift?` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/HandlaggningUpdate.java:10-33〕 |
| `Yrkande` | `UUID id`, `int version`, `String erbjudandeId`, `OffsetDateTime yrkandeDatum`, `String yrkandeStatus`, `OffsetDateTime yrkandeFrom`, `OffsetDateTime yrkandeTom`, `String avsikt`, `List<IndividYrkandeRoll> individYrkandeRoller`, `List<ProduceratResultat> produceradeResultat`, `Beslut beslut?` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Yrkande.java:10-35〕 |
| `IndividYrkandeRoll` | `Idtyp individ`, `String yrkandeRollId` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/IndividYrkandeRoll.java:6-11〕 |
| `Idtyp` | `String typId` (t.ex. `"personnummer"`), `String varde` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Idtyp.java:6-11〕 |
| `ProduceratResultat` | `UUID id`, `int version`, `OffsetDateTime resultatFrom`, `OffsetDateTime resultatTom`, `String yrkandeStatus`, `String avslagsanledning?`, `String typ` (t.ex. `"ERSATTNING"`), `String data` (JSON-sträng) | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/ProduceratResultat.java:9-27〕 |
| `Underlag` | `String typ`, `Integer version`, `String data` (JSON-sträng) | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Underlag.java:6-13〕 |
| `Uppgift` | `UUID id`, `int version`, `OffsetDateTime skapadTs`, `OffsetDateTime utfordTs?`, `OffsetDateTime planeradTs?`, `Idtyp utforarId?`, `String uppgiftStatus?`, `UUID aktivitetId`, `String fSSAinformation`, `UppgiftSpecifikation uppgiftSpecifikation` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Uppgift.java:9-36〕 |
| `UppgiftSpecifikation` | `UUID id`, `Integer version` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/UppgiftSpecifikation.java:7-12〕 |
| `Beslut` | `UUID id`, `int version`, `OffsetDateTime datum`, `Idtyp beslutsfattare`, `List<Beslutsrad> beslutsrader` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Beslut.java:10-21〕 |
| `Beslutsrad` | `UUID id`, `int version`, `String beslutsTyp`, `String beslutsUtfall`, `String avslutsTyp`, `List<ProduceratResultatRef> produceradeResultatRef` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Beslutsrad.java:8-22〕 |
| `ProduceratResultatRef` | `UUID id`, `int version` | 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/ProduceratResultatRef.java:7-13〕 |

Observera skillnader i namngivning: `Handlaggning.skapadTS` (versalt TS) men `Uppgift.skapadTs`
(gemen s); `Uppgift.fSSAinformation`. Kopiera exakt.

### [3.2] `HandlaggningAdapter` och `HandlaggningException`

```java
// KÄLLA: rimfrost-framework-handlaggning-adapter/.../adapter/HandlaggningAdapter.java:61,96
public Handlaggning readHandlaggning(UUID handlaggningId) throws HandlaggningException
public HandlaggningUpdate updateHandlaggning(HandlaggningUpdate handlaggningUpdate) throws HandlaggningException
```

- Bas-URL: `handlaggning.api.base-url` 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/adapter/HandlaggningAdapter.java:28〕.
- Läsning: `GET` (genererad klient `getHandlaggning`); skrivning: `PUT /handlaggning/{id}`
  (`putHandlaggning`) 〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/adapter/HandlaggningAdapter.java:65〕
  〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/adapter/HandlaggningAdapter.java:101〕.
- `ErrorType`: `NOT_FOUND`, `BAD_REQUEST`, `CONFLICT`, `SERVICE_UNAVAILABLE`, `UNEXPECTED_ERROR`
  〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/exception/HandlaggningException.java:6-9〕.
- Mappning: 404→`NOT_FOUND`, 400→`BAD_REQUEST`, nätverks-/anslutningsfel (`ProcessingException`)→
  `SERVICE_UNAVAILABLE`, versionskonflikt vid PUT→`CONFLICT`, övrigt→`UNEXPECTED_ERROR`
  〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/adapter/HandlaggningAdapter.java:73-93〕
  〔rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/adapter/HandlaggningAdapter.java:109-133〕.
- Adaptern injiceras som `protected HandlaggningAdapter handlaggningAdapter` i
  `RegelRequestHandlerBase` och kan `@Inject`:as direkt i regelns egna bönor
  〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:39-40〕.

### [3.3] Versionshantering — regler

**Grundregel:** Ramverket stegar aldrig `version` automatiskt i det regeln returnerar. Regeln
ansvarar för att stega version på objekt den modifierar 〔rimfrost/regler/VERSIONING.md:5-17〕.

| Fält | Syfte | Vem stegar | Källa |
|---|---|---|---|
| `HandlaggningUpdate.version` | Optimistisk låsning i handläggningstjänsten. Backend lagrar mottagen version och avvisar konflikt (`CONFLICT`). | Regeln, när den vill signalera meningsfull tillståndsändring: `handlaggning.version() + 1`. Annars oförändrad. | 〔rimfrost/regler/VERSIONING.md:19-53〕 |
| `ProduceratResultat.version` | Ändringsräknare per resultat | Regeln, när resultatets data ändras: `produceratResultat.version() + 1` | 〔rimfrost/regler/VERSIONING.md:55-89〕 |
| `Yrkande.version` | — | Regeln (se exempel i bekräfta beslut) | 〔rimfrost-regel-bekraftabeslut/src/main/java/se/fk/github/bekraftabeslut/logic/BekraftaBeslutService.java:120〕 |
| `Uppgift.version` | Version av uppgiftsobjektet | Ramverket i manuell/komplettering (statuscallback och done: `uppgift.version() + 1`) | 〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java:319〕 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:194〕 |

Faktisk praxis i kod `[KÄLLA]`:

- Maskinell template och rtf-maskinell: `version(regelRequest.handlaggning().version() + 1)`
  〔rimfrost-template-regel-maskinell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java:54〕
  〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RtfService.java:163〕.
- rtf-manuell `updateData`: `handlaggning.version() + 1` och `ProduceratResultat.version() + 1`
  〔rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/logic/RtfService.java:119〕
  〔rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/logic/RtfService.java:178〕.
- rtf-manuell-komplettering `registerSvar`: `handlaggning.version()` oförändrad
  〔rimfrost-regel-rtf-manuell-komplettering/src/main/java/se/fk/github/rtf/manuell/komplettering/logic/RtfKompletteringService.java:82〕.
- Ramverket självt: vid skapande av OUL-uppgift `version + 1`; vid OUL-statuscallback och vid
  done oförändrad 〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java:138〕
  〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java:329〕
  〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:200〕.

Konsekvens `[HÄRLETT]`: Ett `CONFLICT`-fel vid skrivning tolkas av ramverket som
programmeringsfel i regeln ("Regel service programming fault?") och leder inte till omförsök
〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/RegelMaskinellRequestHandler.java:111-123〕.
Läs alltid färsk handläggning och bygg uppdateringen från den.

### [3.4] `produceradeResultat` — senaste version, merge per id

- Listan innehåller en post per resultat-id (senaste versionen), ingen historik
  〔rimfrost/regler/VERSIONING.md:91-99〕.
- Merge: samma id → det nya vinner; övriga förs över oförändrade 〔rimfrost/regler/VERSIONING.md:100-106〕.

### [3.5] `RegelUtils` (statiska hjälpmetoder)

| Metod | Beteende | Källa |
|---|---|---|
| `Yrkande createYrkandeWithUpdatedProduceradeResultat(Yrkande yrkande, List<ProduceratResultat> uppdateradeResultat)` | Kopierar yrkandet och ersätter `produceradeResultat` med merge enligt `[3.4]` | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelUtils.java:27-35〕 |
| `List<ProduceratResultat> mergeProduceradeResultat(List uppdaterade, List tidigare)` | Alla uppdaterade + tidigare vars id inte finns bland uppdaterade (uppdaterade först) | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelUtils.java:47-56〕 |
| `Underlag createUnderlag(String typ, int version, Object object, ObjectMapper om)` | Serialiserar `object` till JSON i `Underlag.data`; kastar `InternalError` vid JSON-fel | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelUtils.java:65-80〕 |
| `String createProduceratResultatData(Object object, ObjectMapper om)` | JSON-serialisering för `ProduceratResultat.data`; `InternalError` vid fel | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelUtils.java:88-98〕 |

Template-kommentar: allt underlag regeln använt för sitt beslut ska skickas som
`HandlaggningUpdate.underlag`; nya/ändrade resultat ska ligga i
`HandlaggningUpdate.yrkande.produceradeResultat`
〔rimfrost-template-regel-maskinell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java:29-31〕.

---

## [4] Gemensamt regelramverk (`rimfrost-framework-regel`)

### [4.1] Ansvar

Tar emot regelförfrågningar via Kafka, laddar och validerar `config.yaml`, ger
handläggningsadapter och publicerar regelsvar. Konkreta regler implementerar bara affärslogik
〔rimfrost-framework-regel/README.md〕. Paketstruktur: `presentation/kafka` (konsument,
deserializer, handler-kontrakt), `logic` (bas, config-objekt, DTO), `integration` (YAML-laddning,
Kafka-producent) 〔rimfrost-framework-regel/README.md〕.

### [4.2] Kafka-kontrakt (regelförfrågan och -svar)

Definieras i `rimfrost-framework-regel-asyncapi/asyncapi.yaml` (AsyncAPI 3.0.0).

**Förfrågan** `RegelRequestMessage` = KogitoCloudEvent + `data`:

| Fält i `data` | Typ | Obligatoriskt | Betydelse | Källa |
|---|---|---|---|---|
| `handlaggningId` | uuid | ja | Handläggningsärendet | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:40-50〕 |
| `aktivitetId` | uuid | ja | Aktiviteten i processen (sätts per callActivity i process-BPMN, se `[10.3]`) | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:51-54〕 |
| `replyTo` | string | nej (men krävs i praktiken) | Topic dit svaret ska skickas | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:55-57〕 |
| `processvyUrl` | string | nej | Relativ sökväg till processens processvy (t.ex. `/process/vah`) | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:58-61〕 |

Header `correlationId` (uuid) finns i kontraktet 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:32-38〕.

**Svar** `RegelResponseMessage` = KogitoCloudEvent + `data`:

| Fält | Typ | Källa |
|---|---|---|
| `handlaggningId` | uuid (obl.) | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:101-111〕 |
| `utfall` | enum `Ja`, `Nej`, `Utredning`, `Error` (obl.) | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:112-120〕 |
| `error` | `RegelErrorInformation` (`felkod`, `felmeddelande`), endast när utfall är `Error` | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:87-99〕 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:121-124〕 |

Java-enumen: `Utfall { JA("Ja"), NEJ("Nej"), UTREDNING("Utredning"), ERROR("Error") }`
〔rimfrost-framework-regel-asyncapi/src/main/java/se/fk/rimfrost/framework/regel/Utfall.java:6-8〕.

**Intern DTO** `RegelDataRequest` (det regelhanteraren får): `id`, `handlaggningId`,
`aktivitetId`, `kogitorootprocid`, `kogitorootprociid`, `kogitoparentprociid`, `kogitoprocid`,
`kogitoprocinstanceid`, `kogitoprocist`, `kogitoprocversion`, `type`, `replyTo`
〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/dto/RegelDataRequest.java:6-32〕.
`processvyUrl` mappas inte till `RegelDataRequest` `[HÄRLETT]` (fältet saknas i DTO:n).

### [4.3] Felkoder (`RegelFelkod`)

Prefixet `RIMFROST_` är reserverat för ramverket; regler får definiera egna koder. Okänd kod ska
behandlas som `RIMFROST_OTHER`. Mönster: `^[A-Z0-9]([_A-Z0-9]*[A-Z0-9]){0,1}$`
〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:71-85〕.

Konstanter 〔rimfrost-framework-regel-error-codes/src/main/java/se/fk/rimfrost/framework/regel/error/RegelFelkod.java:5-16〕:

Kolumnen "Används i ramverkskoden" bygger på sökning efter `RegelFelkod.<KONSTANT>` i alla
`rimfrost-framework-*/src/main` `[KÄLLA]`.

| Konstant | Används i ramverkskoden när |
|---|---|
| `RIMFROST_HANDLAGGNING_READ_FAILURE` | Läsning av handläggning misslyckades (maskinell: efter omförsök; manuell/komplettering/regel-oul: direkt) |
| `RIMFROST_HANDLAGGNING_WRITE_FAILURE` | Skrivning misslyckades (maskinell: efter omförsök eller vid versionskonflikt; regel-oul: vid skapande av OUL-uppgift) |
| `RIMFROST_CLOUD_EVENT_DATA_WRITE_FAILURE` | Lagring av cloud event data i regel-oul |
| `RIMFROST_MANUELL_REGEL_COMMON_DATA_READ_FAILURE` / `_WRITE_FAILURE` | Läsning/lagring av common data i regel-oul |
| `RIMFROST_PROCESS_TOPIC_INFO_WRITE_FAILURE` | Lagring av process topic info i regel-oul |
| `RIMFROST_OTHER` | Oväntat fel / fångat undantag (även när OUL-uppgiften inte kan skapas) |
| `RIMFROST_CLOUD_EVENT_DATA_READ_FAILURE`, `RIMFROST_PROCESS_TOPIC_INFO_READ_FAILURE`, `RIMFROST_OUL_READ_FAILURE`, `RIMFROST_OUL_WRITE_FAILURE`, `RIMFROST_OUL_NOT_FOUND` | **Definierade men används inte** i ramverkskoden |

AsyncAPI-beskrivningen listar bara sju av de tolv konstanterna (`[AVVIKELSE-26]`).

Exempel på regelspecifika koder: `RTF_MASKINELL_FOLKBOKFORING_READ_ERROR` m.fl. i en egen klass
〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RegelFelkod.java:3-9〕.
Subprocessen sätter felkoden `TIMEOUT` (utan prefix) vid timeout
〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelService.java:52-59〕.

### [4.4] Mottagningskedja (Kafka → regel)

1. `RegelConsumer.onMessage` lyssnar på logiska kanalen `regel-requests` (`@Incoming`,
   `@Blocking`) 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/presentation/kafka/RegelConsumer.java:21-26〕.
2. `RegelMessageHandler.consumeRegelRequest` lägger `handlaggningId` i MDC, mappar payload till
   `RegelDataRequest` och anropar den enda `RegelRequestHandlerInterface`-bönan
   〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/presentation/kafka/RegelMessageHandler.java:24-45〕.
3. `RegelRequestHandlerInterface.handleRegelRequest(RegelDataRequest)` implementeras av
   ramverket för respektive typ (`RegelMaskinellRequestHandler`, `RegelManuellRequestHandler`,
   `RegelKompletteringRequestHandler`) — **inte** av regeln själv
   〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/presentation/kafka/RegelRequestHandlerInterface.java:5-8〕.

Kanalens fysiska topic sätts i regelns `application.properties`:
`mp.messaging.incoming.regel-requests.topic=<regel>-requests`. Ramverket sätter connector,
`auto.offset.reset=earliest` och deserializer 〔rimfrost-framework-regel/src/main/resources/application.properties:12-15〕.

### [4.5] Sändning (regel → Kafka)

- `RegelKafkaProducer.sendRegelResponse(RegelResponse, String topic)` skickar via emitter på
  kanalen `regel-responses` men **överstyr topic** med `OutgoingKafkaRecordMetadata.withTopic(topic)`
  — svaret hamnar alltså på `replyTo`, inte på kanalens konfigurerade topic. Buffer 1024
  〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/kafka/RegelKafkaProducer.java:26-46〕.
- `mp.messaging.outgoing.regel-responses.topic` måste ändå sättas: värdet används som
  CloudEvent-`type` i svaret och injiceras som `@ConfigProperty` (krävs vid start)
  〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:33-34〕
  〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:74〕.
- Sändfel (`IllegalStateException`) loggas och sväljs (fire-and-forget)
  〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:79-106〕
  〔rimfrost-framework-regel/docs/krav.md〕.
- `sendErrorResponse` skickar inget om `handlaggningId`, `cloudEventData` eller
  `regelErrorInformation` är null (loggar varning)
  〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:153-165〕.
- `replyTo` valideras aldrig och får inget default; saknas det går svaret förlorat
  〔rimfrost/ASYNC_ROUTING.md:61-62〕. Koden kastar `NullPointerException` via
  `Objects.requireNonNull(replyTo)` `[HÄRLETT]` 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:84〕.

### [4.6] `config.yaml` — regelmetadata

**Laddning** (`RegelConfigProviderYaml`, `@Startup`):
1. Miljövariabeln `REGEL_CONFIG_PATH` om satt, annars property `application.config.path`
   (default `src/main/resources/config.yaml`).
2. Finns filen inte på den sökvägen: varning i logg och fallback till classpath-resursen
   `config.yaml`. (I en container finns inte `src/main/resources/…`, så classpath används där
   `[HÄRLETT]`.)

〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/config/RegelConfigProviderYaml.java:20-58〕
〔rimfrost-framework-regel/src/main/resources/application.properties:4〕.

**Validering:** innehållet valideras mot `schema/regel_schema.yaml` (JSON Schema 2020-12) innan
parsning; brott ger `IllegalStateException("Config violates JSON Schema: …")` → startfel
〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/config/YamlConfigLoader.java:102-133〕.
Validering finns också som återanvändbart GitHub Actions-workflow och lokalt med
`check-jsonschema` 〔rimfrost-framework-regel/README.md〕.

**Schema** 〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml〕:

| Sektion | Obligatorisk | Obligatoriska fält | Valfria fält | Källa |
|---|---|---|---|---|
| `uppgift` | ja | `version` (int ≥1), `aktivitet` | `id` (uuid), `path` (krävs i praktiken för manuell/komplettering), `metadata` | 〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml:15-37〕 |
| `specifikation` | ja | `id` (uuid), `version`, `namn`, `uppgiftbeskrivning`, `verksamhetslogik`, `roll`, `applikationsId`, `applikationsversion` | `metadata` | 〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml:38-72〕 |
| `regel` | ja | `id`, `version`, `namn`, `beskrivning` | `metadata` | 〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml:74-96〕 |
| `lagrum` | ja | `id`, `version`, `giltigFom` (date), `forfattning`, `kapitel`, `paragraf`, `stycke`, `punkt` (alla int ≥1) | `metadata` | 〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml:98-136〕 |
| `utokadUppgiftsbeskrivning` | nej | — | `beskrivning` (inget `metadata`) | 〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml:138-143〕 |

`additionalProperties: false` på rot och i varje sektion — okända nycklar ger startfel
〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml:11〕.

**Vad fälten används till i ramverket** `[KÄLLA]`:

| Fält | Används till | Källa |
|---|---|---|
| `specifikation.id`, `specifikation.version` | `Uppgift.uppgiftSpecifikation` i handläggningen och OUL-spec | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:145-151〕 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:118-119〕 |
| `specifikation.namn` | OUL-uppgiftens `regel` | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:111〕 |
| `specifikation.uppgiftbeskrivning` | OUL-uppgiftens `beskrivning` | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:112〕 |
| `specifikation.verksamhetslogik`, `specifikation.roll` | OUL-uppgiftens `verksamhetslogik`, `roll` | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:113-114〕 |
| `uppgift.path` | OUL-uppgiftens `url` — portalen använder den för att välja micro-frontend | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:115〕 〔rimfrost/frontend/micro-fe/README.md〕 |
| `utokadUppgiftsbeskrivning.beskrivning` | `GET /utokadUppgiftsbeskrivning` | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/RegelManuellController.java:28-37〕 |

Saknas `utokadUppgiftsbeskrivning` anropar controllern ändå `.getBeskrivning()` på null →
troligen HTTP 500 vid `GET /utokadUppgiftsbeskrivning` `[EJ VERIFIERAT]`
〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/RegelManuellController.java:33-34〕.

**Regler för innehåll** 〔rimfrost/regler/CONFIG_YAML.md:72-80〕:
- `id` i varje sektion: nytt globalt unikt UUID v4 per sektion och regel; återanvänd aldrig
  (se `[AVVIKELSE-07]` — exemplen bryter mot detta).
- `version`: heltal, börja på 1, öka när innehållet ändras på ett sätt som påverkar beteendet.
- `metadata`: enligt CONFIG_YAML.md och schemat ett valfritt objekt med godtyckliga nycklar i
  `uppgift`, `specifikation`, `regel`, `lagrum` — **men** Java-klasserna saknar fältet, så det ger
  troligen startfel vid parsning. Använd inte `metadata` (`[AVVIKELSE-23]`).
- Ingen miljöspecifik konfiguration i `config.yaml` — den hör hemma i `application.properties`
  〔rimfrost/regler/CONFIG_YAML.md:5〕.

Fullständigt exempel:

```yaml
# KÄLLA: rimfrost/regler/CONFIG_YAML.md:20-70 (förkortat)
uppgift:
  id: d435beb0-c1c1-471a-916f-a111db0ef08d   # generera nytt
  version: 1
  path: /regel/manuell-exempel               # obligatoriskt för manuell/komplettering
  aktivitet: "Bedömma rätten till"
specifikation:
  id: 494726b5-5122-4efc-8a5d-578dcd44df0f
  version: 1
  namn: "Har kunden rätt till VAH?"
  uppgiftbeskrivning: "Kontrollerar om personen är folkbokförd i Sverige samt om personen har ett jobb"
  verksamhetslogik: C
  roll: ANSVARIG_HANDLAGGARE
  applikationsId: 3.5.2
  applikationsversion: kontroll_folkbokford_arbetsgivare_1.0
regel:
  id: caf86a5f-9776-4fce-a854-8d365fa75067
  version: 1
  namn: "Har kunden arbetsgivare och är folkbokförd"
  beskrivning: "Maskinell kontroll som kollar …"
lagrum:
  id: 6955bd9b-2019-4589-a460-7bcfd8a43eb5
  version: 1
  giltigFom: 2010-02-11
  forfattning: Husdjursbalken
  kapitel: 4
  paragraf: 7
  stycke: 2
  punkt: 3
utokadUppgiftsbeskrivning:                   # endast manuell/komplettering
  beskrivning: "…"
```

### [4.7] `RegelRequestHandlerBase` — skyddade hjälpmetoder

Basklass för ramverkens hanterare (maskinell och komplettering ärver den; manuell har egen
motsvarande kod). Regelutvecklare behöver normalt inte ärva den, men metoderna förklarar
beteendet 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:26-48〕.

| Metod | Beteende | Rad |
|---|---|---|
| `createCloudEvent(RegelDataRequest)` | Kopierar Kogito-attribut, `type=responseTopic`, `source=kafkaSource` | 63-77 |
| `sendResponse(id, ce, Utfall, replyTo)` / `sendResponse(id, ce, RegelErrorInformation, replyTo)` | Skickar svar till `replyTo`; sväljer `IllegalStateException` | 79-106 |
| `createHandlaggningUpdate(handlaggning, uppgift, kogitoprocInstanceId, version)` | Kopierar handläggning, sätter uppgift och version | 117-130 |
| `createUppgift(aktivitetId, status)` | Ny `Uppgift` (nytt UUID, version 1, `fSSAinformation="FSSAinformation.HANDLAGGNING_PAGAR"`, specifikation från config) | 132-143 |
| `getHandlaggning(id, ce)` | Läser; vid fel kastar `RegelCancelledException` med `RIMFROST_HANDLAGGNING_READ_FAILURE` | 176-190 |
| `createRegelErrorInformation(felkod, meddelande)` | Bygger felobjekt | 167-174 |

Källa för radnummer: 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java〕.

### [4.8] Konfiguration (framework-regel)

| Property | Obligatorisk | Default | Beskrivning | Källa |
|---|---|---|---|---|
| `kafka.source` | **ja** (ingen default) | — | CloudEvent `source` i svar; unik per regel | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:30-31〕 |
| `mp.messaging.incoming.regel-requests.topic` | ja | (kanalnamn) | Regelns request-topic | 〔rimfrost-template-regel-maskinell/src/main/resources/application.properties:13〕 |
| `mp.messaging.outgoing.regel-responses.topic` | **ja** (injiceras) | — | Används som CloudEvent `type` | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:33-34〕 |
| `application.config.path` | nej | `src/main/resources/config.yaml` | Sökväg till config.yaml | 〔rimfrost-framework-regel/src/main/resources/application.properties:4〕 |
| env `REGEL_CONFIG_PATH` | nej | — | Överstyr ovan | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/config/RegelConfigProviderYaml.java:20〕 |
| `handlaggning.api.base-url` | nej | `http://rimfrost-k8s-handlaggning:8080` | Handläggningstjänsten | 〔rimfrost-framework-regel/src/main/resources/application.properties:22〕 |
| `quarkus.container-image.*` | nej | `build=true`, `group=rimfrost`, `tag=latest`, `push=false` | Containerbygge | 〔rimfrost-framework-regel/src/main/resources/application.properties:6-10〕 |

### [4.9] Test-JAR (`rimfrost-framework-regel`, classifier `tests`)

Innehåller allt under `src/test/java` utom `*Test.class`
〔rimfrost-framework-regel/pom.xml:144-150〕.

| Klass | Användning | Källa |
|---|---|---|
| `RegelTestBase` | Abstrakt bas med in-memory Kafka och grundkonfiguration | 〔rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/RegelTestBase.java:51〕 |
| `KafkaConnector` / `RegelKafkaConnector` | Hjälp för in-memory request/response-kanaler | 〔rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/RegelKafkaConnector.java:12〕 |
| `RegelTestData` | Testdata för förfrågningar | 〔rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/RegelTestData.java:5〕 |
| `WireMockHandlaggning` | Abstrakt `QuarkusTestResourceLifecycleManager` med mappning mot `/handlaggning/`; ärvs för att mocka fler endpoints | 〔rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/WireMockHandlaggning.java:34-80〕 |
| `ErbjudandeReferensdataTestService` | `@ApplicationScoped @DefaultBean`-implementation av `ErbjudandeReferensdataInterface` som alltid returnerar `DEFAULT_ERBJUDANDE_NAMN = "Test erbjudande"`; ersätts automatiskt om regeln har en egen böna | 〔rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/ErbjudandeReferensdataTestService.java〕 |

I tester ersätts Kafka-connectorer med `smallrye-in-memory`:

```properties
# KÄLLA: rimfrost-template-regel-maskinell/src/test/resources/application.properties
mp.messaging.incoming.regel-requests.connector=smallrye-in-memory
mp.messaging.outgoing.regel-responses.connector=smallrye-in-memory
```

### [4.10] Läsa `config.yaml` från regelkod (`RegelConfigProvider`)

Injicera `RegelConfigProvider` och anropa `getConfig()` (används så i manuell-templaten)
〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/config/RegelConfigProvider.java〕
〔rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java〕.
`RegelConfig` är en vanlig JavaBean med getters:

| Klass | Fält (Java-typ) | Källa |
|---|---|---|
| `RegelConfig` | `uppgift`, `specifikation`, `regel`, `lagrum`, `utokadUppgiftsbeskrivning` | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/RegelConfig.java〕 |
| `Uppgift` (config) | `UUID id`, `int version`, `String path`, `String aktivitet` | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Uppgift.java〕 |
| `Specifikation` | `UUID id`, `int version`, `String namn`, `uppgiftbeskrivning`, `verksamhetslogik`, `roll`, `applikationsId`, `applikationsversion` | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Specifikation.java〕 |
| `Regel` | `UUID id`, `int version`, `String namn`, `String beskrivning` | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Regel.java〕 |
| `Lagrum` | `UUID id`, `int version`, `Date giltigFom`, `String forfattning`, `String kapitel`, `String paragraf`, `String stycke`, `String punkt` (strängar i Java trots heltal i schemat) | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Lagrum.java〕 |
| `UtokadUppgiftsbeskrivning` | `String beskrivning` | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/UtokadUppgiftsbeskrivning.java〕 |

Ingen klass har ett `metadata`-fält, trots att schemat tillåter `metadata`. SnakeYAML kastar som
standard ett fel för okända egenskaper, så `metadata` i `config.yaml` ger troligen startfel
(`[AVVIKELSE-23]`).

### [4.11] Interna klasser (behöver normalt inte användas av regler)

För att en AI ska kunna placera klassnamn som dyker upp i stackspår eller kod:

| Klass | Repo | Roll |
|---|---|---|
| `RegelKafkaMapper` (två varianter), `RegelMapper`, `RegelRequestDeserializer` | framework-regel | Mappning mellan Kafka-payload, `RegelDataRequest` och `RegelResponse` |
| `RegelCancelledException` | framework-regel | Bär `RegelErrorInformation` genom hanterarna; fångas och blir felsvar |
| `RegelMaskinellMapper`, `HandlaggningUpdateConflictException` | framework-regel-maskinell | Bygger `RegelMaskinellRequest`; signalerar `CONFLICT` vid skrivning |
| `RegelManuellUppgiftDoneHandler`, `RegelManuellMiddlewareServiceInterface` | framework-regel-manuell | Kontrakt mellan service-bas, middleware och request-hanterare |
| `DelayedException` | framework-regel-manuell | Definierad men oanvänd |
| `RegelKompletteringDoneHandler` | framework-regel-komplettering | Kontrakt för `POST /done` |
| `CloudEventAttributesMapper` | framework-regel-oul | CloudEventData ↔ attribut-map som skickas till OUL och ekas tillbaka |
| `CloudEventDataStorage`, `ProcessTopicInfoStorage`, `RegelCommonDataStorage` (+ `Panache*`-implementationer) | framework-regel-oul | Lagringsgränssnitt för korrelationstabellerna |
| `OulMessageHandler`, `OulKafkaMapper`, `OulStatusNotificationDeserializer` | framework-oul | Konsumtion av OUL-statusnotifieringar |
| `OulAdapter`, `OulException`, `CreateOperativUppgiftRequest`, `OperativUppgift` | framework-oul-adapter | REST-klient mot OUL (`createOperativUppgift`, `unassignOperativUppgift`, `endOperativUppgift`) |
| `HandlaggningMapper`, `SidMapper` | adaptrarna | Mappning mot genererade OpenAPI-modeller |

Källor: filerna under respektive repos `src/main/java`, t.ex.
〔rimfrost-framework-oul-adapter/src/main/java/se/fk/rimfrost/framework/oul/adapter/OulAdapter.java:67-145〕
〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/CloudEventAttributesMapper.java〕
〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/DelayedException.java〕.


---

## [5] Maskinell regel

### [5.1] Syfte

Helt automatiserad regel utan mänskliga beslut. Ramverket tar emot förfrågan, läser
handläggningen, anropar regellogiken, skriver tillbaka och svarar 〔rimfrost/README.md:86-92〕
〔rimfrost-framework-regel-maskinell/README.md〕.

### [5.2] Beroenden (Maven)

```xml
<!-- KÄLLA: rimfrost-template-regel-maskinell/pom.xml (förenklat) -->
<parent>fk-maven-quarkus-parent 1.12.0</parent>
se.fk.rimfrost.framework.regel.maskinell:rimfrost-framework-regel-maskinell:1.1.8
se.fk.rimfrost.framework.regel.maskinell:rimfrost-framework-regel-maskinell:1.1.8  [tests]  (test)
se.fk.rimfrost.framework.regel:rimfrost-framework-regel:1.4.4                      [tests]  (test)
org.kie:kie-dmn-core:10.2.0                                                                  (för DMN)
+ quarkus-messaging-kafka, quarkus-smallrye-health, smallrye-reactive-messaging-in-memory (test),
  snakeyaml, awaitility, quarkus-junit5, kogito-quarkus-test-utils, wiremock
```

Källa: 〔rimfrost-template-regel-maskinell/pom.xml〕. Regler som anropar externa tjänster lägger
till adaptrar, t.ex. `rimfrost-adapter-folkbokford` och `rimfrost-adapter-arbetsgivare`, och
indexerar dem 〔rimfrost-regel-rtf-maskinell/src/main/resources/application.properties〕.

### [5.3] Filer att skapa (utgå från template)

| Fil | Innehåll | Källa |
|---|---|---|
| `src/main/java/<paket>/logic/<Regel>Service.java` | `@ApplicationScoped`, `implements RegelMaskinellServiceInterface` | 〔rimfrost-template-regel-maskinell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java〕 |
| `src/main/resources/application.properties` | Kafka-topics, `kafka.source`, container-namn, index-dependency | 〔rimfrost-template-regel-maskinell/src/main/resources/application.properties〕 |
| `src/main/resources/config.yaml` | Alla fyra obligatoriska sektioner (`uppgift.path` kan utelämnas) | 〔rimfrost-template-regel-maskinell/src/main/resources/config.yaml〕 |
| `src/main/resources/dmn/<Modell>.dmn` (valfritt) | DMN-beslutstabell | 〔rimfrost-regel-rtf-maskinell/src/main/resources/dmn/RtfDecisionModel.dmn〕 |
| `src/test/java/...` | Tester som ärver test-JAR-basklasser + `WireMock<Regel>` + testdata | `[5.10]` |
| `src/test/resources/application.properties` | In-memory-connectorer | 〔rimfrost-template-regel-maskinell/src/test/resources/application.properties〕 |
| `src/test/resources/mappings/*.json` | WireMock-stubbar (`get-handlaggning-…`, `put-handlaggning`) | 〔rimfrost-template-regel-maskinell/src/test/resources/mappings/put-handlaggning.json〕 |

Inget REST-gränssnitt, ingen databas, ingen micro-frontend.

### [5.4] Gränssnitt att implementera

```java
// KÄLLA: rimfrost-framework-regel-maskinell/.../logic/RegelMaskinellServiceInterface.java
public interface RegelMaskinellServiceInterface {
   RegelMaskinellResult processRegel(RegelMaskinellRequest regelMaskinellRequest);
}
```

Exakt en böna måste implementera gränssnittet (CDI-injektion i ramverkets hanterare)
〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/RegelMaskinellRequestHandler.java:29-30〕.

**Indata** `RegelMaskinellRequest` (alla `@NotNull`): `Handlaggning handlaggning`,
`Uppgift uppgift` (skapad av ramverket), `UUID processInstansId` (= `kogitoprocinstanceid`)
〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/dto/RegelMaskinellRequest.java〕.

**Utdata** — förseglat gränssnitt `RegelMaskinellResult` med två varianter
〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/dto/RegelMaskinellResult.java〕:

| Variant | Fält | Betydelse |
|---|---|---|
| `RegelMaskinellSuccessResult` | `HandlaggningUpdate handlaggningUpdate`, `Utfall utfall` (båda `@NotNull`) | Ramverket skriver uppdateringen och svarar med utfallet 〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/dto/RegelMaskinellSuccessResult.java〕 |
| `RegelMaskinellErrorResult` | `RegelErrorInformation regelErrorInformation` | Affärsmässigt fel: ramverket svarar `ERROR` med denna info, skriver inget 〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/dto/RegelMaskinellErrorResult.java〕 |

Mall:

```java
// MALL — baserad på rimfrost-template-regel-maskinell/.../RegelTemplateService.java
@ApplicationScoped
public class <Regel>Service implements RegelMaskinellServiceInterface {
   @Inject ObjectMapper objectMapper;

   @Override
   public RegelMaskinellResult processRegel(RegelMaskinellRequest req) {
      var uppgift = ImmutableUppgift.builder().from(req.uppgift()).utfordTs(OffsetDateTime.now()).build();
      var underlag = List.of(RegelUtils.createUnderlag("<UNDERLAGSTYP>", 1, minData, objectMapper));
      var update = ImmutableHandlaggningUpdate.builder()
            .id(req.handlaggning().id())
            .version(req.handlaggning().version() + 1)
            .yrkande(req.handlaggning().yrkande())        // ev. RegelUtils.createYrkandeWithUpdatedProduceradeResultat(...)
            .processInstansId(req.processInstansId())
            .skapadTS(req.handlaggning().skapadTS())
            .avslutadTS(req.handlaggning().avslutadTS())
            .handlaggningspecifikationId(req.handlaggning().handlaggningspecifikationId())
            .uppgift(uppgift)
            .underlag(underlag)
            .build();
      return ImmutableRegelMaskinellSuccessResult.builder().handlaggningUpdate(update).utfall(Utfall.JA).build();
   }
}
```

### [5.5] Ramverkets flöde (steg för steg)

`RegelMaskinellRequestHandler.handleRegelRequest` 〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/RegelMaskinellRequestHandler.java:40-154〕:

| Steg | Vad | Vid fel | Rad |
|---|---|---|---|
| 1 | Bygg `CloudEventData` från förfrågan | Loggar och **returnerar utan svar** (se `[AVVIKELSE-15]`) | 43-54 |
| 2 | Läs handläggning med `RetryUtil.getWithRetries`. **Alla** `HandlaggningException` (även `NOT_FOUND` och `BAD_REQUEST`) ger `Result.empty()` och försöks alltså igen | Efter uttömda omförsök: svar `ERROR`/`RIMFROST_HANDLAGGNING_READ_FAILURE` | 59-73, 156-166 |
| 3 | Skapa `Uppgift` (`createUppgift(aktivitetId, null)` → status `null`) | — | 75 |
| 4 | Anropa `processRegel` | Undantag → `ERROR`/`RIMFROST_OTHER` | 82-96 |
| 5 | Om `RegelMaskinellErrorResult` → skicka felsvar med regelns info | — | 98-103 |
| 6 | Skriv `handlaggningUpdate` med `RetryUtil.runWithRetries` (ramverket skriver exakt det regeln returnerar — även `uppgift` måste regeln sätta) | `CONFLICT` → direkt `RIMFROST_HANDLAGGNING_WRITE_FAILURE` (inget omförsök); övriga fel försöks igen; uttömda omförsök → samma felkod | 107-136 |
| 7 | Skicka svar med `utfall` till `replyTo` | — | 139 |
| — | Oväntat undantag i steg 2–7 | `ERROR`/`RIMFROST_OTHER` | 141-153 |

Viktigt: Läsning och skrivning har separata omförsök; affärslogiken körs aldrig om på grund av
skrivfel 〔rimfrost/FAULT_HANDLING_OVERVIEW.md:15-31〕.

### [5.6] Omförsök: `RetryUtil` och `Result`

- `RetryUtil.getWithRetries(Supplier<Result<T>>, List<Integer> intervalSekunder)`: anropar
  leverantören; `Result.empty()` = försök igen efter nästa intervall; `Result.of(v)` = klart
  (även `Result.of(null)` räknas som lyckat). Max `intervals.size() + 1` anrop, sedan
  `RetriesExhaustedException` 〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/helpers/retry/RetryUtil.java:20-42〕
  〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/helpers/retry/Result.java:16-39〕.
- `RetryUtil.runWithRetries(Supplier<Boolean>, intervals)`: samma, `true` = klart
  〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/helpers/retry/RetryUtil.java:52-73〕.
- Väntan sker med `TimeUnit.SECONDS.sleep` i samma tråd; avbrott → `RuntimeException`
  〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/helpers/retry/RetryUtil.java:75-85〕.
- **Undantag från leverantören fångas inte av `RetryUtil`** — regeln måste själv fånga
  adapterns undantag och returnera `Result.empty()` (se `[AVVIKELSE-16]`).
- Intervall: `rimfrost.framework.regel.maskinell.retry.intervals`, default
  `15,30,60,120,240,480,960,1920,3840,7680,15360,30720` sekunder
  〔rimfrost-framework-regel-maskinell/src/main/resources/application.properties:8〕. Summa
  61 425 s ≈ 17 h `[HÄRLETT]`. Eftersom konsumenten är `@Blocking` blockeras
  meddelandehanteringen under väntan `[HÄRLETT]`. Sätt korta intervall i test.
- Regeln ansvarar själv för omförsök mot andra tjänster än handläggning
  〔rimfrost/FAULT_HANDLING_OVERVIEW.md:33-46〕.

Mönster från rtf-maskinell `[KÄLLA]`:

```java
// KÄLLA: rimfrost-regel-rtf-maskinell/.../logic/RtfService.java:54-55,74-86,206-221 (förkortat)
@ConfigProperty(name = "rimfrost.framework.regel.maskinell.retry.intervals")
List<Integer> retryIntervals;

try {
   folkbokfordResponse = RetryUtil.getWithRetries(() -> getFolkbokfordResponse(pnr), retryIntervals);
} catch (RetriesExhaustedException e) {
   return ImmutableRegelMaskinellErrorResult.builder()
         .regelErrorInformation(createRegelErrorInformation(RegelFelkod.RTF_MASKINELL_FOLKBOKFORING_READ_ERROR, "..."))
         .build();
}

private Result<FolkbokfordResponse> getFolkbokfordResponse(String pnr) {
   try { return Result.of(folkbokfordAdapter.getFolkbokfordInfo(request)); }
   catch (FolkbokfordException e) {
      if (e.getErrorType() == FolkbokfordException.ErrorType.NOT_FOUND) return Result.of(null); // "finns inte" = svar
      return Result.empty();                                                                  // tillfälligt fel = försök igen
   }
}
```

Källor: 〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RtfService.java:54-55〕
〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RtfService.java:74-86〕
〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RtfService.java:206-241〕.

### [5.7] DMN (beslutstabeller)

Ramverket har en generisk `DmnService` (`@ApplicationScoped`) som bygger och cachar en
`DMNRuntime` per classpath-resurs 〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/DmnService.java〕:

```java
// KÄLLA: rimfrost-framework-regel-maskinell/.../logic/DmnService.java:27-35,48,127,138
DMNResult result = dmnService.evaluation("dmn/MyModel.dmn", "https://my/namespace", "MyModel")
      .set("input1", value1)
      .set("input2", value2)
      .evaluate();
Optional<Object> v = dmnService.evaluation(...).set(...).evaluateDecision("beslutsnamn");
```

- Modellen saknas → `IllegalArgumentException`; byggfel → `IllegalStateException`
  〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/DmnService.java:48-58〕.
- Kräver `org.kie:kie-dmn-core` (finns i template-pom) 〔rimfrost-template-regel-maskinell/pom.xml〕.
- rtf-maskinell använder en **egen** lokal `DmnService` och mappar DMN-strängen till `Utfall`
  (`"NEJ"`→NEJ, `"JA"`→JA, annat→UTREDNING); `null` → `"UTREDNING"`
  〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/DmnService.java〕
  〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RtfService.java:181-203〕.
  För nya regler: använd ramverkets `DmnService` `[HÄRLETT]`.
- DMN-beslutslogik för rtf: JA om folkbokförd, UTREDNING om ej folkbokförd men anställd, annars
  NEJ 〔rimfrost-guide/src/sections/RulesSection.vue〕.

### [5.8] Konfiguration (maskinell)

| Property | Obl. | Exempel/default | Källa |
|---|---|---|---|
| `quarkus.container-image.name` | ja (unikt) | `rimfrost-regel-<regel>` | 〔rimfrost-template-regel-maskinell/src/main/resources/application.properties:3〕 |
| `kafka.source` | ja (unikt) | `RegelRtfMaskinell` | 〔rimfrost-template-regel-maskinell/src/main/resources/application.properties:6〕 |
| `quarkus.index-dependency.regel-maskinell.*` | ja | group/artifact för framework-regel-maskinell | 〔rimfrost-template-regel-maskinell/src/main/resources/application.properties:8-9〕 |
| `mp.messaging.incoming.regel-requests.topic` | ja | `<regel>-requests` | 〔rimfrost-template-regel-maskinell/src/main/resources/application.properties:13〕 |
| `mp.messaging.outgoing.regel-responses.topic` | ja | `<regel>-responses` | 〔rimfrost-template-regel-maskinell/src/main/resources/application.properties:17〕 |
| `handlaggning.api.base-url` (+ `%dev.`) | nej | `http://rimfrost-k8s-handlaggning:8080` | 〔rimfrost-template-regel-maskinell/src/main/resources/application.properties:21-22〕 |
| `rimfrost.framework.regel.maskinell.retry.intervals` | nej | se `[5.6]` | 〔rimfrost-framework-regel-maskinell/src/main/resources/application.properties:8〕 |
| `<adapter>.api.base-url` | om adapter används | `folkbokford.api.base-url=http://rimfrost-k8s-folkbokford:8080` | 〔rimfrost-regel-rtf-maskinell/src/main/resources/application.properties〕 |
| `quarkus.test.integration-test-name-pattern` | nej | `.*IT` | 〔rimfrost-template-regel-maskinell/src/main/resources/application.properties〕 |

Notera: ramverkets egna `application.properties` indexerar `rimfrost-referensdata-erbjudande`
〔rimfrost-framework-regel-maskinell/src/main/resources/application.properties:5-6〕.

### [5.9] config.yaml för maskinell

Alla fyra sektioner krävs. `uppgift.path` och `utokadUppgiftsbeskrivning` används inte
〔rimfrost-template-regel-maskinell/src/main/resources/config.yaml〕. README:ns YAML-exempel
saknar `regel` och `lagrum` och skulle ge startfel — följ templaten (`[AVVIKELSE-05]`).

### [5.10] Tester (test-JAR `rimfrost-framework-regel-maskinell`, classifier `tests`)

Test-JAR:en innehåller paketen `base/` och `helpers/`
〔rimfrost-framework-regel-maskinell/pom.xml:145-151〕.

| Klass | Täcker | Krokar att överskrida | Källa |
|---|---|---|---|
| `AbstractRegelMaskinellTest` | Bas: in-memory Kafka + WireMock-återställning före varje test | — | 〔rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/AbstractRegelMaskinellTest.java:27-60〕 |
| `AbstractRegelMaskinellHandlaggningTest` | Verifierar PUT till handläggning (parametriserade `@CsvSource`) | `createExpectedUnderlag()`, `createExpectedProduceradeResultat()` | 〔rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/AbstractRegelMaskinellHandlaggningTest.java:55-95〕 |
| `AbstractRegelMaskinellResponseTest` | Kafka-svar | — | 〔rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/AbstractRegelMaskinellResponseTest.java:12-14〕 |
| `AbstractRegelMaskinellSequenceTest` | End-to-end | — | 〔rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/AbstractRegelMaskinellSequenceTest.java:13-16〕 |
| `RegelMaskinellTestData` | Testdata | — | 〔rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/RegelMaskinellTestData.java〕 |
| `WireMockRegelMaskinell` | WireMock för handläggning; ärv och lägg till egna base-urls i `wiremockMapping` | `wiremockMapping(WireMockServer)` | 〔rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/helpers/WireMockRegelMaskinell.java〕 |

Användning:

```java
// KÄLLA: rimfrost-template-regel-maskinell/src/test/java/se/fk/github/regeltemplate/RegelTemplateHandlaggningTest.java
@QuarkusTest
@QuarkusTestResource.List({ @QuarkusTestResource(WireMockRegelTemplate.class) })
public class RegelTemplateHandlaggningTest extends AbstractRegelMaskinellHandlaggningTest {
   @Override protected ArrayList<Underlag> createExpectedUnderlag() { ... }
   @Override protected List<ProduceratResultat> createExpectedProduceradeResultat() { ... }
}

// KÄLLA: rimfrost-template-regel-maskinell/src/test/java/se/fk/github/regeltemplate/WireMockRegelTemplate.java
public class WireMockRegelTemplate extends WireMockRegelMaskinell {
   @Override protected Map<String, String> wiremockMapping(WireMockServer server) {
      Map<String, String> map = new HashMap<>(super.wiremockMapping(server));
      // map.put("abc.api.base-url", server.baseUrl());
      return map;
   }
}
```

Enhetstest av `processRegel` direkt: ärv `AbstractRegelMaskinellTest`, injicera servicen och anropa
den med `RegelTemplateTestData.newRegelMaskinellRequest()`
〔rimfrost-template-regel-maskinell/src/test/java/se/fk/github/regeltemplate/RegelTemplateProcessRegelTest.java〕
〔rimfrost-template-regel-maskinell/src/test/java/se/fk/github/regeltemplate/RegelTemplateTestData.java〕.

### [5.11] Checklista — ny maskinell regel

1. Skapa repo från `rimfrost-template-regel-maskinell`; byt `RegelTemplate*` och paketnamn.
2. `pom.xml`: `artifactId`/`groupId`; lägg till adaptrar.
3. `application.properties`: container-namn, `kafka.source`, request-/response-topics, adapter-URL:er,
   `quarkus.index-dependency` för adaptrar.
4. `config.yaml`: nya UUID:n i alla sektioner, `aktivitet`, `specifikation.*`, `regel.*`, `lagrum.*`.
5. Implementera `processRegel` (underlag, ev. producerade resultat, version + 1, utfall).
6. Externa anrop: `RetryUtil` + `Result.empty()/of()`; affärsfel via `RegelMaskinellErrorResult`
   med egna felkoder (inte `RIMFROST_`-prefix).
7. Ev. DMN under `src/main/resources/dmn/` + ramverkets `DmnService`.
8. Tester: Handlaggning-, Response-, Sequence-test + egna tester; WireMock-mappningar.
9. Skapa subprocess-repo (`[9]`) och registrera i en process (`[10]`).

---

## [6] Manuell regel

### [6.1] Syfte

Regel där en handläggare agerar via portalen. Ramverket skapar en uppgift i OUL och väntar tills
handläggaren markerar den som klar (`POST /{handlaggningId}/done`); först då skickas regelsvaret
〔rimfrost/README.md:86-92〕 〔rimfrost-framework-regel-manuell/README.md〕.

### [6.2] Beroenden (Maven)

```
// KÄLLA: rimfrost-template-regel-manuell/pom.xml
parent fk-maven-quarkus-parent:1.12.0
se.fk.rimfrost.framework.regel.manuell:rimfrost-framework-regel-manuell:1.4.5
se.fk.rimfrost.framework.regel.manuell:rimfrost-framework-regel-manuell:1.4.5   [tests] (test)
se.fk.rimfrost.framework.regel:rimfrost-framework-regel:1.4.4                    [tests] (test)
se.fk.rimfrost.template.regel.manuell.openapi:rimfrost-template-regel-manuell-openapi-jaxrs-spec:1.1.1   → byt mot regelns eget OpenAPI-repo
se.fk.rimfrost.referensdata:rimfrost-template-referensdata-erbjudande:0.0.1                               → byt mot riktig impl. (se [12])
+ quarkus-smallrye-health, quarkus-messaging-kafka, smallrye-reactive-messaging-in-memory, quarkus-junit, wiremock, quarkus-junit-mockito, awaitility
```

Källa: 〔rimfrost-template-regel-manuell/pom.xml〕. rtf-manuell använder
`rimfrost-referensdata-erbjudande` och `rimfrost-regel-rtf-manuell-openapi-jaxrs-spec`
〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:12-14〕.

### [6.3] Filer att skapa

| Fil | Innehåll | Källa |
|---|---|---|
| `logic/<Regel>Service.java` | `@ApplicationScoped @Startup`, `extends RegelManuellServiceBase implements RegelManuellServiceInterface<T, Y>` | 〔rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java〕 |
| `logic/<Regel>MiddlewareServiceImpl.java` | `@ApplicationScoped`, `extends RegelManuellMiddlewareService<T, Y>` — **tom klass men obligatorisk** | 〔rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateMiddlewareServiceImpl.java〕 |
| `logic/<Regel>Mapper.java` (valfritt) | Mappar `Handlaggning` → `T` | 〔rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateMapper.java〕 |
| `presentation/rest/<Regel>Controller.java` | `@Produces/@Consumes("application/json") @ApplicationScoped @Path("/regel/<regel>")`, `extends RegelManuellController<T, Y>` (tom) | 〔rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/presentation/rest/RegelTemplateController.java〕 |
| `resources/application.properties` | Se `[6.7]` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties〕 |
| `resources/config.yaml` | `uppgift.path` = controllerns `@Path`; `utokadUppgiftsbeskrivning` | 〔rimfrost-template-regel-manuell/README.md〕 |
| `resources/db/migration/V001__<prefix>_tables.sql` | `<prefix>_common_data`, `<prefix>_cloud_event_data` | 〔rimfrost-template-regel-manuell/src/main/resources/db/migration/V001__regeltemplate_tables.sql〕 |
| `resources/db/migration/V002__<prefix>_process_topic_info_table.sql` | `<prefix>_process_topic_info` | 〔rimfrost-template-regel-manuell/src/main/resources/db/migration/V002__regeltemplate_process_topic_info_table.sql〕 |
| `src/test/...` | Tester som ärver manuell-test-JAR | `[6.11]` |
| `src/test/resources/config-test.yaml` | Testkonfig | 〔rimfrost-template-regel-manuell/src/test/resources/config-test.yaml〕 |
| Separat repo `rimfrost-regel-<regel>-openapi` | OpenAPI för `T`/`Y` | `[11]` |
| Separata repon `-fe`, `-bff`, `-subprocess` | Frontend, BFF, BPMN | `[13]`, `[9]` |

`T` = svarstyp för `GET /{handlaggningId}`; `Y` = body för `PATCH /{handlaggningId}`. Båda ska
vara klasser genererade från regelns OpenAPI-spec
〔rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateMiddlewareServiceImpl.java〕.

### [6.4] Gränssnitt och basklasser

```java
// KÄLLA: rimfrost-framework-regel-manuell/.../logic/RegelManuellServiceInterface.java
public interface RegelManuellServiceInterface<T, Y> {
   T readData(Handlaggning handlaggning);                              // GET /{id}
   HandlaggningUpdate updateData(Handlaggning handlaggning, Y request); // PATCH /{id}
   void done(UUID handlaggningId);                                     // POST /{id}/done
}

// KÄLLA: rimfrost-framework-regel-manuell/.../logic/RegelManuellServiceBase.java:9-23
public abstract class RegelManuellServiceBase {
   @Inject protected ObjectMapper objectMapper;
   protected void sendRegelResponse(UUID handlaggningId, Utfall utfall); // avslutar regelkörningen
}
```

Källor: 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellServiceInterface.java〕
〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellServiceBase.java:9-23〕.

- Klassen **måste** annoteras `@ApplicationScoped`; ramverket tillhandahåller inte annoteringen
  〔rimfrost-framework-regel-manuell/README.md〕.
- `done()` måste anropa `sendRegelResponse(handlaggningId, utfall)`; annars avslutas aldrig
  regelkörningen (inget svar, OUL-uppgiften lever kvar) `[HÄRLETT]`
  〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellServiceBase.java:19-22〕.
- `done()` kan göra egen logik innan, t.ex. skriva handläggning och beräkna utfall (bekräfta
  beslut: JA om alla ersättningar beviljats, annars NEJ)
  〔rimfrost-regel-bekraftabeslut/src/main/java/se/fk/github/bekraftabeslut/logic/BekraftaBeslutService.java:131-176〕.
- `updateData` returnerar `HandlaggningUpdate` som ramverket skriver; returnera inte `null`
  (templaten gör det som TODO) `[HÄRLETT]`.
- Uppgiften (`Uppgift`) att bära med i `HandlaggningUpdate` hämtas från korrelationsdata:
  `oulUppgiftService.getCorrelationData(handlaggning.id()).uppgift()`
  〔rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/logic/RtfService.java:115-126〕.

**Middleware** (`RegelManuellMiddlewareService<T, Y>`, abstrakt) — ligger mellan controller och
service 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellMiddlewareService.java:26-68〕:

| Metod | Beteende |
|---|---|
| `read(id)` | Läser handläggning → SID-kontroll → vid skyddad identitet: försöker ta bort tilldelning i OUL och kastar 403 → annars `regelService.readData(h)` |
| `update(id, y)` | Läser handläggning → `regelService.updateData(h, y)` → skriver; `CONFLICT` → 500 |
| `done(id)` | `regelService.done(id)` |

**Controller** (`RegelManuellController<T, Y>`, abstrakt, `implements RegelOulControllerApi`)
〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/RegelManuellController.java:21-61〕:

| Endpoint (relativt `@Path` på subklassen) | Metod | Delegerar till | Rad |
|---|---|---|---|
| `/utokadUppgiftsbeskrivning` | GET | `config.yaml` → `{ "beskrivning": … }` | 26-37 |
| `/{handlaggningId}` | GET | `middleware.read` → `T` (200) | 39-45 |
| `/{handlaggningId}` | PATCH | `middleware.update` (`@Valid @NotNull Y`) | 47-52 |
| `/{handlaggningId}/done` | POST | `middleware.done` | 54-60 |

Extra endpoints: implementera ytterligare genererat API-gränssnitt i controllern (bekräfta
beslut lägger till `GET /avslutstyp`, `/beslutstyp`, `/beslutsutfallstyp`, `/yrkandestatus`)
〔rimfrost-regel-bekraftabeslut/src/main/java/se/fk/github/bekraftabeslut/presentation/rest/BekraftaBeslutController.java〕.

### [6.5] Ramverkets flöde

**A. Kafka-förfrågan** (`RegelManuellRequestHandler.handleRegelRequest`)
〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:96-135〕:

1. Bygg `CloudEventData` (type = response-topic, source = `kafka.source`).
2. Läs handläggning (fel → `RIMFROST_HANDLAGGNING_READ_FAILURE`, inga omförsök).
3. Slå upp erbjudandenamn via `ErbjudandeReferensdataInterface.getErbjudandeNamn(yrkande.erbjudandeId)`.
4. Bygg `OulUppgiftSpec` från `config.yaml` + förfrågan och anropa
   `OulUppgiftService.createOulUppgift(spec)` (se `[8.3]`).
5. Inget Kafka-svar i detta steg — regeln väntar.
6. Fel i 1–4 → felsvar till `replyTo` (`RegelCancelledException` bär specifik felkod, annars
   `RIMFROST_OTHER`).

**B. Handläggaren arbetar** — portalen öppnar micro-frontenden (`uppgift.path`), som via BFF
anropar `GET`/`PATCH /{handlaggningId}` (`[6.4]`). SID kontrolleras vid varje GET.

**C. `POST /{handlaggningId}/done`** → `regelService.done(id)` → regeln anropar
`sendRegelResponse(id, utfall)` → `RegelManuellRequestHandler.handleUppgiftDone`
〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:152-224〕:

| Steg | Vad | Vid fel |
|---|---|---|
| 1 | Läs korrelationsdata | saknas → HTTP 500, inget Kafka-svar |
| 2 | Läs handläggning | HTTP 404/400/503/500 enligt `ErrorType`, inget Kafka-svar |
| 3 | `endOulUppgift(oulUppgiftId, "Uppgift klar")` | 404 räknas som lyckat; annat → HTTP-fel, inget Kafka-svar (handläggaren kan försöka igen) |
| 4 | Skicka `RegelResponse` med utfall till lagrad `replyTopic` | sändfel loggas |
| 5 | `cleanupCorrelation` (best-effort) | loggas |
| 6 | Uppdatera handläggning: uppgift version+1, status `AVSLUTAD`, `utfordTs=now` | loggas och ignoreras (svar redan skickat) |

### [6.6] SID-kontroll (skyddad identitet)

- Vid `GET /{handlaggningId}`: individer hämtas från `yrkande.individYrkandeRoller` och skickas i
  `POST /sid/status` till SID-tjänsten 〔rimfrost-framework-regel-manuell/docs/krav.md:78-100〕.
- Skyddad identitet → `tryUnassignOulUppgift` (fel sväljs) → HTTP 403; `readData` anropas inte
  〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellMiddlewareService.java:46-55〕.
- Saknas korrelationsdata/uppgifts-id: unassign hoppas över, 403 returneras ändå
  〔rimfrost-framework-regel-manuell/docs/krav.md:91-101〕.
- SID-fel mappas till 404/400/503/500 〔rimfrost-framework-regel-manuell/docs/krav.md:86-87〕.
- SID kontrolleras **bara** vid `GET /{handlaggningId}`; `PATCH` och `POST /done` gör ingen
  SID-kontroll `[KÄLLA]` 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellMiddlewareService.java:58-68〕.
- `sid.api.base-url` krävs (SidAdapter har ingen default i manuell-ramverket)
  〔rimfrost-framework-sid-adapter/src/main/java/se/fk/rimfrost/framework/sid/adapter/SidAdapter.java:36-37〕
  〔rimfrost-framework-regel-manuell/README.md〕.

### [6.7] Konfiguration (manuell)

Komplett lista. Ramverkens egna defaults kommer från framework-regel, -regel-manuell och
-regel-oul 〔rimfrost-framework-regel-oul/src/main/resources/application.properties〕
〔rimfrost-framework-regel-manuell/src/main/resources/application.properties〕.

| Property | Obl. | Template-värde / default | Källa |
|---|---|---|---|
| `quarkus.index-dependency.rimfrost-framework-regel-manuell.*` | ja | | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:1-2〕 |
| `quarkus.index-dependency.rimfrost-referensdata-erbjudande.*` | ja | artifact för referensdata-impl | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:4-5〕 |
| `quarkus.index-dependency.<regel>-openapi-jaxrs-spec.*` | ja | regelns OpenAPI-artefakt | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:7-9〕 |
| `quarkus.container-image.name` | ja | `rimfrost-regel-<regel>` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:13〕 |
| `kafka.source` | ja | template `RegelTemplate` (exempel: `RegelRtfManuell`) | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:17〕 |
| `kafka.subtopic` | ja | `<regel>` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:18〕 |
| `kafka.cancelled.topic` | **ja** (saknas i template!) | `<regel>-cancelled` | 〔rimfrost-framework-regel-oul/src/main/resources/application.properties:24-26〕 〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:22〕 `[AVVIKELSE-02]` |
| `mp.messaging.incoming.regel-requests.topic` | ja | `<regel>-requests` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:22〕 |
| `mp.messaging.incoming.operativt-uppgiftslager-status-notification.topic` | ja | `operativt-uppgiftslager-status-notification.${kafka.subtopic}` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:25〕 |
| `mp.messaging.outgoing.regel-responses.topic` | ja | `<regel>-responses` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:29〕 |
| `quarkus.flyway.default-schema` | ja | `regel_<regel_snake>` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:33〕 |
| `regel.persistence.table-prefix` | ja | `<regel_snake>` (måste matcha SQL-filerna) | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:34〕 |
| `%prod.quarkus.datasource.username/password/jdbc.url` | ja (prod) | `${DB_USERNAME}`, `${DB_PASSWORD}`, `${DB_URL}` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties〕 |
| `handlaggning.api.base-url` | nej | `http://rimfrost-k8s-handlaggning:8080` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:41〕 |
| `oul.api.base-url` | nej | `http://rimfrost-k8s-uppgiftslager:8080` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:42〕 |
| `sid.api.base-url` | **ja** | `http://rimfrost-k8s-sid:8080` | 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:43〕 |
| `referensdata.api.base-url` | ja om `rimfrost-referensdata-erbjudande` används | `http://rimfrost-k8s-referensdata:8080` | 〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:43〕 |
| `rimfrost.framework.regel.manuell.rest.exception.catchall.response.message` | nej | `Internal Server Error` | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/ExceptionMapper.java:14〕 |

Ramverket sätter själv bl.a. `quarkus.hibernate-orm.schema-management.strategy=validate`,
`quarkus.flyway.migrate-at-start=true`, `create-schemas=true`, naming strategy och
Kafka-serializer för `regel-responses` 〔rimfrost-framework-regel-oul/src/main/resources/application.properties:10-18〕
〔rimfrost-framework-regel-manuell/src/main/resources/application.properties〕.

### [6.8] Databas (Flyway)

Tabellerna måste skapas av **regelns egna** Flyway-migreringar; ramverket levererar inga
〔rimfrost-framework-regel-oul/docs/db-schema.md:3-5〕 〔rimfrost-template-regel-manuell/README.md〕.
Använd SQL från templaten (eller ramverkets test-migration) — **inte** SQL:en i
`db-schema.md`, som är inaktuell (`[AVVIKELSE-01]`). Hibernate kör `validate`, så kolumner
måste matcha entiteterna exakt.

```sql
-- KÄLLA: rimfrost-template-regel-manuell/src/main/resources/db/migration/V001__regeltemplate_tables.sql
CREATE TABLE <prefix>_common_data (
    handlaggning_id UUID NOT NULL PRIMARY KEY,  uppgift_id UUID NOT NULL,  uppgift_version INTEGER NOT NULL,
    uppgift_skapad_ts TIMESTAMPTZ NOT NULL,  uppgift_utford_ts TIMESTAMPTZ,  uppgift_planerad_ts TIMESTAMPTZ,
    uppgift_utforar_id_typ_id VARCHAR(255),  uppgift_utforar_id_varde VARCHAR(255),
    uppgift_status VARCHAR(255) NOT NULL,  uppgift_aktivitet_id UUID NOT NULL,
    uppgift_fssa_information VARCHAR(255) NOT NULL,  uppgift_specifikation_id UUID NOT NULL,
    uppgift_specifikation_version INTEGER NOT NULL,  oul_uppgift_id UUID,
    version BIGINT NOT NULL DEFAULT 0,  created_at TIMESTAMPTZ NOT NULL,  updated_at TIMESTAMPTZ NOT NULL);
CREATE TABLE <prefix>_cloud_event_data (
    handlaggning_id UUID NOT NULL PRIMARY KEY,  event_id UUID NOT NULL,
    kogitorootprociid UUID NOT NULL,  kogitoparentprociid UUID NOT NULL,  kogitoprocinstanceid UUID NOT NULL,
    kogitorootprocid VARCHAR(255) NOT NULL,  kogitoprocid VARCHAR(255) NOT NULL,
    kogitoprocist VARCHAR(255),  kogitoprocversion VARCHAR(255),
    type VARCHAR(255) NOT NULL,  source VARCHAR(255) NOT NULL,
    version BIGINT NOT NULL DEFAULT 0,  created_at TIMESTAMPTZ NOT NULL,  updated_at TIMESTAMPTZ NOT NULL);
-- KÄLLA: .../V002__regeltemplate_process_topic_info_table.sql
CREATE TABLE <prefix>_process_topic_info (
    handlaggning_id UUID NOT NULL PRIMARY KEY,  reply_topic VARCHAR(255) NOT NULL,
    version BIGINT NOT NULL DEFAULT 0,  created_at TIMESTAMPTZ NOT NULL,  updated_at TIMESTAMPTZ NOT NULL);
```

Tabellnamnen bildas som `<regel.persistence.table-prefix>_<entitetstabell>` av
`RegelPhysicalNamingStrategy` (ärver snake_case-strategin och lägger till prefix + `_`; saknas
`regel.persistence.table-prefix` kastas `IllegalStateException` vid start); entiteterna heter
`common_data`, `cloud_event_data`, `process_topic_info` 〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/RegelPhysicalNamingStrategy.java〕
〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/CloudEventDataEntity.java:21〕.
Prefixet måste vara unikt per regel 〔rimfrost-framework-regel-oul/README.md〕.

### [6.9] REST-felmappning (manuell)

| Undantag/situation | HTTP | Källa |
|---|---|---|
| `RegelManuellException(status, msg)` (kastas av ramverk och regel) | given status | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/RegelManuellExceptionMapper.java〕 |
| Valideringsfel (`@Valid`) | 400 | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/ValidationExceptionMapper.java〕 |
| `WebApplicationException` | dess status | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/WebApplicationExceptionMapper.java〕 |
| Övriga undantag | 500 med konfigurerbart meddelande | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/ExceptionMapper.java:14-26〕 |
| `HandlaggningException`/`SidException`: NOT_FOUND/BAD_REQUEST/SERVICE_UNAVAILABLE/övrigt | 404/400/503/500 | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellMiddlewareService.java〕 |
| Versionskonflikt vid PATCH | 500 ("Programming fault in regel service?") | 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellMiddlewareService.java〕 |
| Skyddad identitet | 403 | `[6.6]` |

Regeln själv bör kasta `RegelManuellException` med lämplig `Response.Status` vid fel i
`readData`/`updateData` (exempel: adapterfel → 400/503/500; okänt id → 400)
〔rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/logic/RtfService.java:69-101〕
〔rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/logic/RtfService.java:143-149〕.

### [6.10] Informationsaccess (krav, ej implementerat)

Kraven säger att `GET /{handlaggningId}` ska returnera en container `{ data, informationsaccess }`
med `uppgiftId`, `regelId`, `regelVersion` 〔rimfrost-framework-regel-manuell/docs/krav.md:102-122〕.
Koden returnerar `T` direkt 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/RegelManuellController.java:39-45〕.
Se `[AVVIKELSE-08]`. Bygg BFF:er mot faktisk kod.

### [6.11] Tester (test-JAR `rimfrost-framework-regel-manuell`, classifier `tests`)

Test-JAR innehåller `base/` och `helpers/` 〔rimfrost-framework-regel-manuell/pom.xml:169-175〕.
`AbstractRegelManuellTest` mockar `OulUppgiftService` med `@InjectMock` (OUL-gränsen)
〔rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellTest.java:96-99〕
〔rimfrost-framework-regel-manuell/docs/test-strategy.md〕.

| Klass | Täcker | Källa |
|---|---|---|
| `AbstractRegelManuellTest` | Bas; kräver property `regel.manuell.base-path` (controllerns path); hjälpmetoder `waitForRegelRequestProcessed`, `waitForRegelManuellReady`, `sendPostRegelManuellHandlaggningDone`, `sendGetUtokadUppgiftsbeskrivning` | 〔rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellTest.java:78-232〕 |
| `AbstractRegelManuellHandlaggningTest` | REST-flöden mot handläggning | 〔rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellHandlaggningTest.java〕 |
| `AbstractRegelManuellOulTest` | OUL-integration | 〔rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellOulTest.java〕 |
| `AbstractRegelManuellResponseTest` | Regelsvar | 〔rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellResponseTest.java〕 |
| `AbstractRegelManuellUtokadUppgiftsbeskrivningTest` | Utökad beskrivning | 〔rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellUtokadUppgiftsbeskrivningTest.java〕 |
| `RegelManuellTestData`, `WireMockRegelManuell` | Testdata, WireMock | 〔rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/helpers/WireMockRegelManuell.java〕 |

Testkonfiguration (template) 〔rimfrost-template-regel-manuell/src/test/resources/application.properties〕:

```properties
# KÄLLA: rimfrost-template-regel-manuell/src/test/resources/application.properties (förkortat)
application.config.path=src/test/resources/config-test.yaml
quarkus.index-dependency.rimfrost-framework-regel-manuell-tests.group-id=se.fk.rimfrost.framework.regel.manuell
quarkus.index-dependency.rimfrost-framework-regel-manuell-tests.artifact-id=rimfrost-framework-regel-manuell
quarkus.index-dependency.rimfrost-framework-regel-manuell-tests.classifier=tests
quarkus.flyway.default-schema=regel_regeltemplate_test
regel.persistence.table-prefix=regeltemplate
regel.manuell.base-path=/regel/Template
mp.messaging.incoming.regel-requests.connector=smallrye-in-memory
mp.messaging.incoming.operativt-uppgiftslager-status-notification.connector=smallrye-in-memory
mp.messaging.outgoing.regel-responses.connector=smallrye-in-memory
quarkus.kafka.devservices.enabled=false
```

rtf-manuell lägger också till `mp.messaging.incoming.regel-oul-cancelled.connector=smallrye-in-memory`,
WireMock dev services och `quarkus.flyway.locations=filesystem:src/main/resources/db/migration`
〔rimfrost-regel-rtf-manuell/src/test/resources/application.properties〕.

### [6.12] Checklista — ny manuell regel

1. Skapa `rimfrost-regel-<regel>-openapi` från `rimfrost-template-regel-manuell-openapi`; definiera
   `GET /{handlaggningId}`-svar (`T`) och `PATCH /{handlaggningId}`-body (`Y`); publicera (`[11]`).
2. Skapa regelrepo från `rimfrost-template-regel-manuell`; byt `RegelTemplate*`, paket, `@Path`.
3. `pom.xml`: byt template-openapi mot regelns; byt `rimfrost-template-referensdata-erbjudande`
   mot `rimfrost-referensdata-erbjudande` (eller egen impl.).
4. `application.properties`: allt i `[6.7]`, **inklusive `kafka.cancelled.topic`**.
5. Byt namn på SQL-filer och tabeller så att de matchar `regel.persistence.table-prefix`.
6. `config.yaml`: nya UUID:n; `uppgift.path` = controllerns `@Path`; `utokadUppgiftsbeskrivning`.
7. Implementera `readData`, `updateData` (version + 1 vid ändring, uppgift från
   korrelationsdata), `done` (anropa `sendRegelResponse`).
8. Tester (ärver test-JAR), `regel.manuell.base-path` i testkonfig.
9. Skapa subprocess (`[9]`), micro-frontend och BFF (`[13]`), registrera i `remotes.json`.


---

## [7] Kompletteringsregel

### [7.1] Syfte

Exponerar komplettering som en Kafka-anropbar regel. Vid förfrågan körs en
fullständighetskontroll: är yrkandet komplett svarar regeln `JA` direkt; annars skapas en
OUL-uppgift och svaret (`JA`) skickas först när handläggaren markerat kompletteringen klar.
Anroparen behöver inte skilja vägarna åt 〔rimfrost-framework-regel-komplettering/README.md〕
〔rimfrost-framework-regel-komplettering/docs/krav.md:13-22〕.

### [7.2] Beroenden (Maven)

```
// KÄLLA: rimfrost-template-regel-komplettering/pom.xml
parent fk-maven-quarkus-parent:1.12.0
se.fk.rimfrost.framework.regel.komplettering:rimfrost-framework-regel-komplettering:0.1.1
se.fk.rimfrost.framework.regel.komplettering:rimfrost-framework-regel-komplettering:0.1.1  [tests] (test)
se.fk.rimfrost.framework.regel:rimfrost-framework-regel:1.4.4                              [tests] (test)
se.fk.rimfrost.template.regel.komplettering.openapi:rimfrost-template-regel-komplettering-openapi-jaxrs-spec:0.0.2  → byt mot regelns eget
se.fk.rimfrost.framework.regel.oul:rimfrost-framework-regel-oul-openapi-jaxrs-spec:0.0.3
se.fk.rimfrost.referensdata:rimfrost-template-referensdata-erbjudande:0.0.1                → byt mot riktig impl.
```

### [7.3] Filer att skapa

| Fil | Innehåll | Källa |
|---|---|---|
| `logic/<Regel>KompletteringService.java` | `@ApplicationScoped implements RegelKompletteringService<T>` | 〔rimfrost-template-regel-komplettering/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java〕 |
| `presentation/<Regel>KompletteringController.java` | `@Path("/regel/<regel>") @ApplicationScoped extends RegelKompletteringController<T>` (tom) | 〔rimfrost-template-regel-komplettering/src/main/java/se/fk/github/regeltemplate/presentation/RegelTemplateController.java〕 |
| `resources/application.properties` | Se `[7.7]` | 〔rimfrost-template-regel-komplettering/src/main/resources/application.properties〕 |
| `resources/config.yaml` | Som manuell (`uppgift.path` = controllerns path) | 〔rimfrost-template-regel-komplettering/src/main/resources/config.yaml〕 |
| `resources/db/migration/V001__…_tables.sql`, `V002__…_process_topic_info_table.sql` | Samma tabeller som manuell (`[6.8]`) med regelns prefix | 〔rimfrost-template-regel-komplettering/src/main/resources/db/migration/V001__regel_template_komplettering_tables.sql〕 |
| Separat repo `rimfrost-regel-<regel>-openapi` | Datatyp `T` | 〔rimfrost-template-regel-komplettering-openapi/openapi.yaml〕 |

Ingen middleware-klass behövs (till skillnad från manuell) `[KÄLLA]` — controllern injicerar
servicen direkt 〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/RegelKompletteringController.java:44-66〕.
Templatens README beskriver fel filer — se `[AVVIKELSE-04]`.

### [7.4] Gränssnitt

```java
// KÄLLA: rimfrost-framework-regel-komplettering/.../logic/RegelKompletteringService.java
public interface RegelKompletteringService<T> {
   boolean isKompletteringRequired(Handlaggning handlaggning);          // true = information saknas
   T readSvarData(Handlaggning handlaggning);                            // GET /{id}
   HandlaggningUpdate registerSvar(Handlaggning handlaggning, T request); // PATCH /{id}
}
```

- **Samma typ `T`** för GET-svar och PATCH-body 〔rimfrost-framework-regel-komplettering/docs/krav.md:63-66〕.
- `isKompletteringRequired` anropas både vid Kafka-förfrågan och vid `POST /done`
  〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/logic/RegelKompletteringRequestHandler.java:65〕
  〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/logic/RegelKompletteringRequestHandler.java:136〕.
- Det finns ingen `done()`-metod att implementera; ramverket avslutar och svarar alltid `JA`.

Exempel (rtf-manuell-komplettering) `[KÄLLA]`:

```java
// KÄLLA: rimfrost-regel-rtf-manuell-komplettering/.../logic/RtfKompletteringService.java:21-34
public boolean isKompletteringRequired(Handlaggning handlaggning) {
   var yrkande = handlaggning.yrkande();
   boolean harPersonnummer = yrkande.individYrkandeRoller().stream()
         .anyMatch(r -> "personnummer".equals(r.individ().typId())
               && r.individ().varde() != null && !r.individ().varde().isBlank());
   boolean saknarAvsikt = yrkande.avsikt() == null || yrkande.avsikt().isBlank();
   return !harPersonnummer || saknarAvsikt;
}
```

`registerSvar` i exemplet bygger nytt `Yrkande` (ersätter personnummer-roll, sätter avsikt) och
returnerar `HandlaggningUpdate` med oförändrad version och utan `uppgift`/`processInstansId`
〔rimfrost-regel-rtf-manuell-komplettering/src/main/java/se/fk/github/rtf/manuell/komplettering/logic/RtfKompletteringService.java:52-88〕.

### [7.5] Ramverkets flöde

**A. Kafka-förfrågan** 〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/logic/RegelKompletteringRequestHandler.java:56-104〕:
1. Bygg CloudEvent, läs handläggning, slå upp erbjudandenamn.
2. `isKompletteringRequired == false` → svar `JA` direkt till `replyTo`, ingen OUL-uppgift.
3. Annars → `createOulUppgift(spec)` (som manuell) och vänta.
4. Fel → felsvar (`RegelCancelledException`-kod, annars `RIMFROST_OTHER`).

**B. REST** (`RegelKompletteringController<T>`) 〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/RegelKompletteringController.java〕:

| Endpoint | Metod | Beteende | Rad |
|---|---|---|---|
| `/{handlaggningId}` | GET | Läs handläggning → SID (403 + unassign) → `readSvarData` | 72-84 |
| `/{handlaggningId}` | PATCH | Läs → `registerSvar` → skriv; `CONFLICT` → 500 | 91-98 |
| `/{handlaggningId}/done` | POST | `handleKompletteringDone` | 116-121 |
| `/utokadUppgiftsbeskrivning` | GET | Från `config.yaml` | 163-174 |

**C. `POST /done`** 〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/logic/RegelKompletteringRequestHandler.java:106-201〕:

| Steg | Vid fel / resultat | HTTP |
|---|---|---|
| Läs korrelationsdata | saknas (t.ex. timeout har städat) → `CorrelationDataReadException` | **409** |
| Läs handläggning | `NOT_FOUND` → 404; annat → 500 | 404/500 |
| `isKompletteringRequired` fortfarande true | `KompletteringIncompleteException` | **422** |
| `endOulUppgift(…, "Uppgift klar")` | 404 = ok; annat → `EndOulUppgiftException` | 500 |
| Svar `JA` till lagrad `replyTopic` | — | 204 |
| Städa korrelation; uppdatera handläggning (uppgift `AVSLUTAD`) | fel loggas och ignoreras | — |

Felmappning (exception mappers) 〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/exception/CorrelationDataReadExceptionMapper.java〕
〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/exception/KompletteringIncompleteExceptionMapper.java:19〕
〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/exception/HandlaggningNotFoundExceptionMapper.java〕
〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/exception/CatchAllExceptionMapper.java〕:
`CorrelationDataReadException`→409, `KompletteringIncompleteException`→422,
`HandlaggningNotFoundException`→404, `HandlaggningReadException`→500, `EndOulUppgiftException`→500,
`WebApplicationException`→dess status, övrigt→500 (meddelande via
`rimfrost.framework.regel.komplettering.rest.exception.catchall.response.message`).

### [7.6] Timeout

Krav: om komplettering initieras ska svar skickas när den är klar **eller när timeout löpt ut**
〔rimfrost-framework-regel-komplettering/docs/krav.md:119-124〕. Ramverket har ingen egen timer;
timeout hanteras i subprocessens BPMN (timer + `handleTimeout` → `ERROR`/`TIMEOUT`) och städning
sker via cancelled-event om det skickas (`[8.5]`, `[9.4]`) `[HÄRLETT]`.

### [7.7] Konfiguration (komplettering)

Som manuell (`[6.7]`) med följande skillnader i template/exempel:

| Property | Template | Kommentar | Källa |
|---|---|---|---|
| `kafka.source` / `kafka.subtopic` | `RegelTemplateKomplettering` / `regel-template-komplettering` | | 〔rimfrost-template-regel-komplettering/src/main/resources/application.properties:10-11〕 |
| `quarkus.kafka.devservices.topics` | `…-requests,…-responses,operativt-uppgiftslager-status-notification` | dev services | 〔rimfrost-template-regel-komplettering/src/main/resources/application.properties:12〕 |
| `mp.messaging.incoming.regel-requests.topic` / `outgoing.regel-responses.topic` | `regel-template-komplettering-requests/-responses` | | 〔rimfrost-template-regel-komplettering/src/main/resources/application.properties:15-18〕 |
| `quarkus.flyway.default-schema` / `regel.persistence.table-prefix` | `regel_template_komplettering` | | 〔rimfrost-template-regel-komplettering/src/main/resources/application.properties:21-22〕 |
| `oul.api.base-url`, `referensdata.api.base-url`, `sid.api.base-url` | satta | `sid` har default i ramverket | 〔rimfrost-template-regel-komplettering/src/main/resources/application.properties:29-31〕 〔rimfrost-framework-regel-komplettering/src/main/resources/application.properties:17〕 |
| `mp.messaging.incoming.operativt-uppgiftslager-status-notification.topic` | **saknas** | se `[AVVIKELSE-06]` | |
| `kafka.cancelled.topic` | **saknas** | se `[AVVIKELSE-02]` | |

### [7.8] Informationsaccess

Kravet `FRKOMP-FR-06` (container `{data, informationsaccess}`) är inte implementerat; GET
returnerar `readSvarData` direkt 〔rimfrost-framework-regel-komplettering/docs/krav.md:95-115〕
〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/RegelKompletteringController.java:82〕.
Se `[AVVIKELSE-08]`.

### [7.9] Extra lagring i exemplet

rtf-manuell-komplettering har en tredje migrering `V003__rtf_manuell_komplettering_tillstand.sql`
med tabell `rtf_manuell_komplettering_tillstand` 〔rimfrost-regel-rtf-manuell-komplettering/src/main/resources/db/migration/V003__rtf_manuell_komplettering_tillstand.sql〕.
Ingen Java-kod i repot använder den `[HÄRLETT]` (troligen kvarleva). Behövs inte i nya regler.

### [7.10] Tester

Test-JAR innehåller `**/Abstract*` och `**/logic/Test*` 〔rimfrost-framework-regel-komplettering/pom.xml:128-134〕:
`AbstractRegelKompletteringTestBase` (ärver `RegelTestBase`, mockar OUL med `@InjectMock`)
〔rimfrost-framework-regel-komplettering/src/test/java/se/fk/rimfrost/framework/regel/komplettering/AbstractRegelKompletteringTestBase.java:52-62〕
och `TestRegelKompletteringService`. Templaten har bara ett enkelt servicetest
〔rimfrost-template-regel-komplettering/src/test/java/se/fk/github/regeltemplate/logic/RegelTemplateServiceTest.java〕.
Det saknas färdiga abstrakta flödestester motsvarande manuell `[KÄLLA]`.

### [7.11] Checklista — ny kompletteringsregel

1. OpenAPI-repo från `rimfrost-template-regel-komplettering-openapi` med datatypen `T`.
2. Regelrepo från `rimfrost-template-regel-komplettering`; byt namn/paket/`@Path`.
3. Implementera `isKompletteringRequired`, `readSvarData`, `registerSvar`.
4. `application.properties`: som `[7.7]` **plus** `kafka.cancelled.topic` och
   `mp.messaging.incoming.operativt-uppgiftslager-status-notification.topic=operativt-uppgiftslager-status-notification.${kafka.subtopic}`.
5. SQL-migreringar med rätt prefix.
6. `config.yaml` med nya UUID:n.
7. Subprocess: kompletteringen anropas från samma subprocess som huvudregeln
   (`template_komplettering.bpmn`, `[9]`).
8. Micro-frontend + BFF + `remotes.json` (`[13]`).

---

## [8] regel-oul och OUL-integration

### [8.1] Syfte

`rimfrost-framework-regel-oul` ger OUL-integration och korrelationslagring så att en
regelkörning kan avslutas i ett separat anrop (typiskt `POST /done`) långt efter att den
startades 〔rimfrost-framework-regel-oul/README.md〕. Används av manuell och komplettering.

### [8.2] `OulUppgiftService` — publikt API

〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java〕

| Metod | Beteende | Rad |
|---|---|---|
| `void createOulUppgift(OulUppgiftSpec spec) throws OulServiceException` | Skapar OUL-uppgift, uppdaterar handläggning (version+1, ny `Uppgift` med OUL-status), sparar cloud event data, process topic info (`replyTo`) och common data. Fel efter skapande → avslutar uppgiften best-effort, städar och kastar vidare | 116-157 |
| `void tryEndOulUppgift(UUID, String reason)` | Avslutar, loggar och sväljer fel | 171-180 |
| `void endOulUppgift(UUID, String reason) throws OulServiceException` | Avslutar, kastar vid fel | 192-201 |
| `void tryUnassignOulUppgift(UUID)` / `unassignOulUppgift(UUID)` | Tar bort tilldelning | 214-243 |
| `OulCorrelationData getCorrelationData(UUID handlaggningId)` | `oulUppgiftId`, `uppgift`, `replyTopic`, `cloudEventData`; `null` om någon del saknas | 254-270 |
| `void cleanupCorrelation(UUID)` | Best-effort-borttagning av alla tre raderna | 277-282 |
| `void handleOulStatus(OulStatus)` | Statuscallback (se `[8.4]`) | 302-357 |

`OulUppgiftSpec` (bygg med `ImmutableOulUppgiftSpec.builder()`): `handlaggningId`,
`handlaggning`, `replyTo`, `cloudEventData`, `cloudEventAttributes`, `regel`, `beskrivning`,
`verksamhetslogik`, `roll`, `url`, `erbjudande`, `aktivitetId`, `uppgiftSpecifikationId`,
`uppgiftSpecifikationVersion`, `version` (default `"1"`)
〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/entity/OulUppgiftSpec.java:21-77〕.
Normalt bygger ramverket specen åt regeln (`[6.5]`, `[7.5]`); regelutvecklare anropar
`OulUppgiftService` direkt bara för `getCorrelationData` (t.ex. för att få `uppgift`).

### [8.3] OUL-förfrågan och routningsfält

`buildOulRequest` sätter bl.a. `subTopic = kafka.subtopic` och
`processInfo.replyTopic = spec.replyTo` 〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java:359-376〕
〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java:80-81〕.

| Fält | Syfte | Källa |
|---|---|---|
| `sub_topic` | Styr vart OUL publicerar statusnotifieringar: `operativt-uppgiftslager-status-notification.<sub_topic>` | 〔rimfrost/ASYNC_ROUTING.md:64-86〕 |
| `processInfo.reply_topic` | Ursprungligt `replyTo`; ekas tillbaka oförändrat i varje notifiering; används aldrig av OUL för routning | 〔rimfrost/ASYNC_ROUTING.md:82-86〕 |

Därför måste `kafka.subtopic` och statusnotifieringstopicen matcha exakt:

```properties
# KÄLLA: rimfrost/ASYNC_ROUTING.md:115-121
kafka.subtopic=my-regel
mp.messaging.incoming.operativt-uppgiftslager-status-notification.topic=operativt-uppgiftslager-status-notification.my-regel
```

### [8.4] Statusnotifieringar från OUL

- `rimfrost-framework-oul`: `OulKafkaConsumer` lyssnar på kanalen
  `operativt-uppgiftslager-status-notification` (`@Blocking`) och anropar
  `OulHandlerInterface.handleOulStatus(OulStatus)` 〔rimfrost-framework-oul/src/main/java/se/fk/rimfrost/framework/oul/presentation/kafka/OulKafkaConsumer.java:16-17〕
  〔rimfrost-framework-oul/src/main/java/se/fk/rimfrost/framework/oul/logic/OulHandlerInterface.java:5-7〕.
- `OulStatus`: `handlaggningId`, `uppgiftId`, `utforarId?`, `uppgiftStatus`, `processInfo`, `planeradTill?`
  〔rimfrost-framework-oul/src/main/java/se/fk/rimfrost/framework/oul/logic/dto/OulStatus.java:10-26〕.
- `OulUppgiftService` implementerar `OulHandlerInterface`; regeln behöver inte implementera något
  〔rimfrost-framework-regel-oul/README.md〕.
- Beteende: saknas common data → ignoreras tyst (förväntat t.ex. efter done); annars uppgift
  version+1, utförare, planerad tid och status uppdateras i lagringen och i handläggningen (utan
  att stega handläggningens version). Fel → OUL-uppgiften avslutas best-effort, korrelation städas
  och felsvar skickas till `processInfo.replyTopic`
  〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java:284-357〕.

### [8.5] Cancelled-event (avbrott, t.ex. BPMN-timeout)

- regel-oul lyssnar på kanalen `regel-oul-cancelled`, topic `${kafka.cancelled.topic}`
  〔rimfrost-framework-regel-oul/src/main/resources/application.properties:24-27〕
  〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/presentation/kafka/RegelOulCancelledConsumer.java:32-37〕.
- Meddelande: CloudEvent med `data.handlaggningId` (kontrakt `regel.oul.cancelled`)
  〔rimfrost-framework-regel-oul-asyncapi/asyncapi.yaml:10-45〕.
- Beteende 〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/RegelOulCancelledService.java:45-87〕:
  1. Ingen korrelationsdata → ignoreras.
  2. Om en böna implementerar `RegelOulCancelledHandler.handleCancelled(UUID)` anropas den först
     (fel loggas) — valfri krok för regelspecifik städning
     〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/RegelOulCancelledHandler.java:11-22〕.
  3. `tryEndOulUppgift(id, "BPMN_CANCELLED")`.
  4. `cleanupCorrelation`.
  5. **Inget regelsvar skickas.**
- Avsedd producent: processen/subprocessen när BPMN-timern löper ut
  〔rimfrost/sequences/kommunicering/kommunicering_timeout.mmd〕. Ingen subprocess-template eller
  exempel-subprocess publicerar dock cancelled-event idag (`[AVVIKELSE-03]`).

### [8.6] Persistens

- PostgreSQL, Hibernate `validate`, Flyway `migrate-at-start`, schema = `quarkus.flyway.default-schema`
  〔rimfrost-framework-regel-oul/src/main/resources/application.properties:8-18〕.
- Konsumenten måste ange `quarkus.datasource.jdbc.url`, `quarkus.flyway.default-schema`,
  `regel.persistence.table-prefix` 〔rimfrost-framework-regel-oul/src/main/resources/application.properties:8-9〕.
- Tre tabeller per regel: `<prefix>_common_data` (OUL-uppgifts-id + uppgiftsmetadata),
  `<prefix>_cloud_event_data` (CloudEvent-attribut), `<prefix>_process_topic_info` (`replyTo`)
  〔rimfrost-framework-regel-oul/README.md〕.
- Korrekt kolumnuppsättning: entiteterna
  〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/RegelCommonDataEntity.java〕
  〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/CloudEventDataEntity.java〕
  〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/ProcessTopicInfoEntity.java〕
  och test-migreringen 〔rimfrost-framework-regel-oul/src/test/resources/db/migration/oul/V001__framework_tables.sql〕.
  SQL i `[6.8]`.

### [8.7] Kravöversikt (regel-oul)

`FROUL-FR-01` uppgiftshantering, `-02` statusnotifieringar, `-03` persistens av
korrelationstillstånd, `-04` cancelled-event, `FROUL-PR-01` tabeller och namngivning
〔rimfrost-framework-regel-oul/docs/krav.md:12-115〕.

### [8.8] Tester

Test-JAR (`base/`, `helpers/`): `OulUppgiftServiceTestBase`, `WireMockRegelOul`
〔rimfrost-framework-regel-oul/pom.xml:191-197〕
〔rimfrost-framework-regel-oul/src/test/java/se/fk/rimfrost/framework/regel/oul/base/OulUppgiftServiceTestBase.java:34〕
〔rimfrost-framework-regel-oul/src/test/java/se/fk/rimfrost/framework/regel/oul/helpers/WireMockRegelOul.java:26〕.

---

## [9] Regel-subprocess (BPMN)

### [9.1] Syfte

En regel-subprocess är en Kogito-subprocess som används av en flödesprocess och kommunicerar
med en regels mikrotjänst via Kafka 〔rimfrost-template-regel-subprocess/README.md:5-6〕. Den
paketeras som Maven-bibliotek (inget eget körbart system) och läggs som beroende i processen
〔rimfrost-template-regel-subprocess/README.md:46-60〕 〔rimfrost-process-vah/pom.xml〕.

### [9.2] Innehåll i templaten

| Fil | Roll | Källa |
|---|---|---|
| `template.bpmn` | Yttre subprocess: anropar `Komplettering` (calledElement `template_komplettering_id`) och `Regel` (`template_regel_id`) | 〔rimfrost-template-regel-subprocess/src/main/resources/template.bpmn〕 |
| `template_regel.bpmn` | Process-id `template_regel_id`; skickar förfrågan, väntar på svar, timeout, omförsök | 〔rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn〕 |
| `template_komplettering.bpmn` | Samma mönster för kompletteringsregeln | 〔rimfrost-template-regel-subprocess/src/main/resources/template_komplettering.bpmn〕 |
| `application.properties` | Response-topic-properties och Kafka-kanaler | 〔rimfrost-template-regel-subprocess/src/main/resources/application.properties〕 |
| `pom.xml` | `rimfrost-framework-regel-asyncapi:1.1.4`, `rimfrost-framework-process:1.6.4` | 〔rimfrost-template-regel-subprocess/pom.xml〕 |

För en regel utan komplettering räcker en regel-BPMN (rtf-maskinell-subprocess har bara
`rtf_maskinell.bpmn`) 〔rimfrost-regel-rtf-maskinell-subprocess/src/main/resources/rtf_maskinell.bpmn〕.

### [9.3] BPMN-element i regel-BPMN

| Element | Typ | Implementation | Källa |
|---|---|---|---|
| `Application` | startEvent | — | 〔rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn〕 |
| `Init` | serviceTask | `RegelService.init` sätter `attempts=0` | 〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelService.java:61-64〕 |
| `Create Request` | serviceTask | `RegelService.createRegelRequest(handlaggningId, aktivitetId, responseTopicProperty)` | 〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelService.java:30-44〕 |
| (send) | intermediateThrowEvent/message | meddelande `<regel>-requests` | 〔rimfrost-regel-rtf-maskinell-subprocess/src/main/resources/rtf_maskinell.bpmn〕 |
| (vänta) | event-baserad gateway: message `<regel>-responses` eller timer `#{responseTimeout}` | — | 〔rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn〕 |
| `Received Response` | serviceTask | `RegelService.onRegelResponse` → `RegelProcessResult(utfall, error)` | 〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelService.java:24-28〕 |
| gateway | villkor `response.getError() != null` | — | 〔rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn〕 |
| `Increment attempts` | scriptTask; villkor `attempts < maxAttempts` → vänta `#{retryDelay}` och försök igen; `attempts == maxAttempts` → ge upp | — | 〔rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn:364-367〕 |
| `Handle error` | serviceTask | `RegelService.handleError` → `RegelProcessResult(ERROR, error)` | 〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelService.java:46-50〕 |
| `Handle timeout` | serviceTask | `RegelService.handleTimeout` → `ERROR` med felkod `TIMEOUT` | 〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelService.java:52-59〕 |

Processvariabler: `request`, `result`, `handlaggningId`, `response`, `aktivitetId`, `maxAttempts`,
`attempts`, `errorInfo`, `responseTimeout`, `retryDelay`
〔rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn:69-78〕. `Create
Request` har data-inputs `handlaggningId`, `aktivitetId`, `responseTopicProperty`; den sistnämnda
sätts till **property-namnet** (t.ex. `TEMPLATE_RESPONSE_TOPIC_NAME`), inte topic-namnet
〔rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn:196-220〕
〔rimfrost/ASYNC_ROUTING.md:125-148〕.

**Exakt flöde i `template_regel.bpmn`** (extraherat ur sekvensflödena; samma struktur i
`rtf_maskinell.bpmn`, `rtf_manuell_regel.bpmn` och `bekraftabeslut.bpmn`) `[KÄLLA]`
〔rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn:127〕
〔rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn:339-383〕:

```
Application → Init (attempts=0) → Create Request → [XOR-merge] → skicka <regel>-requests
  → [händelsebaserad gateway]
      ├─ svar på <regel>-responses → [XOR]
      │     ├─ response.getError() == null → Received Response (result = utfall) → slut
      │     └─ response.getError() != null → Handle error (result = ERROR + error)
      │            → Increment attempts → [XOR]
      │                 ├─ attempts <  maxAttempts → timer #{retryDelay} → [XOR-merge] (samma förfrågan skickas igen)
      │                 └─ attempts == maxAttempts → slut (result = ERROR)
      └─ timer #{responseTimeout} → Handle timeout (result = ERROR, felkod TIMEOUT) → slut
```

Viktiga konsekvenser:
- Omförsök sker **bara när regeln svarar med fel** (`ERROR`); en timeout avslutar direkt utan
  omförsök.
- Vid omförsök skickas samma förfrågan (`request`) igen; `Create Request` körs inte om.
- `maxAttempts` = totalt antal försök; `maxAttempts=1` betyder inga omförsök.

**Yttre `template.bpmn`** kör först callActivity `Komplettering` och sedan `Regel`. Om
kompletteringens resultat är `ERROR` (`result.getUtfall() == Utfall.ERROR`) körs inte regeln, och
subprocessen avslutas med kompletteringens fel 〔rimfrost-template-regel-subprocess/src/main/resources/template.bpmn:37-136〕
〔rimfrost-regel-rtf-manuell-subprocess/src/main/resources/rtf_manuell.bpmn:133-139〕.

Redigera BPMN med VS Code-pluginen "Apache KIE Kogito Bundle" 〔rimfrost/processer/README.md:53-64〕.

### [9.4] Indata från anropande process

Den anropande processens callActivity skickar in: `handlaggningId`, `aktivitetId` (fast UUID per
anrop i BPMN), `maxAttempts`, `responseTimeout` och `retryDelay` (ISO-8601-perioder) och får
tillbaka `result` (`RegelProcessResult`). Värdena i `rimfrost-process-vah` `[KÄLLA]`:

| callActivity (calledElement) | `aktivitetId` | `maxAttempts` | `responseTimeout` | `retryDelay` | Källa |
|---|---|---|---|---|---|
| Rätt till försäkring maskinell (`rtf_maskinell`) | `d4ab4820-68d9-41e0-abe1-cd8f9865d275` | 3 | `PT20S` | `PT1S` | 〔rimfrost-process-vah/src/main/resources/vah.bpmn:243-312〕 |
| Rätt till försäkring manuell (`rtf_manuell`) | `c58dd666-b3c1-4a30-91b8-76c3495668c6` | 1 | från config `rtf-manuell.response-timeout=PT10M` | från config `rtf-manuell.retry-delay=PT10S` | 〔rimfrost-process-vah/src/main/resources/vah.bpmn:315-381〕 〔rimfrost-process-vah/src/main/resources/vah.bpmn:318-319〕 〔rimfrost-process-vah/src/main/resources/application.properties〕 |
| Bekräfta beslut (`bekraftabeslut`, manuell regel) | `8cde2355-aea5-4951-916f-08319b2f1e99` | 3 | `PT3M` | `PT1M` | 〔rimfrost-process-vah/src/main/resources/vah.bpmn:382-440〕 |

Viktigt `[HÄRLETT]`: För manuella regler måste `responseTimeout` vara längre än den tid en
handläggare behöver. Exemplets värden (10 respektive 3 minuter) är demovärden. När timern löper
ut får processen `ERROR`/`TIMEOUT` medan OUL-uppgiften lever kvar om inget cancelled-event skickas
(`[AVVIKELSE-03]`).

### [9.5] Konfiguration (subprocess)

```properties
# KÄLLA: rimfrost-template-regel-subprocess/src/main/resources/application.properties:25-46 (förkortat)
TEMPLATE_RESPONSE_TOPIC_NAME=${PROCESS_ID}-template-regel-responses
TEMPLATE_KOMPLETTERING_RESPONSE_TOPIC_NAME=${PROCESS_ID}-template-regel-komplettering-responses

mp.messaging.incoming.template-regel-responses.connector=smallrye-kafka
mp.messaging.incoming.template-regel-responses.auto.offset.reset=earliest
mp.messaging.incoming.template-regel-responses.group.id=template-regel-responses-consumer
mp.messaging.incoming.template-regel-responses.topic=${TEMPLATE_RESPONSE_TOPIC_NAME}
mp.messaging.incoming.template-regel-responses.value.deserializer=org.apache.kafka.common.serialization.StringDeserializer

mp.messaging.outgoing.template-regel-requests.connector=smallrye-kafka
mp.messaging.outgoing.template-regel-requests.value.serializer=org.apache.kafka.common.serialization.StringSerializer
```

- `PROCESS_ID` definieras i den **anropande processen** (t.ex. `PROCESS_ID=vah`), så svarstopic
  blir processpecifik, t.ex. `vah-rtf-manuell-responses` 〔rimfrost-process-vah/src/main/resources/application.properties〕
  〔rimfrost/ASYNC_ROUTING.md:53-57〕.
- Utgående kanal (`<regel>-requests`) har ingen explicit `topic`; SmallRye använder då
  kanalnamnet som topic. Kanalnamnet måste alltså vara lika med regelns
  `mp.messaging.incoming.regel-requests.topic` `[HÄRLETT]`
  〔rimfrost-regel-rtf-manuell-subprocess/src/main/resources/application.properties〕
  〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:26〕.
- Meddelandenamnen i BPMN (message-element) måste vara lika med kanalnamnen
  〔rimfrost-template-regel-subprocess/README.md:38-39〕.

### [9.6] Checklista — ny subprocess (15 steg från templaten)

〔rimfrost-template-regel-subprocess/README.md:30-44〕:

1. Uppdatera inkommande Kafka-topics i `application.properties`.
2. Uppdatera utgående Kafka-topics.
3. Byt `-template-regel-responses` i `TEMPLATE_RESPONSE_TOPIC_NAME`.
4. Byt `-template-regel-komplettering-responses` i `TEMPLATE_KOMPLETTERING_RESPONSE_TOPIC_NAME`.
5. Byt namn på dessa två properties till något unikt.
6. Uppdatera process-ID i `template_regel.bpmn`.
7. Uppdatera process-ID i `template_komplettering.bpmn`.
8. Uppdatera "Called element" i `template.bpmn` till ID:n från 6 och 7.
9. Uppdatera meddelanden i `template_regel.bpmn` till topics enligt `application.properties`.
10. Samma för `template_komplettering.bpmn`.
11. Sätt `responseTopicProperty` i "Create Request" (regel) till namnet från steg 5.
12. Samma för kompletteringen.
13. Byt namn på `template_regel.bpmn`.
14. Byt namn på `template_komplettering.bpmn`.
15. Uppdatera `artifactId` i `pom.xml`.

Exempel på färdig implementation: `rimfrost-regel-rtf-maskinell-subprocess` (bara regel) och
`rimfrost-regel-rtf-manuell-subprocess` (regel + komplettering, process-id:n `rtf_manuell`,
`rtf_manuell_regel`, `rtf_manuell_komplettering`) 〔rimfrost-regel-rtf-manuell-subprocess/src/main/resources/rtf_manuell.bpmn〕
〔rimfrost-regel-rtf-manuell-subprocess/src/main/resources/rtf_manuell_regel.bpmn〕
〔rimfrost-regel-rtf-manuell-subprocess/src/main/resources/rtf_manuell_komplettering.bpmn〕.

---

## [10] Process (toppnivå)

### [10.1] Syfte

En flödesprocess är en Kogito-process som producerar ett resultat baserat på resultat från en
eller flera underprocesser (flödesprocesser eller regel-subprocesser)
〔rimfrost-template-process/README.md:1-7〕.

### [10.2] Template och filer

| Fil | Roll | Källa |
|---|---|---|
| `pom.xml` | Kogito/jBPM-beroenden + `rimfrost-framework-process:1.6.4` + subprocess-beroenden (template: `rimfrost-template-regel-subprocess:1.1.3`) | 〔rimfrost-template-process/pom.xml〕 |
| `src/main/resources/application.properties` | `PROCESS_ID`, Kafka-kanaler, Kogito-persistens | 〔rimfrost-template-process/src/main/resources/application.properties〕 |
| `src/main/resources/template.bpmn` | Processen | 〔rimfrost-template-process/src/main/resources/template.bpmn〕 |
| `src/test/.../TemplateSmokeTest.java` | Röktest | 〔rimfrost-template-process/src/test/java/se/fk/github/rimfrost/process/template/TemplateSmokeTest.java〕 |

Konfiguration 〔rimfrost-template-process/src/main/resources/application.properties:31-51〕:

```properties
# KÄLLA: rimfrost-template-process/src/main/resources/application.properties (förkortat)
PROCESS_ID=template
mp.messaging.incoming.template-handlaggning-requests.connector=smallrye-kafka
mp.messaging.incoming.template-handlaggning-requests.auto.offset.reset=earliest
mp.messaging.incoming.template-handlaggning-requests.value.deserializer=org.apache.kafka.common.serialization.StringDeserializer
mp.messaging.outgoing.template-handlaggning-responses.connector=smallrye-kafka
mp.messaging.outgoing.template-handlaggning-responses.value.serializer=org.apache.kafka.common.serialization.StringSerializer
kogito.messaging.as-cloudevents=true
kogito.persistence.type=jdbc
kogito.persistence.proto.marshaller=false
kie.flyway.enabled=true
quarkus.datasource.db-kind=postgresql
%prod.quarkus.datasource.jdbc.url=${DB_URL}
```

Processinstanser persisteras i PostgreSQL med Kogitos JDBC-persistens; i dev/test startar Dev
Services en databas 〔rimfrost-template-process/README.md:24-48〕.

Steg enligt `processer/README.md` 〔rimfrost/processer/README.md:27-66〕:
1. Nytt repo från templaten; ersätt "Template".
2. Kafka-kanaler (incoming/outgoing) i `application.properties`.
3. Lägg till subprocess-beroenden i `pom.xml`.
4. I BPMN: Start Message = incoming-kanalen, End Message = outgoing-kanalen, callActivity
   "Called Element" = subprocessens process-id.
5. Röktest.

### [10.3] Exempel: `rimfrost-process-vah`

- Beroenden: `rimfrost-regel-rtf-manuell-subprocess:1.1.8`, `rimfrost-regel-rtf-maskinell-subprocess:1.1.7`,
  `rimfrost-regel-bekraftabeslut-subprocess:1.1.7`, `rimfrost-framework-process:1.6.4`
  〔rimfrost-process-vah/pom.xml〕.
- callActivities: `rtf_maskinell`, `rtf_manuell`, `bekraftabeslut`
  〔rimfrost-process-vah/src/main/resources/vah.bpmn〕.
- `PROCESS_ID=vah`; incoming `vah-handlaggning-requests`; outgoing `handlaggning-responses`
  〔rimfrost-process-vah/src/main/resources/application.properties〕.
- `aktivitetId`, `maxAttempts`, `responseTimeout`, `retryDelay` per callActivity: se tabellen i `[9.4]`.
- Flöde (extraherat ur sekvensflödena) `[KÄLLA]` 〔rimfrost-process-vah/src/main/resources/vah.bpmn:458-474〕:

```
start (vah-handlaggning-requests) → "Start vård av husdjur process" (ProcessService.startProcess)
  → rtf_maskinell → [XOR]
       ├─ ERROR      → "Handle error" → "Avsluta process med error" (endProcessWithError) → slut
       ├─ UTREDNING  → rtf_manuell (med komplettering) → [XOR] ERROR → felavslut, annars ↓
       └─ JA / NEJ   → ↓
  → bekraftabeslut → [XOR] ERROR → felavslut, annars "Informera om beslut" (endProcess) → slut (handlaggning-responses)
```

  Den manuella regeln körs alltså bara när den maskinella regeln svarar `UTREDNING`.

### [10.4] `ProcessService` och `RegelProcessResult`

- `ProcessService.startProcess`, `endProcess(handlaggningId, RegelProcessResult)` →
  `resultat = "GODKÄND"` om utfall `JA`, annars `"EJ GODKÄND"`; `endProcessWithError` →
  `resultat = "FEL"` med `error` 〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/ProcessService.java:20-59〕
  〔rimfrost-framework-process/README.md〕.
- `RegelProcessResult(Utfall utfall, RegelErrorInformation error?)` är `Serializable` (lagras som
  processvariabel) 〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelProcessResult.java:10-35〕.

### [10.5] Workflow-scenariot (`rimfrost-service-workflow`)

1. Extern anropare: `POST /yrkande` med `replyTo`.
2. Workflow lagrar `replyTo` i tabellen `handlaggning_reply_topic` per `handlaggningId`.
3. Workflow slår upp processens topic via erbjudande-topic-tjänsten och skickar CloudEvent med
   bara `handlaggningId`.
4. Processen svarar på fasta topicen `handlaggning-responses`.
5. Workflow slår upp `replyTo` och routar svaret. Omstart: `POST /handlaggning/{id}/process`
   med nytt `replyTo`.

Källa: 〔rimfrost/ASYNC_ROUTING.md:150-168〕. (Workflow-repot är inte klonat.)


---

## [11] OpenAPI-/AsyncAPI-repon

### [11.1] Ramverkskontrakt

| Repo | Innehåll | Genererat Java-paket (`[HÄRLETT]` från importer) | Källa |
|---|---|---|---|
| `rimfrost-framework-regel-asyncapi` | `RegelRequestMessage`, `RegelResponseMessage`, `Utfall`, `RegelErrorInformation` | `se.fk.rimfrost.framework.regel` | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml〕 〔rimfrost-framework-regel-asyncapi/src/main/java/se/fk/rimfrost/framework/regel/Utfall.java〕 |
| `rimfrost-framework-regel-oul-asyncapi` | `RegelOulCancelledMessage` på `regel.oul.cancelled` | — | 〔rimfrost-framework-regel-oul-asyncapi/asyncapi.yaml:10-45〕 |
| `rimfrost-framework-regel-oul-openapi` | `GET /utokadUppgiftsbeskrivning`, `POST /{handlaggningId}/done` → `RegelOulControllerApi`, `GetUtokadUppgiftsbeskrivningResponse` | `se.fk.rimfrost.framework.regel.oul.jaxrsspec.controllers.generatedsource` | 〔rimfrost-framework-regel-oul-openapi/openapi.yaml:13-41〕 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/RegelManuellController.java〕 |
| `rimfrost-framework-regel-manuell-openapi` | Samma endpoints; **ersatt** av regel-oul-openapi | — | 〔rimfrost-framework-regel-manuell-openapi/README.md〕 |

### [11.2] Regelns eget OpenAPI-repo (manuell/komplettering)

Templates: `rimfrost-template-regel-manuell-openapi` och `rimfrost-template-regel-komplettering-openapi`
〔rimfrost-template-regel-manuell-openapi/README.md〕 〔rimfrost-template-regel-komplettering-openapi/README.md〕.

Struktur (Gradle, multi-projekt):

| Fil | Innehåll | Källa |
|---|---|---|
| `openapi.yaml` | OpenAPI 3.1.0; endast regelns egna endpoints | 〔rimfrost-template-regel-manuell-openapi/openapi.yaml〕 |
| `gradle.properties` | `group`, `version`, `description` | 〔rimfrost-template-regel-manuell-openapi/gradle.properties〕 |
| `settings.gradle` | Två delprojekt: `<repo>-jaxrs-spec` och `<repo>-spec` | 〔rimfrost-template-regel-manuell-openapi/settings.gradle〕 |
| `build.gradle` | `fkOpenApi`-plugin: `-spec` genererar `openapi-yaml`; `-jaxrs-spec` genererar JAX-RS-gränssnitt/modeller (`jaxrs-spec`) med `compileOnly`-beroenden på jakarta/jackson | 〔rimfrost-template-regel-manuell-openapi/build.gradle:28-48〕 |

Manuell template-spec: `GET /{handlaggningId}` (operationId `getData`, svar `GetDataResponse`) och
`PATCH /{handlaggningId}` (operationId `updateData`, body `PatchDataRequest`, 204)
〔rimfrost-template-regel-manuell-openapi/openapi.yaml:8-47〕. Komplettering:
`getKomplettering`/`patchKomplettering` med samma schema för båda
〔rimfrost-template-regel-komplettering-openapi/openapi.yaml:8-35〕.

`POST /done` och `GET /utokadUppgiftsbeskrivning` ska **inte** definieras här — de kommer från
ramverkskontraktet 〔rimfrost-regel-rtf-manuell/docs/teknisk-spec.md〕.

Genererade klasser hamnar i `<group>.jaxrsspec.controllers.generatedsource.model` `[HÄRLETT]`:
template `se.fk.rimfrost.template.regel.manuell.openapi.jaxrsspec.controllers.generatedsource.model.GetDataResponse`
〔rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java〕;
rtf-manuell (group `se.fk.rimfrost.regel.rtf.manuell`) →
`se.fk.rimfrost.regel.rtf.manuell.jaxrsspec.controllers.generatedsource.model.Beslutsutfall`
〔rimfrost-regel-rtf-manuell-openapi/gradle.properties〕
〔rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/logic/RtfService.java:184〕.

I regelns `application.properties` måste `-jaxrs-spec`-artefakten indexeras:
`quarkus.index-dependency.<namn>.group-id/artifact-id` 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:7-9〕.

Om controllern implementerar ett genererat `…ControllerApi` med extra endpoints (som bekräfta
beslut) måste även de endpointsen finnas i specen 〔rimfrost-regel-bekraftabeslut/src/main/java/se/fk/github/bekraftabeslut/presentation/rest/BekraftaBeslutController.java〕.

### [11.3] Regelns AsyncAPI-repo

`rimfrost-regel-rtf-manuell-asyncapi` finns men regelns Kafka-meddelanden följer ramverkets
generiska kontrakt `[HÄRLETT]` 〔rimfrost-regel-rtf-manuell-asyncapi/README.md〕. Ingen template
för regel-asyncapi finns.

---

## [12] Referensdata-erbjudande

- Gränssnitt: `String getErbjudandeNamn(String id)`
  〔rimfrost-framework-referensdata-interface/src/main/java/se/fk/rimfrost/framework/referensdata/ErbjudandeReferensdataInterface.java:3-6〕.
- Används av manuell och komplettering vid skapande av OUL-uppgift (erbjudandets namn)
  〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java〕
  〔rimfrost-framework-regel-manuell/docs/krav.md:14-15〕.
- **Template-impl.** (`rimfrost-template-referensdata-erbjudande`) returnerar alltid `"Erbjudande"`
  (stub med TODO) 〔rimfrost-template-referensdata-erbjudande/src/main/java/se/fk/rimfrost/referensdata/TemplateErbjudandeReferensdata.java:6-16〕.
  Används som default i template-regler 〔rimfrost-template-regel-manuell/pom.xml〕.
- **Riktig impl.** (`rimfrost-referensdata-erbjudande`): `ReferensdataAdapter.getErbjudande(id).namn()`;
  `ReferensdataException` → `RuntimeException`; indexerar `rimfrost-adapter-referensdata`; kräver
  `referensdata.api.base-url` 〔rimfrost-referensdata-erbjudande/src/main/java/se/fk/rimfrost/referensdata/erbjudande/ErbjudandeReferensdata.java:9-28〕
  〔rimfrost-referensdata-erbjudande/src/main/resources/application.properties〕
  〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:43〕.
- Exakt en böna måste implementera gränssnittet på classpath `[HÄRLETT]` (CDI-injektion).
- Egen impl.: utgå från templaten, byt klassnamn, `groupId`/`artifactId`, implementera TODO
  〔rimfrost-template-referensdata-erbjudande/README.md〕.

---

## [13] Frontend: micro-frontend, BFF och portal

### [13.1] Arkitektur

- Portalen (`rimfrost-portal-handlaggare`, Vue) är skal (host); varje manuell regel har en
  micro-frontend (remote) och en egen BFF 〔rimfrost-template-micro-fe/README.md:40-77〕.
- Flöde: portalen hämtar uppgifter (via portal-bff/OUL) → handläggaren väljer uppgift → portalen
  hämtar registret `GET /api/route-manifest` från portal-bff → laddar remoten via Module
  Federation och skickar props `handlaggningId` (och ev. `regeltyp`) → MFE anropar sin BFF → BFF
  anropar regelns backend → MFE skickar `task-done`-event → portalen visar toast och laddar om
  〔rimfrost-template-micro-fe/README.md:79-86〕 〔rimfrost/sequences/portal_micro_fe.mmd〕.
- Varför egen BFF per MFE: isolering, rätt storlek på API, oberoende driftsättning
  〔rimfrost-template-micro-fe/README.md:102-107〕.

### [13.2] Micro-frontend (`rimfrost-template-micro-fe`)

| Aspekt | Detalj | Källa |
|---|---|---|
| Stack | Vue 3, Vite, `@module-federation/vite`, FKUI (`@fkui/vue`), Pinia, Vitest | 〔rimfrost-template-micro-fe/vite.config.ts:1-5〕 〔rimfrost-template-micro-fe/README.md:467-506〕 |
| Federation | `name` (unikt = `scope` i `remotes.json`), `filename: remoteEntry.js`, `exposes`, `manifest: true`, `publicPath: "auto"`, `shared` (vue, @fkui/vue, pinia som singletons) | 〔rimfrost-template-micro-fe/vite.config.ts:9-24〕 |
| Props | `handlaggningId: string`, `regeltyp?: string` | 〔rimfrost-template-micro-fe/README.md:232-240〕 |
| Slutför | `window.dispatchEvent(new CustomEvent("task-done", { detail: { handlaggningId, success: true, message } }))` | 〔rimfrost/frontend/micro-fe/README.md〕 |
| BFF-URL | `src/config/env.ts`: `window.__<APP>_ENV__.RUNTIME_BFF_URL` → `VITE_BFF_URL` → `""`; `ensureEnvLoaded()` laddar `runtime-config.js` från eget ursprung (`new URL(import.meta.url).origin`) | 〔rimfrost-template-micro-fe/src/config/env.ts:1-60〕 |
| Global namnrymd | per app, t.ex. `__TEMPLATE_MICRO_FE_ENV__`, `__RTF_MANUELL_FE_ENV__`, `__BEKRAFTABESLUT_FE_ENV__`; aldrig delad `window._env_` | 〔rimfrost-template-micro-fe/public/runtime-config.js〕 〔rimfrost/frontend/deployment/README.md:104-118〕 |
| Lokala portar | dev 3038, preview 3039, dev-proxy `/api` → `http://localhost:9009` | 〔rimfrost-template-micro-fe/vite.config.ts:33-54〕 〔rimfrost-template-micro-fe/.env.example〕 |
| Storlek | Komponenten ska rymmas i ca `calc(100vw - 260px)` × `calc(100vh - 64px)` | 〔rimfrost-template-micro-fe/README.md:565-569〕 |
| StegIndikator | Komponent för processsteg (`steg`, `aktivtSteg` 0-baserat, `etikett`, `orientering`) | 〔rimfrost-template-micro-fe/README.md:389-465〕 |
| Container | Apache httpd, port 8080; `runtime-config.js` monteras på `/usr/local/apache2/htdocs/runtime-config.js` | 〔rimfrost-template-micro-fe/README.md:748-791〕 〔rimfrost/frontend/deployment/README.md:23-48〕 |

Exempel-MFE:er: `remoteApp`/`./VardAvHusdjur` (rtf-manuell), `remoteKompletteringApp`/`./RtfKomplettering`
(komplettering), `bekraftaBeslutApp`/`./BekraftaBeslut` 〔rimfrost-regel-rtf-manuell-fe/vite.config.ts〕
〔rimfrost-regel-rtf-manuell-komplettering-fe/vite.config.ts〕 〔rimfrost-regel-bekraftabeslut-fe/vite.config.ts〕.

### [13.3] BFF (`rimfrost-template-micro-fe-bff`)

- Java/Quarkus, synkron REST, ingen lagring, ingen Kafka 〔rimfrost-template-micro-fe-bff/docs/teknisk-spec.md:5-7〕.
- Endpoints (template): `GET /api/health`, `POST /api/task` (läs; body `{handlaggningId}`),
  `PATCH /api/task`, `POST /api/task/done` (vidarebefordrar `Authorization`),
  `GET /api/uppgiftsbeskrivning` 〔rimfrost-template-micro-fe-bff/src/main/java/se/fk/github/templatebff/TemplateBFFController.java:24-81〕.
- Backend-klient (`@RegisterRestClient(configKey="backend")`): `GET /{id}`, `PATCH /{id}`,
  `POST /{id}/done`, `GET /utokadUppgiftsbeskrivning` — alltså regelns ramverksendpoints
  〔rimfrost-template-micro-fe-bff/src/main/java/se/fk/github/templatebff/integration/BackendClient.java:14-29〕.
- Konfiguration 〔rimfrost-template-micro-fe-bff/src/main/resources/application.properties:1-20〕:

| Property / env | Default | Betydelse |
|---|---|---|
| `quarkus.http.port` | `9009` | Port |
| `quarkus.rest-client.backend.url` / env `BACKEND_URL` | `http://localhost:8080/regel/bekraftabeslut` | Full bas-URL till regelns controller-path (utan avslutande `/`) |
| `quarkus.http.cors.origins` / env `CORS_ORIGINS` | `http://localhost:3000,http://localhost:3030` | Måste innehålla **portalens** origin |
| `quarkus.http.cors.access-control-allow-credentials` / env `CORS_ALLOW_CREDENTIALS` | `false` | Cookies cross-origin |

- Exempel-BFF:ernas API mot MFE skiljer sig (rtf-manuell-bff: `/api/task/{id}`,
  `/api/{id}/patchErsattningar` …; komplettering-bff: `/api/{id}/komplettering` …; bekräfta beslut:
  `/api/regel/bekraftabeslut/...`) — det finns ingen gemensam standard i praktiken `[KÄLLA]`
  〔rimfrost-regel-rtf-manuell-bff/src/main/resources/application.properties〕
  〔rimfrost-regel-rtf-manuell-komplettering-bff/src/main/java/se/fk/github/rtfmanuellkompletteringbff/RtfManuellKompletteringBffController.java:25-48〕
  〔rimfrost-regel-bekraftabeslut-bff/src/main/resources/application.properties〕.
- `rimfrost-framework-bff` (valfritt bibliotek): `GlobalExceptionMapper`, `UpstreamHealthCheck`
  (registreras per tjänst via `@Produces @Readiness`), automatisk vidarebefordran av
  `Authorization`-header, test-JAR med WireMock-hjälpare 〔rimfrost-framework-bff/README.md〕.

### [13.4] Registrering i portalen (`remotes.json`)

```json
// KÄLLA: rimfrost-portal-bff/src/main/resources/remotes.json (utdrag)
{ "routes": {
    "rtf-manuell":    { "scope": "remoteApp",         "module": "VardAvHusdjur",   "devEntry": "http://localhost:3031/mf-manifest.json", "prodEntry": "https://cdn.example.com/rtf-manuell/mf-manifest.json" },
    "bekraftabeslut": { "scope": "bekraftaBeslutApp", "module": "BekraftaBeslut",  "devEntry": "http://localhost:3033/mf-manifest.json", "prodEntry": "https://cdn.example.com/bekraftabeslut/mf-manifest.json" },
    "remoteExample":  { "scope": "remoteExample",     "module": "ExampleComponent", "devEntry": "http://localhost:3039/mf-manifest.json", "prodEntry": "https://cdn.example.com/template/mf-manifest.json" } } }
```

- `scope` = federationens `name`; `module` = exponerad komponent utan `./`; `devEntry`/`prodEntry`
  = URL till `mf-manifest.json` 〔rimfrost/frontend/micro-fe/README.md〕.
- Portal-bff serverar filen på `GET /api/route-manifest`; override via env
  `PORTAL_REMOTES_CONFIG_PATH` (monterad ConfigMap) 〔rimfrost-portal-bff/README.md〕
  〔rimfrost-portal-bff/src/main/java/se/fk/github/portalbff/PortalBffController.java:65-72〕.
- Den inbyggda filens `prodEntry` är platshållare 〔rimfrost/frontend/deployment/README.md:120-126〕.
- Portalen väljer remote utifrån uppgiftens `url` (= `uppgift.path` i regelns `config.yaml`)
  〔rimfrost/frontend/micro-fe/README.md〕. Hur `/regel/rtf-manuell` matchas mot nyckeln
  `rtf-manuell` görs i `rimfrost-portal-handlaggare` (ej klonat) `[EJ VERIFIERAT]`.
- Kompletterings-MFE:n saknas i den inbyggda `remotes.json` `[KÄLLA]` 〔rimfrost-portal-bff/src/main/resources/remotes.json〕.

### [13.5] Driftsättning och fallgropar

- **CORS:** `Origin` är alltid portalens origin; BFF:ens `CORS_ORIGINS` måste innehålla den
  〔rimfrost/frontend/deployment/quick-guide.md:94-96〕. Micro-fe-templatens lokala `Dockerfile`
  aktiverar `mod_headers` och sätter `Header set Access-Control-Allow-Origin "*"`
  〔rimfrost-template-micro-fe/Dockerfile:45-48〕 — tvärtemot `rimfrost/frontend/micro-fe/README.md`
  (`[AVVIKELSE-24]`). Dockerfilen anger att den speglar den Dockerfile som CI-workflowet genererar;
  att CI-imagen har samma header är `[EJ VERIFIERAT]`.
- **runtime-config för remotes:** `index.html` körs aldrig när appen laddas som remote; `env.ts`
  måste själv hämta `runtime-config.js` (fixat i templaten) — symptom annars: API-anrop 404:ar mot
  portalens adress 〔rimfrost/frontend/deployment/README.md:110-118〕.
- **Lokal-dev-värden** i `rimfrost-kubernetes` (localhost-portar, ingress-annoteringar) ska inte
  kopieras till andra miljöer 〔rimfrost/frontend/deployment/README.md:142-150〕.
- Lokal hel miljö: `rimfrost-kubernetes` (`./deploy.sh`, `./port-forward.sh`, `./populate_oul.sh`)
  〔rimfrost/frontend/deployment/quick-guide.md:18-21〕.
- Images publiceras till `ghcr.io/forsakringskassan/*`; BFF-portar: portal-bff 9001, rtf-manuell-bff
  9002, bekraftabeslut-bff 9003, template-bff 9009 〔rimfrost/frontend/deployment/README.md:23-36〕;
  komplettering-bff 9004 〔rimfrost-regel-rtf-manuell-komplettering-bff/src/main/resources/application.properties〕.

---

## [14] Felhantering och ansvarsfördelning

### [14.1] Översikt

| Situation | Maskinell | Manuell / komplettering (Kafka-steget) | Manuell / komplettering (REST-steget) |
|---|---|---|---|
| Läsning handläggning misslyckas | Omförsök; sedan `ERROR`/`RIMFROST_HANDLAGGNING_READ_FAILURE` | Inga omförsök; `ERROR`/`RIMFROST_HANDLAGGNING_READ_FAILURE` | HTTP 404/400/503/500 |
| Skrivning misslyckas | Omförsök; sedan `RIMFROST_HANDLAGGNING_WRITE_FAILURE` | OUL-uppgift avslutas, korrelation städas, felsvar | HTTP-fel (PATCH) |
| Versionskonflikt | `RIMFROST_HANDLAGGNING_WRITE_FAILURE` utan omförsök | som skrivfel | HTTP 500 |
| Regeln kastar undantag | `ERROR`/`RIMFROST_OTHER` | `ERROR`/`RIMFROST_OTHER` | HTTP 500 (catch-all) |
| Affärsfel | `RegelMaskinellErrorResult` med egen felkod | — | `RegelManuellException(status)` / 422 (komplettering) |
| Externa tjänster (inte handläggning) | Regeln ansvarar (RetryUtil) | Regeln ansvarar | Regeln ansvarar |
| Sändning av svar misslyckas | Loggas | Loggas | Loggas |

Källor: 〔rimfrost/FAULT_HANDLING_OVERVIEW.md:13-90〕
〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/RegelMaskinellRequestHandler.java:40-154〕
〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:96-135〕
〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java:116-157〕.

### [14.2] Principer

- Ramverket garanterar att ett regelsvar alltid skickas för varje förfrågan (utfall eller fel)
  〔rimfrost-framework-regel-manuell/docs/krav.md:126-129〕 — med undantag för fall där
  CloudEvent-data inte kan byggas (`[AVVIKELSE-15]`) och där `replyTo` saknas.
- Den sista fångsten av undantag är ett skyddsnät, inte en ersättning för egen felhantering
  〔rimfrost/FAULT_HANDLING_OVERVIEW.md:28-31〕.
- Manuell done-flöde: svar skickas före städning, så fel i städning/handläggningsuppdatering
  hindrar inte svaret 〔rimfrost/FAULT_HANDLING_OVERVIEW.md:83-86〕.
- Fel efter att OUL-uppgift skapats → uppgiften avslutas för att inte lämna en föräldralös uppgift i
  handläggarens inkorg 〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java:106-110〕.
- 404 vid avslut av OUL-uppgift räknas som lyckat 〔rimfrost-framework-regel-manuell/docs/krav.md:68〕.

---

## [15] Konfigurationsreferens (samlad)

`R` = krävs, `V` = valfri, `—` = gäller inte. Detaljer och källor i respektive avsnitt.

| Property | Maskinell | Manuell | Komplettering | Process | Subprocess | Detaljer |
|---|---|---|---|---|---|---|
| `kafka.source` | R | R | R | — | — | `[4.8]` |
| `mp.messaging.incoming.regel-requests.topic` | R | R | R | — | — | `[4.4]` |
| `mp.messaging.outgoing.regel-responses.topic` | R | R | R | — | — | `[4.5]` |
| `quarkus.container-image.name` | R | R | R | R | — | `[5.8]` |
| `quarkus.index-dependency.*` (ramverk, referensdata, openapi, adaptrar) | R | R | R | R | — | `[1.7]` |
| `application.config.path` / env `REGEL_CONFIG_PATH` | V | V | V | — | — | `[4.6]` |
| `handlaggning.api.base-url` | V (default) | V | V | — | — | `[3.2]` |
| `rimfrost.framework.regel.maskinell.retry.intervals` | V | — | — | — | — | `[5.6]` |
| `kafka.subtopic` | — | R | R | — | — | `[8.3]` |
| `mp.messaging.incoming.operativt-uppgiftslager-status-notification.topic` | — | R | R | — | — | `[8.3]` |
| `kafka.cancelled.topic` | — | R | R | — | — | `[8.5]` |
| `quarkus.flyway.default-schema` | — | R | R | — | — | `[6.8]` |
| `regel.persistence.table-prefix` | — | R | R | — | — | `[6.8]` |
| `quarkus.datasource.*` (`DB_URL`, `DB_USERNAME`, `DB_PASSWORD` i prod) | — | R | R | R | — | `[6.7]`, `[10.2]` |
| `oul.api.base-url` | — | V (default) | V | — | — | `[2.3]` |
| `sid.api.base-url` | — | R | V (default) | — | — | `[6.6]` |
| `referensdata.api.base-url` | — | R* | R* | — | — | `[12]` (*med riktig impl.) |
| `rimfrost.framework.regel.{manuell,komplettering}.rest.exception.catchall.response.message` | — | V | V | — | — | `[6.9]`, `[7.5]` |
| `PROCESS_ID` | — | — | — | R | (används) | `[9.5]` |
| `<NAMN>_RESPONSE_TOPIC_NAME` | — | — | — | — | R | `[9.5]` |
| `mp.messaging.incoming.<regel>-responses.*` / `outgoing.<regel>-requests.*` | — | — | — | — | R | `[9.5]` |
| `kogito.persistence.*`, `kie.flyway.enabled` | — | — | — | R | — | `[10.2]` |
| `regel.manuell.base-path` (endast test) | — | R (test) | — | — | — | `[6.11]` |

---

## [16] Kafka-topicreferens

### [16.1] Namnkonventioner

| Topic | Producent | Konsument | Exempel | Källa |
|---|---|---|---|---|
| `<process>-handlaggning-requests` | workflow | process | `vah-handlaggning-requests` | 〔rimfrost-process-vah/src/main/resources/application.properties〕 |
| `handlaggning-responses` | process | workflow | fast namn | 〔rimfrost/ASYNC_ROUTING.md:160-162〕 |
| `<regel>-requests` | subprocess (kanalnamn = topic) | regel (`regel-requests`-kanalen) | `rtf-manuell-requests` | 〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:26〕 |
| `${PROCESS_ID}-<regel>-responses` (= `replyTo`) | regel (dynamiskt) | subprocess | `vah-rtf-manuell-responses` | 〔rimfrost/ASYNC_ROUTING.md:53-57〕 |
| `<regel>-responses` | — (värde i `outgoing.regel-responses.topic`, används som CloudEvent `type`) | — | `rtf-manuell-responses` | `[4.5]` |
| `operativt-uppgiftslager-status-notification.<subtopic>` | OUL | regel (manuell/komplettering) | `…status-notification.rtf-manuell` | 〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:27〕 |
| `<regel>-cancelled` (`kafka.cancelled.topic`) | (process/subprocess — saknas idag) | regel-oul | `rtf-manuell-cancelled` | 〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:22〕 |

### [16.2] Matchningsregler (måste stämma)

1. Subprocessens utgående kanalnamn `<regel>-requests` == regelns
   `mp.messaging.incoming.regel-requests.topic` `[HÄRLETT]`.
2. BPMN-meddelandenamnen == kanalnamnen i subprocessens `application.properties`
   〔rimfrost-template-regel-subprocess/README.md:38-39〕.
3. `responseTopicProperty` i BPMN == namnet på property som innehåller svarstopicen
   〔rimfrost/ASYNC_ROUTING.md:125-148〕.
4. Subprocessens inkommande kanal `<regel>-responses` har `topic=${<NAMN>_RESPONSE_TOPIC_NAME}`
   〔rimfrost-template-regel-subprocess/src/main/resources/application.properties:29-33〕.
5. `kafka.subtopic` == suffix i `operativt-uppgiftslager-status-notification.<…>.topic`
   〔rimfrost/ASYNC_ROUTING.md:109-121〕.
6. Processens Start/End Message == processens incoming/outgoing-kanaler
   〔rimfrost/processer/README.md:53-64〕.

---

## [17] Steg-för-steg-recept

### [17.1] Beroendeordning mellan repon (bygg/publicera i denna ordning)

```
1. rimfrost-regel-<regel>-openapi            (endast manuell/komplettering)
2. rimfrost-regel-<regel>                     (regeltjänsten; beror på 1)
3. rimfrost-regel-<regel>-subprocess          (BPMN-bibliotek; behöver bara topicnamn)
4. rimfrost-process-<process>                 (lägg till 3 som beroende + callActivity)
5. rimfrost-regel-<regel>-bff                 (manuell/komplettering; pekar på 2)
6. rimfrost-regel-<regel>-fe                  (pekar på 5)
7. remotes.json i rimfrost-portal-bff / ConfigMap (pekar på 6)
8. Driftsättning (rimfrost-kubernetes eller motsvarande)
```

`[HÄRLETT]` från beroenden i pom-filer och konfiguration 〔rimfrost-template-regel-manuell/pom.xml〕
〔rimfrost-process-vah/pom.xml〕 〔rimfrost-template-micro-fe-bff/src/main/resources/application.properties〕.

### [17.2] Recept — ny manuell regel "exempel" (namn `x`)

| # | Repo | Åtgärd | Avsnitt |
|---|---|---|---|
| 1 | `rimfrost-regel-x-openapi` | Från `rimfrost-template-regel-manuell-openapi`; `group=se.fk.rimfrost.regel.x`; definiera `GetDataResponse`/`PatchDataRequest` | `[11.2]` |
| 2 | `rimfrost-regel-x` | Från `rimfrost-template-regel-manuell`; service, middleware-impl, controller `@Path("/regel/x")` | `[6.3]` |
| 3 | `rimfrost-regel-x` | `application.properties`: `kafka.source=RegelX`, `kafka.subtopic=x`, `kafka.cancelled.topic=x-cancelled`, `regel-requests.topic=x-requests`, `regel-responses.topic=x-responses`, status-notification-topic, `flyway.default-schema=regel_x`, `table-prefix=x`, URL:er | `[6.7]` |
| 4 | `rimfrost-regel-x` | Döp om SQL-filer/tabeller till `x_*` | `[6.8]` |
| 5 | `rimfrost-regel-x` | `config.yaml`: nya UUID:n, `uppgift.path: /regel/x` | `[4.6]` |
| 6 | `rimfrost-regel-x-subprocess` | Från `rimfrost-template-regel-subprocess`; kanaler `x-requests`/`x-responses`, `X_RESPONSE_TOPIC_NAME=${PROCESS_ID}-x-responses`, process-id `x_regel` | `[9.6]` |
| 7 | process | Beroende på subprocessen; callActivity `calledElement=x` (yttre) eller `x_regel`; `aktivitetId`, `maxAttempts`, `responseTimeout`, `retryDelay` | `[10.3]` |
| 8 | `rimfrost-regel-x-bff` | Från `rimfrost-template-micro-fe-bff`; `BACKEND_URL=http://<host>/regel/x`; `CORS_ORIGINS` inkl. portal | `[13.3]` |
| 9 | `rimfrost-regel-x-fe` | Från `rimfrost-template-micro-fe`; unikt federation-`name`, `exposes`, egen `__X_FE_ENV__` | `[13.2]` |
| 10 | portal-bff | `remotes.json`-post | `[13.4]` |

### [17.3] Recept — ny maskinell regel

Se checklista `[5.11]`. Kortversion: template → `processRegel` → `application.properties`/`config.yaml`
→ tester → subprocess (`[9]`, bara regel-BPMN behövs) → process.

### [17.4] Recept — ny kompletteringsregel

Se checklista `[7.11]`. Kompletteringen körs i praktiken före en manuell regel i samma
subprocess (`template.bpmn` anropar först Komplettering, sedan Regel)
〔rimfrost-template-regel-subprocess/src/main/resources/template.bpmn〕
〔rimfrost-guide/src/sections/RulesSection.vue〕.

### [17.5] Verifiering före driftsättning (checklista)

- [ ] `config.yaml` validerar: `check-jsonschema --schemafile …/regel_schema.yaml src/main/resources/config.yaml`
  〔rimfrost-framework-regel/README.md〕
- [ ] Alla UUID:n i `config.yaml` är nya (sök efter UUID:n från templates, se `[AVVIKELSE-07]`)
- [ ] Manuell/komplettering: `kafka.cancelled.topic` och status-notification-topic satta
- [ ] SQL-tabellnamn == `regel.persistence.table-prefix` + `_common_data`/`_cloud_event_data`/`_process_topic_info`
- [ ] `uppgift.path` == controllerns `@Path`, och det finns en matchande post i `remotes.json`
  (exemplen använder path `/regel/<regel>` och nyckel `<regel>`; exakt matchningsregel i portalen är
  `[EJ VERIFIERAT]`, se `[13.4]`)
- [ ] `config.yaml` innehåller inga `metadata`-sektioner (`[AVVIKELSE-23]`)
- [ ] Topic-namn matchar enligt `[16.2]`
- [ ] `./mvnw -s settings.xml clean verify` grönt


---

## [18] Kända avvikelser och fallgropar

Format per post: **ID — rubrik** · Allvar (`HÖG` = startfel/felaktigt beteende, `MEDEL` =
vilseledande för implementation, `LÅG` = kosmetiskt) · Vad är sant · Källor för båda sidor ·
Rekommendation.

### [AVVIKELSE-01] `db-schema.md` stämmer inte med entiteterna · HÖG

- Dokumentet anger t.ex. kolumnerna `kogito_process_id`, `kogito_process_instance_id`,
  `reply_to` i `cloud_event_data` och bara sju kolumner i `common_data`
  〔rimfrost-framework-regel-oul/docs/db-schema.md:14-50〕.
- Entiteterna/test-migreringen använder `kogitoprocid`, `kogitoprocinstanceid`, `type`,
  `source` m.fl. och femton kolumner i `common_data`
  〔rimfrost-framework-regel-oul/src/test/resources/db/migration/oul/V001__framework_tables.sql〕
  〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/RegelCommonDataEntity.java〕.
- Hibernate kör `validate` → tjänsten startar inte om man följer dokumentet `[HÄRLETT]`
  〔rimfrost-framework-regel-oul/src/main/resources/application.properties:13〕.
- **Gör:** kopiera SQL från template-migreringarna (`[6.8]`).

### [AVVIKELSE-02] `kafka.cancelled.topic` saknas i templates och flera exempel · MEDEL `[EJ VERIFIERAT]`

- regel-oul kräver `kafka.cancelled.topic` (`topic=${kafka.cancelled.topic}`)
  〔rimfrost-framework-regel-oul/src/main/resources/application.properties:24-26〕.
- Saknas i `rimfrost-template-regel-manuell`, `rimfrost-template-regel-komplettering`,
  `rimfrost-regel-rtf-manuell-komplettering` och `rimfrost-regel-bekraftabeslut`; finns bara i
  `rimfrost-regel-rtf-manuell` 〔rimfrost-template-regel-manuell/src/main/resources/application.properties〕
  〔rimfrost-template-regel-komplettering/src/main/resources/application.properties〕
  〔rimfrost-regel-rtf-manuell-komplettering/src/main/resources/application.properties〕
  〔rimfrost-regel-bekraftabeslut/src/main/resources/application.properties〕
  〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:22〕.
- Konsekvensen är oklar. Ett oexpanderbart property-uttryck kan ge startfel, men
  `rimfrost-framework-regel-manuell` och `-komplettering` sätter inte heller värdet i sina egna
  testkonfigurationer trots att de beror på regel-oul 0.1.2, vilket talar emot ett säkert startfel
  〔rimfrost-framework-regel-manuell/src/test/resources/application.properties〕. Alternativet är
  att cancelled-konsumenten inte lyssnar på något användbart topic. Bara regel-oul:s egna tester
  sätter både connector och topic 〔rimfrost-framework-regel-oul/src/test/resources/application.properties〕.
  (Värdet kan även sättas via miljövariabel `KAFKA_CANCELLED_TOPIC` i driftmiljön `[HÄRLETT]`.)
- **Gör:** sätt alltid `kafka.cancelled.topic=<regel>-cancelled` i manuella och kompletteringsregler.

### [AVVIKELSE-03] Ingen producent för cancelled-event · MEDEL

- regel-oul städar vid cancelled-event 〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/RegelOulCancelledService.java:45-87〕.
- Sekvensdiagram visar att processen ska skicka `RegelOulCancelledMessage` när BPMN-timern löper ut
  〔rimfrost/sequences/kommunicering/kommunicering_timeout.mmd〕.
- Varken `rimfrost-template-regel-subprocess`, `rimfrost-framework-process` eller någon
  exempel-subprocess skickar sådana meddelanden; `handleTimeout` returnerar bara `ERROR`/`TIMEOUT`
  〔rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelService.java:52-59〕
  〔rimfrost-template-regel-subprocess/src/main/resources/application.properties〕.
- Konsekvens `[HÄRLETT]`: efter timeout kan OUL-uppgift och korrelationsdata ligga kvar; ett senare
  `POST /done` skickar ett svar som ingen väntar på.
- **Gör:** om timeout-städning behövs måste subprocessen utökas med ett utgående meddelande till
  regelns cancelled-topic (kontrakt i `rimfrost-framework-regel-oul-asyncapi`).

### [AVVIKELSE-04] Komplettering-templatens README är kopierad från manuell · MEDEL

- README nämner `RegelTemplateMapper`, `RegelTemplateMiddlewareServiceImpl`, `presentation/rest/`,
  metoderna `readData`/`updateData`/`done` och fyra testklasser
  〔rimfrost-template-regel-komplettering/README.md:27〕
  〔rimfrost-template-regel-komplettering/README.md:61〕
  〔rimfrost-template-regel-komplettering/README.md:83〕.
- Repot har bara `RegelTemplateService` (`isKompletteringRequired`/`readSvarData`/`registerSvar`),
  `presentation/RegelTemplateController` och `RegelTemplateServiceTest`
  〔rimfrost-template-regel-komplettering/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java〕
  〔rimfrost-template-regel-komplettering/src/test/java/se/fk/github/regeltemplate/logic/RegelTemplateServiceTest.java〕.
- **Gör:** följ koden och `[7]`.

### [AVVIKELSE-05] Maskinell README: YAML-exemplet ger startfel · MEDEL

- Exemplet innehåller bara `uppgift` och `specifikation` (och `uppgift.path`)
  〔rimfrost-framework-regel-maskinell/README.md:92-107〕.
- Schemat kräver `regel` och `lagrum` 〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml:5-9〕.
- **Gör:** använd templatens `config.yaml`.

### [AVVIKELSE-06] Komplettering prenumererar inte på OUL-status-topic · MEDEL `[EJ VERIFIERAT]`

- Krav enligt ASYNC_ROUTING: status-notification-topic måste matcha `kafka.subtopic`
  〔rimfrost/ASYNC_ROUTING.md:109-121〕.
- Saknas i komplettering-template och -exempel 〔rimfrost-template-regel-komplettering/src/main/resources/application.properties〕
  〔rimfrost-regel-rtf-manuell-komplettering/src/main/resources/application.properties〕.
  Utan property blir topic = kanalnamnet `operativt-uppgiftslager-status-notification` (utan suffix)
  `[HÄRLETT]`, så statusuppdateringar (tilldelning m.m.) synkas inte till handläggningen. Avslut
  via `POST /done` fungerar ändå.
- **Gör:** lägg till raden.

### [AVVIKELSE-07] Exempelregler återanvänder UUID:n från templates · MEDEL

- CONFIG_YAML.md: återanvänd aldrig UUID 〔rimfrost/regler/CONFIG_YAML.md:74〕.
- `specifikation.id` `a42ffaed-2f20-47e8-8499-f2f79ae2f45f` förekommer i fem config.yaml (tre
  templates + rtf-manuell + rtf-manuell-komplettering); flera andra UUID:n delas mellan templates
  och exempel 〔rimfrost-template-regel-maskinell/src/main/resources/config.yaml〕
  〔rimfrost-regel-rtf-manuell/src/main/resources/config.yaml〕
  〔rimfrost-regel-rtf-manuell-komplettering/src/main/resources/config.yaml〕
  〔rimfrost-regel-rtf-maskinell/src/main/resources/config.yaml〕.
- Konsekvens: `uppgiftSpecifikation.id` i handläggningen och OUL kan inte skilja regler åt `[HÄRLETT]`.
- **Gör:** generera nya UUID:n för alla fyra sektioner.

### [AVVIKELSE-08] Informationsaccess-container är inte implementerad · MEDEL

- Kraven FRMM-FR-09 och FRKOMP-FR-06 beskriver `{ data, informationsaccess }`
  〔rimfrost-framework-regel-manuell/docs/krav.md:102-122〕
  〔rimfrost-framework-regel-komplettering/docs/krav.md:95-115〕.
- Koden returnerar regelns data direkt 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/RegelManuellController.java:39-45〕
  〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/RegelKompletteringController.java:82〕.
- **Gör:** behandla som planerad förändring; BFF:er ska läsa `T` direkt tills vidare.

### [AVVIKELSE-09] Manuell README: "tre saker" — middleware-klassen saknas · MEDEL

- README listar service, controller och konfiguration 〔rimfrost-framework-regel-manuell/README.md:34〕.
- `RegelManuellMiddlewareService` är abstrakt och måste ärvas av en `@ApplicationScoped`-klass i
  regeln 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellMiddlewareService.java:26〕
  〔rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateMiddlewareServiceImpl.java〕.
- README anger också kanalnamn `{regel-name}-in`; faktisk kanal är `regel-requests`
  〔rimfrost-framework-regel-manuell/README.md:88〕
  〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/presentation/kafka/RegelConsumer.java:21〕.
- README säger att `rimfrost-framework-regel-manuell` bara kräver `sid.api.base-url`; i praktiken
  krävs hela listan i `[6.7]`.

### [AVVIKELSE-10] ASYNC_ROUTING scenario 1–2 är inaktuella · LÅG

- Scenario 1 visar `protected void handleRequest(RegelDataRequest)` + `sendResponse(...)` i regeln
  〔rimfrost/ASYNC_ROUTING.md:96-104〕. Dagens API: regeln implementerar `processRegel` och ramverket
  skickar svaret (`[5.4]`).
- Scenario 2 säger att regeln ska skicka `replyTo` till OUL-specen 〔rimfrost/ASYNC_ROUTING.md:113〕;
  ramverket gör det själv 〔rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java:108〕.

### [AVVIKELSE-11] regel-oul README om vem som skapar tabellerna · LÅG

- README: "Ramverket skapar tre tabeller … Migrationer hanteras av Flyway"
  〔rimfrost-framework-regel-oul/README.md:74-83〕.
- db-schema.md och templates: konsumenten skapar tabellerna med egna migreringar
  〔rimfrost-framework-regel-oul/docs/db-schema.md:3-5〕.
- Sant: konsumenten (ramverket har inga `src/main` migreringar; bara test-migrering)
  〔rimfrost-framework-regel-oul/src/test/resources/db/migration/oul/V001__framework_tables.sql〕.

### [AVVIKELSE-12] rtf-manuell `teknisk-spec.md` har fel REST-tabell · LÅG

- Anger `PATCH /{id}/ersattning/{ersattningId}` och `POST /{id}`
  〔rimfrost-regel-rtf-manuell/docs/teknisk-spec.md:42-44〕.
- Controllern ärver bara ramverkets `GET/PATCH /{id}`, `POST /{id}/done`
  〔rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/presentation/rest/RtfManuellController.java〕
  〔rimfrost-regel-rtf-manuell-openapi/openapi.yaml〕.

### [AVVIKELSE-13] BFF-dokumentationens miljövariabler och endpoints är inaktuella · MEDEL

- `rimfrost/frontend/bff/README.md` och quick-guide nämner `PORT`, `BE_URL`, `BE_RULE_PATH`
  〔rimfrost/frontend/bff/README.md:27-29〕 〔rimfrost/frontend/deployment/quick-guide.md:60-65〕.
- Templaten använder `quarkus.http.port`, `BACKEND_URL`, `CORS_ORIGINS`, `CORS_ALLOW_CREDENTIALS`
  〔rimfrost-template-micro-fe-bff/src/main/resources/application.properties:1-13〕;
  rtf-manuell-bff kombinerar `BACKEND_URL`/`BE_RTF_MANUELL_URL`/`BE_RULE_PATH`
  〔rimfrost-regel-rtf-manuell-bff/src/main/resources/application.properties〕.
- Läsning av data: micro-fe-README säger `POST /api/task`, quick-guide säger `GET /api/task`
  〔rimfrost/frontend/micro-fe/README.md:67-69〕 〔rimfrost/frontend/deployment/quick-guide.md:37-39〕;
  templaten har `POST /api/task` 〔rimfrost-template-micro-fe-bff/src/main/java/se/fk/github/templatebff/TemplateBFFController.java:42-44〕.

### [AVVIKELSE-14] Micro-fe-templatens README om stack och portar · LÅG

- Säger "BFF Layer — Node.js / Express" och "Backend — Java / Spring"
  〔rimfrost-template-micro-fe/README.md:51〕 〔rimfrost-template-micro-fe/README.md:58〕; BFF:erna är
  Quarkus och regler Quarkus 〔rimfrost-template-micro-fe-bff/docs/teknisk-spec.md:5〕.
- Säger BFF-port 9002 och `VITE_PORT=3033` 〔rimfrost-template-micro-fe/README.md:163-183〕; templaten
  använder 3038/3039 och BFF 9009 〔rimfrost-template-micro-fe/vite.config.ts:33-54〕
  〔rimfrost-template-micro-fe/.env.example〕.

### [AVVIKELSE-15] Maskinell: inget svar om CloudEvent-data inte kan byggas · LÅG `[HÄRLETT]`

- Undantag vid `createCloudEvent` loggas och metoden returnerar utan svar
  〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/RegelMaskinellRequestHandler.java:44-54〕.
  Subprocessen väntar då tills timeout.

### [AVVIKELSE-16] FAULT_HANDLING_OVERVIEW: "RetryUtil fångar adapterundantag" · MEDEL

- Dokumentet säger att undantag från adaptern fångas internt och behandlas som `Result.empty()`
  〔rimfrost/FAULT_HANDLING_OVERVIEW.md:39-42〕.
- `RetryUtil` fångar inga undantag från leverantören 〔rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/helpers/retry/RetryUtil.java:20-42〕.
  Ett undantag avbryter omförsöken och fångas av hanterarens yttre `catch` → `RIMFROST_OTHER` `[HÄRLETT]`.
- **Gör:** fånga adapterns undantag i leverantören (mönstret i `[5.6]`).

### [AVVIKELSE-17] Länkad `FAULT_HANDLING.md` finns inte · LÅG

〔rimfrost/FAULT_HANDLING_OVERVIEW.md:9〕 — filen finns inte i `rimfrost`-repot `[KÄLLA]`.

### [AVVIKELSE-18] `sid.api.base-url` saknas i rtf-manuells main-konfiguration · MEDEL `[EJ VERIFIERAT]`

- `SidAdapter` kräver propertyn utan default 〔rimfrost-framework-sid-adapter/src/main/java/se/fk/rimfrost/framework/sid/adapter/SidAdapter.java:36-37〕;
  manuell-ramverket sätter ingen default 〔rimfrost-framework-regel-manuell/src/main/resources/application.properties〕.
- rtf-manuell sätter den bara i testkonfig 〔rimfrost-regel-rtf-manuell/src/test/resources/application.properties〕;
  template och bekräfta beslut sätter den i main 〔rimfrost-template-regel-manuell/src/main/resources/application.properties:43〕.
  Sannolikt sätts den via miljövariabel i driftmiljön.

### [AVVIKELSE-19] Bekräfta beslut: `Yrkande.version` sätts från handläggningens version · LÅG `[HÄRLETT]`

〔rimfrost-regel-bekraftabeslut/src/main/java/se/fk/github/bekraftabeslut/logic/BekraftaBeslutService.java:148-151〕 —
`ImmutableYrkande.builder().from(handlaggning.yrkande()).version(handlaggning.version() + 1)`.
Troligen avsett `yrkande().version() + 1` (jämför `updateData`, rad 120). Kopiera inte mönstret.

### [AVVIKELSE-20] Javadoc i `RegelKompletteringService` nämner fel sökvägar · LÅG

Javadoc anger `GET/PATCH /{handlaggningId}/komplettering` och kravet "FRALL-FR-07.7"
〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/logic/RegelKompletteringService.java:10-20〕;
controllern använder `/{handlaggningId}` 〔rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/RegelKompletteringController.java:73〕.
(Komplettering-BFF:en exponerar `/api/{id}/komplettering` mot sin MFE men anropar backend på `/{id}`.)

### [AVVIKELSE-21] Oanvänd kanal i manuell template · LÅG

`mp.messaging.incoming.operativt-uppgiftslager-responses.topic` sätts
〔rimfrost-template-regel-manuell/src/main/resources/application.properties:24〕 men ingen `@Incoming`
med det namnet finns i ramverken `[KÄLLA]` (sökning i framework-oul och framework-regel-oul). Ofarlig.

### [AVVIKELSE-22] Övrigt (LÅG)

| Fynd | Källa |
|---|---|
| Stavfel `quarkus.wiremock.devservics.files-mapping` i rtf-manuell testkonfig | 〔rimfrost-regel-rtf-manuell/src/test/resources/application.properties:30〕 |
| Oanvänd tabell `rtf_manuell_komplettering_tillstand` | 〔rimfrost-regel-rtf-manuell-komplettering/src/main/resources/db/migration/V003__rtf_manuell_komplettering_tillstand.sql〕 |
| Kompletterings-MFE saknas i inbyggd `remotes.json` | 〔rimfrost-portal-bff/src/main/resources/remotes.json〕 |
| `processvyUrl` i asyncapi mappas inte till `RegelDataRequest` | 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:58-61〕 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/dto/RegelDataRequest.java〕 |
| rtf-maskinell har lokala kopior av `DmnService` och en egen `RegelFelkod`-klass (namnkrock med ramverkets) | 〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/DmnService.java〕 〔rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RegelFelkod.java〕 |
| `rimfrost-regel-beraknaersattning` använder gammalt ramverk (0.1.3) | 〔rimfrost-regel-beraknaersattning/pom.xml〕 |
| BFF-templaten saknar README (känt i egen spec) | 〔rimfrost-template-micro-fe-bff/docs/teknisk-spec.md:47-52〕 |
| `fSSAinformation` sätts till platshållare (`TODO`) av ramverket | 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java:140〕 〔rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java:78〕 |

### [AVVIKELSE-23] `metadata` i `config.yaml` tillåts av schemat men saknas i Java-klasserna · HÖG `[HÄRLETT]`

- Schemat och CONFIG_YAML.md tillåter `metadata` i `uppgift`, `specifikation`, `regel`, `lagrum`
  〔rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml:33-35〕
  〔rimfrost/regler/CONFIG_YAML.md:78〕.
- Config-klasserna har inget `metadata`-fält 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Regel.java〕
  〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Uppgift.java〕.
  Parsningen använder SnakeYAML:s `Constructor(RegelConfig.class)` respektive `loadAs` utan
  `skipMissingProperties` 〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/config/YamlConfigLoader.java:52-55〕
  〔rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/config/YamlConfigLoader.java:87〕.
  SnakeYAML kastar som standard `YAMLException` ("Unable to find property …") för okända
  egenskaper → startfel. Ingen `config.yaml` i något repo använder `metadata`, så det är otestat.
- Försök att verifiera genom att köra SnakeYAML mot kopior av klasserna stoppades av sessionens
  behörighetsregler; slutsatsen bygger på SnakeYAML:s dokumenterade standardbeteende.
- **Gör:** använd inte `metadata` förrän klasserna har fältet.

### [AVVIKELSE-24] `frontend/micro-fe/README.md` säger att templaten saknar CORS-header · LÅG

- README: CORS-headers saknas i micro-fe-templatets Apache-konfiguration 〔rimfrost/frontend/micro-fe/README.md〕.
- Templatens Dockerfile sätter `Header set Access-Control-Allow-Origin "*"` 〔rimfrost-template-micro-fe/Dockerfile:45-48〕.

### [AVVIKELSE-25] `rimfrost-framework-oul/README.md` beskriver producent som inte finns · LÅG

- README: `integration/kafka` innehåller DTO och producent för att skapa uppgifter, samt
  `handleOulResponse` 〔rimfrost-framework-oul/README.md〕.
- Repot innehåller bara konsumentsidan (`presentation/kafka`) och `OulHandlerInterface` med en
  metod, `handleOulStatus`; uppgifter skapas via REST (`rimfrost-framework-oul-adapter`)
  〔rimfrost-framework-oul/src/main/java/se/fk/rimfrost/framework/oul/logic/OulHandlerInterface.java:5-7〕
  〔rimfrost-framework-oul-adapter/src/main/java/se/fk/rimfrost/framework/oul/adapter/OulAdapter.java:67〕.

### [AVVIKELSE-26] Felkodslistor och versioner är inte synkade · LÅG

- AsyncAPI-beskrivningen listar 7 `RIMFROST_*`-koder 〔rimfrost-framework-regel-asyncapi/asyncapi.yaml:77-85〕;
  `RegelFelkod` har 12 〔rimfrost-framework-regel-error-codes/src/main/java/se/fk/rimfrost/framework/regel/error/RegelFelkod.java:5-16〕.
- `rimfrost-framework-process` beror på `rimfrost-framework-regel-error-codes:0.0.3`, regelramverken
  på `1.1.1` 〔rimfrost-framework-process/pom.xml〕 〔rimfrost-framework-regel/pom.xml〕.
- Fem konstanter används inte av ramverket (`[4.3]`).

---

## [19] Ej täckt / öppna frågor

| Område | Status | Vad som saknas |
|---|---|---|
| Kommuniceringsregel | Planerad. `framework-`, `template-`, `regel-kommunicering` är tomma; bara sekvensdiagram och `rimfrost-regel-kommunicering-openapi` finns | Ramverk, template, implementation 〔rimfrost/sequences/kommunicering/kommunicering_handlaggare_hanterar_kundkontakt.mmd〕 〔rimfrost-regel-kommunicering-openapi/openapi.yaml〕 |
| Tjänsterna (`rimfrost-service-*`) | Ej klonade | Handläggnings-, OUL-, workflow-, SID-tjänstens interna beteende och REST-scheman |
| Adaptrar (`rimfrost-adapter-*`) | Ej klonade | `FolkbokfordAdapter`, `ArbetsgivareAdapter`, `ReferensdataAdapter` m.fl. — signaturer endast härledda från användning |
| Portalen (`rimfrost-portal-handlaggare`) | Ej klonad | Exakt matchning `uppgift.url` → `remotes.json`-nyckel |
| `rimfrost-kubernetes` | Ej klonad | Faktisk driftkonfiguration, miljövariabler, topic-skapande |
| `rimfrost-ersattning-data` | Ej klonad | `ErsattningData`, `Beslutsutfall` (används av rtf-manuell och bekräfta beslut) |
| BPMN i detalj | Delvis | Exakta gateway-/sekvensflöden i subprocess-BPMN (öppna i Kogito-verktyget) |
| Startbeteende | Ej verifierat | Inget har byggts eller körts; `[EJ VERIFIERAT]`-poster bör testas med `./mvnw -s settings.xml clean verify` |
| Kafka-topics | Ej dokumenterat | Hur topics skapas i miljöer (dev services skapar dem lokalt via `quarkus.kafka.devservices.topics`) 〔rimfrost-regel-rtf-manuell/src/main/resources/application.properties:23〕 |
| Claude-kommandon i `rimfrost/.claude/commands/` | Ej beskrivna | `pr-merged`, `pr-review`, `pr-suggest`, `req-reverse` (genererar `docs/krav.md`), `req-trace` (kopplar tester till krav via `@DisplayName`) 〔rimfrost/.claude/commands/req-reverse.md〕 〔rimfrost/.claude/commands/req-trace.md〕 |
| FKPOC-planer (`docs/FKPOC-*/plan.md`) | Ej sammanfattade | Historiska implementationsplaner; använd för bakgrund, inte som sanning |

---

## [20] Ordlista

| Term | Betydelse | Var i koden |
|---|---|---|
| Handläggning | Ett ärende under handläggning; har ett yrkande, uppgift, underlag | `Handlaggning` `[3.1]` |
| Yrkande | Kundens anspråk på ett erbjudande | `Yrkande` |
| Erbjudande | Förmån/försäkring yrkandet gäller; namn via referensdata | `Yrkande.erbjudandeId`, `[12]` |
| Producerat resultat | Resultat som regler skapar/ändrar, t.ex. ersättning (`typ="ERSATTNING"`) | `ProduceratResultat` |
| Underlag | Data regeln använt för sitt beslut (JSON) | `Underlag` |
| Uppgift | Regelns arbetsuppgift i handläggningen | `Uppgift` |
| Operativ uppgift / OUL | Uppgift i Operativt uppgiftslager, syns i handläggarens inkorg | `[8]` |
| Utfall | Regelns svar: `JA`, `NEJ`, `UTREDNING`, `ERROR` | `Utfall` |
| Felkod | Maskinläsbar felkod; `RIMFROST_*` reserverat | `RegelFelkod` `[4.3]` |
| Komplettering | Insamling av saknad information i yrkandet | `[7]` |
| SID | Skyddad identitet | `SidAdapter` `[6.6]` |
| Lagrum | Juridisk grund (författning, kapitel, paragraf, stycke, punkt) | `config.yaml` `[4.6]` |
| Verksamhetslogik, roll | Klassificering och ansvarig roll för uppgiften (t.ex. `C`, `ANSVARIG_HANDLAGGARE`) | `config.yaml` |
| Aktivitet / `aktivitetId` | Processaktivitet; fast UUID per anrop i process-BPMN | `RegelDataRequest.aktivitetId` `[10.3]` |
| `replyTo` / `reply_topic` | Topic dit regelsvaret skickas | `[4.5]`, `[8.3]` |
| `sub_topic` / `kafka.subtopic` | Suffix för OUL-statusnotifieringar | `[8.3]` |
| `kogitoprocinstanceid` m.fl. | Kogito-korrelationsattribut i CloudEvent | `[2.5]` |
| Processinstans-ID | `processInstansId` = `kogitoprocinstanceid` | `RegelMaskinellRequest` |
| Subprocess / regelprocess | BPMN som anropar en regel | `[9]` |
| Flödesprocess | Toppnivå-BPMN (t.ex. VAH) | `[10]` |
| VAH | "Vård av husdjur" — exempelprocess | `rimfrost-process-vah` |
| RTF | "Rätt till försäkring" — exempelregler | `rimfrost-regel-rtf-*` |
| MFE / micro-frontend | Vue-remote som laddas i portalen | `[13]` |
| BFF | Backend-for-frontend per MFE | `[13.3]` |
| Utökad uppgiftsbeskrivning | Längre beskrivning som visas i portalen | `GET /utokadUppgiftsbeskrivning` |
| FU | Beslutsutfall "förenklad utredning"/ytterligare utredning (värde i rtf-manuell) | 〔rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/logic/RtfService.java:191-195〕 |
| BOM | Bill of materials för release `rimfrost-1_2` | `[1.6]` |

Betydelsen av "FU" är `[HÄRLETT]`; koden visar bara enumvärdet.

---

## [21] Källindex

Alla filer som refereras i dokumentet, grupperade per repo (genererat från referenserna).

### [21.rimfrost]

- `rimfrost/.claude/commands/req-reverse.md`
- `rimfrost/.claude/commands/req-trace.md`
- `rimfrost/ASYNC_ROUTING.md`
- `rimfrost/FAULT_HANDLING_OVERVIEW.md`
- `rimfrost/FRAMEWORK.md`
- `rimfrost/README.md`
- `rimfrost/bom/rimfrost-1_2-bom/rimfrost.md`
- `rimfrost/frontend/bff/README.md`
- `rimfrost/frontend/deployment/README.md`
- `rimfrost/frontend/deployment/quick-guide.md`
- `rimfrost/frontend/micro-fe/README.md`
- `rimfrost/processer/README.md`
- `rimfrost/regler/CONFIG_YAML.md`
- `rimfrost/regler/VERSIONING.md`
- `rimfrost/sequences/`
- `rimfrost/sequences/kommunicering/kommunicering_handlaggare_hanterar_kundkontakt.mmd`
- `rimfrost/sequences/kommunicering/kommunicering_timeout.mmd`
- `rimfrost/sequences/portal_micro_fe.mmd`
- `rimfrost/sequences/ratt_till_forsakring_flode.mmd`
- `rimfrost/sequences/start_end_process.mmd`

### [21.rimfrost-framework-bff]

- `rimfrost-framework-bff/README.md`

### [21.rimfrost-framework-handlaggning-adapter]

- `rimfrost-framework-handlaggning-adapter/CHANGELOG.md`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/adapter/HandlaggningAdapter.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/exception/HandlaggningException.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Beslut.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Beslutsrad.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Handlaggning.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/HandlaggningUpdate.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Idtyp.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/IndividYrkandeRoll.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/ProduceratResultat.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/ProduceratResultatRef.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Underlag.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Uppgift.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/UppgiftSpecifikation.java`
- `rimfrost-framework-handlaggning-adapter/src/main/java/se/fk/rimfrost/framework/handlaggning/model/Yrkande.java`

### [21.rimfrost-framework-oul]

- `rimfrost-framework-oul/README.md`
- `rimfrost-framework-oul/pom.xml`
- `rimfrost-framework-oul/src/main/java/se/fk/rimfrost/framework/oul/logic/OulHandlerInterface.java`
- `rimfrost-framework-oul/src/main/java/se/fk/rimfrost/framework/oul/logic/dto/OulStatus.java`
- `rimfrost-framework-oul/src/main/java/se/fk/rimfrost/framework/oul/presentation/kafka/OulKafkaConsumer.java`

### [21.rimfrost-framework-oul-adapter]

- `rimfrost-framework-oul-adapter/src/main/java/se/fk/rimfrost/framework/oul/adapter/OulAdapter.java`

### [21.rimfrost-framework-process]

- `rimfrost-framework-process/README.md`
- `rimfrost-framework-process/pom.xml`
- `rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/ProcessService.java`
- `rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelProcessResult.java`
- `rimfrost-framework-process/src/main/java/se/fk/rimfrost/framework/process/RegelService.java`

### [21.rimfrost-framework-referensdata-interface]

- `rimfrost-framework-referensdata-interface/src/main/java/se/fk/rimfrost/framework/referensdata/ErbjudandeReferensdataInterface.java`

### [21.rimfrost-framework-regel]

- `rimfrost-framework-regel/README.md`
- `rimfrost-framework-regel/docs/krav.md`
- `rimfrost-framework-regel/pom.xml`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/config/RegelConfigProvider.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/config/RegelConfigProviderYaml.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/config/YamlConfigLoader.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/integration/kafka/RegelKafkaProducer.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelRequestHandlerBase.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/RegelUtils.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Lagrum.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Regel.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/RegelConfig.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Specifikation.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/Uppgift.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/config/UtokadUppgiftsbeskrivning.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/logic/dto/RegelDataRequest.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/presentation/kafka/RegelConsumer.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/presentation/kafka/RegelMessageHandler.java`
- `rimfrost-framework-regel/src/main/java/se/fk/rimfrost/framework/regel/presentation/kafka/RegelRequestHandlerInterface.java`
- `rimfrost-framework-regel/src/main/resources/application.properties`
- `rimfrost-framework-regel/src/main/resources/schema/regel_schema.yaml`
- `rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/ErbjudandeReferensdataTestService.java`
- `rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/RegelKafkaConnector.java`
- `rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/RegelTestBase.java`
- `rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/RegelTestData.java`
- `rimfrost-framework-regel/src/test/java/se/fk/rimfrost/framework/regel/WireMockHandlaggning.java`

### [21.rimfrost-framework-regel-asyncapi]

- `rimfrost-framework-regel-asyncapi/asyncapi.yaml`
- `rimfrost-framework-regel-asyncapi/src/main/java/se/fk/rimfrost/framework/regel/Utfall.java`

### [21.rimfrost-framework-regel-error-codes]

- `rimfrost-framework-regel-error-codes/src/main/java/se/fk/rimfrost/framework/regel/error/RegelFelkod.java`

### [21.rimfrost-framework-regel-kommunicering]

- `rimfrost-framework-regel-kommunicering/README.md`

### [21.rimfrost-framework-regel-komplettering]

- `rimfrost-framework-regel-komplettering/README.md`
- `rimfrost-framework-regel-komplettering/docs/krav.md`
- `rimfrost-framework-regel-komplettering/pom.xml`
- `rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/logic/RegelKompletteringRequestHandler.java`
- `rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/logic/RegelKompletteringService.java`
- `rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/RegelKompletteringController.java`
- `rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/exception/CatchAllExceptionMapper.java`
- `rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/exception/CorrelationDataReadExceptionMapper.java`
- `rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/exception/HandlaggningNotFoundExceptionMapper.java`
- `rimfrost-framework-regel-komplettering/src/main/java/se/fk/rimfrost/framework/regel/komplettering/presentation/rest/exception/KompletteringIncompleteExceptionMapper.java`
- `rimfrost-framework-regel-komplettering/src/main/resources/application.properties`
- `rimfrost-framework-regel-komplettering/src/test/java/se/fk/rimfrost/framework/regel/komplettering/AbstractRegelKompletteringTestBase.java`

### [21.rimfrost-framework-regel-manuell]

- `rimfrost-framework-regel-manuell/README.md`
- `rimfrost-framework-regel-manuell/docs/krav.md`
- `rimfrost-framework-regel-manuell/docs/test-strategy.md`
- `rimfrost-framework-regel-manuell/pom.xml`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/DelayedException.java`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellMiddlewareService.java`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellRequestHandler.java`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellServiceBase.java`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/logic/RegelManuellServiceInterface.java`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/ExceptionMapper.java`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/RegelManuellController.java`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/RegelManuellExceptionMapper.java`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/ValidationExceptionMapper.java`
- `rimfrost-framework-regel-manuell/src/main/java/se/fk/rimfrost/framework/regel/manuell/presentation/rest/WebApplicationExceptionMapper.java`
- `rimfrost-framework-regel-manuell/src/main/resources/application.properties`
- `rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellHandlaggningTest.java`
- `rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellOulTest.java`
- `rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellResponseTest.java`
- `rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellTest.java`
- `rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/base/AbstractRegelManuellUtokadUppgiftsbeskrivningTest.java`
- `rimfrost-framework-regel-manuell/src/test/java/se/fk/rimfrost/framework/regel/manuell/helpers/WireMockRegelManuell.java`
- `rimfrost-framework-regel-manuell/src/test/resources/application.properties`

### [21.rimfrost-framework-regel-manuell-openapi]

- `rimfrost-framework-regel-manuell-openapi/README.md`

### [21.rimfrost-framework-regel-maskinell]

- `rimfrost-framework-regel-maskinell/README.md`
- `rimfrost-framework-regel-maskinell/pom.xml`
- `rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/DmnService.java`
- `rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/RegelMaskinellRequestHandler.java`
- `rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/dto/RegelMaskinellErrorResult.java`
- `rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/dto/RegelMaskinellRequest.java`
- `rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/dto/RegelMaskinellResult.java`
- `rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/dto/RegelMaskinellSuccessResult.java`
- `rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/helpers/retry/Result.java`
- `rimfrost-framework-regel-maskinell/src/main/java/se/fk/rimfrost/framework/regel/maskinell/logic/helpers/retry/RetryUtil.java`
- `rimfrost-framework-regel-maskinell/src/main/resources/application.properties`
- `rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/AbstractRegelMaskinellHandlaggningTest.java`
- `rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/AbstractRegelMaskinellResponseTest.java`
- `rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/AbstractRegelMaskinellSequenceTest.java`
- `rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/AbstractRegelMaskinellTest.java`
- `rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/base/RegelMaskinellTestData.java`
- `rimfrost-framework-regel-maskinell/src/test/java/se/fk/rimfrost/framework/regel/maskinell/helpers/WireMockRegelMaskinell.java`

### [21.rimfrost-framework-regel-oul]

- `rimfrost-framework-regel-oul/README.md`
- `rimfrost-framework-regel-oul/docs/db-schema.md`
- `rimfrost-framework-regel-oul/docs/krav.md`
- `rimfrost-framework-regel-oul/pom.xml`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/CloudEventAttributesMapper.java`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/OulUppgiftService.java`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/RegelOulCancelledHandler.java`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/RegelOulCancelledService.java`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/logic/entity/OulUppgiftSpec.java`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/presentation/kafka/RegelOulCancelledConsumer.java`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/CloudEventDataEntity.java`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/ProcessTopicInfoEntity.java`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/RegelCommonDataEntity.java`
- `rimfrost-framework-regel-oul/src/main/java/se/fk/rimfrost/framework/regel/oul/storage/internal/RegelPhysicalNamingStrategy.java`
- `rimfrost-framework-regel-oul/src/main/resources/application.properties`
- `rimfrost-framework-regel-oul/src/test/java/se/fk/rimfrost/framework/regel/oul/base/OulUppgiftServiceTestBase.java`
- `rimfrost-framework-regel-oul/src/test/java/se/fk/rimfrost/framework/regel/oul/helpers/WireMockRegelOul.java`
- `rimfrost-framework-regel-oul/src/test/resources/application.properties`
- `rimfrost-framework-regel-oul/src/test/resources/db/migration/oul/V001__framework_tables.sql`

### [21.rimfrost-framework-regel-oul-asyncapi]

- `rimfrost-framework-regel-oul-asyncapi/asyncapi.yaml`

### [21.rimfrost-framework-regel-oul-openapi]

- `rimfrost-framework-regel-oul-openapi/openapi.yaml`

### [21.rimfrost-framework-sid-adapter]

- `rimfrost-framework-sid-adapter/src/main/java/se/fk/rimfrost/framework/sid/adapter/SidAdapter.java`

### [21.rimfrost-guide]

- `rimfrost-guide/README.md`
- `rimfrost-guide/src/sections/RulesSection.vue`

### [21.rimfrost-portal-bff]

- `rimfrost-portal-bff/README.md`
- `rimfrost-portal-bff/src/main/java/se/fk/github/portalbff/PortalBffController.java`
- `rimfrost-portal-bff/src/main/resources/remotes.json`

### [21.rimfrost-process-vah]

- `rimfrost-process-vah/pom.xml`
- `rimfrost-process-vah/src/main/resources/application.properties`
- `rimfrost-process-vah/src/main/resources/vah.bpmn`

### [21.rimfrost-referensdata-erbjudande]

- `rimfrost-referensdata-erbjudande/src/main/java/se/fk/rimfrost/referensdata/erbjudande/ErbjudandeReferensdata.java`
- `rimfrost-referensdata-erbjudande/src/main/resources/application.properties`

### [21.rimfrost-regel-bekraftabeslut]

- `rimfrost-regel-bekraftabeslut/src/main/java/se/fk/github/bekraftabeslut/logic/BekraftaBeslutService.java`
- `rimfrost-regel-bekraftabeslut/src/main/java/se/fk/github/bekraftabeslut/presentation/rest/BekraftaBeslutController.java`
- `rimfrost-regel-bekraftabeslut/src/main/resources/application.properties`

### [21.rimfrost-regel-bekraftabeslut-bff]

- `rimfrost-regel-bekraftabeslut-bff/src/main/resources/application.properties`

### [21.rimfrost-regel-bekraftabeslut-fe]

- `rimfrost-regel-bekraftabeslut-fe/vite.config.ts`

### [21.rimfrost-regel-beraknaersattning]

- `rimfrost-regel-beraknaersattning/pom.xml`
- `rimfrost-regel-beraknaersattning/src/main/java/se/fk/github/regelmaskinell/logic/RegelService.java`

### [21.rimfrost-regel-kommunicering]

- `rimfrost-regel-kommunicering/README.md`

### [21.rimfrost-regel-kommunicering-openapi]

- `rimfrost-regel-kommunicering-openapi/openapi.yaml`

### [21.rimfrost-regel-maskinell-subprocess]

- `rimfrost-regel-maskinell-subprocess/README.md`

### [21.rimfrost-regel-rtf-manuell]

- `rimfrost-regel-rtf-manuell/docs/teknisk-spec.md`
- `rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/logic/RtfService.java`
- `rimfrost-regel-rtf-manuell/src/main/java/se/fk/github/manuellregelratttillforsakring/presentation/rest/RtfManuellController.java`
- `rimfrost-regel-rtf-manuell/src/main/resources/application.properties`
- `rimfrost-regel-rtf-manuell/src/main/resources/config.yaml`
- `rimfrost-regel-rtf-manuell/src/test/resources/application.properties`

### [21.rimfrost-regel-rtf-manuell-asyncapi]

- `rimfrost-regel-rtf-manuell-asyncapi/README.md`

### [21.rimfrost-regel-rtf-manuell-bff]

- `rimfrost-regel-rtf-manuell-bff/src/main/resources/application.properties`

### [21.rimfrost-regel-rtf-manuell-fe]

- `rimfrost-regel-rtf-manuell-fe/vite.config.ts`

### [21.rimfrost-regel-rtf-manuell-komplettering]

- `rimfrost-regel-rtf-manuell-komplettering/src/main/java/se/fk/github/rtf/manuell/komplettering/logic/RtfKompletteringService.java`
- `rimfrost-regel-rtf-manuell-komplettering/src/main/resources/application.properties`
- `rimfrost-regel-rtf-manuell-komplettering/src/main/resources/config.yaml`
- `rimfrost-regel-rtf-manuell-komplettering/src/main/resources/db/migration/V003__rtf_manuell_komplettering_tillstand.sql`

### [21.rimfrost-regel-rtf-manuell-komplettering-bff]

- `rimfrost-regel-rtf-manuell-komplettering-bff/src/main/java/se/fk/github/rtfmanuellkompletteringbff/RtfManuellKompletteringBffController.java`
- `rimfrost-regel-rtf-manuell-komplettering-bff/src/main/resources/application.properties`

### [21.rimfrost-regel-rtf-manuell-komplettering-fe]

- `rimfrost-regel-rtf-manuell-komplettering-fe/vite.config.ts`

### [21.rimfrost-regel-rtf-manuell-openapi]

- `rimfrost-regel-rtf-manuell-openapi/gradle.properties`
- `rimfrost-regel-rtf-manuell-openapi/openapi.yaml`

### [21.rimfrost-regel-rtf-manuell-subprocess]

- `rimfrost-regel-rtf-manuell-subprocess/src/main/resources/application.properties`
- `rimfrost-regel-rtf-manuell-subprocess/src/main/resources/rtf_manuell.bpmn`
- `rimfrost-regel-rtf-manuell-subprocess/src/main/resources/rtf_manuell_komplettering.bpmn`
- `rimfrost-regel-rtf-manuell-subprocess/src/main/resources/rtf_manuell_regel.bpmn`

### [21.rimfrost-regel-rtf-maskinell]

- `rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/DmnService.java`
- `rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RegelFelkod.java`
- `rimfrost-regel-rtf-maskinell/src/main/java/se/fk/github/maskinellregelratttillforsakring/logic/RtfService.java`
- `rimfrost-regel-rtf-maskinell/src/main/resources/application.properties`
- `rimfrost-regel-rtf-maskinell/src/main/resources/config.yaml`
- `rimfrost-regel-rtf-maskinell/src/main/resources/dmn/RtfDecisionModel.dmn`

### [21.rimfrost-regel-rtf-maskinell-subprocess]

- `rimfrost-regel-rtf-maskinell-subprocess/src/main/resources/rtf_maskinell.bpmn`

### [21.rimfrost-regler]

- `rimfrost-regler/README.md`

### [21.rimfrost-template-micro-fe]

- `rimfrost-template-micro-fe/.env.example`
- `rimfrost-template-micro-fe/Dockerfile`
- `rimfrost-template-micro-fe/README.md`
- `rimfrost-template-micro-fe/public/runtime-config.js`
- `rimfrost-template-micro-fe/src/config/env.ts`
- `rimfrost-template-micro-fe/vite.config.ts`

### [21.rimfrost-template-micro-fe-bff]

- `rimfrost-template-micro-fe-bff/docs/teknisk-spec.md`
- `rimfrost-template-micro-fe-bff/src/main/java/se/fk/github/templatebff/TemplateBFFController.java`
- `rimfrost-template-micro-fe-bff/src/main/java/se/fk/github/templatebff/integration/BackendClient.java`
- `rimfrost-template-micro-fe-bff/src/main/resources/application.properties`

### [21.rimfrost-template-process]

- `rimfrost-template-process/README.md`
- `rimfrost-template-process/pom.xml`
- `rimfrost-template-process/src/main/resources/application.properties`
- `rimfrost-template-process/src/main/resources/template.bpmn`
- `rimfrost-template-process/src/test/java/se/fk/github/rimfrost/process/template/TemplateSmokeTest.java`

### [21.rimfrost-template-referensdata-erbjudande]

- `rimfrost-template-referensdata-erbjudande/README.md`
- `rimfrost-template-referensdata-erbjudande/pom.xml`
- `rimfrost-template-referensdata-erbjudande/src/main/java/se/fk/rimfrost/referensdata/TemplateErbjudandeReferensdata.java`

### [21.rimfrost-template-regel-kommunicering]

- `rimfrost-template-regel-kommunicering/README.md`

### [21.rimfrost-template-regel-komplettering]

- `rimfrost-template-regel-komplettering/README.md`
- `rimfrost-template-regel-komplettering/pom.xml`
- `rimfrost-template-regel-komplettering/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java`
- `rimfrost-template-regel-komplettering/src/main/java/se/fk/github/regeltemplate/presentation/RegelTemplateController.java`
- `rimfrost-template-regel-komplettering/src/main/resources/application.properties`
- `rimfrost-template-regel-komplettering/src/main/resources/config.yaml`
- `rimfrost-template-regel-komplettering/src/main/resources/db/migration/V001__regel_template_komplettering_tables.sql`
- `rimfrost-template-regel-komplettering/src/test/java/se/fk/github/regeltemplate/logic/RegelTemplateServiceTest.java`

### [21.rimfrost-template-regel-komplettering-openapi]

- `rimfrost-template-regel-komplettering-openapi/README.md`
- `rimfrost-template-regel-komplettering-openapi/openapi.yaml`

### [21.rimfrost-template-regel-manuell]

- `rimfrost-template-regel-manuell/README.md`
- `rimfrost-template-regel-manuell/pom.xml`
- `rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateMapper.java`
- `rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateMiddlewareServiceImpl.java`
- `rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java`
- `rimfrost-template-regel-manuell/src/main/java/se/fk/github/regeltemplate/presentation/rest/RegelTemplateController.java`
- `rimfrost-template-regel-manuell/src/main/resources/application.properties`
- `rimfrost-template-regel-manuell/src/main/resources/db/migration/V001__regeltemplate_tables.sql`
- `rimfrost-template-regel-manuell/src/main/resources/db/migration/V002__regeltemplate_process_topic_info_table.sql`
- `rimfrost-template-regel-manuell/src/test/resources/application.properties`
- `rimfrost-template-regel-manuell/src/test/resources/config-test.yaml`

### [21.rimfrost-template-regel-manuell-openapi]

- `rimfrost-template-regel-manuell-openapi/README.md`
- `rimfrost-template-regel-manuell-openapi/build.gradle`
- `rimfrost-template-regel-manuell-openapi/gradle.properties`
- `rimfrost-template-regel-manuell-openapi/openapi.yaml`
- `rimfrost-template-regel-manuell-openapi/settings.gradle`

### [21.rimfrost-template-regel-maskinell]

- `rimfrost-template-regel-maskinell/README.md`
- `rimfrost-template-regel-maskinell/pom.xml`
- `rimfrost-template-regel-maskinell/src/main/java/se/fk/github/regeltemplate/logic/RegelTemplateService.java`
- `rimfrost-template-regel-maskinell/src/main/resources/application.properties`
- `rimfrost-template-regel-maskinell/src/main/resources/config.yaml`
- `rimfrost-template-regel-maskinell/src/test/java/se/fk/github/regeltemplate/RegelTemplateProcessRegelTest.java`
- `rimfrost-template-regel-maskinell/src/test/java/se/fk/github/regeltemplate/RegelTemplateTestData.java`
- `rimfrost-template-regel-maskinell/src/test/resources/application.properties`
- `rimfrost-template-regel-maskinell/src/test/resources/mappings/put-handlaggning.json`

### [21.rimfrost-template-regel-subprocess]

- `rimfrost-template-regel-subprocess/README.md`
- `rimfrost-template-regel-subprocess/pom.xml`
- `rimfrost-template-regel-subprocess/src/main/resources/application.properties`
- `rimfrost-template-regel-subprocess/src/main/resources/template.bpmn`
- `rimfrost-template-regel-subprocess/src/main/resources/template_komplettering.bpmn`
- `rimfrost-template-regel-subprocess/src/main/resources/template_regel.bpmn`

---

*Slut på dokumentet RIMFROST-MASTER.*
