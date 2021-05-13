---
layout: single
title: "[Web-crawling] Collecting NRK online news articles using BeautifulSoup"
description: "[Web-clawing] Collecting NRK online news articles using BeautifulSoup"
headline: "[Web-clawing] Collecting NRK online news articles using BeautifulSoup"
typora-copy-images-to: ../images/2021-05-12
---

## Crawling a news title from a NRK article

- The ULR of the article for this crawling practice is: <https://www.nrk.no/norge/helseministeren-med-kraftig-advarsel-mot-sterkol-i-butikk-1.15494124>

  

- As it is seen below, the news title belongs to h1.title class. 

  <center><img src ='/images/2021-05-12/1.png'></center>
- Now, I'm ready for crawling the news title with requests and BeautifulSoup:




```python
import requests
from bs4 import BeautifulSoup
```

First, using requests we can access to the  url. Afterwards, using BeautifulSoup's selected_one function, the title can be crawled. I should remember where the title is located in the web page.


```python
url = 'https://www.nrk.no/norge/helseministeren-med-kraftig-advarsel-mot-sterkol-i-butikk-1.15494124'
resp = requests.get(url)
soup = BeautifulSoup(resp.text)
title_tag = soup.select_one('h1.title')
title_tag.get_text()
```




    'Helseministeren advarer Høyres landsmøte mot å åpne for sterkøl i butikk'

As I put the correct tag, 'h1.title', the title is well crawled.



- Now, I can make a function which returns news titles when news URLs are given.


```python
def get_nrk_news_title(news_url):
    url = news_url
    resp = requests.get(url)
    
    soup = BeautifulSoup(resp.text)
    title_tag = soup.select_one('h1.title')

    return title_tag.get_text()
```



I tested with two articles and It works well!


```python
get_nrk_news_title('https://www.nrk.no/innlandet/familie-har-romt-huset-i-et-halvt-ar-pa-grunn-av-helsefarlig-gass-1.15487079')
```




    'Familie har rømt huset i et halvt år på grunn av helsefarlig gass'




```python
get_nrk_news_title('https://www.nrk.no/trondelag/mann-skadet-under-kommunal-omsorg-pa-froya_-politiet-har-avhort-to-personer-1.15491076')
```




    'To personer har vært i politiavhør etter at mann ble brannskadet i omsorgsbolig på Frøya'


<br>





## Crawling a news content from a NRK article 

- As it is seen below, the news content is written with <p> tags under the div class = g55. 

<center><img src ='/images/2021-05-12/2.png'></center>

- Then, using soup.select() function and  for loop, I can crawl each sentence and put them together.


  ```python
  content = ''
  for p in soup.select('div.g55 p'):
      content += p.get_text()
  content
  ```

  


  ```
  '«Jeg vil sterkt fraråde å tillate salg av drikke med alkoholinnhold opp til 8 prosent i dagligvarebutikker», skriver Høie.Advarselen kommer i et svarbrev til partiets helsepolitiske talsperson Sveinung Stensland. Som NRK har omtalt tidligere, har han stilt spørsmål om hvordan det å selge drikkevarer med alkoholinnhold på opptil 8 prosent i butikken vil påvirke grunnlaget for Vinmonopolet. Svaret fra Høie er krystallklart:«Det vil svekke vinmonopolordningen, føre til økt tilgjengelighet til alkoholholdig drikk og dermed økt fare for skader som følge av alkoholbruk samt kunne ha konsekvenser for andre alkoholpolitiske virkemidler.»Bryggeri- og drikkevareforeningen er heller ikke blant dem som applauderer forslaget om å selge sterkøl i butikkene.– Vi er skeptiske til å åpne for sterkere alkohol i dagligvare, sier foreningens direktør Erlend Fuglum.Erlend Fuglum, direktør i Bryggeri- og drikkevareforeningenDe mener det kan svekke Vinmonopolet som alkoholpolitisk instrument.– I tillegg er jo polet viktigere og langt mer tilgjengelig for de mange små og mellomstore enn dagligvarebutikkene er, sier han.– Når det gjelder sterkøl, som er det vi driver med, mener vi det viktigste som kunne vært gjort for bryggeriene er at de kunne få selge sitt egenproduserte sterkøl direkte fra bryggeriet, legger han til. Stensland er fornøyd med svaret, som han mener er enda klarere enn han hadde håpet på.– Dette er krystallklar tale fra helseministeren, ikke noe snikksnakk, sier han. \nFORNØYD MED SVARET: Stortingsrepresentant Sveinung Stensland (H)Stensland mener forslaget er uklokt og må stanses.– Jeg regner med landsmøtet tar med seg både den faglige og politiske begrunnelsen for at vi ikke må uthule Vinmonopolets vareutvalg, sier han.– Her har avholdsfolk og vinsnobber god grunn til å forene krefter, legger han til. Høyre skal i løpet av helgen snekre nytt partiprogram. Et av forslagene landsmøtet skal ta stilling til er om det skal åpnes for sterkere drikkevarer i dagligvarebutikkene. Det er dissens i programkomiteen, men flertallet står bak forslaget.En av dem som støtter forslaget er stortingsrepresentant Heidi Nordby Lunde (H).– Det er for at norske forbrukere, butikker og produsenter skal kunne ha muligheten til å kjøpe, selge og omsette godt norskprodusert øl i større omfang enn det som er tilgjengelig i dag, sa hun til NRK tidligere denne uken.Mye av håndverksølet har en høyere alkoholprosent enn det dagligvarebutikkene har lov til å selge. Dermed må øltørste nordmenn ta turen til Vinmonopolet.STÅR BAK FORSLAGET: Stortingsrepresentant Heidi Nordby Lunde (H)Lunde reagerer kraftig på svaret fra Bent Høie: I en e-post til NRK sier hun:– Det henger jo ikke på greip at Høie i et folkehelseperspektiv mener det er lurere at folk som bare skal ha øl skal tvinges på polet, der det samtidig er sprit og vin.At Vinmonopolet vil gå konkurs fordi de ikke kan selge øl, eller at Norge mister all råderett over alkoholpolitiske virkemidler på grunn av EØS-avtalen, kaller Lunde skremselspropaganda.– Det virker som om de er redde for å gå på en smell på landsmøtet, og med denne taktikken øker de sannsynligheten for det.Lunde skriver at da Høyre var på forbrukernes side og utvidet åpningstidene i butikkene på åttitallet, var kjøpmennene imot.– Derfor er jeg ikke så overrasket over at etablerte aktører er mot mer konkurranse, da EØS-avtalen først og fremst vil føre til at også utenlandsk øl vil konkurrere om hylleplass.– De store, norske, etablerte produsentene har dessuten tilpasset seg dagens reguleringer, og vannet ut sitt eget øl for å få det ned til 4,7 prosent og inn i butikkhyllene. Det er jo bare trist for norske øltradisjoner, avslutter Heidi Lunde.'
  ```

  As I put the correct location, the while content is well crawled.

  

- Now I can make a function which returns news contents when news URLs are given.


```python
def get_nrk_news_content(news_url):
    url = news_url
    resp = requests.get(url)
    
    soup = BeautifulSoup(resp.text)
    
    content = ''
    for p in soup.select('div.g55 p'):
        content += p.get_text()
    return content
```



Again, I tested with two articles and It works well!


```python
get_nrk_news_content('https://www.nrk.no/innlandet/familie-har-romt-huset-i-et-halvt-ar-pa-grunn-av-helsefarlig-gass-1.15487079')
```


    'Stein Løvik-Skaug forteller at familien på fire plutselig kjente en kjemisk lukt både inne i huset og utenfor en dag i november i fjor.– Det var som å skru på en bryter. Det kom en plutselig lukt som om noen hadde stått og lakkert en bil nede i kjelleren.Det fysiske ubehaget kom like fort som lukta.– Vi fikk vondt i hodet og tåkesyn.Via forsikringsselskapet engasjerte familien firmaene Humid AS og Polygon til å gjøre undersøkelser.Resultatet var sjokkerende.Det ble funnet spor av løsemidler og en rekke skadelige stoffer, som finnes i blant annet drivstoff, oljer og white-spirit.Verdiene var over 50 ganger så høye som normalt.«Stoffene som ble funnet kan gi løsemiddelskader. Det anbefales ikke å bo i huset før sanering er foretatt, står det i rapporten.»Familien måtte komme seg ut. De flyttet først til slektninger i nærheten. Stein Skaug brukte gassmaske den tiden han måtte være i huset for å pakke ned ting.Nå har forsikringsselskapet betalt leiebolig for dem i snart et halvt år.Forurensingen ser ut til å ha trengt inn i kjelleren via vannrør eller lignende. Men foreløpig har ingen funnet ut hvor de farlige stoffene kommer fra.Familien fra Furnes anmeldte det som miljøkriminalitet like etter at de måtte flytte ut.Politiet i Innlandet bekrefter at de etterforsker saken.– Det er fortsatt for tidlig å si om det foreligger en straffbar overtredelse av forurensningsloven, og vi har ingen mistenkte i saken, sier politiadvokat Einar Gauslaa Bergem.Politiet samarbeider med Ringsaker kommune, som er lokal forurensningsmyndighet, for å kartlegge kilden til forurensningen.Men det har de foreløpig ikke klart.– Det er en veldig vanskelig sak, som vi har klødd oss i hodet for å finne ut av. Det er litt mystisk at det skulle oppstå en sånn forurensning, sier teknisk sjef i Ringsaker kommune, Tor Simonsen.Teknisk sjef i Ringsaker kommune, Tor Simonsen, sier hele saken er litt mystisk.Han mener saken er svært alvorlig, når en familie må flytte ut av boligen sin på denne måten.Det har kommet tips om at noen har dumpet forurenset masse i nærheten, men det er ikke dokumentert. Teknisk sjef sier tipset blir fulgt opp, men at de avventer politiets etterforskning og samarbeider med dem.Han erkjenner at det har tatt for lang tid å finne ut hva som har skjedd.– Det er egentlig veldig, veldig rart at vi ikke har funnet årsaken ennå.Stein Løvik-Skaug og familien vet ikke når, eller om, de kan flytte tilbake til huset sitt. Stein Løvik-Skaug og familien synes det går for tregt.Nå, nesten et halvt år etterpå, vet de fortsatt ikke om huset kan bli beboelig igjen.– Nå er det mye rykter og mye synsing. Nå vil jeg vite.Han sier det er verst for barna, som savner hjemmet sitt. Men også han og kona synes det er tungt og slitsomt. Likevel er det ikke aktuelt å flytte tilbake.– Jeg vil ha rapporter som viser at huset er trygt å bo i. Da flytter vi hjem.'




```python
get_nrk_news_content('https://www.nrk.no/trondelag/mann-skadet-under-kommunal-omsorg-pa-froya_-politiet-har-avhort-to-personer-1.15491076')
```




    'En mann i 50-årene ble brannskadet under kommunal omsorg på Frøya i Trøndelag.I dagene som fulgte, begynte personell ved brannskadeavdelingen på Haukeland sykehus å mistenke at skadene skyldtes et bad.Mannen er fullstendig pleietrengende. Han bor i en leilighet i et kommunalt bofellesskap.Ifølge kommunen ble mannen badet – han er ikke i stand til å bade selv.Les hva vi vet om hendelsesforløpet lenger ned i artikkelen.To ansatte i Frøya kommune er midlertidig tatt ut av tjeneste. Disse to har formelt sett velferdspermisjon.Politiet har opprettet undersøkelsessak for å komme til bunns i hva som skjedde.I tillegg ble det tirsdag kjent at Statsforvalteren i Trøndelag har åpnet tilsynssak mot Frøya kommune.Onsdag var politiet i omsorgsboligen for å undersøke rommet hvor mannen skal ha fått skadene.Så langt er to personer avhørt i saken. Politiet vil ikke si hvem disse to er.– Per nå er ingen mistenkt eller siktet. Vi vet foreløpig ikke om det har skjedd noe straffbart. Derfor koder vi saken som en undersøkelsessak, sier lensmann i Orkdal tjenesteenhet, Tor Kristian Haugan.Ifølge ham etterforsker politiet hendelsen for fullt.– Vi har flere etterforskere på saken.INNLAGT HER: Mannen i 50-årene er fremdeles innlagt på Haukeland sykehus. Der får han behandling for brannskader han skal ha fått som følge av et bad.Statsforvalteren i Trøndelag jobber med å sette sammen et team som skal jobbe med tilsynssaken.– Både lege, jurist og personell med erfaring fra arbeid med psykisk utviklingshemmede vil jobbe med saken.Det sier Hilde Bøgseth, seksjonsleder hos Statsforvalteren i Trøndelag.Noe av det de vil gjøre, er å innhente alt av dokumentasjon som finnes om mannens helsesituasjon, og om systemet rundt ham.– Vi vil blant annet prøve å finne ut av hvilke rutiner det er for å kjenne om vannet er godt nok temperert, sier Bøgseth til NRK onsdag ettermiddag.Ifølge henne skal de også ta stilling til om det er aktuelt å samarbeide med politiet i saken.Mannen ble badet i den kommunale omsorgsboligen. \nMannen ble syk. Legevakt og ambulanse ble tilkalt.\nMannen ble sendt til St. Olav med det som ble regnet som «akutt sykdom».Mannen ble sendt fra St. Olav til Haukeland. Der er landets ypperste ekspertise på brannskader.Haukeland fattet mistanke om at skadene kom av ytre påkjenning. Nærmere bestemt et bad.\nFrøya kommune ble varslet om mistanken.\nFrøya kommune varslet Statens helsetilsyn og Statsforvalteren i Trøndelag.\nTo ansatte ble tatt ut av tjeneste.Haukelands mistanke om at brannskadene kom etter et bad ble styrket.Frøya kommune meldte saken til politiet. Politiet opprettet undersøkelsessak.Statsforvalteren har åpnet tilsynssak mot Frøya kommune.Mannen i 50-årene er fortsatt innlagt på Haukeland sykehus, hvor han får behandling for brannskadene.Verken sykehuset eller kommunen vil uttale seg om tilstanden til mannen.– Vi er glade for at saken undersøkes, og bistår så godt vi kan med opplysninger, sier Frøya-ordfører, Kristin Strømskag.'



