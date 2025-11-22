# Optimizely CMS 12
> 70 spørsmål, Må ha 70% for å bli godkjent, Kun et riktig svar, Færre “lure-spørsmål”, Basert på første versjon av CMS12 (f.eks. .NET 7 var ikke sluppet enda), ContentGraps ikke lansert 

## Optimizely Certification topics
Knowledge areas // topics
- Product knowledge: Concept, Editing, Administrating, Finding information
- Installation, Operation & Configuration: Environment, Solution Set up, DXP cloud hosting, Add-ons
- Website Implementation & Delivery: Technical Architecture, Implementing Search and Navigation, Authentication & Authorization, Classes and Interfaces, Globalization, Performance & Caching, Dynamic Data Store, Visitor Groups
- Framework Components: Plug-ins, Scheduled jobs, Initialization, Extending the CMS UI, Content Providers, Notifications
- Content Models: Rendering, Content types, Essential classes, Properties, Validations, Content Approvals, Optimizely Forms, Projects

## Viktige URL
- https://world.optimizely.com
- https://world.optimizely.com/support
- https://docs.developers.optimizely.com/
- https://feedback.optimizely.com/ 

## Breaking Changes
NuGet package struktur 
- EPiServer.Cms.AspNet er delt opp i flere forskjellige pakker 
- EPiServer.ServiceLocation.StructureMap fjernet 
- EPiServer.Logging.Log4Net fjernet 

Logging: EPiServer.Logging er nå bare et lager oppå ASP.NET Core logging 

Routing 
- Access checks - Autentisering skjer nå etter at routing er ferdig 
- RequestLanguage - språk settes etter routing er ferdig 
- Html.ActionLink/Html.BeginForm byttet ut med Html.ContentLink/ Html.BeginContentForm. 

- Plugin: Fjernet GuiPlugIn PagePlugin
- VirtualPathProviders: Fungerer ikke lenger, går an å bruke MappingPhysicalFileProvider
- EPiServerProfile er fjernet: https://docs.developers.optimizely.com/content-management- system/docs/breaking-changes-in-content-cloud-cms-12 

## Rettighetsnivåer
- I tillegg til de vanlige CRUD finnes Publish og Administer 

## Rettigheter
Tilgang til edit og admin med virtual roles i appsettings
- <location path="EPiServer">
- <location path="EPiServer/CMS/admin">

- Kan settes på en side i edit eller sidestruktur i admin, kan settes på blokk- og sidetyper (også i kode), kan settes på språk i admin (ikke fra kode), kan settes på funksjoner, tabs/paneler. Kan IKKE sette rettigheter på egenskaper. 
- Virtuelle roller finnes kun runtime (de er ikke presisert noe sted)
- Check if current user can see a site: pageDataObject.QueryDistinctAccess(AccessLevel requestedLevel)
- Check if current user can edit a site: currentPage.ACL.QueryDistinctAccess(EPiServer.Security.AccessLevel.Edit);

## Edit mode
- Navigation Pane og Asset Pane kan pinnes.
- Kan bla.a. legge til språk. 
- to måter å vise en side på i cms, enten preview eller all properties view. 

## WasteBasket
Ligger nederst i navigationpane på tannhjul. Multisite sider deler wastebasketId og RootId. Info som vises i søppel er navn, dato slettet, hvem slettet. Viser ikke opprinnelig plassing. Krever admin rettigheter for å se søppelkurven. Slettes etter 30 (hardkoda) dager. Jobber kjører 1 gang i uka som default. Hele eller enkeltsider kan slettes manuelt. Advarsel ved sletting av innhold som er i bruk. Kan ignoreres. Hvis man restorer fra søppel, vet man ikke hvor det ender opp på siden, kan ikke restore til en annen lokasjon enn der den opprinnelig var.

## ImageEditor
When adding image to CMS, in config files, we can define preset sizes for images in image editor and there are som pre- defined sizes already in image editor that we can use to scale an image.

## Display in navigation
- Kun et flagg, ingen form for automatisk filtrering
- Vi må altså ta hensyn tilflagget selv i kode
- Check-box som sier noe om siden skal vises i menyer eller ikke. Finnes ikke ikkebygd funksjonalitet i episerver rundt dette. Om man skal bruke det flagget til å filtrere innhold fra menyer eller ikke, så er det helt opptil hvordan man skriver koden for det. Må selv lage funksjonaliteten dersom vi ønsker å bruke det.

## Language gadget
- Språkhåndtering per side
- Enkelt å se hva som er publisert på hvilket språk
- Egen pakke, kan kjøres for hele nettstedet
- AI oversettelser
- (CMS11, usikker på om CMS12?) Underliggende teknologi er MS Cognitive Services (Bing Translator). Kan konfigureres til å automatisk oversette innhold i optimizely. Må opprette «Subscription Key» før bruk. 80% bra

## Sortering av undersider
- Settings.SortIndex fungerer kun når foreldreside har “Acording to sort index”
- Drag & drop endrer automatisk siden og foreldreside. 
- Under settings på en spesifikk side kan man se dropdown «Sort subpages» hvor man kan finne måter å sortere på. Hvis sortering byttes må man republisere siden fordi det er en egenskap på startsiden.

## Digital Experience Platform (DXP)
- EPiServer.CMS.UI.Core 11.1.0 og EPiServer.CMS.Core 11.2.1 og oppover
- EPiServer CMO, Mail, Relate og Search er add-ons som ikke støttes.
- 24/7 Overvåkning og support
- Cloudflare
    - DDOS Protection
    - Web Application Firewall (WAF)
- Miljø for Integration, Preproduction og Production
- Pris basert på sidevisninger

## DXP - Deploy
- Deployment-API:  Deploy, Content Synchronization, Export, Storage Containers (List), Epinova DevOps extension bruker dette
- Poweshell Module – EpiCloud: Deploy, Reset, Complete, DatabaseExport

## DXP - PaaS portal
Deploy
- Pushe kode fra Integration -> PreProduction -> Production
- Slot-deploy
- ZeroDownTime Deploy
- Inkludere DB og Blobs er bare mulig ved første deploy til et miljø

Content Synchronization
- Prod -> Prep eller Inte
- Prep -> Inte
- Inte -> Prep

- Troubleshoot: Restarte miljø, Purge CDN Cache, Laste ned backup av DP, Åpne logging, Se Outbound IP-adresser – Hostnames
- API
- Appsettings

## DXP
- Full backup av databasen hver time
- Backup av transaksjonslogg hvert 5. Minutt
- Backup av kode hver 24 time.
- SQL backup lagres i Azure Storage Account i 35 dager.
- Blob blir ikke tatt backup av, men er Disaster resilient
- Failover i DXP ved at data replikeres over fra primær- til sekundærmiljø.
- Failover region er i read only mode. Detekteres automatisk hvis man har valgt å skru det på.
- Trafikken rutes automatisk tilbake igjen til primary når den kommer opp igjen.
- Secondary geographically redundant location within the same delivery region
- Logging foregår som normalt. 

## Scheduled Jobs
- Needs to be decorated with [ScheduledPlugIn]
- Own jobs need to inherit from ScheduledJobBase and implement static method “Execute”.
- Runs as AnonymousUser and without HttpContext (null) when it starts automatically. If I start the job manually, it runs as my user. 
- Manuell kjøring påvirker ikke planlagte kjøringer
- [ScheduledPlugIt(Restartable = true)] : use to make restartable job, it will try 10 times, then run with next scheduled run.
- Kan ikke fortsette der den slapp (uten at du legger opp til det i kode)
- GuiPlugin er ikke lengre i bruk
- Har kortere cache ved henting av sider (1 min)
- Intervall settes i admin – husk å sjekke medfølgende jobber
- De fleste jobber kan stoppes (can set IsStoppable to true to be able to stop the job (gives message and logs when done)
- Kan gi statusmeldinger underveis
- Gir melding når ferdig
- Logger tidspunkt for ferdig (ikke start)
- 15 stk. jobber medfølger basisinstallasjon

## Link validation (scheduled job)
A job that checks all links in solution and checks if they’re broken. It doesn’t fix them, but makes an overview over them in the Report Center (Link Status Report). This can be configured in appsetting.json.

## Publish Delayed Content Versions (scheduled job)
Default kjøres en gang per time. Medfører mulig forsinkelse på 00:59:59

## Automatic emptying of trash (scheduled job)
Innholdet slettes etter 30 (hardcoded) dager, hvis endre på det må egen kode skrives. Kjøres 1 gang i uken (default), kan endre frekvens (kan IKKE endre på 30 dager)

## Trim content version job (scheduled job)
- Trim Content Version job: hvis ikke, trimmes det ved lagring av ny versjon av en side. Hvis det er mange versjoner av en side, blir ikke versjonene slettet før jobben er kjørt og den publiserer siden på nytt.

## Url
- Interne lenker. -Husk feature for querystring
- Url segment settes kun ved Create, ikke publish
- Url segment kan enders ved publisering
- NB! Endres ikke automatisk ved endring av nav
- Sett url-segment tomt for å automatisk generere på nytt
- Flytting av side medfører url-endring av undersider for alle språkversjoner (upresist spørsmål mht. søskensider)
- Simple address: kortadresse, type epinova.no/bananspråk, kan ha slash: "/my/custom/campaing/url".
- Hvis en side flyttes på innenfor sine søsken, skjer det ikke noe med undersidene, men hvis det flyttes til annet sted i strukturen påvirker det URL for undersider. Finnes Scheduled job for å generere alle på nytt --> "Replace Page Names in Web Address".

## Version Gadget
Må legges til manuelt per redaktør. Kan republisere og slette tidligere versjoner for et gitt språk (brukes også for å slette språkversjoner og sider). For å legge til gadget trykker man på tannhjul  add gadgets  version gadget

## Language
- Replacement language erstatter språket selv om siden finnes en oversatt side på det språket (bytter ut et språk med et annet, bryr seg ikke om det finnes oversat versjon av siden eller ikke)
- Fallback language erstatter bare språket dersom siden ikke finnes oversatt til det språket (vil vises til bruker hvis feks norsk ikke finnes, men engelsk gjør det, Kan ha inntil 5 fallback språk pr språk (setes opp i edit mode)). For ikke å få noe reservespråk, for eksempel at søskenelementer på en engelsk side kommer opp på norsk, fordi det ikke finnes en oversatt tilsvarende engelsk side, så kan man unngå dette ved å ikke ha noe reservespråk/fallback fra engelsk. Dette vil føre til at de siden som ikke er oversatt til engelsk ikke vil vises.
- Settes opp i Edit modus => Side => Tools => Language Settings.
- MasterLanguage er det første språket siden opprettes på. Den har verdi i alle egenskaper. Masterlanguage er ikke nødvendigvis entydig på hele siten, kan være forskjellige masterlanguage på samme site. Andre språk har bare det som er tagget med [CultureSpecific] attributtet. Dette gir eget ikon for egenskapen i edit (noen egenskaper er kun redigerbare på hovedspråket, derfor det er ReadOnly på siden)
- Alle egenskaper kan være CultureSpecific unntatt egenskaper på MediaData.
- Kan skjule språkhåndtering med UIShowGlobalizationUserInterface i appsettings
- Optimizely har en LocalizationService som er provider basert. Settes opp i Startup.cs. Brukes for å hente ut oversatte tekster - GetString("my/key/here").
- For å hente alle språkversjoner av en side: IContentRepository.GetLanguageBranches<SitePageData>(ref)
- For å hente alle CultureInfo for en side: currentContenent.ExistingLanguages
- Tannhjul i navigationpane har filter for valgt språk
- Sider som ikke finnes på valgt språk vises i grått med italic
- For å slette en språkversjon brukes VersionGadget
- Rettigheter til språk settes i admin, og ikke i kode.
- @Html.Translate respekterer fallback og replacement.
- Forskjell på CurrentCulture og CurrentUiCulture
- CurrentCulture: bestemmer ut ifra hvilken URL jeg går inn på. Systemspråk (dato, tid osv)(hvis jeg besøker siden)
- CurrentUiCulture: gjelder i redaktør og admin grensesnitt, siden og innholdet i episerver (hvis jeg går inn i edit)
- Ledetekster i edit/admin oversettes med xml i lang-mappa. F.eks endring av label på MainBody gjøres altså med xml-fil
- NB! Navn på språkfiler er avgjørende for hvilken fil som brukes. Sorteres alfabetisk. First come, first served. Filnavnet som kommer først er det som vil gjelde hvis alle oversettelser finnes to steder. 

## Dekorering av ContentType
```c#
[ContentType(GUID = "87376439-180b-4c8a-850c-3d1210f420e", Order = 10)]
[ImageUr1("~/UI/icons/empty-png")]
[AvailableContentTypes(Availability = Availability.Specific, Include = new[] { typeof(SearchPage) }, IncludeOn = new[] { typeof(StartPage) })]
[Access(Access = EPiServer.Security.AccessLevel.Create, NameSeparator = ". Users = "bob,alice", Roles = "edit,admin", VisitorGroups = "chromeSurfer,firefoxSurfer")]
public class ArticlePage : EditorialPageBase, IHasImage
```
- Dekoreres med [ContentType] her setter man displayname, order, description.
- ImageUrl: setter på bildet på ContentType
- AvailableContentTypes: setter tilgjengelighet på et barn på denne content typen og content typen selv
- Include: defines content types that can be created as children of a content of this type
- IncludeOn: defines content types that can be parent of a content of this type

## Dekorering av Property
```c#
[Display(Order = 300, GroupName = SystemTabNames.Content)]
[EditorDescriptor(EditorDescriptorType = typeof(TextareaEditorDescriptor))]
[Searchable(true)]
[CultureSpecific]
[UIHint("Abstract")]
[ScaffoldColumn(true)]
public virtual string MainIntro { get; set; }
```
I tillegg finnes validatorer: StringLength, RegularExpression, Range, AllowedTypes, Required, EmailAd mm.

- Searchable(true) hvis jeg bare skriver searchable så er det true (alt av string er automatisk searchable) 
- UIHint(“Abstract”): styling av property (hvordan egenskapen skal se ut i editmodus) 
- ScaffoldColumn(false): hvis false så blir det ikke en synlig egenskap I edit modus I episerver. Verdier en redaktøren ikke trenger å redigere på/se/tukle med, men som skal sees på siden. 
- Editable: hvis settes til false så kan man se den i edit modus, men ikke endre på den (ikke-CultureSpecific ish) 
- AllowedTypes: I et contentArea kan man bestemme hvilke typer som skal være tillatt. (AvailiableTypesAttribute ligner!) 
- [Required]: dekoreres på property to make a model property required 
- Can create custom validation in CMS that would prevent content from  being published if the validation rule was broken: Implement IValidate<T> interface for a content type & create class that inherits ValidationAttribute and apply it to a property. Signature of IValidate<T>.Validate() method: IEnumerable<ValidationError> Validate(T instance) 
- UiMaxVersions: max antall versjoner som skal lagres av en side eller blokk. Når man publiserer en side, så sletter episerver de som er over grensen. Settes den til 0 er det samme som uendelig antall versjoner (skriver til episerver.config og recycler app-pool. Utføres kun på en server i LB. Overskrives ved ny deploy)

## Permissions for functions
- Settings => Permissions for Functions
- Gi brukere/grupper tilgang til følgende:
    - Allows users to move data/pages between page providers
    - Detailed error messages for troubleshooting (det skrur vi ofte av)

## Admin
- Ingen angre mulighet i admin. Full eller ingen tilgang til alt, ingen granulert tilgang
- Rettigheter kan settes for kun en side i edit, mens for hele strukturen I admin

## Admin - konvertering av sidetyper
- Fra side til annen side
- Fungerer ikke med blokker (men Tomas Gulla har laget NuGet for det (CMS 11))
- Prøver å automappe egenskaper

## Antall versjoner av content
- Settes MaximumVersions = 0 => uendelig antall versjoner.
- Settes i appsettings.json (ikke lengre i admin)

```json
{
    "EPiServer": {
        "Cms": {
        "MaximumVersions": 25,
        //more code
```
- Antall versjoner er per språk
- Versjoner fjernes ikke ved publisering.
- Versjoner fjernes av «Trim Content Versions» jobb.
- Draft kan lages fra en tidligere versjon
- Versjoner vises av «Version gadaget»

## Admin - manage websites
- Admin => Manage Websites for å legge til nye siter og administrere eksisterende siter.
- Opp til 100 siter
- Viktig å ha riktige URL-er med tanke på lisensiering
- Hver site må ha unik startside
- Hver site sin startside vil markeres med hus-ikon i Edit
- Kan/bør krysse av for "Use site-specific assets", hvis ikke kan alle siter bruke hverandre sine blokker og bilder. 
- Benefits of multisite setup: Share the same content assets, such as media files and blocks. Run your websites on the same IIS application as long as you configure the site IIS to respond to multiple host names. Easily remove websites.

## Admin - Import/Export
Export/Import => Content items (også blocks), Content types, Frames, Tabs, Categories og Visitor groups. Filen ExportedFile.episerverdata kan endres til ExportedFile.zip og utforskes. Inneholder XML

## Admin - Changelog
Change log logger alt av actions på content. Scheduled job for å rydde opp i over 30 dager gamle eventer

## Admin - ContentTypes
- Default verdier settes i admin eller i kode med override SetDefaultVaules() på modellen din. (skjer kun ved create)
- Hvis man gjør endringer i admin modus på content types, vil ikke nye kodeendringer automatisk synkroniseres. Dette varsles kun ved å vise en warning i admin gui. Da overrider man default values og endringer i kode vil ikke lenger bli synkronisert. Men hvis jeg legger til en ny egenskap så stopper ikke synkroniseringen, for da er det ikke noen konflikter. Samme gjelder rettigheter på sidetyper. Kan sette rettigheter på grensesnitt, blokker, filer, sidetyper, blokktyper, funksjoner, språk, faner. Brukere, grupper, visitor groups.

## Container page
- Container page implementeres ved å lage en model uten controller. (ikke la deg lure til å tro at du kan skippe template el.)
- Vises annerledes i sidetreet
- Kan også registrere en UIDescriptor for samme visning

```c#
[UIDescriptorRegistration]
public class BlogPostUIEditorDescriptor :
ExtendedUIDescriptor<BlogPost>
{
    public BlogPostUIEditorDescriptor() : base("epi-
iconBubble"){ }
}
```
Sidetype/model uten view/controller. Da finnes det ikke noe måte å route til eksekvering av visning. Uten controller er Default AllPropertiesView. Kan også registrere en [UIDesctiptorRegistration] MyUIDescriptor: UIDescriptor<MyCustomPage> for tilpasset utseende (ikon/css class)/oppførsel (DefaultView.AllPropertiesView). Så kan jeg f.eks. si at i sidetreet skal vi få en eller annen css klasse som trigger et eller annet.

## Konvensjoner
- Sende med MyPageType currentPage til Index metoden for å automatisk resolva siden man står på. Samme gjelder for blokker currentBlock.
- Kan også benytte currentContent
- Casing er irrellevant med tanke på spørsmål på sertifiseringen. (NB! CURRENTPAGE vil også fungere)

## Blokker
- Kan være shared eller private (local block)
- Blokker har ingen public url (this is one of the characteristics of blocks, and it's main difference from pages. Must implement/inherit BlockData)
- Nytt for CMS 12 er Component – ikke controller
- Blokker må kastes til IContent før de lagres via kode. contentRepository.Save(myBlock as IContent).
- Må castes til IContent for å kunne sette navn (var block = myBlock as IContent). A block is IContent at runtime, but not before)
- ContentReference.GlobalBlockFolder
- Can improve performance of a block by removing controller (use only view). Then the name of cs file needs to be exactly the same that cshtml file (without in name)
- Publishing a block: ContentRepository.Save(block as IContent, SaveAction.Publish);
- Indeksering av blokker kan gjøres på 3 måter
    - Markere blokken med [IndexInContentAreas]
    - Lage en bool som heter IndexInContentAreas på blokken og sette den spesifikk per instans av blokken
    - Bruke search convention IContentIndexerConventions.ShouldIndexInContentAreaConvention og implementere logikken der.

- How check if block is of certain type: get block using ContentLink and ContentLoader. Cast it to the specific type. Then check if it is null eln: If(currentBlock != null && currentBlock is IframeBlock block{ ... do whatever with block}
- Not a way to check if a block is of type TeaserBlock: if(block.GetType() == typeof(TeaserBlock))
- Konvensjon: Sende med MyPage/BlockType currentPage/currentBlock/currentContent til Index metoden for å automatisk resolva/model binde siden man står på → public ActionResult(ArticlePage currentPage)

## Media
- Filer må arve fra MediaData (baseklassen)
- ImageData for bilder og VideoData for video. 
- Tagge med [MediaDescriptor(«filtype»)] for å fortelle hvilken filtype  som skal bli hvilken type i Optimizely. 
- Egenskaper på Media støtter ikke globalisering. 
- Media og blokker har felles struktur og versjonering (ikke felles)
- ContentFolder kan ikke oversettes og har ikke historikk/versjonering. 
- Media publiseres automatisk ved opplasting til arkivet. 
- Media publiseres med siden ved bruk av «For this page» (men bare første gang) 

## FindPagesWithCriteria
- Måte å finne mange sider basert på forskjellige kriterier. 
- Finnes i IpageCriteriaQueryService, den har hverken cache (spør DB hver gang) eller filtrering. Har to metoder: 
    - FindPagesWithCriteria gir bare publisert innhold for bruker
    - FindAllPagesWithCriteria tar ikke hensyn til rettigheter eller publisering (viktig da å kjøre et av optimizely sine innebygde filtre før man presenterer dette for besøking)
- For å filtrere mottatt resultatsett benyttes for eksempel FilterForVisitor(template, rettighet, publisert).
- NB! Flytter last fra webserver til databaseserver. (Kanskje man har mange lastbalanserte webservere, men bare en dbserver).
- ExcludeDeleted(): tar ikke med ting som er i papirkurven i søkeresultat eller vises for redaktør.
- Kriterier som kan settes: required, condition, name, type og value.
- FilterForVisitor (ekskluderer sider som ikke har template, rettighet eller er publisert, har også FilterPublished, FilterAccess og HasTemplate). Jo flere kriterier man spesifiserer, jo lettere blir søket. Selvom innhold er satt som expired så vises det fortsatt på siden. Sjekker for IsPublished, men vises fortsatt. Dette fordi det også var brukt HasExpired, og det burde ikke brukes. Det må alltid heller legges på FilterForVisitor hvis det er innhold som av en eller annen grunn ikke skal være synlig for bruker (expired, recycle bin etc.).
- Filterforvisitor.Filter funker hvis man ikke først henter sidene gjennom contentloader, men de gangene man ikke gjør det så vil det funke å bruke FilterForVisitor.Filter()
- FilterForVisitor.Filter: filtrerer bort alt som ikke er sidetyper (hvis brukt på blokk får man null i resultat).
- FilterForVisitor: finds all pages that are visible for user. 

## Rendering
- [TemplateDescriptor] på controllers for å fortelle hvilken type de kan rendre. Tagen på malen defineres her. 
- Hvis man ikke har har controllers, IViewTemplateModelRegistrator, og registrere opp at hvis det er en spesifikk type så skal det rendres med et bestemt view. 
- Bør rendre blokker uten controllere pga. ytelse. Kan legge viewet til blokken i Views\Shared\DisplayTemplates\MyBlock.cshtml eller registreres custom via IviewTemplateModelRegistrator
- EditAttributes gjør det samme som PropertyFor med OnPageEdit støtte, men den rendrer ut edit attribute som attributter på h1 tagget. Ting som kan gjøres i OnPageEditing: endre navn, trykke på lenker i rich text fields. Hvis bare CurrentContent.MainBody = plain text.

```
@Html.CurrentContent.MainBody //plain text
@Html.Raw(Model.CurrentContent.MainBody) //html
@Html.DisplayFor(m => m.CurrentContent.MainBody, new { Tag = "Mobile"}) //html-no ope support (ope = on page edit support fra Epi)
@Html.PropertyFor(m => m.CurrentContent.MainBody, new { Tag = "Desktop"}) //html with ope support
@Html.FullRefreshPropertiesMetaData // hvis man vil at siden skal lastes inn på nytt ved endring.
```
## De 4 viktige services (iht. Episerver)
- IContentLoader, IContentRepository, IContentEvents, IPageCriteriaQueryService
- Fordelen med interface er at de er enklere å mocke enn konkreter i enhetstester.

- IContentLoader: for å laste innhold. can be used to get children of a page by passing the page’s ContentReference to GetChildren(). Only has methods to query content, vs. IContentRepository has methods to both query&update content.
- IContentRepository: for å opprette og lagre innhold. IContentRepository er raskere og kan lettere sende mock objekt i tester, men det er IKKE bedre ytelse. DataFactory er legacy, skal ikke brukes lenger.
- IContentEvents: for å lytte på eventer som skjer på innhold (event handlers that are menber of this interface: CheckedInContent, CheckedOutcontent, DeletedContent and RejectedContent)
- IPageCriteriaQueryService: FindPagesCriteria, for å finne sider med visse kriterier.
- IContentLoader was designed for read-only tasks. IContentRepository provides read and write only capabilities. As IContentLoader only provides read-only abilities, it exposes fewer methods.

## Create content runtime
- Hvis man skal opprette innhold programatisk er det viktig at det er et IContentRepository, da har IContentLoad valgt lasting av innhold. Man må si hvilken sidetype det skal være, hvor den skal bli lagret under, og gi den et navn. Til sist lagre det man har opprettet (newArticle). Må velge en SaveAction. Det kreves access level «Edit» for å benytte SaveAction.CheckIn og SaveAction.CheckOut i IContentRepository. I eksemplet under valgt Publish fordi det skal publiseres. Rettigheter må også settes, NoAccess vil si at man får lov til å opprette det innholdet.
- Ved oppretting av side, si hvilken type GetDefault<ArticlePage>, si hvor den skal bli lagret ContentReference.StartPage og gi siden et navn og lagre. Må da igjen velge en SaveAction.

## Update content runtime
```c#
var contentRepository = ServiceLocator.Current.GetInstance<IcontentRepository>();
var clone = currentContent.CreateWritableClone() as StartPage;
clone.Name = “Foo”;
contentRepository.Save(clone, SaveAction.Publish, AccessLevel.NoAccess);
```

## EditorDescriptor
- Brukes for å bestemme client side editor for en type 
- a class defining how the client will display a property or a group of properties. With an EditorDescriptor we can choose what Dojo/Dijit widget (javascript file) that is responsible for rendering the property in edit mode.
- ISelectionFactory for å velge items som skal vises 
- [SelectOne] --> rendrer dropdownlist, sier hva slags SelectionFactoryType man skal bruke, over property I content type 
- [SelectMany] --> rendrer en checkbox liste
- [AutoSuggestSelection] og implementere ISelectionQuery --> vil gi meg text-box jeg kan skrive i, så kommer forslag under.
- Create custom Editor: [EditorDescriptorRegistration(TargetType = typeof(String[]), UIHint = Global.SiteUiHints.Strings)], dnd the class has to inherig EditorDescriptor. Then you can create override methods to have custom logic.
- EditorDescriptors is a concept introduced by Episerver in CMS7, and is a class defining how the client will display a property or a group of properties. With an EditorDescriptor we can choose what Dojo/Dijit widget (javascript file) that is responsible for rendering the property in edit mode.
- UIDescriptor is the "template" for the content type, like when you want to hide/show tabs, views etc. https://docs.developers.optimizely.com/content-management-system/docs/describing-content-in-the-ui 
- EditorDescriptor is more "for the property" when you can define the widget you want to use for your property. But you can also use it to change how your content is displayed (using IMetadataExtender which it implements). 

## Lisenser
- Optimizely CMS: needs license
- Optimizely Commerce: needs license
- A/B testing: no additional license fee
- Azure == Instance bound (needs to be activated in GUI)
- On premise == Server bound. Knyttes til mac- eller ip-adresse (går av seg selv)
- DXC Service har ikke lisens. Man leier plattformen (PAAS – platform as a service). 
- Lisensfilen heter License.config ligger default på root, men kan flyttes til et sted du spesifiserer i <episerverframework>.
- Bare en lisensfil for alle installasjoner og epi-produkter.
- Multisite deler filstruktur på disk, kodebase, database, rootpage og wastbasket. Has no additional license, can use multisite defining the maximum number of sites the installation is licensed for.

## Content Approvals
- https://fast.wistia.net/embed/iframe/c5fcmuj6t3?videoFoam=true
- Change approvals for f.eks endring av sortering i sidetreet, rettigheter på innhold, språk innstillinger (fallback/replacement) og utløpsdato. SaveAction.RequestApproval
- En content approval per språk, hvis jeg godkjenner norsk så er den ikke nødvendigvis godkjent på engelsk.

## A/B testing
- https://fast.wistia.net/embed/iframe/zw4482b8h9?videoFoam=true
- Site Stickiness
    - Select a target page and a timeout period (1-60 minutes). The A/B test counts a conversion if a visitor goes from the target page to any other page on your website during the time period.
    - If the visitor closes the browser then opens your target page again within the specified time period, a new page view is not counted.
    - However, a conversion is counted if the visitor goes from the target page to another page during the second visit.
- IKPI for å implementere ditt eget KPI. To metoder som må overrides. Validate og Evaluate.
- Validate: Leser input fra UiMarkup for å opprette KPI instansen.
- Evaluate: Bestemmer om en konvertering har funnet sted eller ikke.
- En vinner kan publiseres automatisk hvis det resultatet er statistically significant. Hvis ikke må den publiserers manuelt. Man kan velge hvilken versjon man vil publisere.
- Samme bruker ser samme versjon hver gang (cookie)
- Tapersiden finnes om egen versjon og kan ev. Republiseres (version gadget)
- "Start now" or "Schedule for later"

How winners are determined: (dette er notater fra CMS11)
1  Select a target page and a timeout period (1-60 minutes). 
2. Visitor sees either the original (A) or the variation (B) of content. A/B testing logs which version the visitor sees. If they return to the content, the visitor sees the same version. If they clear cookies, and revisit the content, they are considered a new visitor in the test. If a visitor clicks on the advertisement, the target page appears and A/B testing logs the actions as a conversion. If the visitor goes from the target page to any other page on your website during the time periode, the A/B test counts a conversion. If the visitor closes the browser then opens your target page again within the specified time period, a new page view is not counted. However, a conversion is counted if the visitor goes from the target page to another page during the second visit. 
3. When the test completes, the version that achieves the best result/ most clicks is declared the winner. Can manually pick winner or it can get automatically published if statistically significant. 
4. The challenger (tapersiden) is saved as own version and kan be re- published (version gadget).

A/B testing - How site stickiness conversion is created (dette er notater fra CMS11)
- Site Stickiness: converts when a user visits the content under test and then visits any other page within the same browser session. Results: Views are the number of visitors that visited the web page. Conversions are the number of visitors that clicked through to any other page within the specified time (in minutes).
- Landing Page: the chosen page is the one that a user must click on in order to count as a conversion. Results: views are the number of visitors that visit the page under test. Conversions are the number of visitors that clicked through to the landing page at any point in the future while the test was running.
- Time on Page: monitors how long a visitor spends on a page and converts after a specified amount of time. Views: number of visitors that viewed the page under test. Conversions: the number of visitors that remained on the page for the minimum time specified (in seconds)

A/B testing – conversion goals and IKPi interface and when user creates a test, what methods are called (dette er notater fra CMS11)
- You can create custom KPIs (conversion goals), using goal objects in the KPI framework, to use in Episerver A/B testing or in any other package that relies on creating instance-based goal objects.
- To create a custom KPI, implement the IKPi interface located in the EpiServer.Marketing.KPI.Manager.DataClass namespace. Two methods needsto be overrided/implemented
    - Validate: Leser input fra UiMarkup for å opprette KPI instansen (leser input fra redaktør i edit)
    - Evaluate: Bestemmer om en konvertering har funnet sted eller ikke (konvertering eller ikke)
- When a user creates a test the Evaluate function is called.
- 3 original conversion goals. Episerver commerce adds 3 more.
- Samme bruker ser samme versjon hver gang ( pga. cookie). "Start now" or "Schedule for later" (gjelder selve A/B testen)

## Visitor Groups/Personalization
- Personalisere innhold
- VisitorGroup kan tilgjengeliggjøres som en «security role» for å gi tilgang til innhold (rettighetsstyring - kan brukes for å filtrere på hvem som skal kunne se deler av innholdet og sette rettigheter i strukturen)
- En gjestegruppe kan inngå som et kriterie i en annen gjestegruppe
- Rollen "VisitorGroupAdmin" må være definert for at man skal kunne administrere VisitorGroups. 

(dette er notater fra CMS11)
- Kan benyttes på XhtmlString (main body) og ContentArea. Kan da personalisere deler av teksten I main body eller blokker I ContentArea. Can add one or more visitor groups to a single block.
- Personalisere innhold: merkere teksten i TinyMCE og trykke på Personalized content knappen. På ContentArea høyreklikker man på kontekst menyen til selve blokken, velger personalize i den menyen.
- Site preview: øye på toppmenyen, kan også simulere VisitorGroup der for å se hvordan nettsiden ser ut for div. grupper
- VisitorGroupAdmins (CmsAdmins and CmsEditors are already defined in the configuration).
- Kan ha fallback til "Everyone”. Everyone er en virtuell gruppe og ligger alltid sist fordi når man setter opp personalisert innhold vil man gå fra toppen og den første man treffer er den som vises, fallback som ligger sist. I fallback på
- VisitorGroup kan man si at de som serfer fra Oslo skal se et type innhold, og fallback for de som ikke serfer fra Oslo.
- All, Any, Point (hvor mange kriterier må treffes)
    - All: må få treff på alle kriterier for at gjestegruppen skal være aktiv (isMatch = true)
    - Any: “eller” operatør. Hvis det blir treff på en av dem så er VisitorGroup aktiv.
    - Point: vekter hvert kriterie med et vekt tall, så setter man terskel verdi som sier hvor høy summen av alle kriteriene må være for at gjestegruppen skal aktiveres. Hvis man treffer på et kriterie på 5 poeng, så treffer man terskelverdien på 5 og da er VisitorGroup aktiv.

## Visitor groups
- ContentArea og XhtmlString kan personaliseres.
- En blokk eller string fragment kan ha en eller mange gjestegrupper
- Kan ha fallback til "Everyone"

- Preview finnes I toppmeny
- 11 Kriterier i standardinstallasjon. Ytterligere 11 i egen optional Nuget
- Innebygde kriterier:
    - Site Criteria: Number of visits, User profile, Visited category, Visited page
    - Time and place Criteria: Time of day, Geographic Coordinate, Geograpich Location
    - Url Criteria: Landing URL, Referrer, Search Keyword
    - Visitor Groups: Visitor Group Membership.
- Episerver Visitor Group Criteria Pack: Display Channel, IP Range, OS & Browser, Role, Selected Language, Download File, Event, Query String, Time On Site, Time Period

## Visitor groups - definere kriterier
- Arve av CriterionModelBase
- Må override metoden Copy
- Validere med IvalidateCriterionModel
- Kan bruke SelectionFactory for å hente inn verdier fra annen kilde

- VisitorGroupCriterion attributt
- Arve av CriterionBase
- Sjekk hvis det brukeren kvalifiseres i gruppen i metoden IsMatch

## Projects
- Projects er default aktivt. Deaktiveres med appsettings
"EPiServer": { "CmsUI": {  "ProjectUI": { "ProjectModeEnabled": false },

## Optimizely Forms
- Forms only works for MVC and supports for CSM9 and above.
- Form definitions are saved IContent (just like blocks, do they can be treated as content) in Optimizely (ContentRepository), while submissions are saved in DSS
- Hvis man skal kryptere form submissions, så må man gjøre 4 ting.
    - Lag en nøkkel i Azure Key Vault
    - Installer nuget EPiServer.Forms.Crypto.AzureKeyVault
    - Skru på session state
    - Endre innstillinger i Forms.config (overskrives ved Nuget oppdatering)

## Partial Routing
- En partial router registreres i Startup via endpoints
- Det er to metoder man må implementere.
    - RoutePartial blir kalt når vanlig routing har sendt deg til en side av type TContent og det er mer igjen av urlen og rute. /url/to/page/ remaining/path.
    - GetPartialVirtual kalles når man lager utgående url på innhold av typen TOutgoing.

## ASP.NET identity
- API that supports UI login functionality. Manages users, passwords, profile data, roles, claims, tokens, email confirmation and more.
- To install Optimizely Asp.Net Identity, you need to set up the following:
    - Set authentication mode to none i web.config.
    - Remove membership and rolemanager providers i web.config
    - Create an OwinStartup class and add app.AddCmsAspNetIdentity<ApplicationUser>() og app.UseCookieAuthentication(new CookieAuthenticationOptions())

## Virtual roles
- addClaims: et claim legges til current principal for hver virtuelle rolle brukeren er medlem av. Må brukes hvis man bruker claims basert autentisering.
- replacePrincipal: current principal erstattes med en principal object wrapper som supporterer virtuelle roller ved å override IsInRole metoden. Støtter ikke claims. ReplacePrincipal som ikke har støtte for claims, gjør sånn at current principle erstattes med en principal object wrapper som supporterer virtuelle roller.
- <virtualRoles addClaims="false" replacePrincipal="false”>. Står begge til false, vil virtuelle roller bare slå til når man sjekker rettigheter basert på ACL i EPiServer. Alle principal.IsInRole på virtuelle roller vil retutnere false.
- Hvis begge disse er satt til false så brukes ikke disse virtuelle rollene til noe som helst, annet enn i sidetreet.
- Hvis jeg spør i kode så vil den alltid gi IsInRole false. Hvis begge er satt til true så blir det en exception.
- If replacePrincipal="false", virtual roles will only be evaluated when checking access rights based on ACLs in EPiServer CMS. Any principal.IsInRole calls for a virtual role will return false. <virtualRoles addClaims="false" replacePrincipal="false">.
- Innebygd episerver rolle som heter AuthenticatedRole. Sjekker i kode at man er innlogget, det er en tilstand brukeren på nettstedet har.

## Initialization
- Implement [InitializableModule] eller [ModuleDependency(typeof(ModuleNameToWaitFor))] to register a module that is initializable  (ModuleDevendency har man bedre kontroll og må brukes hvis ens egen modul er avhengig av andre moduler, den venter til en annen initialisering er ferdig før jeg eksekverer min kode)
- Må implementere IInitializableModule eller IConfigurableModule for IoC
- IConfigurableModule for IoC, da får man mulighet til å spesifisere containere (har igså en for HttpModuler)
- IInitializableHttpModule for å initializere http eventer.

## Plugins
- [GuiPlugin] for Admin og rapporter(AdminConfigMenu, AdminMenu, ReportMenu, SidSettingsArea, SystemSettings).
- Gadgets kan lages ved å bruke [Component] attributtet og skrive Dojo. Disse kan plugges inn i dashboard og edit(navigation pane og assets pane).
- IFrameComponent kan f.eks benyttes .aspx i widget.
- Hver component har sin egen module.config.
- AssetsPane, DefaultAssetsGroup, DefaultNavigationGroup, NavigationPane.
- React kan benyttes
- A plug-in is an extension that is available to all CMS users that have access to the working area containing the plug-in

## DDS
- Bruke DynamicDataStoreFactory.GetStore() og DynamicDataStoreFactory.CreateStore().
- Tagge klasse med [EPiServerData] og arve IdynamicData (hvis man vil ha objekter som lagres I DDS og få en ID på det)
    - [EPiServerDataStore(AutomaticallyCreateStore=true, AutomaticallyRemapStore=true, StoreName="MyStore")]
- Ofte bedre å bruke egne klasser som arver fra IContent (dette er alternativet til DDS). Dette gir rettighetsstyring, versjonering, kan navigeres til osv. DDS mangler UI.
- DDS er essensielt en «object relational mapper» (ORM).

## ContentProviders
- Arve fra ContentProvider.
- Registreres i kode eller i appSettings
- Data blir boende i ekstern kilde.
- En måte å få innhold inn i Optimizely på. Arve fra ContentProvider, registreres i kode eller config for å få det i sidetreet.
- Entrypoint sier hvor provideren skal gjelde. Registrere også metodene denne provideren supporterer her.
- Må sette name, type, entrypoint og capabilities. Ut ifra hva slags capabilities man velger må man implementere forskjellige metoder. API-et supporterer følgende metoder (capabilities): Move, Create, Change, Languages, Versions
-  Custom ContentProvider can deliver these custom types: pages, blocks, media assets and folders.
- Can initialize new content providers in an initialization module or in the web.config
- Full text search is supported by Default Content Provider.

## Caching
- Object cache.
    - Data fra EPiServer. Objects and content interfaces, alt er objekter på epi (sider, blokker)
    - Bruker HttpRuntime cache.
    - Alle objekter er read only.
    - Styres av pageCacheSlidingExpiration I <episerver><applicationSettings>.
    - Bruk CacheManger for å støtte Load balancing.
    - Invaliders autoamtisk ved behov. The default object cache expiration on web server is set for 12h
- Output cache.
    - Standard Asp.Net Output caching. Html markup, cacher markup, sette utløpstid
    - [ContentOutputCache] på controller. Invalidates when content is published
    - styres av httpCacheExpiration i <episerver><applicationSettings>
- Browser cache.
    - Static files, hvor lenge statiske filer på klientside skal caches hos klient (js, css osv).
    - Styres i <system.webServer><staticContent><clientCache> i web.config.
- File cache(Blobs).
    - Bruker IIS kernel cache.
    - Styres I <staticFile expirationTime=»» /> i web.config. 

- Programmatically manage cached objects: Use the CacheEvictionPolicy class for object caching, because it is de-coupled from any specific cache implementation, and immutable so you can re- use existing instances of the class.
- Fordel med å bruke CacheManager er at det håndterer cache invalidering over flere lastbalanserte noder. Methods that take a CacheDependency instance cannot be used with anything but HttpRuntime.Cache
- Hvis lage egen cache, må arve fra ISyncronizedObjectCache, men hvis man skal putte ting inn I er det CacheManager man bruker. [ContentOutputCache(Duration = 1200)] → caches response for 20 minutes

## Add ons
- Installeres fra kode (Nuget).
- Kan plasseres på to forskjellige steder: protected modules og public modules.
- Protected modules er for moduler/addons som krever autentisering (~/modules/protected/AddOnId)
- Public modules er for moduler/addons som er åpne (~/modules/AddOnId)selve roten.
- Social search: addon som muliggjør på vegne av min bruker, publisering av sider fra optimizely I facebook etc. og kan poste til vegg, sider osv. Kan sette opp brukere I optimizely på hvem som får lov å publisere via. Social search addon (rettigheter)
- Deles inn i tre typer: Solution add on: Image Vault feks. Developer:

- Stort sett alt annet. Site Owner: En administrator kan gå inn og legge til moduler. (spellchecking feks)
    - Add-ons might be plug-ins, scheduled jobs, gadget, content types or templates.
    - Add-ons are distributed as NuGet packages in our official NuGet feed.
    - Only approved partners and licensed 3rd party add-ons may be included in our official NuGet feed.
    - Add-ons resources are stored in a ZIP file.

## Frontend rammeverk
- Støtte for ope ved f.eks Vue eller React, så må man legge til
    - data-epi-property-name=»MyProp»
    - sette data-epi-property-renderer=»none».
    - for å vite når man skal re-render, subscribe til topic beta/contentSaved. (window.epi.subscribe(«beta/contentSaved»)) Man
    - kan ikke bruke Html.EditAttributes()

## Search & Navigation
- Inkludert i DXP (Gjelder ikke nye kunder fra 2024)
- Støtter stemming (å bryte ned et ord til stammene av ordet, feks trikk istedenfor trikker for å få flere søk.): Bruk de to metodene som sender Language inn og bruker GetResult() tilbake.
- Cache:
    - GetContentResult cacher default i 1min (StaticallyCacheFor(TimeSpan))
    - GetResult cacher ikke

## Facts
- Remote Events for å invalidere cache mellom siter i lastbalanserte miljøer. WCF over UDP eller TCP på faste servere. Azure Service Bus i skyen.
- ContentType Properties fra kode kan ikke slettes i admin.
- ContentType Properties kan legges til fra admin
- Man kan ikke endre fane for builtin properties
- Episerver UI oppdateres med Nuget
- Edit og Admin sikres ved med dedikert server, og fjerne muligheten i front server.
- Bruk CacheManager når du vil cache i et last balansert miljø. Ikke la deg forvirre av ISyncronizedObjectCache el.
- I et intranett med default membership- og roleprovider kan man utvide med flere egenskaper på brukerne med Episerver.Personalization.EpiServerProfile
- Ny gruppe som skal ha edit-tilgang registreres i web.config under <location path=«Episerver» eller ved bruk av virtuell rolle i episerver.framework.config
- Har pålogget bruker edit-tilgang for gjeldene side?

```c#
bool canEdit = currentPage.ACL.QueryDistinctAccess(EPiServer.Security.AccessLevel.Edit);
//Distinct er viktig her. Metoden QueryAccess returnerer en liste.
```
- Lenker til sider rendres med @Html.ContentLink, og ikke ActionLink
-  AllowedTypes brukes for å begrense tillatte typer i ContentAreas, ContentReferences mm. Pass deg for lignende som f.eks AvailiableTypesAttribute
- Det kreves access level «Edit» for å benytte SaveAction. CheckIn og SaveAction.CheckOut i IcontentRepository
- Alle disse er gyldige måter å sjekke om currentPage er av type StartPage
- ICriterion.IsMatch kalles en gang per instans av kritriet ved rendring av en side (hvis kriterie er brukt to ganger, eksekverer det to ganger)
- IContentEvents hookes opp med IInitializableModule. Husk [ModuleDependency(typeof(EPiServer.Web.InitializationModule))]
- Rendering av en gitt type kan erstattes av tilpasset view via navnekonvensjon: /Views/Shared/DisplayTemplates/MyType.cshtml
- Contentlink er referanse til side, kan brukes til å finne url: Url.ContentUrl(Model.ContentLink)
- Lenker til sider rendres/vises med @Html.ContentLink(page), og ikke ActionLink
