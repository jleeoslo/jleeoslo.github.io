---
layout: single
title: "[Web-crawling] Collecting NRK online news articles using BeautifulSoup"
description: "[Web-clawing] Collecting NRK online news articles using BeautifulSoup"
headline: "[Web-clawing] Collecting NRK online news articles using BeautifulSoup"
typora-copy-images-to: ../images/2021-05-12
---

## Crawling a news title from a NRK article

- The ULR of the article for this crawling practice is: https://www.nrk.no/norge/helseministeren-med-kraftig-advarsel-mot-sterkol-i-butikk-1.15494124

  

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

As I designated the correct path, 'h1.title', the title is well crawled.






## Crawling a news content from a NRK article 





