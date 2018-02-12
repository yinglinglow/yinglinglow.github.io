---
layout: post
title: "CSS selectors for scraping"
date: 2018-02-08
---

the pain of scraping... useful css selectors help!!!!

amazing:
https://data-lessons.github.io/library-webscraping/02-csssel/

# select heading with h1
response.css('h1#firstHeading::text').extract_first()

# select table, th text
response.css('table.infobox.vcard>tr>th::text').extract()

# select nth tr
response.css('table.infobox.vcard>tr:nth-child(' + str(cause + 2) +')>td>a::text').extract()