
# select heading with h1
response.css('h1#firstHeading::text').extract_first()

# select table, th text
response.css('table.infobox.vcard>tr>th::text').extract()

# select nth tr
response.css('table.infobox.vcard>tr:nth-child(' + str(cause + 2) +')>td>a::text').extract()