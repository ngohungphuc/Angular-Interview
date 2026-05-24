# Angular-Interview

## Summary

- Sections:
  - Basisconcepten;
  - TypeScript en Architectuur; - Components en Templates;
  - Directives en Pipes;
  - Data Binding en Forms;
  - Services en Dependency Injection;
  - Routing;
  - RxJS en Observables;
  - Forms;
  - HTTP Client;
  - State Management en Signals; - Performance en Security; - SSR, Hydration en PWA;
  - Micro Frontends en Monorepos;
  - Signals en Moderne Reactivity;
  - Deployment en DevOps

## Basisconcepten (1-25)

1.  Wat is Angular precies en waarin verschilt het van bibliotheken zoals React?

    Angular is een full-fledged (compleet) framework, terwijl React een UI-bibliotheek is. Angular biedt out-of-the-box oplossingen voor routing, HTTP-verzoeken, formulierbeheer en dependency injection zonder dat je afhankelijk bent van pakketten van derden. React focust puur op de view-laag en laat de rest van de architectuur over aan de ontwikkelaar.

2.  Wat is een Single Page Application (SPA)?

    Een SPA is een webapplicatie die slechts één HTML-pagina laadt (index.html). Zodra de gebruiker door de app navigeert, wordt de pagina niet opnieuw ververst via de server. In plaats daarvan herschrijft Angular de DOM dynamisch op basis van de actieve componenten. Dit zorgt voor een vloeiende, app-achtige gebruikerservaring.

3.  Wat is de rol van de Angular CLI?

    De Angular CLI (Command Line Interface) is de officiële tool om Angular-applicaties te initialiseren, ontwikkelen, testen en builden. Veelgebruikte commando's zijn:
    ng new <app-naam> (project aanmaken)
    ng generate component <naam> (of kortweg ng g c <naam>)
    ng serve (lokale ontwikkelserver starten)
    ng build (applicatie compileren voor productie)

4.  Wat is het verschil tussen Just-in-Time (JIT) en Ahead-of-Time (AOT) compilatie?

    JIT: De browser downloadt de TypeScript-code en de Angular-compiler, en compileert de code ter plekke in de browser van de gebruiker. Dit wordt alleen nog gebruikt tijdens development voor snelle herlaadtijden.
    AOT: De Angular-compiler draait tijdens het build-proces (ng build). De HTML-templates en TypeScript worden vooraf omgezet in geoptimaliseerde JavaScript. Dit zorgt voor snellere laadtijden en kleinere bundels in productie.

5.  Wat is de functie van het bestand main.ts?

    main.ts is het absolute startpunt (entry point) van een Angular-applicatie. Hier wordt de applicatie opgestart (gebootstrap). In moderne Angular-apps start dit rechtstreeks met een standalone component:

    ```typescript
    import { bootstrapApplication } from "@angular/platform-browser";
    import { AppComponent } from "./app/app.component";
    import { appConfig } from "./app/app.config";

    bootstrapApplication(AppComponent, appConfig).catch((err) =>
      console.error(err),
    );
    ```

6.  Wat staat er in het bestand angular.json?

    Dit is het configuratiebestand voor de Angular CLI. Het bevat de projectstructuur, build-configuraties, paden naar globale CSS-stijlen en scripts, assets (zoals afbeeldingen) die gekopieerd moeten worden, en instellingen voor verschillende omgevingen (zoals development en productie).

7.  Wat zijn Standalone Components en waarom zijn ze nu de standaard?

    Sinds Angular 14 (en standaard vanaf 17) zijn componenten standalone. Dit betekent dat ze de oude NgModule-structuur overbodig maken. Een component specificeert nu zelf zijn eigen imports. Dit vermindert boilerplate-code (overtollige code) aanzienlijk en maakt componenten beter herbruikbaar en gemakkelijker te begrijpen.

8.  Hoe ziet de minimale structuur van een Standalone Component eruit?
    Een component bestaat uit een TypeScript-klasse met een @Component decorator:

    ````typescript

        import { Component } from '@angular/core';

        @Component({
        selector: 'app-basis',
        standalone: true,
        imports: [],
        template: `<h1>Basis Concept</h1>`,
        styles: [`h1 { color: darkblue; }`]
        })
        export class BasisComponent {}
        ```

    ````

9.  Wat is de rol van app.config.ts in moderne Angular-applicaties?

    Omdat NgModule is uitfaseerd, fungeert app.config.ts als de centrale plek om globale configuraties en services te registreren via providers. Hier configureer je bijvoorbeeld de router en de HTTP-client:

    ```typescript
    import { ApplicationConfig } from "@angular/core";
    import { provideRouter } from "@angular/router";
    import { provideHttpClient } from "@angular/common/http";
    import { routes } from "./app.routes";

    export const appConfig: ApplicationConfig = {
      providers: [provideRouter(routes), provideHttpClient()],
    };
    ```

10. Wat is de betekenis van Semantic Versioning (SemVer) binnen Angular?

    Angular volgt SemVer, wat inhoudt dat versienummers zijn opgebouwd uit MAJOR.MINOR.PATCH (bijvoorbeeld 18.2.1).
    MAJOR: Grote wijzigingen die mogelijk bestaande code breken (breaking changes). Verschijnt elke 6 maanden.
    MINOR: Nieuwe features die volledig achterwaarts compatibel zijn.
    PATCH: Bugfixes en beveiligingsupdates.

11. Wat is de 'Angular Workspace'?

    Een Angular Workspace is de mappenstructuur die wordt gegenereerd door ng new. Eén workspace kan out-of-the-box meerdere applicaties en bibliotheken (libraries) bevatten. Dit wordt geconfigureerd in angular.json onder de sectie projects.

12. Wat is de rol van index.html in een Angular app?

    Dit is de hoofdpagina die naar de browser wordt gestuurd. Het bevat de standaard HTML-boilerplates en de root-tag (bijvoorbeeld <app-root></app-root>). Tijdens de build injecteert Angular automatisch de gegenereerde JavaScript-bundels onderaan dit bestand.

13. Wat is de 'Root Component'?

    De Root Component (meestal AppComponent) is de allereerste component die Angular inlaadt via bootstrapApplication(). Het fungeert als de basis van de componentenboom. Alle andere componenten van je applicatie worden direct of indirect binnen deze root component getoond.

14. Wat doet het commando ng update?

    ng update is een krachtige CLI-tool die je dependencies (zoals @angular/core en @angular/cli) bijwerkt naar de nieuwste versie. Het unieke is dat het ook automatisch schematics (scripts) uitvoert die je bestaande broncode herschrijven naar de nieuwste Angular-syntax.

15. Wat is het verschil tussen een Framework en een Library?

    Een bibliotheek (library) roep jij aan wanneer jij dat wilt (jij hebt de controle, zoals bij React). Een framework (zoals Angular) bepaalt de architectuur en roept jóúw code aan op de momenten dat het framework dat nodig acht (Inversion of Control).

16. Waarom gebruikt Angular TypeScript in plaats van regulier JavaScript?

    TypeScript voegt statische typering toe aan JavaScript. Dit zorgt ervoor dat fouten al tijdens het typen (in de IDE) of tijdens het compileren worden ontdekt, in plaats van pas wanneer de app crasht in de browser van de gebruiker. Daarnaast maakt het refactoring van grote codebases vele malen veiliger.

17. Wat is de betekenis van poly-fills in Angular?

    Polyfills zijn code-scripts die ontbrekende moderne webfunctionaliteiten toevoegen aan oudere browsers. Angular regelt dit tegenwoordig grotendeels automatisch onder de motorkap via de build-engine, waardoor handmatige configuratie in polyfills.ts zelden nog nodig is.

18. Wat is het doel van de map public/ (voorheen assets/)?

    Deze map bevat statische bestanden die ongewijzigd naar de productieomgeving moeten worden gekopieerd. Denk hierbij aan afbeeldingen, iconen, lettertypen (fonts) en lokale JSON-bestanden. Je kunt deze binnen je CSS of HTML aanroepen met relatieve paden.

19. Hoe werkt de nieuwe Built-in Control Flow in Angular templates?

    Vanaf Angular 17 is de oude syntax met *ngIf en *ngFor vervangen door een ingebouwde, veel snellere control flow syntax die begint met een @-teken. Dit vereist geen extra imports en presteert aanzienlijk beter.

```html
@if (isIngelogd) {

<p>Welkom terug!</p>
} @else {
<p>Log aub in.</p>
} @for (item of producten; track item.id) {

<li>{{ item.naam }}</li>
} @empty {
<p>Geen producten gevonden.</p>
}
```

20. Waarom is de track property verplicht in de nieuwe @for loop?

    De track expressie vertelt Angular hoe unieke items in een lijst geïdentificeerd kunnen worden (bijvoorbeeld via een id). Hierdoor kan Angular bij een wijziging in de lijst puur dat ene gewijzigde DOM-element aanpassen, in plaats van de gehele lijst opnieuw te moeten renderen. Dit levert een enorme prestatiewinst op.

21. Wat is de Declaratieve programmeerstijl van Angular?

    In plaats van dat je stap-voor-stap in code programmeert hoe de DOM moet veranderen (imperatief), beschrijf je in Angular templates wat er getoond moet worden op basis van de huidige status (declaratief). Angular zorgt er vervolgens automatisch voor dat de UI synchroon blijft met de data.

22. Wat is een 'Host Element'?

    Het host-element is het custom HTML-element dat bij de selector van je component hoort (bijvoorbeeld <app-basis>). Je kunt styling toepassen op dit buitenste element of er events aan koppelen met de :host pseudo-selector in je CSS, of via de decorator-optie host: { ... }.

23. Wat is de rol van zone.js in traditionele Change Detection?

    zone.js is een bibliotheek die alle asynchrone taken in de browser (zoals setTimeout, clicks en HTTP-verzoeken) onderschept ("monkey-patched"). Zodra zo'n taak klaar is, activeert zone.js automatisch Angular's veranderingsdetectie om te kijken of de UI moet worden bijgewerkt.
    Opmerking (2026): Met de komst van Angular Signals is het nu mogelijk om applicaties volledig Zoneless te draaien, wat leidt tot betere prestaties en kleinere bundels.

24. Wat is het verschil tussen Angular pakketten (@angular/core, @angular/common, enz.)?

    @angular/core: Bevat de absolute kernfunctionaliteiten zoals decorators (@Component), lifecycle hooks en het dependency injection systeem.
    @angular/common: Bevat basisonderdelen die je in templates gebruikt, zoals de ingebouwde pipes (DatePipe, CurrencyPipe) en directives.

25. Wat is de compiler-optie strict in een Angular project?

    Wanneer strict: true staat ingeschakeld in tsconfig.json, dwingt de TypeScript-compiler strenge regels af. Dit omvat onder andere strictNullChecks (variabelen mogen niet stiekem null of undefined zijn) en type-veiligheid voor component-templates. Dit voorkomt veelvoorkomende runtime-fouten in productie.

## TypeScript en Architectuur (26-50)

26. Waarom dwingt Angular het gebruik van TypeScript af in plaats van pure ES6?

    TypeScript biedt compile-time type-checking en ondersteuning voor geavanceerde objectgeoriënteerde concepten zoals interfaces, abstracte klassen en decorators. Dit stelt Angular in staat om dependency injection (DI) op basis van types te implementeren en zorgt ervoor dat fouten in grote bedrijfsapplicaties direct tijdens het typen worden opgevangen in plaats van live bij de eindgebruiker.

27. Wat is het verschil tussen een Interface en een Type alias in Angular?

    Beide definiëren de vorm van een object, maar er zijn structurele verschillen:
    Interface: Is uitbreidbaar via overerving (extends) en ondersteunt declaration merging (als je twee interfaces met dezelfde naam declareert, worden ze samengevoegd). Ideaal voor datamodellen van API's.
    Type: Is flexibeler en kan worden gebruikt voor union types (type Status = 'open' | 'closed'), primitives of tuples. Types kunnen na creatie niet meer worden gewijzigd of uitgebreid via declaration merging.

28. Hoe gebruik je Type Assertions (as en <>) veilig in Angular-services?

    Type assertion vertelt de TypeScript-compiler dat jij de specifieke structuur van een variabele beter weet dan de compiler zelf. Het verandert niets aan de runtime-data. Gebruik bij voorkeur de as syntax:

```typescript
// Type assertion bij een onbekende API-respons
this.http.get("https://api.example.com/data").subscribe((response) => {
  const user = response as UserDto;
  console.log(user.id);
});
```

29. Wat is de rol van de tsconfig.json en de bijbehorende sub-bestanden?

    tsconfig.json is de hoofdconfiguratie voor de TypeScript-compiler. Angular splitst dit op voor specifieke taken:
    tsconfig.app.json: Specifieke instellingen voor de applicatiecode (compiled voor de browser).
    tsconfig.spec.json: Configuratie voor de unit tests (bevat test-specifieke types zoals Jasmine of Vitest).

30. Wat is het nut van de path mapping configuratie in een Angular workspace?

    Met path mapping in tsconfig.json vervang je complexe, relatieve imports (../../../../shared/services/auth) door strakke, absolute aliassen (@shared/services/auth). Dit verhoogt de leesbaarheid en voorkomt dat imports breken wanneer je een bestand verplaatst.

```json

"paths": {
"@shared/_": ["src/app/shared/_"],
"@core/_": ["src/app/core/_"]
}
```

31. Wat zijn TypeScript Decorators en hoe gebruikt Angular ze onder de motorkap?

    Decorators zijn functies die beginnen met een @ en metadata toevoegen aan een klasse, methode of property. De Angular-compiler (ngc) leest deze metadata (zoals @Component of @Injectable) tijdens het build-proces om te bepalen hoe de klasse gecompileerd, geïnstantieerd en gekoppeld moet worden aan templates of services.

32. Hoe implementeer je het 'Smart-Dumb Component' architectuurpatroon?

    Dit patroon scheidt logica van weergave:
    Smart Component (Container): Haalt data op uit services, beheert de state en communiceert met de backend.
    Dumb Component (Presentational): Heeft geen idee waar data vandaan komt. Ontvangt puur data via @Input (of moderne input()) en vuurt events af via @Output (of output()). Dit maakt dumb componenten 100% herbruikbaar.

33. Wat is de 'Core' en 'Shared' mappenstructuur in een moderne Angular architectuur?

    Core-map: Bevat singletons (services die slechts één keer bestaan in de app), zoals authenticatie-services, HTTP-interceptors en globale configuraties.
    Shared-map: Bevat herbruikbare componenten (zoals buttons, spinners), custom directives en pipes die in meerdere feature-modules of standalone componenten gebruikt worden.

34. Wat is Feature-Based Routing en hoe ondersteunt dit een schaalbare architectuur?

    Feature-based routing deelt de applicatie op in logische domeinen (bijv. pakket-beheer, facturatie). Elke feature heeft zijn eigen route-bestand. Hierdoor blijft de hoofdrouting compact en kunnen complete features onafhankelijk van elkaar worden ontwikkeld en door middel van lazy loading pas worden ingeladen wanneer de gebruiker erheen navigeert.

35. Hoe dwing je 'Readonly Data Structures' af in TypeScript binnen Angular?

    Om onverwachte neveneffecten te voorkomen (side-effects waarbij een component stiekem data in een service aanpast), gebruik je het readonly keyword of de TypeScript utility Readonly<T>:

```typescript

export interface UserProfile {
readonly id: string;
readonly username: string;
}

// In een service: de array zelf en de objecten erin mogen niet gemuteerd worden
readonly users = signal<readonly UserProfile[]>([]);
```

36. Wat is de 'Barrel Import' techniek (index.ts)?

    Een barrel is een index.ts bestand dat exports uit verschillende bestanden in dezelfde map bundelt en doorgeeft. Hierdoor hoeft een externe component niet vijf losse bestanden te importeren, maar is één importregel voldoende:

```typescript

// index.ts in de map 'components'
export _ from './button.component';
export _ from './card.component';
export \* from './modal.component';
```

37. Wat zijn Utility Types en hoe pas je Partial<T> of Pick<T, K> toe?

    Utility types transformeren bestaande types in nieuwe varianten:
    Partial<User>: Maakt alle properties van User optioneel (handig bij het updaten van een formulier).
    Pick<User, 'email' 'id' |>: Maakt een nieuw type aan met uitsluitend de properties id en email.

```typescript

// Voorbeeld van Partial bij een HTTP PATCH verzoek
updateUser(userId: string, changes: Partial<UserProfile>) {
return this.http.patch(`/api/users/${userId}`, changes);
}
```

38. Wat is het voordeel van 'Discriminated Unions' in Angular state management?

    Een discriminated union combineert types die een gemeenschappelijke literal property (de discriminator) delen. Hiermee dwing je type-veiligheid af in complexe processen, zoals bij het afhandelen van API-states:

```typescript
interface LoadingState {
  status: "loading";
}
interface SuccessState {
  status: "success";
  data: string[];
}
interface ErrorState {
  status: "error";
  error: string;
}

type ApiState = LoadingState | SuccessState | ErrorState;

function handleState(state: ApiState) {
  switch (state.status) {
    case "loading":
      return "Laden...";
    case "success":
      return `Data geladen: ${state.data.length}`; // Type-veilig toegang tot .data
    case "error":
      return `Fout: ${state.error}`; // Type-veilig toegang tot .error
  }
}
```

39. Hoe werkt de inject() functie versus Constructor Injection?

    Sinds Angular 14+ kun je afhankelijkheden injecteren via de inject() runtime-functie in plaats van via de constructor. Dit maakt code korter, elimineert de noodzaak voor type-duplicatie in constructors en maakt het mogelijk om herbruikbare functies (functions) te schrijven buiten klassen om.

```typescript
// Moderne manier (inject-functie)
export class ProductComponent {
  private productService = inject(ProductService);
}

// Traditionele manier (Constructor)
export class ProductComponent {
  constructor(private productService: ProductService) {}
}
```

40. Wat is de betekenis van Strict Property Initialization?

    Onder strict: true eist TypeScript dat alle niet-optionele class properties een waarde krijgen in de constructor of direct bij de declaratie. Doe je dit niet, dan krijg je een compile-fout. Je kunt dit oplossen door een beginwaarde mee te geven, het type optioneel te maken (?), of in specifieke Angular-gevallen de definite assignment assertion (!) te gebruiken als je zeker weet dat Angular de variabele later vult.

41. Hoe werken TypeScript Generics (`<T>`) in Angular Services?

    Generics maken code herbruikbaar over verschillende datatypes heen zonder aan type-veiligheid in te boeten. Een typisch voorbeeld is een generieke data-service:

```typescript
@Injectable({ providedIn: "root" })
export class CacheService {
  private cache = new Map<string, any>();

  // De methode retourneert exact het type dat je aanvraagt
  get<T>(key: string): T | undefined {
    return this.cache.get(key) as T;
  }

  set<T>(key: string, value: T): void {
    this.cache.set(key, value);
  }
}
```

42. Wat is het verschil tussen de unknown en any types?

    any: Zet de type-checker volledig uit. Je kunt elke methode of property aanroepen op een any variabele, wat kan leiden tot runtime-crashes.
    unknown: Is de type-veilige tegenhanger van any. Je mag alles toewijzen aan unknown, maar je kunt er pas acties op uitvoeren of eigenschappen van opvragen nadat je het type expliciet hebt gecontroleerd via een type guard (zoals typeof of instanceof).

43. Hoe schrijf je een Custom Type Guard in TypeScript?

    Een custom type guard is een functie die controleert of een object aan een specifiek interface voldoet, met behulp van de parameter is Type syntax:

```typescript
interface Admin {
  rechten: string[];
}
interface Klant {
  klantId: string;
}

function isAdmin(gebruiker: Admin | Klant): gebruiker is Admin {
  return (gebruiker as Admin).rechten !== undefined;
}
```

44. Wat is de 'OnPush' Change Detection Strategy op architectuurniveau?

    Standaard controleert Angular bij elk event de gehele applicatie op wijzigingen (ChangeDetectionStrategy.Default). Door een component op ChangeDetectionStrategy.OnPush te zetten, kijkt Angular alléén naar deze component als een @Input referentie verandert, er een event vanuit de component zelf wordt afgevuurd, of als er handmatig een Signal wijzigt. Dit verbetert de prestaties van grote applicaties aanzienlijk.

45. Wat is de impact van Object.freeze() versus TypeScript readonly?

    readonly is een puur hulpmiddel voor de TypeScript-compiler tijdens de ontwikkelfase. Zodra de code is gecompileerd naar JavaScript, verdwijnt readonly en kan het object runtime alsnog worden aangepast.
    Object.freeze() is een native JavaScript runtime-functie. Het bevriest het object daadwerkelijk in de browser; pogingen om eigenschappen aan te passen mislukken (en gooien een foutmelding in strict mode).

46. Hoe configureer en gebruik je Omgevingsvariabelen (Environments) in moderne Angular versies?

    In recente versies genereert Angular niet standaard een environments-map. Je maakt deze aan via ng g environments. Dit levert een environment.ts (voor dev) en environment.development.ts op. In angular.json geef je bij de productie-build aan dat het dev-bestand vervangen moet worden door de productie-variant (fileReplacements).

47. Wat is 'Tree Shaking' en hoe beïnvloedt het je architectuur?

    Tree shaking is een optimalisatieproces tijdens de productie-build waarbij ongebruikte JavaScript-code letterlijk uit de uiteindelijke bundel wordt "geschud" (verwijderd). Standalone componenten en services met @Injectable({ providedIn: 'root' }) zijn uitstekend tree-shakable: als je ze nergens importeert, belanden ze niet in de code van de eindgebruiker.

48. Wat is het gevaar van circulaire afhankelijkheden (Circular Dependencies)?

    Een circulaire afhankelijkheid ontstaat wanneer Klasse A Klasse B importeert, en Klasse B op haar beurt direct of indirect weer Klasse A importeert. Dit leidt tot runtime-fouten waarbij variabelen onverwacht undefined zijn, omdat de runtime de volgorde van initialisatie niet kan bepalen. De CLI waarschuwt je hier gelukkig voor tijdens het builden.

49. Wat is de rol van Enum types en wat is het alternatief (Union Types)?

    Enums definiëren een set benoemde constanten. Hoewel ze handig zijn, genereren ze extra JavaScript-code na compilatie. Een lichter en in Angular veelgebruikt alternatief zijn String Literal Union Types, die puur in TypeScript bestaan en nul overhead opleveren:

```typescript
// Enum manier
export enum RolEnum {
  Admin = "ADMIN",
  Gebruiker = "USER",
}

// Union Type alternatief (aanbevolen voor eenvoudige configuraties)
export type RolType = "ADMIN" | "USER";
```

50. Hoe ga je architectonisch om met API-foutafhandeling middels TypeScript types?

    In plaats van overal in de UI losse try-catch blokken te schrijven, structureer je foutafhandeling centraal in services of interceptors. Je kunt een type definiëren dat ofwel de succesvolle data, ofwel een gestructureerd error-object teruggeeft, zodat de component dwingend verplicht wordt om beide scenario's correct af te handelen.

## Components en Templates (51-75)

51. Wat is de levenscyclus (Lifecycle) van een Angular Component?

    Een component doorloopt een reeks fases vanaf de initialisatie tot aan de vernietiging. Angular biedt hooks (omvat interfaces en methoden) waarmee je op specifieke momenten kunt ingrijpen. De belangrijkste traditionele hooks zijn: ngOnInit, ngOnChanges, ngDoCheck, ngAfterViewInit, en ngOnDestroy.
    Opmerking: In moderne Angular-versies (met Signals) verschuift de focus steeds meer naar constructoren, effect(), en de nieuwe afterNextRender en afterRender globale functies voor DOM-interacties.

52. Wat is het verschil tussen ngOnInit en de constructor van een klasse?

    Constructor: Dit is een native TypeScript-feature. Het is uitsluitend bedoeld voor de basis-initialisatie van de klasse en het injecteren van afhankelijkheden. De component-inputs en de DOM-omgeving zijn op dit moment nog niet beschikbaar.
    ngOnInit: Dit is een Angular lifecycle hook. Deze vuurt zodra Angular klaar is met het initialiseren van de data-bound inputs van de component. Dit is dé plek om API-aanroepen te starten of initiële logica uit te voeren op basis van inputs.

53. Hoe werken de nieuwe Signal-gebaseerde Inputs (input())?

    In plaats van de traditionele @Input() decorator gebruikt modern Angular de input() macro. Dit creëert een read-only Signal van de ingekomen waarde. Dit maakt het opvangen van wijzigingen veel eenvoudiger, omdat je nu direct reactief kunt reageren via computed of effect().

```typescript
import { Component, input, computed } from "@angular/core";

@Component({
  selector: "app-user-card",
  standalone: true,
  template: `<h3>{{ begroeting() }}</h3>`,
})
export class UserCardComponent {
  // Een optionele input met een default waarde
  naam = input("Gast");

  // Een verplichte input zonder default waarde
  leeftijd = input.required<number>();

  // Automatisch herberekenen zodra 'naam' verandert
  begroeting = computed(() => `Welkom, ${this.naam()}!`);
}
```

54. Wat is de betekenis en werking van de nieuwe output() macro?

    De output() macro vervangt de traditionele @Output() decorator en EventEmitter. Het werkt lichter en biedt betere type-veiligheid onder de motorkap.

```typescript
import { Component, output } from "@angular/core";

@Component({
  selector: "app-alert",
  standalone: true,
  template: `<button (click)="sluit()">Sluiten</button>`,
})
export class AlertComponent {
  // Definiëren van de output
  verwijderd = output<boolean>();

  sluit() {
    this.verwijderd.emit(true);
  }
}
```

55. Wat is Two-Way Data Binding met de nieuwe model() functie?

    Voorheen moest je two-way binding opzetten met een combinatie van @Input() en @Output() (volgens het banana-in-a-box [(prop)] patroon). Nu biedt Angular de model() macro. Dit creëert een Signal dat zowel gelezen kan worden door de component, als rechtstreeks gemuteerd kan worden door zowel de ouder- als de kind-component.

```typescript
// In de kind-component
checked = model(false); // Maakt een read-write signal aan

// In de ouder-component template koppelen:
// <app-toggle [(checked)]="isAdmin" />
```

56. Hoe werkt de nieuwe @let syntax in Angular templates?

    De @let syntax stelt je in staat om direct in je HTML-template lokale variabelen te declareren. Dit is ideaal om complexe expressies, herhaalde Signal-aanroepen of het resultaat van een async pipe op te slaan, zonder dat je daarvoor onnodige wrapper-elementen (zoals `<ng-container>`) hoeft te introduceren.

```html
@let gebruikersNaam = profiel().naam.voornaam;

<h1>Welkom {{ gebruikersNaam }}</h1>
<p>Je ingelogde naam is: {{ gebruikersNaam }}</p>

<!-- Handig bij async observables -->

@let data = data$ | async; @if (data) {

<div>{{ data.resultaat }}</div>
}
```

57. Wat is @defer en hoe revolutioneert dit het laden van componenten?

    De @defer control flow syntax maakt Deferrable Views mogelijk. Hiermee kun je zware componenten, directives of pipes in je template automatisch lazy-loaden (uitgesteld inladen) op basis van specifieke condities of gebruikersinteracties (zoals scrollen, swipen of hoveren). Dit verlaagt de initiële laadtijd van je pagina gigantisch.

```html
@defer (on viewport) {
<!-- Deze component wordt pas gedownload en getoond zodra hij in het scherm scrollt -->
<app-zware-grafiek />
} @placeholder {
<div>Grafiek is onderweg...</div>
} @loading {
<app-spinner />
} @error {
<p>Laden van de grafiek mislukt.</p>
}
```

58. Welke pre-built triggers ondersteunt de @defer syntax?

    De @defer syntax ondersteunt krachtige ingebouwde condities:
    on idle: Laadt de component zodra de browser in ruststand is (standaardgedrag).
    on viewport: Laadt zodra het placeholder-element in beeld scrollt.
    on interaction: Laadt zodra de gebruiker op de placeholder klikt of typt.
    on hover: Laadt zodra de muis over de placeholder beweegt.
    on timer(5s): Laadt na een vertraging van 5 seconden.
    when conditie: Een custom boolean expressie of Signal.

59. Wat is het verschil tussen View Encapsulation modi: Emulated, ShadowDom en None?

    View Encapsulation bepaalt hoe stijlen (CSS) van een component worden geïsoleerd:
    Emulated (Standaard): Angular hernoemt je CSS-classes onder de motorkap door er unieke attributen aan te geven (bijv. \_ngcontent-c1). Hierdoor lekt de CSS niet naar andere componenten.
    ShadowDom: Maakt gebruik van de native Shadow DOM-functionaliteit van de browser. Het isoleert de component volledig in een eigen web component capsule.
    None: CSS wordt globaal toegepast. De stijlen van deze component kunnen de rest van de hele applicatie beïnvloeden.

60. Hoe selecteer en manipuleer je elementen met viewChild() en viewChildren()?

    In plaats van de oude @ViewChild decorator gebruikt modern Angular de signaal-gebaseerde functies viewChild() en viewChildren(). Dit geeft een Signal terug dat het gezochte element bevat, wat veel betere integratie biedt met het reactieve systeem.

```typescript
import { Component, viewChild, ElementRef, effect } from "@angular/core";

@Component({
  selector: "app-focus",
  standalone: true,
  template: `<input #inputElement type="text" />`,
})
export class FocusComponent {
  // Zoek de #inputElement template reference op
  inputRef = viewChild<ElementRef<HTMLInputElement>>("inputElement");

  constructor() {
    effect(() => {
      const el = this.inputRef()?.nativeElement;
      el?.focus(); // Automatisch focussen zodra het element beschikbaar is
    });
  }
}
```

61. Wat is Content Projection en hoe gebruik je <ng-content>?

    Content Projection stelt je in staat om HTML of andere componenten van buitenaf in een component te injecteren (vergelijkbaar met children in React). Je gebruikt <ng-content> als placeholder in de template van het ontvangende component.

```html
<!-- Template van app-card -->
<div class="card">
  <div class="card-header">
    <ng-content select="[titel]"></ng-content>
  </div>
  <div class="card-body">
    <ng-content></ng-content>
    <!-- Vangt de rest op -->
  </div>
</div>
```

62. Wat is het verschil tussen contentChild() en viewChild()?

    viewChild(): Zoekt naar elementen of kind-componenten die direct in de eigen template van de component staan.
    contentChild(): Zoekt naar elementen die via Content Projection (`<ng-content>`) vanuit een ouder-component tussen de tags van deze component zijn geplaatst.

63. Wat is de functie van <ng-template>?

    <ng-template> definieert een stuk HTML-code dat niet direct door Angular wordt gerenderd. Het fungeert als een blauwdruk. Je kunt het dynamisch tonen met behulp van structurele logica, @defer blokken of via code met een ViewContainerRef.

64. Wat is het nut van <ng-container>?

    <ng-container> is een logische groeperingstag die zelf niet in de uiteindelijke HTML-DOM terechtkomt. Dit is uiterst nuttig als je styling-neutraal logica wilt toepassen, of als je meerdere elementen wilt groeperen zonder de HTML-structuur te vervuilen met overbodige <div> elementen.

65. Wat is een 'Template Reference Variable' en hoe gebruik je deze?

    Met een hashtag (#naam) kun je een referentie naar een specifiek DOM-element of een Angular-component rechtstreeks in je HTML aanmaken. Je kunt deze variabele vervolgens elders in dezelfde template gebruiken.

```html
<input #mijnInput type="text" />

<!-- De knop leest live de waarde uit het invoerveld uit -->

<button (click)="verwerk(mijnInput.value)">Verstuur</button>
```

66. Hoe werkt de :host selector in component-styling?

    De :host pseudo-class selector wordt gebruikt om de buitenkant (het omhullende HTML-element) van de component zelf te stylen.

```css

/_ Binnen de CSS van app-menu _/
:host {
display: block;
border: 1px solid #ccc;
background-color: fatal; /_ Foutbeveiliging check: gebruik native kleur _/
background-color: #f9f9f9;
}

/_ Stylen op basis van een class op de host _/
:host(.actief) {
border-color: green;
}
```

67. Wat doet de :host-context() selector?

    Met :host-context() kun je de component stylen op basis van een conditie buiten de component. Het zoekt naar een specifieke class of attribuut in de voorouders (parent elementen) van de component. Dit is ideaal voor het implementeren van dark-mode of theming.

```css
/_
  De
  tekst
  wordt
  rood
  als
  ergens
  hoger
  in
  de
  app
  de
  class
  .dark-theme
  actief
  is
  _/
  :host-context(.dark-theme)
  p {
  color: white;
}
```

68. Waarom mag je de DOM niet direct manipuleren via native JavaScript (document.querySelector)?

    Directe DOM-manipulatie omzeilt Angular's Change Detection en het abstractieniveau van het framework. Dit kan leiden tot visuele bugs, security-risico's (XSS) en zorgt ervoor dat je applicatie crasht als je deze probeert te draaien in Server-Side Rendering (SSR) omgevingen waar document simpelweg niet bestaat. Gebruik in plaats daarvan template bindings of de Renderer2 API.

69. Wat is de taak van de Renderer2 service?

    Renderer2 is een ingebouwde Angular service die een veilige abstractielaag biedt voor het manipuleren van DOM-elementen. Door Renderer2 te gebruiken in plaats van native methoden, blijft je code veilig voor SSR (Server-Side Rendering) en web workers.

```typescript
import { inject, Renderer2, ElementRef } from "@angular/core";

export class StyleDirective {
  private el = inject(ElementRef);
  private renderer = inject(Renderer2);

  geefKleur() {
    this.renderer.setStyle(this.el.nativeElement, "color", "blue");
  }
}
```

70. Hoe werkt 'Event Bubbling' in Angular componenten?

    Net als in standaard HTML/JavaScript bubbelen events in Angular standaard omhoog door de DOM-boom. Als een gebruiker op een knop klikt in een diep geneste component, zal het click-event omhoog reizen naar alle bovenliggende elementen, tenzij je dit expliciet stopt met $event.stopPropagation().

71. Wat is het verschil tussen een Component en een Directive?

    Component: Is een directive met een eigen template (HTML) en stijlen (CSS). Het representeert een visueel bouwblok op de pagina.
    Directive: Heeft geen template. Het wordt als attribuut toegevoegd aan een bestaand HTML-element of component om het gedrag of de weergave daarvan te veranderen of uit te breiden (bijvoorbeeld een custom scroll-gedrag).

72. Hoe ga je om met asynchrone data in templates zonder de async pipe?

    Met de introductie van Angular Signals hoef je asynchrone RxJS streams niet meer per se met de async pipe af te handelen. Je kunt een Observable binnen je TypeScript klasse omzetten naar een Signal met de toSignal() functie. De template kan dit Signal vervolgens synchroon uitlezen.

```typescript
import { Component, inject } from "@angular/core";
import { toSignal } from "@angular/core/rxjs-interop";
import { DataService } from "./data.service";

@Component({
  selector: "app-async-data",
  standalone: true,
  template: `
    @for (user of users()) {
      <li>{{ user.name }}</li>
    }
  `,
})
export class AsyncDataComponent {
  private dataService = inject(DataService);
  // Converteert de Observable direct naar een read-only Signal
  users = toSignal(this.dataService.getUsers(), { initialValue: [] });
}
```

73. Wat is de functie van NgZone en wanneer gebruik je runOutsideAngular()?

    NgZone beheert de executie-context van Angular. Soms voer je zware asynchrone taken uit (zoals een intensieve animatie of een setInterval die elke milliseconde tikt) die de interface niet beïnvloeden. Om te voorkomen dat Angular continu Change Detection uitvoert en de app traag wordt, draai je die code buiten Angular:

```typescript
import { inject, NgZone, OnInit } from "@angular/core";

export class AnimatieComponent implements OnInit {
  private zone = inject(NgZone);

  ngOnInit() {
    this.zone.runOutsideAngular(() => {
      // Deze timer triggert GEEN change detection
      setInterval(() => {
        this.doeZwareBerekening();
      }, 16);
    });
  }
}
```

74. Wat is het gevaar van geheugenlekken (Memory Leaks) in components?

    Wanneer een component destructs (ngOnDestroy) maar nog actieve abonnementen (.subscribe()) heeft lopen op globale services of RxJS stromen, blijft het component-object in het geheugen van de browser bestaan. Dit bouwt op termijn op en maakt de applicatie traag. Je lost dit op door over te stappen op Signals, de async pipe te gebruiken, of .unsubscribe() toe te passen.

75. Hoe dwing je strikte types af voor component selectors?

    Je kunt de @Component selector zo configureren dat deze uitsluitend als HTML-attribuut of specifiek element gebruikt mag worden. Dit is handig voor herbruikbare componenten binnen design systems.

```typescript
@Component({
  selector: "button[app-custom-btn]", // Mag alleen als attribuut op een <button> element
  standalone: true,
  template: `<ng-content></ng-content>`,
})
export class CustomBtnComponent {}
```

## Directives en Pipes (76-95)

76. Welke drie hoofdtypen Directives kent Angular?

    Angular kent de volgende drie categorieën directives:
    Components: Directives met een eigen, ingebouwde template (HTML) en styling (CSS).
    Attribute Directives: Veranderen het uiterlijk of het gedrag van een bestaand DOM-element, component of ingebouwde tag (bijv. NgStyle of een custom highlight-directive).
    Structural Directives: Wijzigen de structuur van de DOM door elementen toe te voegen, te muteren of te verwijderen. In oudere code herkenbaar aan het *-teken (bijv. *ngIf). In modern Angular grotendeels vervangen door de ingebouwde @ control flow.

77. Hoe maak en registreer je een moderne Standalone Attribute Directive?

    Een standalone directive gebruikt de @Directive decorator en stelt standalone: true in. Je kunt deze direct importeren in de imports array van de component waar je hem wilt gebruiken.

```typescript
import { Directive, ElementRef, inject, OnInit } from "@angular/core";

@Directive({
  selector: "[appAutofocus]",
  standalone: true,
})
export class AutofocusDirective implements OnInit {
  private el = inject(ElementRef);

  ngOnInit() {
    this.el.nativeElement.focus();
  }
}
```

78. Wat is het verschil tussen @HostBinding/@HostListener en de moderne host property?

    Hoewel de @HostBinding en @HostListener decorators nog steeds werken, adviseert het Angular-team nadrukkelijk om de host metadata-property in de decorator te gebruiken. Dit centraliseert alle host-gedragingen op één plek, verbetert de leesbaarheid en prestaties, en voorkomt rondslingerende decorators in je klasse.

```typescript
// Moderne en aanbevolen aanpak via host property:
@Directive({
  selector: "[appHover]",
  standalone: true,
  host: {
    "[class.is-hovered]": "isHovered",
    "(mouseenter)": "onMouseEnter()",
    "(mouseleave)": "onMouseLeave()",
  },
})
export class HoverDirective {
  isHovered = false;

  onMouseEnter() {
    this.isHovered = true;
  }
  onMouseLeave() {
    this.isHovered = false;
  }
}
```

79. Hoe geef je data mee aan een Directive via Inputs?

    Je kunt de moderne input() macro gebruiken binnen een directive om parameters te accepteren. Als de selector van de directive overeenkomt met de inputnaam, kun je de directive en de waarde in één attribuut combineren.

```typescript
import { Directive, input, inject, effect, ElementRef } from "@angular/core";

@Directive({
  selector: "[appTooltip]",
  standalone: true,
  host: { "[attr.title]": "text()" },
})
export class TooltipDirective {
  // Inputnaam matcht de selector voor strakke HTML-syntax
  text = input.required<string>({ alias: "appTooltip" });
}
```

Gebruik in HTML: `<span appTooltip="Dit is een tip!">Hover mij</span>`

80. Wat is een Structural Directive en hoe werkt het onder de motorkap?

    Een structurele directive manipuleert de DOM-structuur. Onder de motorkap maakt Angular van het element waar de directive op staat een <ng-template>. De directive injecteert vervolgens een TemplateRef (de blauwdruk van het element) en een ViewContainerRef (de plek in de DOM waar het element moet komen) om het element dynamisch te renderen of te slopen.

81. Hoe schrijf je een Custom Structural Directive?

    Hoewel de nieuwe @if control flow de meeste use cases dekt, kun je nog steeds eigen structurele directives bouwen (bijvoorbeeld voor rechtenbeheer):

```typescript
import {
  Directive,
  input,
  inject,
  TemplateRef,
  ViewContainerRef,
} from "@angular/core";
import { AuthService } from "./auth.service";

@Directive({
  selector: "[appHasRole]",
  standalone: true,
})
export class HasRoleDirective {
  private templateRef = inject(TemplateRef<any>);
  private vcr = inject(ViewContainerRef);
  private authService = inject(AuthService);

  appHasRole = input.required<string>();

  constructor() {
    // We reageren op basis van de input
    effect(() => {
      const role = this.appHasRole();
      if (this.authService.hasRole(role)) {
        this.vcr.createEmbeddedView(this.templateRef);
      } else {
        this.vcr.clear();
      }
    });
  }
}
```

82. Wat is de Directive Composition API?

    Geintroduceerd in Angular 15, stelt de Directive Composition API je in staat om bestaande standalone directives rechtstreeks toe te voegen aan een component of andere directive via de hostDirectives array. Hierdoor kun je gedrag hergebruiken zonder overerving (inheritance) te gebruiken.

```typescript
@Component({
  selector: "app-custom-input",
  standalone: true,
  // Dit component krijgt direct en automatisch alle logica van de twee directives mee
  hostDirectives: [
    { directive: MaxLengthDirective },
    { directive: TrimInputDirective },
  ],
  template: `<input type="text" />`,
})
export class CustomInputComponent {}
```

83. Wat is een Pipe in Angular?

    Een Pipe is een klasse met een @Pipe decorator die de PipeTransform interface implementeert. Pipes worden in component-templates gebruikt om data visueel te transformeren (formatteren) zonder de onderliggende waarde in de TypeScript-klasse permanent aan te passen.

84. Wat is het verschil tussen een Pure en een Impure Pipe?

    Dit is een cruciaal onderscheid voor applicatieprestaties:
    Pure Pipe (Standaard): Wordt alleen uitgevoerd wanneer Angular merkt dat de referentie van de invoerwaarde verandert (bij primitives: de waarde zelf; bij objecten/arrays: een nieuwe referentie). Dit maakt ze extreem snel en performant.
    Impure Pipe: Heeft pure: false in de decorator. Angular voert deze pipe uit bij elke veranderingsdetectiecyclus (bij elke muisklik, toetsaanslag of event). Dit kan leiden tot enorme prestatieproblemen als de logica zwaar is.

85. Hoe bouw je een Custom Pure Pipe?

    Een custom pipe implementeert de transform methode. Hier is een voorbeeld dat tekst inkort (truncate):

```typescript
import { Pipe, PipeTransform } from "@angular/core";

@Pipe({
  name: "truncate",
  standalone: true,
  pure: true, // Standaard gedrag, maar goed om expliciet te vermelden
})
export class TruncatePipe implements PipeTransform {
  transform(value: string, limit = 20, suffix = "..."): string {
    if (!value) return "";
    return value.length > limit ? value.substring(0, limit) + suffix : value;
  }
}
```

Gebruik
`{{ langeTekst | truncate:30:'...' }}`

86. Waarom is het aanroepen van functies in een template een anti-patroon, en hoe lossen Pipes dit op?

    Wanneer je een functie aanroept in een template binding `<p>{{ berekenTotaal(item) }}</p>`, wordt deze functie bij elke Change Detection cyclus opnieuw uitgevoerd. Angular weet namelijk niet of de uitkomst van de functie veranderd is. Een Pure Pipe lost dit op doordat Angular de transformatie cachet; zolang de inputreferentie van item niet wijzigt, slaat Angular de executie over en hergebruikt de oude waarde.

87. Wat is het doel en de werking van de ingebouwde AsyncPipe?

    De AsyncPipe accepteert een Observable of een Promise als input. Het zorgt er automatisch voor dat er een abonnement (.subscribe()) wordt geopend, dat de template wordt bijgewerkt zodra er nieuwe data binnenkomt, en—het allerbelangrijkste—dat het abonnement netjes wordt afgesloten (.unsubscribe()) zodra de component uit de DOM wordt verwijderd om geheugenlekken te voorkomen.

88. Hoe verhoudt de AsyncPipe zich tot Angular Signals?

    Met de komst van Signals is de noodzaak voor de AsyncPipe drastisch verminderd. In plaats van een Observable met een async pipe in de template te hangen, converteren we Observables in de TypeScript klasse vaak direct naar Signals via toSignal(). Hierdoor blijft de template synchroon en vrij van RxJS-syntaxis.

    ```ts
    import { toSignal } from "@angular/core/rxjs-interop";

    users = toSignal(this.userService.getUsers(), {
      initialValue: [],
    });
    ```

    ```html
    @for (user of users(); track user.id) {
    <li>{{ user.name }}</li>
    }
    ```

89. Wat doet de ingebouwde KeyValuePipe?

    De KeyValuePipe transformeert een JavaScript object of Map naar een array van sleutel-waarde paren (key and value). Hierdoor kun je met een @for loop eenvoudig door de eigenschappen van een dynamisch object itereren.

```html
@let settings = { theme: 'dark', notifications: true }; @for (entry of settings
| keyValue; track entry.key) {

<p>{{ entry.key }}: {{ entry.value }}</p>
}
```

90. Wat is de ingebouwde JsonPipe en wanneer gebruik je deze?

    De JsonPipe zet een complex JavaScript object of array om naar een stringified JSON-notatie (JSON.stringify). Het wordt vrijwel uitsluitend gebruikt tijdens het ontwikkelen om snel de actuele status van een datamodel of API-respons te debuggen in de HTML-weergave.

```html
<pre>{{ actueleGebruiker$ | async | json }}</pre>
```

91. Kun je meerdere Pipes achter elkaar schakelen (Chaining)?

    Ja, je kunt pipes onbeperkt achter elkaar hangen (chainen). De uitvoer van de eerste pipe dient direct als de invoer voor de volgende pipe. De verwerking loopt van links naar rechts.

```html
<!-- De tekst wordt eerst omgezet naar hoofdletters en daarna ingekort -->
<p>{{ omschrijving | uppercase | truncate:50 }}</p>
```

92. Wat is de invloed van de DatePipe en locale instellingen?

    De DatePipe formatteert datums op basis van de actieve landinstellingen (LOCALE_ID). Standaard hanteert Angular en-US. Als je Nederlandse datums wilt tonen (zoals "maandag" in plaats van "Monday"), moet je de Nederlandse locale registreren in je app.config.ts.

```typescript
import { LOCALE_ID } from "@angular/core";
import registerLocaleCc from "@angular/common/locales/nl";
import { registerLocaleData } from "@angular/common";
registerLocaleData(registerLocaleCc);

export const appConfig: ApplicationConfig = {
  providers: [{ provide: LOCALE_ID, useValue: "nl-NL" }],
};
```

93. Hoe test je een Custom Pipe in een Unit Test?

    Omdat een Pipe in de basis een pure TypeScript klasse is zonder ingewikkelde DOM-koppelingen, kun je deze heel eenvoudig testen zonder de complete TestBed van Angular op te hoeven tuigen. Je instantieert de klasse handmatig en test de transform methode.

```typescript
describe("TruncatePipe", () => {
  const pipe = new TruncatePipe();

  it("moet tekst inkorten als het de limiet overschrijdt", () => {
    expect(pipe.transform("Angular is fantastisch", 7)).toBe("Angular...");
  });
});
```

94. Wat is het gedrag van een Pipe wanneer er null of undefined wordt meegegeven?

    Goedgebouwde pipes moeten altijd bestand zijn tegen null of undefined invoer (defensive programming). Als de invoer leeg is, behoort de pipe direct een logische fallback (zoals een lege string '') terug te geven in plaats van methoden aan te roepen op een niet-bestaand object, wat een runtime crash zou veroorzaken.

95. Hoe kun je een Pipe programmatisch gebruiken in je TypeScript-klasse?

    Soms wil je de transformatie van een pipe direct in je TypeScript logica gebruiken. Omdat pipes standalone klassen zijn met een @Injectable karakter, kun je ze via de inject() functie in je klasse laden en handmatig aanroepen, mits je ze toevoegt aan de providers.

```typescript
export class OverzichtComponent {
  // Injecteer de pipe direct als een service
  private currencyPipe = inject(CurrencyPipe);

  genereerLabel(bedrag: number) {
    return `Totaal: ${this.currencyPipe.transform(bedrag, "EUR")}`;
  }
}
```

## Data Binding en Forms (96-120)

96. Wat zijn de vier vormen van Data Binding in Angular?

    Angular kent vier manieren om data uit te wisselen tussen de TypeScript-klasse en de HTML-template:
    - Interpolatie (`{{ waarde }}`): Stroomt data van de component naar de template (one-way). Zet expressies om in tekst.

    - Property Binding ([property]="waarde"): Stroomt data van de component naar een attribuut of property van een DOM-element of kind-component (one-way).

    - Event Binding (event)="methode()": Stroomt data van de template naar de component (one-way) om te reageren op gebruikersacties zoals kliks of toetsaanslagen.

    - Two-Way Data Binding ([(ngModel)] of [(modelSignal)]): Synchroniseert data in beide richtingen simultaan. Wijzigingen in de UI updaten de klasse en vice versa.

97. Wat is het verschil tussen een DOM Property en een HTML Attribuut?
    - HTML Attribuut: Gedefinieerd in de HTML-broncode en initialiseert de initiële status van het element. Attributen veranderen nooit van waarde.
    - DOM Property: De actuele, levende representatie van het element in de browser-DOM. Properties kunnen veranderen door gebruikersinteractie. Angular Property Binding ([disabled], [value]) grijpt altijd in op DOM properties, niet op HTML attributen.

98. Wat zijn de twee hoofdtypen formulieren binnen Angular en wanneer kies je welke?
    - Template-Driven Forms: De formulierstructuur en validatieregels worden direct in de HTML-template geschreven (met behulp van FormsModule en directives zoals ngModel). Geschikt voor zeer eenvoudige formulieren.
    - Reactive Forms: De formulierstructuur en validatie worden programmatisch opgebouwd in de TypeScript-klasse (via ReactiveFormsModule). Dit biedt volledige controle, superieure testbaarheid, diepe type-veiligheid en krachtige asynchrone datastromen via RxJS of Signals. Dit is de industriestandaard voor serieuze applicaties.

99. Wat zijn Strongly Typed Reactive Forms?

    Sinds Angular 14 zijn formulieren volledig getypeerd. Dit betekent dat TypeScript exact weet welke velden er in je formulier zitten, of ze optioneel zijn en welk datatype ze bevatten. Dit voorkomt dat je per ongeluk typt naar een niet-bestaand formulierveld of verkeerde datatypes toewijst.

```typescript
import { Component, inject } from "@angular/core";
import {
  NonNullableFormBuilder,
  ReactiveFormsModule,
  Validators,
} from "@angular/forms";

@Component({
  selector: "app-login",
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <form [formGroup]="loginForm" (ngSubmit)="onSubmit()">
      <input formControlName="email" type="email" />
      <input formControlName="wachtwoord" type="password" />
      <button type="submit">Inloggen</button>
    </form>
  `,
})
export class LoginComponent {
  private fb = inject(NonNullableFormBuilder);

  // TypeScript leidt automatisch het type af: FormGroup<{ email: FormControl<string>; ... }>
  loginForm = this.fb.group({
    email: ["", [Validators.required, Validators.email]],
    wachtwoord: ["", [Validators.required]],
  });

  onSubmit() {
    if (this.loginForm.valid) {
      // loginData is strict getypeerd als { email: string; wachtwoord: string; }
      const loginData = this.loginForm.getRawValue();
      console.log(loginData.email);
    }
  }
}
```

100. Wat is het verschil tussen FormGroup, FormControl en FormArray?

- FormControl: Beheert de waarde, validatiestatus en interactiegeschiedenis (zoals dirty of touched) van één individueel invoerveld.
- FormGroup: Bundelt een groep van FormControl, FormGroup of FormArray objecten tot een logische eenheid. De validatiestatus van de groep is afhankelijk van de kinderen.
- FormArray: Beheert een dynamische, lineaire lijst van formulier-elementen. Handig voor scenario's waar gebruikers dynamisch velden kunnen toevoegen (zoals een lijst met telefoonnummers).

101. Wat is het nut van de NonNullableFormBuilder?

     Standaard kan de waarde van een FormControl altijd null worden als je het formulier reset via .reset(). Wanneer je de NonNullableFormBuilder gebruikt (of de optie { nonNullable: true } meegeeft), zal het formulier bij een reset automatisch terugvallen op de initiële beginwaarde in plaats van null. Dit houdt je TypeScript types strak en vrij van null checks.

102. Hoe maak en beheer je een dynamische FormArray?

     Met een FormArray kun je velden dynamisch pushen of slopen. Hier is hoe je dit opzet en uitleest in TypeScript:

```typescript
import { Component, inject } from "@angular/core";
import {
  NonNullableFormBuilder,
  ReactiveFormsModule,
  FormArray,
} from "@angular/forms";

@Component({
  selector: "app-hobbys",
  standalone: true,
  imports: [ReactiveFormsModule],
  template: `
    <div [formGroup]="form">
      <div formArrayName="hobbys">
        @for (control of hobbys.controls; track $index) {
          <input [formControlName]="$index" />
          <button (click)="verwijderHobby($index)">Wis</button>
        }
      </div>
      <button (click)="addHobby()">Hobby toevoegen</button>
    </div>
  `,
})
export class HobbysComponent {
  private fb = inject(NonNullableFormBuilder);

  form = this.fb.group({
    hobbys: this.fb.array<string>([]),
  });

  // Getter voor gemakkelijke toegang in de template en type-safety
  get hobbys(): FormArray {
    return this.form.get("hobbys") as FormArray;
  }

  addHobby() {
    this.hobbys.push(this.fb.control(""));
  }

  verwijderHobby(index: number) {
    this.hobbys.removeAt(index);
  }
}
```

103. Wat is het verschil tussen setValue() en patchValue()?

- setValue(): Dwingt je om de structuur van het volledige formulier exact te vullen. Als je één sleutel (key) weglaat of te veel meegeeft, gooit Angular direct een runtime error. Veilig voor complete updates.
- patchValue(): Staat je toe om slechts een subset (een deel) van de formulierwaarden te updaten. Velden die je weglaat in het object worden simpelweg overgeslagen en behouden hun huidige waarde.

104. Wat betekenen de statussen pristine, dirty, untouched en touched?

Dit zijn booleans die Angular op elk formulierelement bijhoudt om interactie te tracken:

- pristine: De gebruiker heeft de waarde van het veld nog niet aangepast.
- dirty: De gebruiker heeft de waarde wel aangepast.
- untouched: De gebruiker heeft nog niet in het veld geklikt en er daarna buiten geklikt (geen blur event).
- touched: De gebruiker heeft het element wel bezocht en verlaten. Ideaal om pas validatiefouten te tonen als de gebruiker klaar is met typen.

105. Hoe schrijf je een Custom Synchrone Validator voor Reactive Forms?

     Een custom validator is een functie die een AbstractControl accepteert en ofwel null teruggeeft (als de invoer geldig is), ofwel een ValidationErrors object (als de invoer ongeldig is).

```typescript
import { AbstractControl, ValidationErrors, ValidatorFn } from "@angular/forms";

// Validator die controleert of een invoer een verboden woord bevat
export function verbodenWoordValidator(woord: RegExp): ValidatorFn {
  return (control: AbstractControl): ValidationErrors | null => {
    const verboden = woord.test(control.value);
    return verboden ? { verbodenWoord: { value: control.value } } : null;
  };
}
```

106. Wat is een Asynchrone Validator (AsyncValidatorFn) en hoe implementeer je deze?

     Asynchrone validators worden gebruikt wanneer de validatie afhankelijk is van een externe bron (zoals een API-check om te zien of een gebruikersnaam al bezet is). Ze moeten een Observable of Promise teruggeven die uiteindelijk ValidationErrors | null emitteert.

```typescript
import { inject, Injectable } from "@angular/core";
import {
  AbstractControl,
  AsyncValidatorFn,
  ValidationErrors,
} from "@angular/forms";
import { HttpClient } from "@angular/common/http";
import { catchError, map, Observable, of } from "rxjs";

@Injectable({ providedIn: "root" })
export class UsernameValidator {
  private http = inject(HttpClient);

  check(): AsyncValidatorFn {
    return (control: AbstractControl): Observable<ValidationErrors | null> => {
      return this.http
        .get<boolean>(`/api/users/check?name=${control.value}`)
        .pipe(
          map((isBezet) => (isBezet ? { usernameBezet: true } : null)),
          catchError(() => of(null)), // Bij API-fout, valideer voor de zekerheid toch goed
        );
    };
  }
}
```

107. Waarom draaien asynchrone validators pas nadat de synchrone validators zijn geslaagd?

     Dit is een ingebouwde prestatie-optimalisatie van Angular. Omdat asynchrone validators vaak netwerkverzoeken (HTTP-calls) doen, wil je de server niet onnodig belasten. Angular wacht daarom tot alle lokale, snelle synchrone checks (zoals Validators.required of Validators.minLength) 100% groen zijn voordat het de zware asynchrone verzoeken afvuurt.

108. Hoe kun je de waarde van een formulier live observeren met RxJS?

     Elke FormControl, FormGroup en FormArray heeft een valueChanges property. Dit is een RxJS Observable die bij elke toetsaanslag of verandering de nieuwste waarde streamt. Dit is ideaal in combinatie met operators zoals debounceTime om zoekopdrachten te vertragen.

```typescript
this.form
  .get("zoekTerm")
  ?.valueChanges.pipe(debounceTime(400), distinctUntilChanged())
  .subscribe((zoekterm) => {
    this.voerZoekopdrachtUit(zoekterm);
  });
```

109. Hoe converteer je formulier-statussen of waarden naar Angular Signals?

     In moderne Angular architecturen wil je formuliergegevens vaak direct koppelen aan het reactieve Signal-ecosysteem. Angular biedt hiervoor de functies valueAsSignal() en statusAsSignal() (beschikbaar in recente versies) of je gebruikt toSignal uit @angular/core/rxjs-interop:

```typescript
import { toSignal } from "@angular/core/rxjs-interop";

// Maakt een live read-only Signal van de formulierwaarde
formulierWaarde = toSignal(this.loginForm.valueChanges, {
  initialValue: this.loginForm.value,
});
```

110. Wat is de interface ControlValueAccessor (CVA) en waarom is het essentieel?

     De ControlValueAccessor interface fungeert als de brug/adapter tussen een custom Angular component (bijv. een prachtig vormgegeven custom slider of toggle-knop) en de officiële Angular Forms API. Door CVA te implementeren, kan jouw eigen component naadloos meedraaien met formControlName of [(ngModel)].

111. Welke vier methoden moet een klasse implementeren voor ControlValueAccessor?

- writeValue(value: any): Wordt door Angular aangeroepen om de waarde vanuit de code naar de UI van jouw component te pushen.
- registerOnChange(fn: any): Geeft je een callback-functie die jij moet aanroepen zodra de gebruiker de waarde in jouw UI aanpast.
- registerOnTouched(fn: any): Geeft je een callback die je moet aanroepen zodra de gebruiker interactie heeft gehad met de component (blur).
- setDisabledState(isDisabled: boolean): (Optioneel) Wordt aangeroepen wanneer Angular de status van het formulierveld verandert naar disabled of enabled.

112. Hoe registreer je een CVA component correct in de @Component providers?

     Om ervoor te zorgen dat de Forms API jouw component herkent, moet je de component registreren onder de ingebouwde NG_VALUE_ACCESSOR token. Je gebruikt hiervoor forwardRef om initialisatieproblemen te voorkomen.

```typescript
import { Component, forwardRef } from "@angular/core";
import { NG_VALUE_ACCESSOR, ControlValueAccessor } from "@angular/forms";

@Component({
  selector: "app-custom-toggle",
  standalone: true,
  providers: [
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: forwardRef(() => CustomToggleComponent),
      multi: true,
    },
  ],
  template: `<button (click)="toggle()">{{ state ? "AAN" : "UIT" }}</button>`,
})
export class CustomToggleComponent implements ControlValueAccessor {
  state = false;
  onChange = (val: boolean) => {};
  onTouched = () => {};

  toggle() {
    this.state = !this.state;
    this.onChange(this.state); // Update Angular Forms
    this.onTouched();
  }

  writeValue(value: boolean): void {
    this.state = value;
  }
  registerOnChange(fn: any): void {
    this.onChange = fn;
  }
  registerOnTouched(fn: any): void {
    this.onTouched = fn;
  }
}
```

113. Wat is Cross-Field Validation en hoe valideer je de relatie tussen twee velden?

     Cross-field validation wordt toegepast wanneer de geldigheid van het ene veld afhangt van de waarde van een ander veld (bijvoorbeeld: 'Wachtwoord' en 'Herhaal Wachtwoord' moeten identiek zijn). Je koppelt deze validator aan de FormGroup in plaats van aan een individuele FormControl.

```typescript
import { AbstractControl, ValidationErrors, ValidatorFn } from "@angular/forms";

export const wachtwoordenMatchenValidator: ValidatorFn = (
  control: AbstractControl,
): ValidationErrors | null => {
  const wachtwoord = control.get("wachtwoord");
  const herhaalWachtwoord = control.get("herhaalWachtwoord");

  return wachtwoord &&
    herhaalWachtwoord &&
    wachtwoord.value !== herhaalWachtwoord.value
    ? { wachtwoordenMatchenNiet: true }
    : null;
};

// Koppelen bij de creatie:
// this.fb.group({ ... }, { validators: [wachtwoordenMatchenValidator] });
```

114. Wat doet de optie updateOn en welke drie modi kent het?

     De updateOn optie bepaalt op welk moment Angular de validatie en de status-updates van een formulier of veld uitvoert. Je kunt dit configureren bij het aanmaken van een control:

- 'change' (Standaard): Valideert direct bij elke toetsaanslag.
- 'blur': Voert de validatie pas uit zodra de gebruiker uit het invoerveld klikt. Ideaal om flikkerende foutmeldingen tijdens het typen te voorkomen.
- 'submit': Valideert pas op het moment dat de gebruiker het formulier verstuurt.

115. Hoe kun je programmatisch validatieregels dynamisch toevoegen of slopen?

     Je kunt tijdens runtime de validatie-eisen van een veld aanpassen met de methoden setValidators() en clearValidators(). Belangrijk is dat je daarna .updateValueAndValidity() aanroept om Angular te dwingen de status direct opnieuw te berekenen.

```typescript

wijzigNaarVerplicht() {
    const veld = this.form.get('bedrijfsnaam');
    veld?.setValidators([Validators.required]);
    veld?.updateValueAndValidity(); // Cruciale stap!
}
```

116. Hoe ga je om met geneste objectstructuren in Reactive Forms?

     Voor complexe datamodellen met geneste structuren kun je FormGroup objecten in elkaar nesten. Dit houdt je code overzichtelijk en modulair georganiseerd.

```typescript
this.form = this.fb.group({
  naam: ["", Validators.required],
  adres: this.fb.group({
    // Geneste FormGroup
    straat: ["", Validators.required],
    postcode: ["", Validators.required],
  }),
});

// Waarde uitlezen: this.form.value.adres.straat;
```

117. Wat is het gevaar van het direct muteren van formulierwaarden buiten de Forms API om?

     Wanneer je de interne objecten van form.value direct via JavaScript muteert (this.form.value.naam = 'Jan'), omzeil je de interne status-tracking van Angular. Het formulier raakt corrupt: de UI wordt niet geüpdatet, validaties draaien niet en statussen zoals dirty kloppen niet meer. Gebruik altijd patchValue() of setValue().

118. Hoe disable of enable je een formulierveld op de juiste manier?

     Het rechtstreeks toevoegen van het disabled attribuut in de HTML-template bij een reactive formulier genereert een console-waarschuwing. Je moet dit gedrag programmatisch sturen in je TypeScript klasse:

```typescript
// Veld deactiveren
this.form.get("email")?.disable();

// Veld weer activeren
this.form.get("email")?.enable();
```

119. Waarom verdwijnen gedisabled velden uit form.value en hoe los je dit op?

     Wanneer een FormControl op disabled staat, wordt de waarde ervan automatisch weggelaten uit het standaard this.form.value object. Dit is ingebouwd gedrag van Angular. Als je de complete dataset inclusief de uitgeschakelde velden wilt exporteren naar een API-verzoek, gebruik je .getRawValue():

```typescript
// Geeft ALTIJD alle velden terug, ongeacht of ze disabled zijn
const completeData = this.form.getRawValue();
```

120. Hoe reset je een formulier correct zonder dat er validatiefouten oplichten?

     Als je .reset() aanroept op een formulier, worden alle waarden geleegd (of teruggezet naar de non-nullable defaults). Tegelijkertijd zet Angular de statussen weer terug naar pristine en untouched. Hierdoor verdwijnen eventuele foutmeldingen in de UI en staat het formulier er weer bij alsof het net voor het eerst geladen is.

```typescript

verwerkEnWis() {
    // Verwerk data...
    this.form.reset(); // Volledig schoonpoetsen van waarden en status
}

```

## Services en Dependency Injection (121-140)

121. Wat is de primaire rol van een Service in Angular?

     Een Service is een klasse die specifiek is ontworpen om herbruikbare businesslogica, databeheer, API-communicatie of state-management te centraliseren. Het scheidt deze logica van de presentatielaag (de componenten). Hierdoor blijven componenten slank, makkelijk te onderhouden en uitsluitend verantwoordelijk voor de gebruikersinterface.

122. Wat is Dependency Injection (DI) en hoe werkt het in Angular?

     Dependency Injection is een ontwerppatroon waarbij een klasse zijn benodigde afhankelijkheden (zoals services) niet zelf aanmaakt via new Klasse(), maar deze van buitenaf aangeleverd krijgt. Angular heeft een ingebouwd, krachtig DI-framework dat automatisch de juiste instanties opzoekt, aanmaakt en injecteert op basis van een gecentraliseerd registersysteem (de Injector).

123. Wat doet de @Injectable() decorator?

     De @Injectable() decorator markeert een klasse als een onderdeel dat kan worden opgenomen in het Angular DI-systeem. Dit vertelt de Angular-compiler dat deze klasse dependencies mag ontvangen en zelf als dependency geïnjecteerd mag worden in andere componenten, directives of services.

124. Wat is de betekenis van providedIn: 'root'?

     Wanneer je { providedIn: 'root' } meegeeft aan de @Injectable() decorator, registreer je de service in de root-injector van de applicatie. Dit heeft twee gigantische voordelen:
     De service wordt een Singleton: er bestaat gedurende de hele levensduur van de applicatie slechts één enkele, gedeelde instantie van.
     De service is Tree-shakable: als de service nergens in de applicatie daadwerkelijk wordt geïmporteerd of gebruikt, wordt de code tijdens de productie-build automatisch verwijderd uit de JavaScript-bundel.

```typescript
@Injectable({
  providedIn: "root", // Singleton en tree-shakable
})
export class ConfigService {}
```

125. Hoe werkt de moderne inject() functie versus de traditionele constructor-injectie?

     Sinds Angular 14+ kun je de runtime-functie inject() gebruiken om afhankelijkheden op te halen. Dit heeft de constructor-injectie grotendeels vervangen. De inject() functie mag alleen worden aangeroepen binnen een injection context, zoals tijdens de initialisatie van class properties of in de constructor.

```typescript
// Moderne, aanbevolen aanpak (Property Initialization)
export class GebruikerComponent {
  private dataService = inject(DataService);
}

// Traditionele aanpak (Constructor)
export class GebruikerComponent {
  constructor(private dataService: DataService) {}
}
```

126. Welke voordelen biedt de inject() functie op architectuurniveau?

     De inject() functie biedt unieke voordelen ten opzichte van constructor-injectie:
     Betere compositie: Je kunt functionele, herbruikbare logica schrijven buiten klassen om (zoals custom RxJS operators of route guards die services nodig hebben).
     Geen type-duplicatie: Geen ellenlange constructors meer met private parameters.
     Eenvoudigere overerving: Bij overerving (extends) hoef je de afhankelijkheden niet meer handmatig door te geven aan super().

127. Wat is de hiërarchische structuur van Angular's DI-systeem?

     Angular maakt gebruik van een hiërarchisch (gelaagd) injectorsysteem. Als een component een service aanvraagt, zoekt Angular eerst in de lokale injector van die component zelf. Wordt de service daar niet gevonden, dan reist de zoekopdracht omhoog naar de parent-componenten, vervolgens naar de Environment/Route-injector, en uiteindelijk naar de Root-injector. Wordt de service nergens gevonden, dan gooit Angular een NullInjectorError.

128. Wat gebeurt er als je een service registreert in de providers array van een Component?

     Wanneer je een service toevoegt aan de providers array van een @Component, verbreek je het Singleton-gedrag voor dat specifieke deel van de app. Angular maakt nu een gloednieuwe, unieke instantie van de service aan die exclusief toebehoort aan deze component en zijn onderliggende kind-componenten. Zodra de component wordt vernietigd, wordt ook deze service-instantie vernietigd.

```typescript
@Component({
  selector: "app-editor",
  standalone: true,
  providers: [LocalHistoryService], // Unieke instantie voor elke editor-component
  template: `...`,
})
export class EditorComponent {
  private history = inject(LocalHistoryService);
}
```

129. Wat is de rol van viewProviders in een component?

     viewProviders lijkt sterk op providers, maar met een belangrijk verschil in bereik (scope). Services die in viewProviders zijn gedefinieerd, zijn uitsluitend beschikbaar voor de elementen in de eigen template van de component. Ze zijn onzichtbaar voor componenten of HTML die via Content Projection (`<ng-content>`) van buitenaf in de component zijn geprojecteerd.

130. Hoe werken de DI Resolution Modifiers (@Optional, @Self, @SkipSelf, @Host) met inject()?

     Resolution modifiers veranderen de manier waarop Angular door de hiërarchie zoekt naar een service. In moderne code geef je deze mee als opties-object aan de inject() functie:
     - optional: true: Voorkomt een crash als de service niet bestaat; geeft null terug.
     - self: true: Zoekt alleen in de injector van de component zelf, niet daarboven.
     - skipSelf: true: Slaat de eigen injector over en begint direct te zoeken bij de ouders.
     - host: true: Zoekt tot en met het host-element van de huidige component.

```typescript
// Voorbeeld met moderne modifiers
const logger = inject(LoggerService, { optional: true, skipSelf: true });
```

131. Wat is een InjectionToken en wanneer gebruik je deze?

     Je kunt alleen TypeScript klassen rechtstreeks als DI-token gebruiken. Als je interfaces, configuratie-objecten of simpele strings wilt injecteren, kan dat runtime niet op basis van het type (aangezien types verdwijnen na compilatie). Een InjectionToken maakt een uniek, runtime-beschikbaar object aan dat als identificatie (sleutel) fungeert binnen het DI-systeem.

```typescript
import { InjectionToken } from "@angular/core";

export interface AppConfig {
  apiUrl: string;
}
// Creatie van het token
export const APP_CONFIG = new InjectionToken<AppConfig>("app.config");

// Registratie in app.config.ts:
// providers: [{ provide: APP_CONFIG, useValue: { apiUrl: '/api' } }]
```

132. Hoe injecteer je een InjectionToken met de inject() functie?

     Omdat een InjectionToken een javascript-variabele is, kun je deze rechtstreeks aanrekenen in de inject() functie om de gekoppelde waarde op te vragen:

```typescript
export class ApiService {
  // Direct en type-veilig uitlezen van de token-waarde
  private config = inject(APP_CONFIG);

  getData() {
    console.log(this.config.apiUrl);
  }
}
```

133. Wat is het verschil tussen useClass, useValue, useExisting en useFactory?

     Dit zijn de vier manieren (provider-strategieën) om een token te koppelen aan een concrete waarde:
     - useClass: Vertelt Angular om een nieuwe instantie van een opgegeven klasse aan te maken.
     - useValue: Koppelt het token aan een statische, kant-en-klare waarde (zoals een configuratie-object).
     - useExisting: Maakt een alias aan naar een ander, reeds geregistreerd token.
     - useFactory: Gebruikt een op maat gemaakte functie om de waarde dynamisch te berekenen. Perfect voor scenario's waarbij je eerst parameters moet controleren.

134. Hoe implementeer je de useFactory provider met dependencies?

     Met useFactory kun je runtime bepalen welke service wordt teruggegeven. Je kunt afhankelijkheden aan de factory meegeven via de deps array:

```typescript
export const loggerProvider = {
  provide: LoggerService,
  useFactory: (config: AppConfig) => {
    // Dynamische beslissing op basis van configuratie
    return config.production ? new ProductionLogger() : new DebugLogger();
  },
  deps: [APP_CONFIG], // Afhankelijkheden voor de factory-functie
};
```

135. Wat betekent de optie multi: true bij een provider?

     Standaard overschrijft een nieuwe provider met hetzelfde token de vorige registratie. Als je multi: true instelt, vertelt dit Angular dat dit token een array van waarden bevat. Elke nieuwe provider met multi: true voegt zijn waarde toe aan deze lijst. Dit wordt intensief gebruikt voor plug-in architecturen, zoals HTTP-interceptors of validatietoken-systemen.

```typescript
// Meerdere interceptors registreren onder hetzelfde ingebouwde token
providers: [
  { provide: HTTP_INTERCEPTORS, useClass: AuthInterceptor, multi: true },
  { provide: HTTP_INTERCEPTORS, useClass: ErrorInterceptor, multi: true },
];
```

136. Hoe maak je een asynchrone initialisatie-service met APP_INITIALIZER?

     De APP_INITIALIZER is een speciaal ingebouwd multi-token. Hiermee kun je code uitvoeren voordat de Angular-applicatie volledig opstart (bijvoorbeeld om eerst kritieke instellingen of gebruikersrechten van de server op te halen). Angular wacht met booten tot de geretourneerde Promise of Observable is afgerond.

```typescript
import { APP_INITIALIZER, inject } from "@angular/core";

function initConfigFactory(configService: ConfigService) {
  return () => configService.loadRemoteConfig(); // Moet een Promise of Observable retourneren
}

// In app.config.ts:
providers: [
  {
    provide: APP_INITIALIZER,
    useFactory: initConfigFactory,
    deps: [ConfigService],
    multi: true,
  },
];
```

137. Kunnen services geheugenlekken (Memory Leaks) veroorzaken?

     Ja. Een service met providedIn: 'root' leeft gedurende de gehele applicatiecyclus en wordt nooit automatisch vernietigd. Als zo'n root-service zich abonneert (.subscribe()) op een oneindige Observable van een externe bron, of als een component een referentie (callback) achterlaat in de service, blijft die data onnodig in het geheugen hangen. Gebruik Signals of zorg voor handmatige opruiming.

138. Bestaat er een lifecycle hook zoals ngOnInit binnen een Service?

     Nee. Services kennen geen visuele weergave en ondersteunen daarom de meeste component lifecycle hooks (zoals ngOnInit, ngOnChanges of ngAfterViewInit) niet. De enige officiële lifecycle hook die een service ondersteunt is ngOnDestroy.

139. Hoe werkt ngOnDestroy binnen een Service?

     Als een service is geregistreerd op component-niveau (dus niet als root-singleton), zal Angular de ngOnDestroy methode van de service automatisch aanroepen zodra de betreffende component uit de DOM wordt gesloopt. Dit is de ideale plek om openstaande RxJS abonnementen of websockets netjes te sluiten.

```typescript
@Injectable()
export class LiveChatService implements OnDestroy {
  private socketSubscription = new Subscription();

  ngOnDestroy() {
    this.socketSubscription.unsubscribe(); // Voorkom actieve datastromen in de achtergrond
  }
}
```

140. Hoe isoleer en mock je een Service in een Unit Test met de moderne architectuur?

     Bij het testen van een component wil je de echte HTTP-services isoleren (mocken) zodat je test onafhankelijk en snel draait. Je kunt een service eenvoudig vervangen in de TestBed configuratie met behulp van useValue.

```typescript
describe("GebruikerComponent", () => {
  let component: GebruikerComponent;
  // Nep-implementatie van de service
  const mockDataService = {
    getGebruikers: () => of([{ id: "1", naam: "Mock Jan" }]),
  };

  beforeEach(async () => {
    await TestBed.configureTestingModule({
      imports: [GebruikerComponent],
      providers: [
        // Vervang de echte service door de mock variant
        { provide: DataService, useValue: mockDataService },
      ],
    }).compileComponents();
  });
});
```

## Routing (141-155)

141. Wat is de primaire taak van de Angular Router?

     De Angular Router is verantwoordelijk voor het navigeren tussen verschillende weergaven (componenten) binnen een Single Page Application (SPA). Hij observeert de URL in de adresbalk van de browser en vertaalt deze naar de juiste componentenboom, zonder dat de browser de complete pagina opnieuw hoeft op te vragen bij de server.

142. Hoe configureer je de router in een moderne Standalone applicatie?

     In moderne Angular-apps zonder NgModule definieer je een array van Route objecten en registreer je deze in app.config.ts via de helper-functie provideRouter().

```typescript
// app.routes.ts
import { Routes } from "@angular/router";
import { HomeHeuvelComponent } from "./home.component";

export const routes: Routes = [
  { path: "", component: HomeHeuvelComponent },
  {
    path: "over-ons",
    loadComponent: () =>
      import("./over-ons.component").then((m) => m.OverOnsComponent),
  },
];

// app.config.ts
export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes)],
};
```

143. Wat is de rol van de <router-outlet> tag?

     De <router-outlet> is een ingebouwde directive die fungeert als een dynamische placeholder in je HTML-template. Angular weet dat hij de component die gekoppeld is aan de actieve URL exact op de plek van deze tag moet renderen. Je kunt meerdere outlets nesten voor complexe lay-outs.

144. Hoe werkt Lazy Loading op componentniveau met loadComponent?

     In plaats van een component direct te importeren (wat ervoor zorgt dat de code direct in de hoofd-bundel belandt), gebruik je de loadComponent property in combinatie met een JavaScript import() expressie. De browser downloadt de code van die specifieke component pas op het moment dat de gebruiker de route daadwerkelijk bezoekt.

```typescript

{
path: 'dashboard',
loadComponent: () => import('./dashboard/dashboard.component').then(m => m.DashboardComponent)
}
```

145. Wat is het verschil tussen RouterLink en programmatische navigatie via de Router service?

     RouterLink: Een HTML-directive (routerLink="/profiel") die je rechtstreeks op <a> of <button> tags plaatst. Het zorgt ervoor dat de klik correct wordt onderschept door de router en is essentieel voor SEO en toegankelijkheid (A11y).
     Programmatische navigatie: Het aanroepen van de Router service in je TypeScript-logica met this.router.navigate(['/profiel']). Dit gebruik je wanneer de navigatie pas mag plaatsvinden nadat er een actie is voltooid, zoals een succesvolle API-call of formulier-validatie.

```typescript
export class ActieComponent {
  private router = inject(Router);

  gaVerder() {
    // Logica...
    this.router.navigate(["/volgende-stap"]);
  }
}
```

146. Hoe definieer en lees je een Dynamische Route Parameter (:id) uit?

     Je kunt variabelen opnemen in je route-pad door er een dubbele punt voor te zetten. Om deze parameter uit te lezen, injecteer je de ActivatedRoute service, die de parameters aanbiedt als een RxJS Observable of via een synchrone momentopname (snapshot).

```typescript
// Route definitie: { path: 'gebruiker/:id', component: DetailComponent }

export class DetailComponent implements OnInit {
  private route = inject(ActivatedRoute);

  ngOnInit() {
    // Manier 1: Live stream (reageert als de ID verandert terwijl de component open blijft)
    this.route.paramMap.subscribe((params) => {
      const id = params.get("id");
    });

    // Manier 2: Eenmalige momentopname
    const directId = this.route.snapshot.paramMap.get("id");
  }
}
```

147. Wat is de moderne 'Component Input Binding' voor route parameters?

     In recente versies van Angular kun je route-parameters, query-parameters en route-data rechtstreeks als reguliere component-inputs ontvangen. Dit elimineert de noodzaak om ActivatedRoute handmatig te injecteren. Je activeert dit door withComponentInputBinding() mee te geven aan provideRouter.

```typescript
// app.config.ts
providers: [provideRouter(routes, withComponentInputBinding())];

// Binnen DetailComponent ({ path: 'gebruiker/:id' })
export class DetailComponent {
  // Angular vult dit signaal automatisch met de waarde uit de URL!
  id = input.required<string>();
}
```

148. Hoe ga je om met Query Parameters (?search=angular)?

     Query-parameters zijn optionele parameters aan het einde van een URL. In tegenstelling herhaalde route-parameters veranderen ze de actieve route-component zelf niet. Je geeft ze mee via queryParams en leest ze uit via queryParamMap.

```typescript
// Navigeren met query params: /zoeken?categorie=boeken
this.router.navigate(["/zoeken"], { queryParams: { categorie: "boeken" } });

// Uitlezen:
this.route.queryParamMap.subscribe((params) => {
  const cat = params.get("categorie");
});
```

149. Wat is de taak van Route Guards?

     Route Guards beschermen routes tegen onbevoegde toegang. Ze controleren runtime of een gebruiker aan bepaalde criteria voldoet (zoals ingelogd zijn of de juiste admin-rechten bezitten) voordat de router de gevraagde component daadwerkelijk inlaadt of activeert.

150. Hoe schrijf je een moderne Functional Route Guard (CanActivateFn)?

     Klasse-gebaseerde guards (die interfaces implementeerden) zijn volledig uitgefaseerd. We gebruiken nu compacte, functionele guards. Omdat het functies zijn, gebruik je de inject() functie om services binnen te halen.

```typescript
import { inject } from "@angular/core";
import { CanActivateFn, Router } from "@angular/router";
import { AuthService } from "./auth.service";

export const secureGuard: CanActivateFn = (route, state) => {
  const authService = inject(AuthService);
  const router = inject(Router);

  if (authService.isAuthenticated()) {
    return true; // Toegang toegestaan
  }

  // Toegang geweigerd, stuur door naar login
  return router.createUrlTree(["/login"]);
};

// Koppelen in routes: { path: 'admin', component: AdminComponent, canActivate: [secureGuard] }
```

151. Wat is het verschil tussen canActivate, canActivateChild en canDeactivate?

     canActivate: Bepaalt of de specifieke route zelf geactiveerd mag worden.
     canActivateChild: Controleert centraal of de onderliggende sub-routes (children) van deze route geactiveerd mogen worden.
     canDeactivate: Controleert of de gebruiker de huidige route mag verlaten. Dit is ideaal om een waarschuwing te tonen als een gebruiker een formulier half heeft ingevuld en per ongeluk weg wil klikken ("Weet je zeker dat je onopgeslagen wijzigingen wilt weggooien?").

152. Wat is een Resolver (ResolveFn) en waarom gebruik je deze?

     Een Resolver haalt asynchrone data (zoals een API-verzoek) op voordat de router de component op het scherm toont. Dit voorkomt dat een gebruiker een halflege pagina of een flikkerende lay-out ziet terwijl de data nog onderweg is. De opgevraagde data wordt direct meegeleverd bij het laden van de component.

```typescript
import { inject } from "@angular/core";
import { ResolveFn } from "@angular/router";
import { DataService } from "./data.service";

export const productResolver: ResolveFn<Product> = (route, state) => {
  const id = route.paramMap.get("id")!;
  return inject(DataService).getProduct(id); // Router wacht tot deze call klaar is
};

// Koppelen: { path: 'product/:id', component: ProdComponent, resolve: { product: productResolver } }
```

153. Wat is de betekenis van de pathMatch: 'full' configuratie?

     Bij het configureren van een redirect (omleiding) is pathMatch: 'full' essentieel. Het vertelt de router dat de URL exact moet matchen met het opgegeven pad. Zonder deze optie matcht een leeg pad path: '' met elke willekeurige URL (omdat elk pad begint met niks), wat resulteert in een oneindige redirect-lus.

```typescript

{ path: '', redirectTo: '/home', pathMatch: 'full' }
```

154. Hoe vang je niet-bestaande pagina's op (Wildcard Route)?

     Om te voorkomen dat de app crasht als een gebruiker een typefout maakt in de URL, definieer je helemaal onderaan je route-array een wildcard route met twee sterretjes (). Deze route vangt alles op wat niet door de eerdere regels is afgedekt en toont bijvoorbeeld een NotFoundComponent.

```typescript
export const routes: Routes = [
  { path: "home", component: HomeComponent },
  // ... alle andere routes ...
  { path: "**", component: NotFoundComponent }, // Moet ALTIJD als allerlaatste staan!
];
```

155. Hoe werkt de routeringstrategie 'HashLocationStrategy' versus HTML5 PushState?

     HTML5 PushState (Standaard): Genereert natuurlijke URL's zonder vreemde tekens (bijv. [domain.com/dashboard](https://domain.com/dashboard)). Dit vereist dat de productieserver zo is geconfigureerd dat hij bij elke aanvraag index.html teruggeeft (URL rewriting).
     HashLocationStrategy: Maakt gebruik van een hashtag in de URL ([domain.com/#/dashboard](https://domain.com/#/dashboard)). Het deel achter de # wordt nooit naar de server gestuurd. Dit is een handige fallback als je geen controle hebt over de serverconfiguratie of als je de app direct vanaf een lokaal bestandssysteem moet draaien. Je activeert dit via provideRouter(routes, withHashLocation()).

## RxJS en Observables (156-170)

156. Wat is RxJS en waarom is het fundamenteel voor Angular?

     RxJS (Reactive Extensions for JavaScript) is een bibliotheek voor reactief programmeren die gebruikmaakt van Observables. Het is ontworpen om asynchrone datastromen (zoals HTTP-verzoeken, events, websockets en gebruikersinvoer) te transformeren, combineren en beheren. RxJS is fundamenteel voor Angular omdat de kernarchitectuur van het framework (zoals de HttpClient, forms en de router) volledig is gebouwd rondom deze streams.

157. Wat is het fundamentele verschil tussen een Promise en een Observable?

     Hoewel beide worden gebruikt voor asynchrone logica, verschillen ze op cruciale punten:

158. Wat is het verschil tussen een 'Cold' en een 'Hot' Observable?

     Cold Observable: Begint pas met het produceren van data op het moment dat er een subscriber is. Elke subscriber krijgt zijn eigen, unieke datastroom vanaf het begin te zien (bijv. een HttpClient request).
     Hot Observable: Produceert data onafhankelijk van het aantal subscribers. De databron leeft buiten de observable (bijv. muisbewegingen of een live websocket). Nieuwe subscribers horen de stroom pas vanaf het moment dat ze aanhaken en missen eerdere emissies.

159. Wat is de rol van een Subject en hoe verschilt deze van een reguliere Observable?

     Een reguliere Observable is uitsluitend unicast (één producer per subscriber) en passief (je kunt er van buitenaf geen data in duwen). Een Subject is een speciale variant die zowel een Observable (je kunt erop subscriben) als een Observer (je kunt er data in duwen met .next()) is. Bovendien is een Subject multicast: hij deelt één enkele executie met meerdere subscribers tegelijk.

160. Wat zijn de specifieke verschillen tussen Subject, BehaviorSubject en ReplaySubject?

     Subject: Heeft geen beginwaarde. Subscribers ontvangen alleen waarden die na hun inschrijving worden uitgezonden.
     BehaviorSubject: Vereist een verplichte beginwaarde. Nieuwe subscribers krijgen direct bij inschrijving de allerlaatste uitgezonden waarde (of de beginwaarde) te horen. Je kunt de huidige waarde ook synchroon opvragen via .value.
     ReplaySubject: Onthoudt een specifiek aantal oude waarden (buffer). Nieuwe subscribers krijgen direct die set aan historische waarden om de oren, ongeacht hoe lang geleden ze zijn uitgezonden.

161. Wat doet de pipe() functie in RxJS?

     De pipe() functie is de montagestroomlijn van je datastroom. Het stelt je in staat om pure functies (operators) achter elkaar te schakelen om de data stap voor stap te transformeren, filteren of combineren voordat deze de uiteindelijke subscriber bereikt.

```typescript
// De datastroom transformeren via een pipe
const gefilterdeStream$ = dezeStream$.pipe(
  filter((waarde) => waarde !== null),
  map((waarde) => waarde.toString().toUpperCase()),
);
```

162. Wat is het verschil tussen de transformatie-operators map, switchMap, mergeMap en concatMap?

     Dit zijn de zogenaamde Flattening Operators, gebruikt wanneer een operator een nieuwe Observable teruggeeft:
     map: Transformeert een waarde direct naar een andere waarde (geen Observable).
     switchMap: Schakelt direct over naar de nieuwste Observable en annuleert onmiddellijk alle voorgaande, nog lopende Observables. Perfect voor zoek-autocompletes.
     mergeMap (flatMap): Start alle binnenkomende Observables tegelijkertijd en verwerkt ze parallel. Volgorde van afronding maakt niet uit.
     concatMap: Wacht netjes tot de huidige Observable klaar is (complete) voordat hij de volgende in de wachtrij start. Behoudt de exacte volgorde.

163. Wanneer kies je voor exhaustMap?

     exhaustMap negeert alle nieuwe binnenkomende Observables zolang de huidige Observable nog bezig is. Pas wanneer de actieve stroom volledig is afgerond, staat hij open voor een nieuwe input. Dit is de ultieme operator voor een login- of verzendknop, om te voorkomen dat een gebruiker die dubbelklikt per ongeluk twee identieke API-verzoeken afvuurt.

164. Waarom is het handmatig subscriben in componenten gevaarlijk en hoe voorkom je geheugenlekken?

     Wanneer je handmatig .subscribe() aanroept in een component, blijft dat abonnement in het geheugen openstaan, zelfs als de component uit de DOM wordt verwijderd. Dit veroorzaakt geheugenlekken (memory leaks). Je kunt dit op drie moderne manieren oplossen:
     De Async Pipe (| async): Laat Angular het abonnement in de HTML-template beheren. Deze un-subscribet automatisch zodra de component sterft.
     takeUntilDestroyed(): De moderne v16+ operator die de stroom automatisch sluit zodra de huidige injection context (zoals een component) wordt vernietigd.
     RxJS interoperabiliteit via Signals: De stroom converteren naar een Signal (zie vraag 168).

```typescript
export class DataComponent {
  // Optie 2: Sluit automatisch af bij vernietiging van de component
  private data$ = inject(DataService).getStream().pipe(takeUntilDestroyed());
}
```

165. Hoe werkt foutafhandeling in RxJS met catchError?

     Fouten binnen een RxJS stream reizen omlaag naar de subscriber. Als een fout niet wordt opgevangen, klapt de hele stream definitief dicht. Met de catchError operator kun je de fout onderscheppen en een elegante fallback-datastroom (of een lege stream via EMPTY) retourneren, zodat de rest van de applicatie blijft draaien.

```typescript
const veiligeStream$ = apiCall$.pipe(
  catchError((error) => {
    console.error("API Fout:", error);
    return of(["Fallback data"]); // Herstelt de stream met veilige data
  }),
);
```

166. Wat doen de operators forkJoin, combineLatest en zip?

     Dit zijn combinatiestructuur-operators:
     forkJoin: Wacht tot alle opgegeven Observables zijn afgerond (complete) en zendt dan eenmalig de allerlaatste waarden uit als een array. Vergelijkbaar met Promise.all().
     combineLatest: Zodra elke stroom minimaal één waarde heeft uitgezonden, vuurt deze operator een nieuwe array af telkens wanneer een van de bronstromen een nieuwe waarde afgeeft.
     zip: Combineert waarden strikt op basis van index: de eerste waarde van stroom A met de eerste van stroom B, enzovoort.

167. Wat is het verschil tussen share() en shareReplay()?

     Beide veranderen een Cold Observable in een Hot (multicast) variant zodat een API-call niet twee keer wordt uitgevoerd bij twee subscribers.
     share(): Deelt de actieve executie. Als een nieuwe subscriber later inhaakt, mist deze de reeds uitgezonden waarden.
     shareReplay(1): Deelt de executie én cachet het opgegeven aantal waarden (in dit geval de laatste 1). Nieuwe subscribers krijgen direct de gecachte waarde te horen zonder dat de onderliggende bron opnieuw getriggerd hoeft te worden.

168. Hoe converteer je een RxJS Observable naar een Angular Signal?

     Sinds de introductie van Angular Signals kun je een RxJS stream naadloos omzetten naar een synchroon leesbaar Signal met de toSignal() helper uit @angular/core/rxjs-interop. Dit heft ook direct de noodzaak voor handmatige unsubscribes op.

```typescript
import { toSignal } from "@angular/core/rxjs-interop";

export class ProductenComponent {
  private productenService = inject(ProductenService);

  // Converteert de Observable$ direct naar een leesbaar, reactief Signal
  producten = toSignal(this.productenService.productenStream$, {
    initialValue: [],
  });
}
```

169. Hoe converteer je een Angular Signal terug naar een RxJS Observable?

     Als je juist gebruik wilt maken van krachtige RxJS operators (zoals debounceTime of switchMap) op een veranderend Signal, kun je het Signal transformeren naar een Observable met de toObservable() helper.

```typescript
import { toObservable } from "@angular/core/rxjs-interop";

export class ZoekComponent {
  zoekTerm = signal(""); // Een Angular Signal

  // Transformeer naar een Observable stream om asynchroon te debouncen
  zoekResultaten$ = toObservable(this.zoekTerm).pipe(
    debounceTime(300),
    distinctUntilChanged(),
    switchMap((term) => inject(ApiService).zoeken(term)),
  );
}
```

170. Wat is het gevaar van de tap operator?

     De tap operator is uitsluitend bedoeld voor het uitvoeren van side-effects (zoals loggen, een spinner aanzetten of data opslaan in een lokale variabele) zonder de data in de stroom zelf aan te passen. Het gevaar schuilt erin dat ontwikkelaars misbruik maken van tap om complexe businesslogica of datatransformaties door te voeren. Dit breekt het principe van pure, voorspelbare datastromen en maakt unit testing onnodig complex.

## Forms (171-185)

171. Wat zijn de twee formulier-architecturen binnen Angular en wanneer kies je welke?

     Angular biedt twee benaderingen voor het bouwen van formulieren:
     Template-driven Forms: De logica en validatie worden grotendeels in de HTML-template gedefinieerd met behulp van directives zoals [(ngModel)]. Dit is geschikt voor zeer eenvoudige formulieren (zoals een simpele inlogpagina) of snelle prototypes.
     Reactive Forms: De formulierstructuur en validatieregels worden programmatisch opgebouwd in de TypeScript-klasse. Dit is de industriestandaard voor enterprise-applicaties. Het biedt volledige controle, is uitstekend testbaar, ondersteunt complexe (asynchrone) validatie en maakt gebruik van reactieve RxJS-datastromen.

172. Wat zijn de kernklassen van Reactive Forms (FormGroup, FormControl, FormArray)?

     FormControl: Beheert de waarde, de validatiestatus (geldig/ongeldig) en de interactiehistorie (vuil/aangeraakt) van een individueel invoerveld.
     FormGroup: Groepeert een verzameling van FormControl (of andere FormGroup) objecten. De validatiestatus van de groep is afhankelijk van de status van de individuele kinderen.
     FormArray: Beheert een dynamische, lineaire lijst van formulier-elementen. Ideaal voor scenario's waarbij een gebruiker runtime extra velden kan toevoegen of verwijderen (zoals een lijst met e-mailadressen of telefoonnummers).

173. Wat zijn 'Typed Forms' in Angular en hoe verbeteren ze de type-veiligheid?

     Sinds Angular 14 zijn Reactive Forms standaard streng getypeerd (Strictly Typed). Dit betekent dat de FormGroup en FormControl exact weten welk type data ze bevatten. Dit voorkomt runtime-fouten en biedt volledige autocomplete-ondersteuning in je IDE.

```typescript
import { FormControl, FormGroup } from "@angular/forms";

// Angular leidt automatisch af dat dit type FormGroup<{ naam: FormControl<string | null> }> is
const profielForm = new FormGroup({
  naam: new FormControl("Jan"), // Type is afgeleid als string | null
  leeftijd: new FormControl<number>(25, { nonNullable: true }), // Altijd een getal, kan niet null worden gemaakt
});

// Type-safe uitlezen:
const naamWaarde: string | null = profielForm.controls.naam.value;
```

174. Hoe gebruik je de FormBuilder of NonNullableFormBuilder service?

     De FormBuilder is een syntactische suiker-service die het aanmaken van grote, complexe formulierstructuren korter en leesbaarder maakt. De NonNullableFormBuilder (of fb.nonNullable) zorgt er specifiek voor dat velden bij een .reset() actie terugvallen op hun initiële beginwaarde in plaats van null.

```typescript
import { FormBuilder, inject } from "@angular/forms";

export class ProfielComponent {
  private fb = inject(FormBuilder).nonNullable; // Gebruik de non-nullable variant

  profielForm = this.fb.group({
    gebruikersnaam: [""], // FormControl<string>
    adres: this.fb.group({
      stad: ["Amsterdam"],
    }),
    hobbies: this.fb.array<string>([]), // FormArray
  });
}
```

175. Hoe werkt ingebouwde synchrone validatie (Validators)?

     Validatoren zijn pure functies die een FormControl controleren. Angular levert een set ingebouwde functies mee via de Validators klasse, zoals required, minLength, pattern en email. Je geeft ze mee als tweede argument bij het initialiseren van een veld.

```typescript
const emailControl = new FormControl("", [
  Validators.required,
  Validators.email,
]);
```

176. Hoe toon je conditionele validatiefouten op een gebruiksvriendelijke manier in de template?

     Om te voorkomen dat een formulier direct rood kleurt zodra de pagina laadt, controleer je in de template of een veld zowel ongeldig (invalid) als aangeraakt (touched of dirty) is.

```html
<input [formControl]="emailControl" type="email" id="email" />

@if (emailControl.invalid && (emailControl.touched || emailControl.dirty)) {

<div class="foutmelding">
  @if (emailControl.errors?.['required']) { <span>E-mail is verplicht.</span> }
  @if (emailControl.errors?.['email']) {
  <span>Vul een geldig e-mailadres in.</span> }
</div>
}
```

177. Hoe schrijf je een Custom Validator functie?

     Een custom validator is een functie die een AbstractControl ontvangt. Als het veld geldig is, retourneert de functie null. Als het veld ongeldig is, retourneert de functie een object met een fout-sleutel (validation error object).

```typescript
import { AbstractControl, ValidationErrors, ValidatorFn } from "@angular/forms";

// Validator die controleert of een tekst het verboden woord 'admin' bevat
export function verbodenWoordValidator(): ValidatorFn {
  return (control: AbstractControl): ValidationErrors | null => {
    const isInvalide = control.value?.toLowerCase() === "admin";
    return isInvalide ? { verbodenWoord: { waarde: control.value } } : null;
  };
}
```

178. Wat is een Asynchrone Validator (AsyncValidatorFn) en wanneer gebruik je deze?

     Asynchrone validatoren worden gebruikt als de controle afhankelijk is van een externe bron, zoals een API-verzoek naar de server (bijvoorbeeld controleren of een gebruikersnaam al bezet is). Ze retourneren een Promise of een Observable die uiteindelijk een ValidationErrors of null uitzendt. Ze worden als derde argument meegegeven.

```typescript
import { AsyncValidatorFn } from "@angular/forms";
import { inject } from "@angular/core";

export const gebruikersnaamUniekValidator = (
  apiService: ApiService,
): AsyncValidatorFn => {
  return (control) => {
    return apiService.controleerNaam(control.value).pipe(
      map((isBezet) => (isBezet ? { naamBezet: true } : null)),
      catchError(() => of(null)), // Herstel bij netwerkfout, valideer milder
    );
  };
};
```

179. Wat is het verschil tussen setValue() en patchValue() op een FormGroup?

     setValue(): Vereist dat je alle velden binnen de FormGroup exact matcht en vult. Mis je één sleutel, dan gooit Angular direct een foutmelding. Dit is een strikte manier van updaten.
     patchValue(): Staat je toe om een selectief gedeelte van de FormGroup te updaten. Velden die je weglaat in het update-object worden ongewijzigd gelaten. Dit is flexibeler.

180. Hoe luister je reactief naar wijzigingen in een formulier met valueChanges?

     Zowel FormControl, FormGroup als FormArray bezitten een valueChanges property. Dit is een RxJS Observable die telkens een nieuwe waarde uitzendt zodra er ergens in het veld of de groep iets getypt of aangepast wordt.

```typescript
this.zoekForm.controls.zoekterm.valueChanges
  .pipe(
    debounceTime(400), // Wacht tot de gebruiker stopt met typen
    distinctUntilChanged(), // Vuur alleen af als de waarde écht anders is
    takeUntilDestroyed(), // Voorkom memory leaks
  )
  .subscribe((waarde) => {
    this.voerZoekopdrachtUit(waarde);
  });
```

181. Hoe beheer en muteer je een dynamische FormArray?

     Een FormArray stelt je in staat om programmatisch elementen toe te voegen of te verwijderen. Om dit type-safe in de template te gebruiken, is het aan te raden een getter te maken.

```typescript
export class ChecklistComponent {
  private fb = inject(FormBuilder);
  form = this.fb.group({
    taken: this.fb.array<string>([]),
  });

  get takenArray() {
    return this.form.controls.taken;
  }

  voegTaakToe() {
    this.takenArray.push(this.fb.control("", Validators.required));
  }

  verwijderTaak(index: number) {
    this.takenArray.removeAt(index);
  }
}
```

182. Hoe koppel je een dynamische FormArray correct in de HTML-template?

     Om een FormArray te renderen, loop je met de @for loop door de controls van de array heen en koppel je de index aan de formControlName.

```html
<form [formGroup]="form">
  <div formArrayName="taken">
    @for (taak of takenArray.controls; track $index) {
    <div>
      <!-- Koppel de control op basis van zijn numerieke index -->
      <input [formControlName]="$index" placeholder="Vul taak in..." />
      <button type="button" (click)="verwijderTaak($index)">Wis</button>
    </div>
    }
  </div>
  <button type="button" (click)="voegTaakToe()">Taak Toevoegen</button>
</form>
```

183. Wat betekenen de statussen pristine, dirty, untouched en touched?

     Dit zijn de interactievlaggen die Angular bijhoudt voor elk formulier-element:
     pristine: De gebruiker heeft de waarde in dit veld nog niet veranderd.
     dirty: De gebruiker heeft de waarde in het veld wél actief aangepast.
     untouched: De gebruiker heeft nog niet in het veld geklikt (en er weer uit geklikt).
     touched: Het veld heeft de focus verloren (blur event). De gebruiker is er dus in geweest en weer uit gegaan.

184. Hoe kun je de performance van grote formulieren optimalisieren met updateOn?

     Standaard valideert en streamt Angular elke wijziging bij elke toetsaanslag (change). Bij gigantische formulieren of zware asynchrone validatie kan dit voor vertraging zorgen. Je kunt dit gedrag per control of per groep aanpassen naar 'blur' (valideer pas zodra de gebruiker het veld verlaat) of 'submit' (valideer pas bij het verzenden).

```typescript
const naamControl = new FormControl("", {
  validators: [Validators.required],
  // Wijzig de updatestrategie voor betere prestaties
  updateOn: "blur",
});
```

185. Hoe integreer je Reactive Forms naadloos met Angular Signals?

     Hoewel formulieren zelf nog op RxJS (valueChanges) leunen, kun je hun status of waarde heel eenvoudig omzetten naar een synchroon leesbaar Signal via de toSignal helper. Hierdoor sluit je formulier-interactie naadloos aan op de moderne, fijnmazige rendering van Angular.

```typescript
import { toSignal } from "@angular/core/rxjs-interop";

export class ProfielComponent {
  profielForm = new FormGroup({
    naam: new FormControl(""),
  });

  // Een direct leesbaar Signal van de formulierwaarde
  formWaardeSignal = toSignal(this.profielForm.valueChanges);

  // Een computed Signal dat live berekent of de verzendknop actief mag zijn
  isVerzendKnopGereed = computed(() => {
    const huidigeWaarde = this.formWaardeSignal();
    return huidigeWaarse?.naam && huidigeWaarde.naam.length > 2;
  });
}
```

## HTTP Client (186-200)

186. Wat is de rol van de HttpClient in Angular en hoe verschilt deze van de native fetch() API?

     De HttpClient is Angular's ingebouwde service voor het afhandelen van HTTP-verzoeken. In tegenstelling tot de native fetch() API van de browser, die op Promises leunt, is de HttpClient volledig ontworpen rondom RxJS Observables. Dit biedt ingebouwde voordelen zoals:
     Het eenvoudig kunnen annuleren (cancelen) van openstaande verzoeken bij componentvernietiging.
     Geavanceerde interceptie (onderschepping) van verzoeken en antwoorden via middleware.
     Ingebouwde, diepgaande testfaciliteiten zonder externe libraries.
     Automatische JSON-transformatie.

187. Hoe configureer je de HttpClient in een moderne Standalone applicatie?

     In moderne applicaties gebruik je de helper-functie provideHttpClient() in de app.config.ts om de HTTP-functionaliteit globaal te registreren. De oude HttpClientModule is volledig uitgefaseerd.

```typescript
// app.config.ts
import { provideHttpClient } from "@angular/common/http";

export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(), // Registreert de HttpClient-service
  ],
};
```

188. Hoe voer je een type-safe GET-verzoek uit met de HttpClient?

     De HttpClient methoden zijn generiek (generic). Je kunt direct meegeven welke interface of datastructuur je terugverwacht van de server. Angular zorgt er vervolgens voor dat de resulterende Observable correct getypeerd is.

```typescript
export interface Gebruiker {
  id: number;
  naam: string;
}

@Injectable({ providedIn: "root" })
export class DataService {
  private http = inject(HttpClient);

  getGebruikers(): Observable<Gebruiker[]> {
    // Definieer het verwachte type tussen de vishaken < >
    return this.http.get<Gebruiker[]>("https://api.voorbeeld.nl/users");
  }
}
```

189. Hoe geef je Query Parameters mee aan een HTTP-verzoek via HttpParams?

     Om parameters aan een URL toe te voegen (zoals ?sort=desc&page=2), maak je gebruik van de onveranderbare (immutable) HttpParams klasse. Omdat de klasse immutable is, geeft elke methode-aanroep (set() of append()) een nieuw object terug. Dit kun je handig in een chain achter elkaar zetten.

```typescript

import { HttpParams } from '@angular/common/http';

getGefilterdeProducten(categorie: string, pagina: number) {
const params = new HttpParams()
.set('cat', categorie)
.set('page', pagina.toString()); // Waarden moeten altijd strings zijn

return this.http.get<Product[]>('/api/producten', { params });
}
```

190. Hoe voer je een POST-verzoek uit en stuur je een JSON-body mee?

     Bij een POST-verzoek geef je de data-body mee als het tweede argument. Angular herkent objecten automatisch en converteert ze onder de motorkap direct naar een geldige JSON-string, waarbij de HTTP-header Content-Type: application/json automatisch wordt klaargezet.

```typescript
voegProductToe(nieuwProduct: Product): Observable<Product> {
   // Tweede argument is de payload/body
  return this.http.post<Product>('/api/producten', nieuwProduct);
}
```

191. Wat is een 'Functional Interceptor' en hoe configureer je deze?

     Interceptors fungeren als middleware voor je HTTP-verkeer. Ze kunnen uitgaande verzoeken aanpassen (bijvoorbeeld een authenticatie-token toevoegen) of binnenkomende antwoorden inspecteren (bijvoorbeeld fouten centraal loggen). Sinds recente Angular-versies schrijven we interceptors als gestroomlijnde functies in plaats van omslachtige klassen.

```typescript
// auth.interceptor.ts
import { HttpInterceptorFn } from "@angular/common/http";

export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const token = "mijn-geheime-jwt-token";

  // Clone het verzoek omdat HTTP-verzoeken immutable zijn en voeg de header toe
  const gekloondVerzoek = req.clone({
    setHeaders: { Authorization: `Bearer ${token}` },
  });

  // Stuur het gekloonde verzoek door naar de volgende stap in de keten
  return next(gekloondVerzoek);
};
```

192. Hoe registreer je een functionele interceptor in de applicatie?

     Je registreert functionele interceptors binnen de provideHttpClient configuratie in app.config.ts met behulp van de withInterceptors() helper.

```typescript
// app.config.ts
import { provideHttpClient, withInterceptors } from "@angular/common/http";
import { authInterceptor } from "./auth.interceptor";

export const appConfig: ApplicationConfig = {
  providers: [
    provideHttpClient(
      withInterceptors([authInterceptor]), // Voeg hier je interceptors toe in volgorde van uitvoering
    ),
  ],
};
```

193. Wat is de betekenis van HttpContext en hoe gebruik je dit om metadata mee te geven aan een verzoek?

     Soms wil je dat een specifieke API-call anders wordt behandeld door een interceptor (bijvoorbeeld: "Sla deze specifieke call over voor automatische foutafhandeling"). Met HttpContext kun je runtime metadata (vlaggen) meegeven aan een verzoek die de interceptor kan uitlezen.

```typescript
import { HttpContext, HttpContextToken } from "@angular/common/http";

// 1. Maak een token aan met een standaardwaarde
const BYPASS_LOGGING = new HttpContextToken<boolean>(() => false);

// 2. Gebruik het token in een HTTP-verzoek
this.http.get("/api/data", {
  context: new HttpContext().set(BYPASS_LOGGING, true),
});

// 3. Lees het token uit in een interceptor
export const logInterceptor: HttpInterceptorFn = (req, next) => {
  if (req.context.get(BYPASS_LOGGING)) {
    return next(req); // Sla de logging over
  }
  // ... voer normale logging uit ...
  return next(req);
};
```

194. Hoe hanteer je robuuste foutafhandeling bij HTTP-verzoeken met HttpErrorResponse?

     Fouten bij netwerkverzoeken kunnen worden opgevangen met de RxJS catchError operator. Binnen deze operator inspecteer je de fout via de HttpErrorResponse klasse om te bepalen of het een client-side fout (zoals een typefout of netwerkonderbreking) of een server-side fout (zoals een 404 Not Found of 500 Server Error) betreft.

```typescript
import { HttpErrorResponse } from "@angular/common/http";

this.http.get("/api/beveiligd").pipe(
  catchError((error: HttpErrorResponse) => {
    if (error.status === 401) {
      console.warn("Oeps, je bent niet ingelogd!");
    } else if (error.error instanceof ErrorEvent) {
      console.error("Client-side netwerkfout:", error.error.message);
    } else {
      console.error(
        `Server gaf foutcode ${error.status} terug met body:`,
        error.error,
      );
    }
    return throwError(
      () => new Error("Er ging iets mis. Probeer het later opnieuw."),
    );
  }),
);
```

195. Hoe implementeer je een automatische 'Retry'-strategie bij falende
     netwerkverzoeken?

Soms faalt een verzoek door een tijdelijke hik in de internetverbinding. Met de RxJS retry
operator kun je Angular instrueren het verzoek automatisch een aantal keer opnieuw te
proberen voordat de app definitief een foutmelding geeft. Je kunt hierbij een vertraging
(delay) instellen.

```ts
import { retry, timer } from 'rxjs';

getDataMetRetry() {
  return this.http.get('/api/wankele-verbinding').pipe(
    retry({
      count: 3, // Probeer het maximaal 3 keer opnieuw
      delay: (error, retryCount) => timer(retryCount * 1000) // Exponentiële vertraging (1s, 2s, 3s)
    })
  );
}
```

196. Wat is de betekenis van de observe optie in de HttpClient configuratie?

Standaard staat de observe property ingesteld op 'body'. Dit betekent dat Angular de
HTTP-metadata (zoals statuscodes en headers) wegstript en puur de JSON-body
teruggeeft. Als je de headers of de ruwe statuscode nodig hebt, verander je observe naar
'response'.

```ts
getVolledigeResponse() {
  return this.http.get<Product[]>('/api/producten', {
     observe: 'response' // Geeft een HttpResponse<Product[]> terug in plaats van Product[]
  }).subscribe(response => {
    console.log('Status Code:', response.status); // Bijv. 200
    console.log('Specifieke Header:', response.headers.get('X-Custom-Header'));
  });
}
```

197. Hoe configureer je de HttpClient om voortgangsupdates (Progress
     Events) te ontvangen voor grote uploads of downloads?

Voor het tonen van een voortgangsbalk (progress bar) bij het uploaden of downloaden van
grote bestanden, moet je reportProgress: true aanzetten en observe: 'events' configureren.
De stream zendt vervolgens gedurende het proces verschillende HttpEvent typen uit.

```ts
import { HttpEvent, HttpEventType } from '@angular/common/http';

uploadBestand(fileData: FormData): Observable<HttpEvent<any>> {
  return this.http.post('/api/upload', fileData, {
    reportProgress: true,
    observe: 'events'
  }).pipe(
  tap((event: HttpEvent<any>) => {
    if (event.type === HttpEventType.UploadProgress && event.total) {
      const percentage = Math.round((100 * event.loaded) / event.total);
      console.log(`Bestand is voor ${percentage}% geüpload.`);
    } else if (event.type === HttpEventType.Response) {
      console.log('Upload volledig afgerond!', event.body);
    }
    })
  );
}
```

198. Waarom is het annuleren (cancelen) van HTTP-verzoeken met RxJS

Omdat de HttpClient met Observables werkt, stopt het HTTP-verzoek daadwerkelijk op
netwerkniveau zodra er aan de client-kant wordt ge-unsubscribet. Als een gebruiker
bijvoorbeeld op een knop klikt om een zwaar rapport te laden, maar direct daarna naar
een andere pagina navigeert, zorgt een operator zoals takeUntilDestroyed() of switchMap
ervoor dat de browser de actieve download direct afbreekt. Dit bespaart aanzienlijk veel
bandbreedte en serverbelasting

- `map` voor simpele data-transformaties.
- `switchMap` gebruik ik voor live-search of als ik oude requests wil annuleren bij nieuwe input.
- `concatMap` als ik requests serieel wil uitvoeren en de volgorde wil behouden.
- `mergeMap` voor parallelle API-calls.
- `exhaustMap` is handig bij submit-buttons om dubbele requests te voorkomen.

199. Wat is de rol van provideHttpClientTesting() bij het schrijven van
     Unit Tests?

Voor het testen van services die HTTP-calls doen, wil je nooit echte verzoeken naar een
live server sturen. Door provideHttpClientTesting() te registreren in je testomgeving,
activeer je een mock-backend. Hiermee kun je uitgaande verzoeken onderscheppen,
inspecteren en handmatig voorzien van een nep-antwoord (fake response).

```ts
// data.service.spec.ts
import { provideHttpClientTesting, HttpTestingController } from '@angular/common/http/
testing';

describe('DataService', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      providers: [
        DataService,
        provideHttpClient(),
        provideHttpClientTesting() // Activeert de HttpTestingController
      ]
  });
});
});

// ... zie volgende vraag voor test-uitvoering ...
```

200. Hoe schrijf je een concrete Unit Test met de HttpTestingController?
     Met de HttpTestingController kun je verifieerbare verwachtingen (expectOne) opstellen
     voor je HTTP-verkeer en data doorsluizen via de .flush() methode.

```ts
it("zou gebruikers moeten ophalen via een GET-verzoek", () => {
  const service = TestBed.inject(DataService);
  const httpMock = TestBed.inject(HttpTestingController);
  const dummyGebruikers = [{ id: 1, naam: "Test User" }];
  // 1. Start de call
  service.getGebruikers().subscribe((users) => {
    expect(users.length).toBe(1);
    expect(users).toEqual(dummyGebruikers);
  });
  // 2. Valideer of er exact één verzoek is gedaan naar de juiste URL
  const req = httpMock.expectOne("https://api.voorbeeld.nl/users");
  expect(req.request.method).toBe("GET");
  // 3. Los het verzoek op door de dummy data erin te flushen
  req.flush(dummyGebruikers);
  // 4. Controleer of er geen onverwachte verzoeken meer openstaan
  httpMock.verify();
});
```

## State Management en Signals (201-220).

201. Wat is State Management en waarom is het cruciaal in complexe front-end
     applicaties?

State (toestand) is de verzameling van alle data die op een bepaald moment de applicatie definieert,
zoals ingelogde gebruikersgegevens, UI-instellingen (dark mode), gecachte API-data en
formulierinvoer. State Management is het gestructureerd beheren, muteren en distribueren van deze data. Zonder centrale regie over je state leidt een applicatie al snel tot inconsistente data tussen componenten, onvoorspelbare bugs en onnodige re-renders.

202. Wat zijn Angular Signals en welk fundamenteel probleem lossen ze op?

Signals zijn een reactief primitief dat een waarde bevat en de applicatie op de hoogte stelt wanneer
die waarde verandert.
Vóór de introductie van Signals leunde Angular volledig op Zone.js voor veranderingsdetectie
(Change Detection). Zone.js onderschept elk asynchroon event (zoals een klik of een HTTP-call) en
controleert vervolgens de volledige componentenboom van top tot teen op wijzigingen. Signals
lossen dit performance-probleem op door fijnmazige (fine-grained) reactiviteit te introduceren:
Angular weet dankzij een Signal exact welke specifieke component of HTML-tag afhankelijk is van
welke waarde, waardoor alleen dat specifieke stukje DOM wordt bijgewerkt zonder de rest van de
app te scannen.

203. Hoe maak en muteer je een basis signal?

Je maakt een writable (schrijfbaar) signal aan met de signal() functie. Je kunt de waarde op
twee manieren muteren:

- set(newValue): Overschrijft de huidige waarde direct met een gloednieuwe waarde.
- update(fn): Berekent een nieuwe waarde op basis van de huidige waarde (perfect voor
  tellers of het toevoegen van items aan een array).

```ts
import { signal } from "@angular/core";
// Initialisatie
const teller = signal(0);
// Waarde uitlezen (altijd met haakjes aanroepen)
console.log(teller()); // Output: 0
// Direct overschrijven
teller.set(10);
// Updaten op basis van huidige waarde
teller.update((huidigeWaarde) => huidigeWaarde + 1);
```

204. Wat is een computed signal en hoe werkt 'dependency tracking'?

Een computed signal creëert een afgeleid, alleen-lezen (read-only) signal op basis van andere
signals. Angular maakt gebruik van automatische dependency tracking: runtime ontdekt Angular
welke signals er binnen de computed functie worden aangeroepen en registreert deze als
afhankelijkheid. Als een van die onderliggende signals verandert, wordt de computed waarde
automatisch opnieuw berekent.

```ts
import { signal, computed } from '@angular/core';
const aantal = signal(3);
const prijsPerStuk = signal(10);
// Wordt automatisch bijgewerkt zodra 'aantal' of 'prijsPerStuk' verandert
const totaalPrijs = computed(() => aantal() \*
prijsPerStuk());
```

205. Wat verstaat men onder de 'lazy evaluation' en 'caching' van een computed signal?

Een computed signal is uiterst efficiënt dankzij twee principes:

- Lazy Evaluation: De berekening binnen de computed functie wordt niet uitgevoerd op
  het moment dat onderliggende signals veranderen, maar pas op het exacte moment dat
  iemand de waarde van het computed signal daadwerkelijk probeert uit te lezen
  (bijvoorbeeld in de HTML-template).
- Caching (Memoization): Als de waarde eenmaal is berekend, wordt het resultaat
  opgeslagen in een cache. Zolang de onderliggende signals niet muteren, zal herhaaldelijk
  uitlezen van het computed signal direct de gecachte waarde opleveren zonder de
  rekenformule opnieuw uit te voeren.

206. Wat is de functie van een effect en wanneer gebruik je deze?

     Een effect is een operatie die automatisch wordt uitgevoerd telkens wanneer de signals die erin
     worden uitgelezen van waarde veranderen. Omdat een effect asynchroon draait tijdens het
     veranderingsdetectieproces, mag je er nooit direct andere signals in muteren (tenzij expliciet
     toegestaan via speciale configuratie, wat sterk wordt afgeraden).
     Gebruik effecten uitsluitend voor side-effects buiten het Angular-ecosysteem, zoals:
     - Data synchroniseren met localStorage.
     - Handmatige DOM-manipulaties uitvoeren met externe bibliotheken (bijv. een chart
       tekenen).
     - Aangepaste logging bijhouden voor analytics.

```ts
import { effect } from "@angular/core";
export class ThemeComponent {
  theme = signal("light");
  constructor() {
    effect(() => {
      // Synchroniseer de status live met de browser cache
      localStorage.setItem("app-theme", this.theme());
    });
  }
}
```

207. Hoe werkt de opruimfunctie (onCleanup) binnen een effect?

     Vaak start een effect een asynchrone handeling (zoals een setTimeout of een websocket-
     connectie) die moet worden stopgezet zodra het effect opnieuw vuurt of de component wordt
     vernietigd. De effect functie geeft een onCleanup callback mee waarmee je deze resources
     netjes kunt opruimen om memory leaks te voorkomen.

```ts
effect((onCleanup) => {
  const query = ditZoekSignal();
  const timerId = setTimeout(() => console.log("Zoeken naar:", query), 500);
  // Als 'ditZoekSignal' binnen 500ms opnieuw verandert, wist deze callback de oude timer
  onCleanup(() => clearTimeout(timerId));
});
```

208.  Waarom mag je een effect niet overal declareren?

      Een effect heeft een zogenaamde Injection Context nodig om te weten wanneer hij zichzelf
      moet vernietigen. Daarom kun je een effect alleen succesvol aanmaken binnen een constructor,
      als class property initialisatie, of door handmatige doorgave van een Injector. Als je een effect
      probeert te declareren in een reguliere methode (zoals een klik-event handler), zal Angular een
      runtime-fout gooien.

209.  Wat is de untracked functie en hoe voorkom je ongewenste dependency
      tracking?

      Soms wil je in een computed signal of een effect de waarde van een bepaald signal uitlezen,
      zonder dat dit signal als afhankelijkheid wordt geregistreerd. Met untracked() isoleer je het
      signal. Het effect zal dan niet opnieuw triggeren als dat specifieke signal muteert.

```ts
import { untracked } from "@angular/core";
effect(() => {
  const huidigeStatus = logInStatusSignal(); // Dit activeert tracking
  // We willen de gebruikersnaam loggen, maar het effect mag NIET opnieuw vuren
  // puur en alleen omdat de gebruikersnaam verandert.
  const naam = untracked(() => gebruikersNaamSignal());
  console.log(`Gebruiker ${naam} is nu: ${huidigeStatus}`);
});
```

210. Wat zijn Signal-based Inputs (input en input.required)?

     De traditionele @Input() decorator is vervangen door de modernere, type-safe input() API.
     Dit genereert een read-only signal in je component dat automatisch updates ontvangt vanuit de
     parent-template.

```ts
export class GebruikerKaartComponent {
  // Optionele input met een standaardwaarde
  titel = input("Gast"); // Signal<string>
  // Verplichte input zonder standaardwaarde
  id = input.required<string>(); // Signal<string>
}
```

211. Hoe transformeer je invoerwaarden met de transform optie in een
     Signal Input?

Soms wil je data die via een input binnenkomt eerst converteren (bijvoorbeeld een string omzetten
naar een getal, of een lege string omzetten naar de boolean true). Dit doe je met de transform
property.

```ts
export class ToggleComponent {
  // Zorgt ervoor dat <app-toggle disabled /> correct wordt geïnterpreteerd als true
  disabled = input(false, {
    transform: (value: boolean | string) =>
      typeof value === "string" ? true : value,
  });
}
```

212.  Wat zijn Model Inputs (model()) en hoe realiseren ze Two-Way Data
      Binding?

Een model() input definieert een schrijfbaar (writable) signal dat fungeert als een two-way data
binding. De component kan de waarde zelf muteren via .set() of .update(), en deze
wijziging wordt direct teruggekoppeld naar de parent-component. In de parent-template koppel je
dit via de bekende "banana-in-a-box" [(waarde)] syntax.

```ts
// Binnen kind-component:
export class TellerComponent {
  waarde = model(0); // Writable signal!
  increment() {
    this.waarde.update((w) => w + 1); // Parent ziet dit direct
  }
}
// Binnen parent-template:
// <app-teller [(waarde)]="mijnParentTeller" />
```

213. Wat zijn Signal-based Queries (viewChild, viewChildren, contentChild)?

     De oude decorators @ViewChild en @ContentChild zijn vervangen door functies die direct
     een Signal opleveren. Dit betekent dat je niet meer hoeft te wachten op de ngAfterViewInit
     lifecycle hook om veilig interactie te zoeken met elementen uit je template; je kunt er direct reactief
     op reageren via een computed of effect.

```ts
	export class CanvasComponent {
	// Haalt de referentie naar <canvas #mijnCanvas> op als Signal
	canvasEl = viewChild.required<ElementRef<HTMLCanvasElement>>('mijnCanvas
	');
	constructor() {
      effect(() => {
        // Zodra het element beschikbaar is in de DOM, teken het effect direct
        const ctx = this.canvasEl().nativeElement.getContext('2d');
      });
	  }
	}
```

214. Hoe ontwerp je een lichtgewicht 'Signal Store' patroon met een Angular
     Service?

Je hebt voor robuust state management geen zware, complexe libraries (zoals Redux/NgRx) meer
nodig. Een simpele Angular Service in combinatie met private writable signals en publieke read-
only signals (of computed signals) vormt een perfect, waterdicht state-patroon.

```ts
	@Injectable({ providedIn: 'root' })
	export class WinkelwagenStore {
	// 1. Private state (alleen binnen de service muteerbaar)
	private \_items = signal<Product[]>([]);
	// 2. Publieke state (alleen-lezen voor componenten)
	items = this.\_items.asReadonly();
	aantalItems = computed(() => this.\_items().length);
	// 3. Gecontroleerde mutaties (Actions)
	voegToe(product: Product) {
      this.\_items.update(huidigeItems => [...huidigeItems,
      product]);
    }
	}
```

215. Hoe ga je om met asynchrone state (zoals API-calls) binnen een Signal-
     architectuur?

Omdat de HttpClient van Angular op RxJS leunt, transformeer je asynchrone datastromen
naar state met de toSignal() helper. Dit vangt de asynchrone stroom op en verpakt het
resultaat in een synchroon leesbaar signal.

```ts
export class ProductenComponent {
  private api = inject(ApiService);
  // Converteert de Observable direct naar een Signal
  producten = toSignal(this.api.getProducts(), { initialValue: [] });
}
```

216.  Wat is de 'RxJS-Interop' module en waarom is deze belangrijk bij state
      management?

De @angular/core/rxjs-interop module levert de cruciale brugfuncties
toSignal() and toObservable(). Hoewel Signals perfect zijn voor het beheren en tonen
van synchrone status in de UI, blijft RxJS onverslaanbaar voor asynchrone, event-gedreven
operaties (zoals websockets, polling, of race-conditions met switchMap). De interop-module
stelt je in staat om de sterke punten van beide werelden naadloos te combineren in je state-
architectuur.

217.  Hoe muteer je complexe geneste objecten of arrays veilig in een Signal?

      Signals vergelijken waarden standaard op basis van referentiegelijkheid (===). Als je een
      eigenschap binnen een object of array direct aanpast, ziet Angular dit niet als een wijziging en zal
      de UI niet updaten. Je moet objecten en arrays daarom altijd onveranderbaar (immutable)
      muteren met behulp van de spread-operator (...) of methoden zoals .map() en .filter().

```ts
// FOUTIEF (UI update niet, referentie blijft gelijk):
gebruikerSignal().naam = "Piet";
// CORRECT (Nieuw object referentie gecreëerd):
gebruikerSignal.set({ ...gebruikerSignal(), naam: "Piet" });
```

218. Wat is de impact van 'Zoneless Angular' op de toekomst van State
     Management?

Sinds recente releases ondersteunt Angular een volledige Zoneless modus (te configureren via
provideExperimentalZonelessChangeDetection() in app.config.ts).
Dit betekent dat Zone.js compleet uit de applicatie gesloopt kan worden. De applicatie draait
hierdoor lichter, start sneller op en verbruikt minder geheugen. In deze modus zijn Signals de enige
betrouwbare manier geworden om Angular te vertellen wanneer de UI ververst moet worden, wat
het belang van een solide Signal-based state management-architectuur alleen maar vergroot.

219. Kun je een Signal hergebruiken in meerdere componenten?

     Ja, mits de instantie van het signal gedeeld wordt. Als je een signal definieert in een root-service
     (providedIn: 'root'), dan delen alle componenten die deze service injecteren exact
     hetzelfde signal. Verandert Component A de waarde, dan reageert Component B daar direct op. Als
     je een signal in de component-klasse zelf declareert, krijgt elke instantie van die component
     uiteraard zijn eigen, unieke signal-waarde.

220. Hoe test je een component of service die intensief gebruikmaakt van

     Signals?
     Het testen van Signals is extreem eenvoudig omdat ze synchroon van aard zijn. Je hoeft niet te
     werken met complexe asynchrone test-helpers zoals fakeAsync, tick of waitForAsync
     om gewijzigde waarden uit te lezen; je roept simpelweg het signal aan en controleert direct de
     waarde.

```ts
	it('zou het aantal items in de winkelwagen correct moeten
	verhogen', () => {
	const store = TestBed.inject(WinkelwagenStore);
	// Controleer initiële waarde synchroon
	expect(store.aantalItems()).toBe(0);
	// Voer actie uit
	store.voegToe({ id: 1, naam: 'Laptop' });
	// Controleer direct de geupdate computed waarde
	expect(store.aantalItems()).toBe(1);
	});
```

### SSR, Hydration en PWA (221-240)

221. Wat is Server-Side Rendering (SSR) in Angular?

SSR is een techniek waarbij Angular de applicatie op een Node.js-server compileert en een volledig
opgebouwde, statische HTML-pagina naar de browser van de gebruiker stuurt. Dit zorgt voor een
razendsnelle eerste weergave (First Contentful Paint) en een sterk verbeterde SEO-indexering door
zoekmachines. 222. Wat is het verschil tussen SSR en Prerendering (SSG)?

- SSR (Server-Side Rendering): De HTML wordt on-the-fly op de server gegenereerd op het
  exacte moment dat een gebruiker de pagina opvraagt. Ideaal voor dynamische data (zoals
  een gebruikersdashboard of voorraadsysteem).
- SSG (Static Site Generation / Prerendering): De HTML-pagina's worden eenmalig
  gegenereerd tijdens het build-proces van de applicatie. Dit resulteert in statische bestanden
  die direct vanaf een CDN geserveerd kunnen worden. Ideaal voor blogs, documentatie of
  marketingpagina's.

223. Wat is 'Client-Side Hydration' en hoe werkt dit in Angular?

Wanneer de server de statische HTML naar de browser stuurt, kan de gebruiker de pagina direct
zien, maar is deze nog niet interactief (er draait nog geen JavaScript). Hydration is het proces
waarbij Angular in de browser de statische HTML-structuur hergebruikt, de interne
applicatiestructuur (componentenboom) eraan koppelt en event-listeners activeert, zonder de pagina
destructief opnieuw te hoeven renderen. 224. Hoe activeer je SSR en Hydration in een moderne Angular applicatie?

Je voegt SSR-ondersteuning toe aan je project via de Angular CLI met het commando: ng add
@angular/ssr. Dit configureert een Node.js server (server.ts) en activeert hydration
automatisch in je app.config.ts:

```ts
	// app.config.ts
	export const appConfig: ApplicationConfig = {
	providers: [
	provideClientHydration() // Activeert het naadloze
	hydration-proces
	]
	};
```

225. Wat is 'Event Dispatch' (onderdeel van Angular's geavanceerde

Hydration)?
Als een gebruiker op een server-side gerenderde pagina klikt voordat de JavaScript in de browser
volledig is ingeladen, gaan die interacties normaal gesproken verloren. Angular maakt gebruik van
Event Dispatch: een lichtgewicht script vangt de vroege gebruikers-events (zoals clicks) op in de
browser en bewaart deze in een wachtrij. Zodra de applicatie volledig is gehydrateerd, worden deze
clicks alsnog correct uitgevoerd. 226. Waarom crasht een SSR-applicatie bij het direct aanroepen van window of

document?
De Node.js-serveromgeving bezit geen browser-objecten zoals window, document,
localStorage of navigator. Als Angular code tegenkomt die deze objecten rechtstreeks
probeert aan te spreken tijdens het renderen op de server, zal het Node-proces crashen met een
ReferenceError. 227. Hoe gebruik je PLATFORM_ID om code veilig uit te voeren in een SSR-

omgeving?
Om te voorkomen dat browser-specifieke code op de server draait, controleer je het huidige
platform met de functies isPlatformBrowser of isPlatformServer.

```ts
import { PLATFORM_ID } from "@angular/core";
import { isPlatformBrowser } from "@angular/common";
export class OpslagComponent {
  private platformId = inject(PLATFORM_ID);
  getLocalStorageItem() {
    // Voer dit ALLEEN uit als we daadwerkelijk in de browser
    draaien;
    if (isPlatformBrowser(this.platformId)) {
      return localStorage.getItem("sleutel");
    }
    return null;
  }
}
```

228. Wat is de taak van TransferState bij SSR?

Als een component tijdens het server-side renderen een API-call doet om data op te halen, zal
diezelfde component bij aankomst in de browser die API-call normaal gesproken nóg een keer
uitvoeren. TransferState lost dit op: het cachet de data die op de server is opgehaald en sluit
dit als een JSON-string achteraan de HTML aan. In de browser leest de HttpClient deze cache
direct uit, wat een dubbele API-call bespaart. 229. Wat is een Progressive Web App (PWA)?

Een PWA is een webapplicatie die gebruikmaakt van moderne browsertechnologieën om gebruikers
een app-achtige ervaring te bieden. Een PWA kan op het startscherm van een telefoon worden
geïnstalleerd, werkt offline, ondersteunt push-notificaties en laadt dankzij slimme caching
onmiddellijk op. 230. Wat is de rol van een Service Worker in een PWA?

Een Service Worker is een JavaScript-bestand dat op de achtergrond in de browser draait, los van de
webpagina zelf. Het fungeert als een netwerk-proxy: het kan uitgaande netwerkverzoeken
onderscheppen en besluiten om bestanden direct vanuit een lokale cache te serveren, waardoor de
applicatie volledig offline kan functioneren. 231. Hoe voeg je PWA-functionaliteit toe aan Angular?

Dit voeg je eenvoudig toe via de CLI: ng add @angular/pwa. Dit commando genereert
automatisch een configuratiebestand (ngsw-config.json), voegt de benodigde iconen toe en
registreert de Angular Service Worker in je app.config.ts. 232. Hoe werkt het ngsw-config.json configuratiebestand?

In dit bestand bepaal je de caching-strategieën van de Angular Service Worker. Je deelt resources op
in groepen:

- assetGroups: Voor statische bestanden (index.html, CSS, JS, afbeeldingen). Je kunt
  kiezen voor installMode: 'prefetch' om alles direct bij de eerste start lokaal op
  te slaan.
- dataGroups: Voor dynamische API-verzoeken. Hier kies je tussen freshness (eerst
  netwerk proberen, handig voor live data) of performance (eerst cache proberen, handig
  voor semi-statische data).

233. Hoe dwing je een applicatie-update af met de SwUpdate service?

Wanneer je een nieuwe versie van je Angular-app deployed, downloadt de Service Worker deze op
de achtergrond. Met de SwUpdate service kun je de gebruiker direct een melding tonen zodra de
nieuwe versie klaarstaat om geactiveerd te worden.

```ts
	import { SwUpdate } from '@angular/service-worker';
	export class UpdateService {
	private swUpdate = inject(SwUpdate);
	constructor() {
	if (this.swUpdate.isEnabled) {
	this.swUpdate.versionUpdates.subscribe(evt => {
	if (evt.type === 'VERSION_READY') {
	if (confirm('Nieuwe versie beschikbaar. Nu
	herladen?')) {
	window.location.reload();
	}
	}
	});
	}
	}
	}
```

234. Wat is de betekenis van het manifest.webmanifest bestand?

Dit JSON-bestand vertelt de browser hoe de PWA zich moet gedragen wanneer deze geïnstalleerd
wordt op een mobiel apparaat of desktop. Het bevat cruciale metadata zoals de naam van de app, de
achtergrondkleur, de start-URL en de paden naar de icoonbestanden die gebruikt moeten worden op
het startscherm. 235. Hoe test je of de Service Worker correct functioneert tijdens lokale

ontwikkeling?
De Angular Service Worker is standaard uitgeschakeld tijdens het draaien van ng serve om te
voorkomen dat je constant tegen gecachte code aanloopt tijdens het programmeren. Om de PWA
lokaal te testen, moet je een productie-build maken (ng build) en de resulterende dist map
serveren met een losse HTTP-server (zoals de npm package http-server). 236. Wat is 'App Shell' architectuur?

De App Shell is de minimale HTML, CSS en JavaScript die nodig is om de vaste
gebruikersinterface (zoals de navigatiebalk, de header en de zijbalk) van een pagina direct te tonen.
Tijdens een server-build kan Angular deze App Shell vooraf renderen in de index.html.
Hierdoor krijgt de gebruiker direct de visuele lay-out te zien, terwijl de specifieke paginacontent er
vlak daarna in wordt geladen. 237. Welke invloed heeft SSR op Web Vitals zoals TTFB, FCP en INP?

- TTFB (Time to First Byte): Kan iets omhoog gaan, omdat de server tijd nodig heeft om de
  HTML op te bouwen.
- FCP (First Contentful Paint): Gaat drastisch omlaag (verbetert), aangezien de browser
  direct bruikbare HTML ontvangt en kan tekenen.
- INP (Interaction to Next Paint): Moet goed gemonitord worden tijdens de hydration-fase.
  Als de main-thread te lang geblokkeerd raakt door hydration, reageert de pagina tijdelijk
  traag op clicks. Dit wordt opgevangen door Event Dispatch.

238. Wat is de HydrationSkip directive en wanneer zet je deze in?

Als je een component hebt die direct de DOM manipuleert op een manier die Angular niet kan
voorspellen (bijvoorbeeld een component die een externe jQuery-plugin inschiet), zal hydration
fouten (mismatches) gooien. Met het attribuut ngSkipHydration dwing je Angular om
hydration voor die specifieke component over te slaan en hem ouderwets puur aan de client-kant op
te bouwen.

```ts
	<app-legacy-chart ngSkipHydration />
```

239. Hoe kun je dynamische SEO tags instellen bij gebruik van SSR?

Angular biedt de Meta en Title services waarmee je runtime vanuit je componenten metadata
(zoals open-graph tags voor Facebook/LinkedIn of meta-descriptions voor Google) kunt aanpassen.
Omdat dit tijdens SSR al gebeurt, zien scrapers en zoekmachines direct de juiste unieke tags per
pagina.

```ts
import { Meta, Title } from "@angular/platform-browser";
export class ArtikelComponent implements OnInit {
  private title = inject(Title);
  private meta = inject(Meta);
  ngOnInit() {
    this.title.setTitle("Modern Angular Artikel");
    this.meta.updateTag({
      name: "description",
      content: "Diepgaande uitleg over SSR.",
    });
  }
}
```

240. Wat is Server Context in Angular SSR?

Server Context is een ingebouwde configuratie waarmee de applicatie weet via welke specifieke
server-engine of build-tool hij op dat moment gerenderd wordt (bijvoorbeeld of de app draait via
een Express-server of een pre-rendering script). Dit helpt Angular om interne optimalisaties toe te
passen voor het specifieke runtime-platform.
Micro Frontends en Monorepos (241-260) 241. Wat is een Monorepo?

Een Monorepo is een software-architectuur waarbij meerdere softwareprojecten (zoals
verschillende Angular-applicaties, gedeelde bibliotheken en backend-services) in één enkele git-
repository worden beheerd. Het vergemakkelijkt code-sharing, zorgt voor eenduidige tooling en
stroomlijnt grote refactoring-trajecten. 242. Wat is Nx en waarom is het de industriestandaard voor Angular

Monorepos?
Nx is een geavanceerd build-systeem dat specifiek is ontworpen voor het beheren van monorepos.
Het biedt krachtige tools die de nadelen van een grote repository wegnemen, zoals:

- Computation Caching: Het onthoudt eerdere test- en build-resultaten. Is er niets veranderd
  in een component? Dan hergebruikt Nx de cache en is je build direct klaar.
- Affected Commands: Nx analyseert de git-historie en bouwt/test uitsluitend de applicaties
  en bibliotheken die direct of indirect geraakt zijn door de laatste codewijziging.

243. Hoe structureer je code in een Monorepo met 'Apps' en 'Libs'?

- Apps (Applicaties): Bevatten de minimale opstartconfiguratie en routing-mappen van de
  daadwerkelijke projecten. Apps horen zo slank mogelijk te zijn.
- Libs (Bibliotheken): De plek waar 95% van de daadwerkelijke code leeft. Libs worden
  opgedeeld in functionele domeinen (bijv. shared-ui, auth-data-access,
  dashboard-feature). Apps importeren deze Libs vervolgens naadloos via
  TypeScript pad-aliasing (@monorepo/auth).

244. Wat is een Micro Frontend architectuur?

Micro Frontends is een architectuurpatroon waarbij een grote, complexe webapplicatie wordt
opgeknipt in onafhankelijke, kleine sub-applicaties die autonoom door verschillende teams
gebouwd, getest en gedeployed kunnen worden. Deze sub-apps worden runtime samengevoegd in
een centrale 'Host' applicatie. 245. Wat is Module Federation?

Module Federation is een technologie (geïntroduceerd in Webpack 5 en nu breed ondersteund in
moderne bundlers zoals Vite/Rspack via Module Federation v2) die Micro Frontends technisch
mogelijk maakt. Het stelt een JavaScript-applicatie in staat om runtime dynamisch code (zoals
Angular componenten) in te laden vanuit een compleet andere applicatie die op een heel andere
server of URL draait, zonder compile-time afhankelijkheden. 246. Wat is het verschil tussen een 'Host' en een 'Remote' in Module Federation?

- Host (Shell): De hoofdapplicatie die de basislay-out (zoals het hoofdmenu) verzorgt en
  verantwoordelijk is voor het runtime inladen van de sub-applicaties.
- Remote: Een onafhankelijke sub-applicatie (bijv. een betalingsmodule) die specifieke
  componenten of code 'exposeert' (beschikbaar stelt) aan de Host.

247. Hoe configureer je een dynamische Remote route in de Host applicatie?

Met de helper-functies van moderne Module Federation libraries kun je een Remote applicatie
direct koppelen aan Angular's lazy-loaded routing:

```ts
	// app.routes.ts
	import { loadRemoteModule } from '@module-federation/
	enhanced/runtime';
	export const routes: Routes = [
	{
	path: 'instellingen',
	loadChildren: () => loadRemoteModule({
	exposedModule: './Module',
	remoteEntry: 'http://localhost:4201/remoteEntry.js'
	}).then(m => m.InstellingenModule)
	}
	];
```

248. Hoe ga je om met gedeelde afhankelijkheden (Shared Dependencies) in

Module Federation?
Als zowel de Host als de Remote gebruikmaken van @angular/core, wil je niet dat de
browser deze zware bibliotheek twee keer moet downloaden. In de Module Federation configuratie
definieer je deze pakketten als shared. De browser downloadt de bibliotheek dan één keer, en
deelt de actieve instantie runtime tussen alle Micro Frontends. 249. Wat is het risico van 'Version Mismatch' bij gedeelde bibliotheken?

Als de Host applicatie draait op Angular v18 en een Remote applicatie op Angular v19, kan het
runtime delen van @angular/core leiden tot onvoorspelbare crashes door API-verschillen. Het
is een best-practice om binnen een Micro Frontend ecosysteem de core-frameworks strak op
dezelfde versie te houden (wat een monorepo-aanpak extra waardevol maakt). 250. Hoe isoleer je CSS/Styles tussen verschillende Micro Frontends?

Omdat Micro Frontends runtime in dezelfde DOM worden samengevoegd, kunnen globale CSS-
regels van een Remote de stijl van de Host verruineren. In Angular los je dit op door consequent
gebruik te maken van de standaard ViewEncapsulation.Emulated (of ShadowDom)
op componentniveau, waardoor CSS-regels strikt binnen de component-scoping blijven. 251. Hoe communiceer je veilig tussen de Host en een Remote applicatie?

Communicatie moet losgekoppeld (loosely coupled) plaatsvinden. Vermijd directe imports van
elkaars services. Veilige communicatiemethoden zijn:

- Het gebruik van een lichtgewicht, native browser event-systeem via
  window.dispatchEvent(new CustomEvent(...)).
- Het doorgeven van state via query-parameters in de URL.

252. Wat zijn Nx 'Tags' en hoe handhaaf je architectuurregels in een Monorepo?

In een grote monorepo wil je voorkomen dat een generieke bibliotheek (shared-ui) stiekem
code importeert uit een specifieke app-omgeving. Met Nx kun je bibliotheken taggen (bijv.
type:ui, type:feature). Vervolgens dwing je via ESLint-regels restricties af:

```ts
	// eslintrc.json snippet
	"options": {
	"rules": [
	{ "sourceTag": "type:ui", "onlyDependOnLibsWithTags":
	["type:ui", "type:util"] }
	]
	}
```

253. Wat is een 'Shell' applicatie in de context van Micro Frontends?

De Shell is een ander woord voor de Host applicatie. Het is het skelet van de website. De Shell
regelt meestal de authenticatie (inloggen), de globale navigatiebalk, het laden van de juiste
taalbestanden (i18n) en het routeren naar de juiste standalone Remote modules. 254. Wat zijn de nadelen van een Micro Frontend architectuur?

Hoewel het uitstekend schaalt voor grote teams, brengt het significante nadelen met zich mee:

- Complexe CI/CD pipelines: Het orchestreren van losse deployments vereist geavanceerde
  DevOps-kennis.
- Operationele overhead: Lokale ontwikkeling vereist het tegelijkertijd opstarten van
  meerdere servers.
- Kans op performance-degradatie: Als shared dependencies niet optimaal zijn
  geconfigureerd, downloadt de browser onnodig veel dubbele code.

255. Wat is het verschil tussen compile-time en runtime integratie van Micro

Frontends?

- Compile-time: De sub-apps worden tijdens het build-proces via npm-packages
  samengevoegd tot één applicatie. Wijziging in sub-app A? Dan moet de hele hoofd-app
  opnieuw worden gebouwd.
- Runtime (Module Federation): De sub-apps worden als onafhankelijke bundels op eigen
  servers gezet. De hoofd-app haalt de code pas live op het moment dat de gebruiker de
  pagina bezoekt. Dit biedt échte onafhankelijkheid.

256. Wat is Nx Cloud en hoe versnelt dit CI/CD pipelines?

Nx Cloud breidt de lokale computation cache van Nx uit naar een centrale cloud-omgeving. Dit
betekent dat als Developer A lokaal een build heeft gedraaid, de CI/CD pipeline (of Developer B)
die exacte build-resultaten direct uit de cloud kan trekken zonder de code zelf te compileren. Dit
verkort test- en buildtijden in pipelines vaak met 70-80%. 257. Hoe werkt onafhankelijke deployment (Independent Deployment) bij Micro

Frontends?
Dankzij Module Federation kan een team dat verantwoordelijk is voor een Remote (bijv. de 'profiel-
pagina') een bugfix live zetten door alleen de code van hun eigen Remote te builden en te uploaden
naar hun server. De Host applicatie pikt bij de volgende pagina-refresh automatisch de nieuwste
code op van die URL, zonder dat de Host zelf opnieuw gedeployed hoeft te worden. 258. Wat is een 'Polyglot' Micro Frontend en ondersteunt Angular dit?

Een Polyglot Micro Frontend houdt in dat de Host in Angular is gebouwd, maar een Remote in
React of Vue is geschreven. Hoewel Module Federation dit technisch toestaat (het laadt immers
gewoon JavaScript in), is dit binnen Angular-apps complex vanwege de manier waarop Angular's
compiler en runtime lifecycle werken. Het is sterk aan te raden om binnen één Micro Frontend
ecosysteem vast te houden aan hetzelfde basisframework. 259. Wat is de taak van een 'Remote Entry' bestand?

Het remoteEntry.js bestand is het controlecentrum van een Remote applicatie. Het is een
piepklein JavaScript-bestand dat door Module Federation wordt gegenereerd. Het bevat een
catalogus (mapping) van alle componenten die deze specifieke Remote exposeert en vertelt de Host
exact welke specifieke code-chunks hij moet downloaden als er om een module wordt gevraagd. 260. Hoe ga je om met State Management over meerdere Micro Frontends heen?

Houd state strikt lokaal binnen de Micro Frontend zelf. Een Micro Frontend moet functioneren als
een onafhankelijk micro-organisme. Als er echt globale state gedeeld moet worden (zoals de actieve
gebruikerssessie), sluis deze data dan bij het opstarten vanuit de Shell door naar de Remote via
parameters of een gecontroleerd, gedeeld window-event kanaal.
Signals en Moderne Reactiviteit (261-280) 261. Wat is het fundamentele verschil in reactiviteit tussen RxJS en Angular

Signals?

- RxJS (Stream-gebaseerd): Is ontworpen rondom asynchrone gebeurtenissen (events) over
  tijd. Het pusht data door een pijplijn van operators en vereist een expliciet abonnement
  (subscribe of async pipe) om actie te ondernemen. RxJS blinkt uit in asynchrone
  orchestration (zoals debouncing of switchMapping).
- Signals (State-gebaseerd): Zijn ontworpen voor het beheren van toestand (state). Ze zijn
  synchroon en trekken (pull) waarden pas op het moment dat ze nodig zijn. Signals zijn
  fijnmazig: ze weten exact welke specifieke HTML-node in de DOM van hen afhankelijk is.

262. Waarom introduceerde Angular Signals als aanvulling op RxJS?

RxJS is fantastisch voor complexe asynchrone stromen, maar overdreven complex voor het
simpelweg bijhouden van een boolean (zoals isOpen = true). Daarnaast heeft RxJS geen
weet van de Angular DOM-rendering: een gewijzigde RxJS stream dwingt Zone.js nog steeds om
de hele app-componentenboom te scannen. Signals bieden een eenvoudigere syntax voor state en
maken rendementsoptimaliseringen (zoals Zoneless change detection) mogelijk. 263. Wat is de betekenis van 'Glitch-Free Execution' bij Signals?

In RxJS kun je te maken krijgen met een 'glitch': een tijdelijke inconsistente status waarbij een
afgeleide waarde onnodig twee keer berekent wordt omdat twee afhankelijke stromen net na elkaar
updaten (het diamant-probleem). Signals lossen dit native op. Omdat computed signals hun
berekening pas asynchroon uitstellen tot het meetmoment (lazy pull), bestaat er nooit een tijdelijke,
foutieve tussenstatus. 264. Hoe werkt de Equality Evaluation functie in een Signal?

Bij het aanmaken van een signal() kun je een aangepaste equal vergelijkingsfunctie
meegeven. Hiermee bepaal je zelf wanneer Angular een nieuwe waarde als 'gewijzigd' moet
beschouwen. Dit is handig om onnodige UI-updates bij objecten te voorkomen.

```ts
// Signal dat alleen updates triggert als de ID daadwerkelijk
verschilt;
const gebruiker = signal(
  { id: 1, naam: "An" },
  {
    equal: (a, b) => a.id === b.id,
  },
);
```

265. Wat gebeurt er als je een Writable Signal rechtstreeks muteert

zonder .set() of .update()?
Als je een array of object binnen een signal direct muteert (bijv.
mijnSignal().push(item)), verander je weliswaar de data in het geheugen, maar de
referentie van het object blijft exact gelijk. Omdat Angular controleert op referentiegelijkheid, krijgt
het framework de wijziging niet mee. De UI zal dus niet verversen. Werk daarom altijd
onveranderbaar (immutable): mijnSignal.update(lijst => [...lijst,
item]). 266. Waarom kun je een computed signal niet handmatig beschrijven

met .set()?
Een computed signal is inherent read-only. De waarde is een directe, wiskundige afgeleide van
andere onderliggende signals. Als je de waarde handmatig zou kunnen overschrijven, verbreek je de
reactieve keten en is de data niet langer gegarandeerd synchroon met de bronstromen. 267. Kun je een Signal uitlezen binnen een reguliere JavaScript setTimeout?

Ja. Een signal kan overal ter wereld worden uitgelezen door simpelweg de functie aan te roepen
(bijv. status()). Let wel op: als je dit binnen een setTimeout doet, bevind je je niet in een
reactieve context (zoals een computed of effect), dus er zal geen automatische dependency
tracking plaatsvinden voor die setTimeout-omgeving. 268. Hoe gebruik je output() en outputFromObservable() in

moderne componenten?
De oude @Output() en EventEmitter decorators zijn vervangen door de gestroomlijnde
output() API. Als je een bestaande RxJS stream als output wilt aanbieden, gebruik je
outputFromObservable().

```ts
export class FilterComponent {
  // Moderne, slanke output definitie
  statusVeranderd = output<boolean>();
  verstuur(status: boolean) {
    this.statusVeranderd.emit(status);
  }
}
```

269. Wat is de functie van allowSignalWrites in een effect?

Standaard verbiedt Angular het schrijven (muteren) van signals binnen een effect. Dit is een
architectonische bescherming om oneindige lussen (infinite loops) te voorkomen. Je kunt dit
forceren via { allowSignalWrites: true } in de configuratie, maar dit duidt in 99%
van de gevallen op een slecht software-ontwerp. Gebruik in plaats daarvan liever een computed
signal. 270. Hoe werkt dynamic dependency tracking bij conditionele logica in een

computed signal?
Angular hercalculeert de afhankelijkheden van een computed of effect dynamisch bij elke
uitvoering. Als je conditionele logica (if/else) gebruikt, luistert Angular alleen naar de signals
die op dat moment daadwerkelijk door de code-route zijn geraakt.

```ts
	const weergave = computed(() => {
	if (toonUitgebreid()) {
	return `Details: ${detailSignal()}`; // Nu luistert
	Angular naar 'toonUitgebreid' én 'detailSignal'
	}
	return 'Simpel'; // Nu luistert Angular ALLEEN naar
	'toonUitgebreid'
	});
```

271. Hoe converteer je een formulier-waarde direct naar een Signal?

Omdat Reactive Forms onder de motorkap RxJS streams gebruiken, gebruik je de toSignal
helper om de waarde-wijzigingen live om te zetten naar een synchroon uitleesbaar signal.

```ts
export class FormComponent {
  zoekForm = new FormGroup({ term: new FormControl("") });
  // Live gesynchroniseerd signal van de invoer
  zoekTerm = toSignal(this.zoekForm.controls.term.valueChanges, {
    initialValue: "",
  });
}
```

272. Wat is de rol van de linkedSignal API?

linkedSignal (geïntroduceerd als krachtige vla-primitieve) lost een veelvoorkomend
probleem op: een schrijfbaar signal dat moet resetten of mee-veranderen zodra een ander bronsignal
wijzigt. Denk aan een 'actief product' signal; zodra de geselecteerde product-ID verandert, moet het
signal 'hoeveelheid' automatisch resetten naar 1.

```ts
	// Een linkedSignal reset of hercalculeert zodra de bron
	(source) verandert
	const hoeveelheid = linkedSignal({
	source: geselecteerdProductId,
	computation: (newId, previous) => 1 // Reset naar 1 bij
	nieuw product
	});
```

273. Moet je een computed signal handmatig vernietigen om geheugenlekken

te voorkomen?
Nee. computed signals maken gebruik van zwakke referenties (weak references) naar hun
onderliggende afhankelijkheden. Zodra de component die het computed signal gebruikt uit de
DOM wordt verwijderd en vernietigd, wordt het signal automatisch opgeruimd door de garbage
collector van de browser. 274. Wat is het verschil in uitvoeringstijd tussen een effect en een

RxJS .subscribe()?

- RxJS .subscribe(): Vuurt in de regel direct (synchroon) zodra de .next()
  methode op de stream wordt aangeroepen.
- effect: Vuurt altijd asynchroon via een microtask scheduler. Angular bundelt alle
  signal-wijzigingen en voert het effect pas uit nadat de huidige code-executie en
  veranderingsdetectie-cyclus stabiel is afgerond.

275. Waarom werken Signals uitstekend samen met de async/await syntax?

RxJS streams combineren moeizaam met async/await omdat Observables over tijd meerdere
waarden kunnen uitstoten, terwijl Promises (waar async/await op leunt) eenmalig zijn. Signals
daarentegen bevatten altijd direct een synchrone, actuele waarde. Je kunt een signal dus
probleemloos en direct uitlezen in elke asynchrone async/await functie zonder te hoeven
stoeien met .toPromise() conversies. 276. Hoe ga je om met fouten (Error Handling) binnen een computed signal?

Als er een runtime-fout optreedt binnen een computed signal (bijv. je probeert een property te
lezen van een undefined object), zal het signal die fout cachen. Telkens als iemand de waarde
van het signal probeert op te vragen, zal het signal die exacte fout opnieuw opwerpen (throw). Je
vangt dit op met een reguliere try/catch om het signal heen. 277. Kun je Signals gebruiken in Angular Directives?

Ja, absoluut. Signals werken exact hetzelfde in @Directive als in @Component. Je kunt
signal-inputs, model-inputs en effects definiëren binnen een directive om gedrag in de DOM
fijnmazig aan te sturen op basis van reactieve staat. 278. Hoe beïnvloeden Signals de architectuur van Angular Services (Stores)?

Signals maken services een stuk overzichtelijker. Waar je vroeger een complexe combinatie nodig
had van een private BehaviorSubject en een publieke .asObservable(), gebruik je nu
simpelweg een private signal() en een publieke .asReadonly() variant. De code krimpt
met de helft en is direct synchroon leesbaar. 279. Wat is het 'Push-Pull' model van Angular Signals?

Signals werken via een gecombineerd Push-Pull mechanisme:

- Push: Wanneer een bronsignal verandert, stuurt het een minimaal notificatie-signaal (push)
  naar al zijn consumenten (computed signals of de UI) om te zeggen: "Ik ben vuil (dirty)".
  Er wordt nog niets berekend.
- Pull: Pas wanneer de UI daadwerkelijk gerenderd moet worden of een effect start, trekken
  (pull) de consumenten de allernieuwste waarde synchroon naar zich toe en voeren ze de
  berekening uit.

280. Betekent de komst van Signals het definitieve einde van RxJS in Angular?

Nee. RxJS blijft een onmisbaar en krachtig onderdeel van Angular voor alles wat te maken heeft
met complexe asynchrone stromen, timing en events (zoals websockets, event-streams, API-
foutafhandeling met retries, of geavanceerd debouncing). De vuistregel is: Signals voor toestand
(State), RxJS voor stromen (Asynchrone Events).
Deployment en DevOps (281-300) 281. Wat is het doel van de ng build opdracht?

De ng build opdracht compileert de Angular TypeScript- en HTML-code naar sterk
geoptimaliseerde, geminificeerde en gecachte JavaScript- en CSS-bestanden (statische assets). Deze
bestanden worden geplaatst in de dist/ map en zijn klaar om geserveerd te worden door een
webserver. 282. Wat is Ahead-of-Time (AOT) compilatie en waarom is het de standaard

voor productie?
AOT-compilatie betekent dat de Angular-compiler de templates en componenten vertaalt naar
efficiënte JavaScript-code tijdens het build-proces op je machine of CI/CD pipeline. Dit verschilt
van Just-in-Time (JIT) compilatie, waarbij de browser dit runtime pas doet. AOT zorgt voor een
veel snellere opstarttijd in de browser en voorkomt dat de zware Angular-compiler mee gedownload
moet worden naar de client. 283. Wat is de betekenis van 'Cache Busting' in Angular builds?

Wanneer je een productie-build draait, voegt Angular een unieke cryptografische hash toe aan de
bestandsnamen (bijv. main.a8b3cd62.js). Dit heet Cache Busting. Het zorgt ervoor dat als je
een nieuwe release uitrolt, de browser van de eindgebruiker direct herkent dat de bestandsnaam is
veranderd en dwingt hem om het nieuwste bestand te downloaden in plaats van een oude, lokale
cache te gebruiken. 284. Hoe configureer je omgevingsvariabelen (Environment Variables) in

moderne Angular-apps?
In moderne Angular-apps is de environments/ map niet meer standaard aanwezig om builds
clean te houden. Je activeert dit eenvoudig via de CLI: ng g environments. Dit maakt een
environment.ts (voor dev) en environment.development.ts aan. Tijdens een
productie-build vervangt de Angular-compiler de bestanden automatisch op basis van de
configuratie in je angular.json.

```ts
// environment.ts (Productie)
export const environment = {
  production: true,
  apiUrl: "https://api.mijnproductie.nl",
};
```

285. Wat is het belang van de angular.json bestand?

De angular.json is het centrale configuratie-hoofdkwartier van de Angular CLI. Hier
definieer je hoe projecten gebouwd moeten worden, welke polyfills ingeladen worden, welke
styling-preprocessors (SASS/LESS) actief zijn, en stel je specifieke build-budgetten en omgevings-
vervangingen (file replacements) in per build-configuratie (dev, staging, productie). 286. Wat zijn Build Budgets en hoe beschermen ze je applicatieomvang?

Build Budgets zijn drempelwaarden die je instelt in angular.json. Hiermee geef je aan hoe
groot de JavaScript- en CSS-bundels maximaal mogen worden. Als een ontwikkelaar per ongeluk
een loodzware npm-pakket importeert waardoor de bundelgrootte de limiet passeert, zal de build in
de CI/CD pipeline falen met een error. Dit voorkomt dat de app ongemerkt dichtgroeit.

```ts
	"budgets": [
	{ "type": "initial", "maximumWarning": "500kb",
	"maximumError": "1mb" }
	]
```

287. Hoe configureer je een Nginx server om een Angular SPA correct te hosten?

Omdat Angular een Single Page Application (SPA) is, regelt Angular de routing intern in de
browser. Als een gebruiker direct navigeert naar [domain.com/dashboard](https://
domain.com/dashboard) en de pagina ververst, zal Nginx een 404 Not Found geven
omdat dat bestand fysiek niet bestaat op de server. Je moet Nginx configureren om bij elke aanvraag
altijd index.html terug te geven.

```ts
	server {
	listen 80;
	server_name mijnangularapp.nl;
	root /usr/share/nginx/html;
	location / {
	# Probeer het exacte bestand te serveren, val anders
	terug op index.html
	try_files $uri $uri/ /index.html;
	}
	}
```

288. Wat is een Docker Multi-stage Build en hoe pas je dit toe op Angular?

Een multi-stage Dockerfile gebruikt meerdere tijdelijke containers om het uiteindelijke image zo
klein en veilig mogelijk te maken. Fase 1 gebruikt een zware Node.js container om de Angular-app
te bouwen. Fase 2 kopieert alleen de resulterende dist/ bestanden naar een vederlichte, kale
Nginx container.
Dockerfile

# Fase 1: Bouwen

FROM node:20 AS build
WORKDIR /app
COPY . .
RUN npm ci && npm run build --configuration=production

# Fase 2: Serveren

FROM nginx:alpine
COPY --from=build /app/dist/mijn-app/browser /usr/share/
nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf 289. Wat is CI/CD en wat doet het voor een Angular project?

CI/CD (Continuous Integration / Continuous Deployment) is een geautomatiseerd proces (bijv. via
GitHub Actions, GitLab CI of Azure DevOps). Bij elke commit of pull request voert de pipeline
automatisch de linters uit (npm run lint), draait de unit-tests (npm run test), bouwt de
applicatie (npm run build) en uploadt de bestanden bij succes direct naar de cloud of hosting-
provider. 290. Wat is de rol van Source Maps (--source-maps) bij foutopsporing in

productie?
Source Maps koppelen de gecomprimeerde, onleesbare productie-JavaScript code terug naar je
originele TypeScript-bestanden. Standaard staan ze uit in productie om code-diefstal en extra
bundelomvang te voorkomen. Als je onduidelijke foutmeldingen uit productie krijgt (bijv. via
foutrapportage-tools zoals Sentry), kun je source maps uploaden naar die tools om fouten exact te
herleiden naar de juiste regel TypeScript-code. 291. Hoe optimaliseer je npm installatietijden in CI/CD pipelines?

Gebruik in pipelines altijd npm ci (Clean Install) in plaats van npm install. De npm ci
opdracht negeert aanpassingen, kijkt strikt naar de package-lock.json en installeert de
exacte dependencies op een deterministische manier. Dit is tot wel twee keer sneller en garandeert
dat de pipeline exact dezelfde pakketversies gebruikt als de ontwikkelaar lokaal. 292. Wat is 'Tree Shaking' en hoe beïnvloedt dit de deployment?

Tree Shaking is een optimalisatiestap waarbij de bundler (zoals Esbuild) de code analyseert en alle
ongebruikte functies, klassen of dode code uit de uiteindelijke JavaScript-bestanden sloopt ("als je
schudt aan een boom, vallen de dode bladeren eraf"). Grote bibliotheken (zoals Lodash of RxJS)
worden hierdoor alleen voor die specifieke functies meegenomen die je daadwerkelijk gebruikt. 293. Hoe ga je om met CORS-problemen (Cross-Origin Resource Sharing) na

deployment?
CORS is een browser-beveiliging. Als je Angular-app op [https://app.nl](https://
app.nl) draait en data opvraagt bij [https://api.nl](https://api.nl),
blokkeert de browser dit tenzij de API-server expliciet de HTTP-header Access-Control-
Allow-Origin: [https://app.nl](https://app.nl) meestuurt in zijn
antwoord. Je lost CORS-problemen dus op door de backend-server correct te configureren, niet de
Angular-client. 294. Wat is het voordeel van het hosten van een Angular app op een Content

Delivery Network (CDN)?
Omdat een gebouwde Angular-app volledig bestaat uit statische bestanden (HTML, JS, CSS), leent
het zich perfect voor een CDN (zoals Cloudflare, AWS CloudFront of Netlify). Een CDN kopieert
je bestanden naar honderden servers wereldwijd. Als een gebruiker in Tokio je site opvraagt, krijgt
hij de bestanden direct geserveerd vanaf een server in Tokio in plaats van een server in Amsterdam.
Dit verlaagt de laadtijd (latency) gigantisch. 295. Wat is compression (Gzip / Brotli) en waarom is het cruciaal voor DevOps?

JavaScript- en CSS-bestanden zijn tekstbestanden en kunnen extreem efficiënt worden
gecomprimeerd (ingepakt). Door je webserver (Nginx/Apache) zo in te stellen dat deze bestanden
serveert met Brotli of Gzip compressie, verklein je de overgedragen bestandsgrootte over het
netwerk met wel 70%. Dit resulteert in een flitsende laadtijd op mobiele netwerken en bespaart
bakken met bandbreedtekosten. 296. Hoe ga je om met runtime configuraties die pas na de build bekend zijn?

Soms wil je dezelfde Angular-build deployen naar Staging én Productie, waarbij alleen de API-URL
verschilt. Omdat environment.ts hardcoded wordt meegecompileerd, werkt dat hiervoor
niet. De oplossing is om een config.json bestand in de assets/ map te zetten. Je laat
Angular dit bestand runtime inladen via de APP_INITIALIZER (zie vraag 136). Tijdens
deployment kan je DevOps-pipeline die config.json eenvoudig per server overschrijven met
de juiste variabelen. 297. Wat is Semantic Versioning (SemVer) en hoe pas je dit toe op Angular

projecten?
SemVer is het versienummerings-systeem opgebouwd als MAJOR.MINOR.PATCH (bijv.
17.2.1):

- MAJOR: Bijbreken van achterwaartse compatibiliteit (breaking changes).
- MINOR: Toevoegen van functionaliteit op een achterwaarts compatibele manier.
- PATCH: Achterwaarts compatibele bugfixes. Angular volgt dit schema strikt; updates binnen
  dezelfde MAJOR-versie zijn gegarandeerd veilig uit te voeren.

298. Wat doet de ng update opdracht?

De ng update opdracht is een van Angular's meest krachtige tools. Het kijkt naar de nieuwste
frameworkversies en update niet alleen de regels in je package.json, maar voert ook
automatische code-migraties (schematics) uit. Als een bepaalde methode in de nieuwste Angular-
versie is hernoemd, herschrijft ng update automatisch jouw TypeScript-code door het hele
project om aan de nieuwe standaarden te voldoen. 299. Hoe monitor je fouten (Error Logging) van een gedeleployde Angular app?

Omdat Angular in de browser van de gebruiker draait, zie je server-side geen foutmeldingen
voorbijkomen in je backend-logs als de UI crasht. Om dit te tackelen implementeer je een centrale
ErrorHandler-service die onopgevangen runtime-fouten automatisch via een API-call doorsluist
naar een monitoring-platform (zoals Sentry, LogRocket of Azure Application Insights).

```ts
@Injectable()
export class CentraalLogGevaarHandler implements ErrorHandler {
  handleError(error: any): void {
    console.error("Fout gevangen:", error);
    // Sluis de fout direct door naar Sentry/externe API
    // Sentry.captureException(error.originalError || error);
  }
}
```

300. Wat is de betekenis van een 'Green-Blue Deployment' strategie voor een

Angular app?
Bij een Blue-Green deployment draai je twee identieke productie-omgevingen: 'Blue' (de actieve
live-omgeving met de huidige versie) en 'Green' (de nieuwe versie die klaarstaat). De DevOps-
pipeline uploadt de nieuwe Angular-build naar de Green-omgeving en test of alles werkt. Is de
controle succesvol? Dan switcht de router/load-balancer het verkeer binnen een milliseconde van
Blue naar Green. Dit garandeert een zero-downtime deployment; de gebruiker merkt geen enkele
onderbreking tijdens de release
