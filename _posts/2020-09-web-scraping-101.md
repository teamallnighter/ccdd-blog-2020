---
title: "Web Scraping 101"
categories:
  - Web Scraping
comments: true
tags:
  - Node.js
  - Web Scraping
last_modified_at: 2020-09-12T08:25:52-05:00
excerpt: "Web Scraping is the process of pulling data from a website"
author: "Chris Connelly"
image:
  path: /assets/images/carbon.png
---

## What is Web Scraping?

> Web scraping, web harvesting, or web data extraction is data scraping used for extracting data from websites. Web scraping software may access the World Wide Web directly using the Hypertext Transfer Protocol, or through a web browser

### Is Web Scraping Legal?

Starting with the biggest BS around web scraping. Is web scraping legal? Yes, unless you use it unethically. Web scraping is just like any tool in the world. You can use it for good stuff and you can use it for bad stuff. Web scraping itself is not illegal. As a matter of fact, web scraping – or web crawling, were historically associated with well-known search engines like Google or Bing. These search engines crawl sites and index the web. Because these search engines built trust and brought back traffic and visibility to the sites they crawled, their bots created a favorable view towards web scraping. It is all about how you web scrape and what you do with the data you acquire.

A great example when web scraping can be illegal is when you try to scrape nonpublic data. Nonpublic data can be something that is not reachable for everyone on the web. Maybe you have to login to see the data. In this case web scraping is probably unethical, depending on the context. Also it does matter how nice you are technically when scraping a website. To learn more, I urge you to check out the most frequent legal issues associated with web scraping!

Check out [this case](https://digitalcommons.law.scu.edu/cgi/viewcontent.cgi?referer=https://benbernardblog.com/&httpsredir=1&article=2261&context=historical)where linked in took people who where scraping data from the site to court. 

## Tools for Web Scraping

### [Node.JS](https://nodejs.org/en/)

> [Node.js](https://nodejs.org/en/) is an open-source, cross-platform, JavaScript runtime environment that executes JavaScript code outside a web browser.

### [Cheerio.js](https://github.com/cheeriojs/cheerio)

> Fast, flexible & lean implementation of core jQuery designed specifically for the server.

## [Puppeteer](https://github.com/puppeteer/puppeteer)

> Puppeteer is a Node library which provides a high-level API to control Chrome or Chromium over the DevTools Protocol. Puppeteer runs headless by default, but can be configured to run full (non-headless) Chrome or Chromium.

## [Nightmare.js](https://github.com/segmentio/nightmare)

[Nightmare](https://github.com/segmentio/nightmare)is a high-level browser automation library from Segment.


## [IMDB Scraper](https://portfolio.chrisconnelly.dev/imdb-scraper-2)

My IMDB scraper

```javascript
const request = require('request-promise');
const cheerio = require('cheerio');
const fs = require('fs');
const Json2csvParser = require('json2csv').Parser;

const URLS = [
    'https://www.imdb.com/title/tt0102926/?ref_=nv_sr_1', 
    'https://www.imdb.com/title/tt2267998/?ref_=nv_sr_1'
];

(async () => {
    let moviesData = [];

    for(let movie of URLS) {
        const response = await request({
                uri: movie,
                headers: {
                    'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
                    'Accept-Encoding': 'gzip, deflate, br',
                    'Accept-Language': 'en-US,en;q=0.9,fr;q=0.8,ro;q=0.7,ru;q=0.6,la;q=0.5,pt;q=0.4,de;q=0.3',
                    'Cache-Control': 'max-age=0',
                    'Connection': 'keep-alive',
                    'Host': 'www.imdb.com',
                    'Upgrade-Insecure-Requests': '1',
                    'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36'
                },
                gzip: true
            }
        );

        let $ = cheerio.load(response);
           
        let title = $('div[class="title_wrapper"] > h1').text().trim();
        let rating = $('div[class="ratingValue"] > strong > span').text();
        let poster = $('div[class="poster"] > a > img').attr('src');
        let totalRatings = $('div[class="imdbRating"] > a').text();
        let releaseDate = $('a[title="See more release dates"]').text().trim();
        let popularity = $('#title-overview-widget > div.plot_summary_wrapper > div.titleReviewBar > div:nth-child(5) > div.titleReviewBarSubItem > div:nth-child(2) > span').text().trim();
        let genres = [];

        $('div[class="title_wrapper"] a[href^="/genre/"]').each((i, elm) => {
            let genre = $(elm).text();
            
            genres.push(genre);
        });
        
        moviesData.push({
            title,
            rating,
            poster,
            totalRatings,
            releaseDate,
            genres
        });
        
    }


    const json2csvParser = new Json2csvParser();
    const csv = json2csvParser.parse(moviesData);

    fs.writeFileSync('./data.csv', csv, 'utf-8');
    console.log(csv);

})()
```

The outputis a .csv file 

```csv
"title","rating","poster","totalRatings","releaseDate","genres"
"The Silence of the Lambs (1991)","8.6","https://m.media-amazon.com/images/M/MV5BNjNhZTk0ZmEtNjJhMi00YzFlLWE1MmEtYzM1M2ZmMGMwMTU4XkEyXkFqcGdeQXVyNjU0OTQ0OTY@._V1_UX182_CR0,0,182,268_AL_.jpg","1,081,528","14 February 1991 (USA)","[]"
"Gone Girl (2014)","8.1","https://m.media-amazon.com/images/M/MV5BYjgwY2E1N2QtNDJkMi00YzE4LThiYTItYWI5YmE4NWMzMGFhXkEyXkFqcGdeQXVyMjU3OTA4NzQ@._V1_UX182_CR0,0,182,268_AL_.jpg","727,309","3 October 2014 (USA)","[]"
```




