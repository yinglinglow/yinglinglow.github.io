---
layout: post
title: "Scraping with Selenium"
date: 2018-04-15
---

for the longest time my selenium kept having a pop-up which i had to click, then everything ran smoothly.

the below removes the pop-up (if you don't have the option of going headless - e.g. while downloading files:

```python
chrome_options = webdriver.ChromeOptions()
chrome_options.add_experimental_option("useAutomationExtension", False)
```