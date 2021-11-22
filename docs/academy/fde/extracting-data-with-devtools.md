---
title: Extracting data with DevTools - Part 1
description: Learn how to use browser DevTools, CSS selectors and JavaScript to extract data from a website.
menuWeight: 20.2
paths:
- academy/fde/extracting-data-with-devtools-part-1
---

# Extracting data with DevTools - Part 1
We know the basics of HTML, CSS, JavaScript and DevTools and we can finally try doing something more practical - extracting data from a website. Let's try extracting the 50 most popular websites in the world from the [Alexa Top Sites index](https://www.alexa.com/topsites). We will use CSS selectors, JavaScript and DevTools to do that.

## Getting structured data from HTML
When you open the [Alexa Top Sites index](https://www.alexa.com/topsites), you'll see that there's a table on the page with names of websites and various data about the websites' traffic. We will now learn how to extract all this information. Open DevTools and select any of the websites with the selector tool.

[ screenshot of selecting the first website ]

Now you know where to find the name of the first website in the page's HTML, but we want to find all the website's information. To do that, hover over the elements above the one you just found, until you find the element that contains all the data about the selected website.

[ screenshot of selecting the whole line ]

This element is the parent element of all the nested (child) elements and we can find it using JavaScript and extract the data. Notice the `class` attribute of the element. This is where CSS selectors become handy, because we can use them to select an element with a specific class.

> Websites change and the structure or the HTML or the CSS selectors can become outdated. We'll try our best to keep this tutorial updated, but if you find that what you see on the website does not match this guide exactly, don't worry. Everything will work exactly the same. You will only have to use whatever you see on your screen and not in the screenshots here.

## Selecting elements with JavaScript
We know how to find an element manually using the DevTools, but for automated scraping, we need to tell the computer how to find it as well. We can do that using JavaScript and CSS selectors.

The function to do that is called `document.querySelector('some-selector')` and it will find the first element in the page's HTML matching the provided CSS selector. For example `document.querySelector('div')` will find the first `<div>` element. And `document.querySelector('p.my-class')` will find the first `<p>` element with the class `my-class`.

> You can find available CSS selectors and their syntax on the [MDN CSS Selectors page](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors).

At the time of writing the HTML element that contained all the data we wanted had a `<div>` tag and a `tr site-listing` class. This actually means that there were two classes applied to the element. `tr` and `site-listing`. The `site-listing` class looked like the more reliable pick for targeting the element so we chose that one. We ran this command in the DevTools Console to select the element we wanted.

```js
document.querySelector('div.site-listing')
```

[ screenshot of running the command that outputs the div ]

> There are always multiple ways how to select an element using CSS selectors. We always try to choose the one that seems the most reliable, precise and unlikely to change with website updates. For example the `site-listing` class looks like the website's programmers deliberately put it there to signify one result and we therefore think it's the best to use.

## Extracting data from elements
Now that we found the element, we can start poking into it to extract data. First, let's save the element to a variable so that we can work with it repeatedly and then print its text content to the console.

```js
const item = document.querySelector('div.site-listing')
console.log(item.textContent)
```

[ screenshot of printed data ]

As you can see, we were able to extract the data, but the format is still not very useful. For further processing, for example in a spreadsheet, we would like to have each piece of data as a separate field (column). To do that, we will look at the HTML structure in more detail.

### Selecting child elements
In the [Getting structured data from HTML](#getting-structured-data-from-html) section, we were hovering over the elements in the Elements tab to find the element that contains all the data. We can use that to find the individual data points as well. After a bit of inspection we find that all the elements with the data we need are `div` with the class `td`.

> Don't forget that the selectors may have changed from `div` and `td`, but the general principle of finding them will always be the same. Use what you see on your screen.

[ screenshot of hovering over some child element ]

The `document.querySelector()` function looks for a specific element in the whole HTML `document`, so if we called it with `div.td`, it would find the first `<div class="td ...">` in the `document`. Luckily we can also use this function to look for elements within an element.

There's even a similar function called [`querySelectorAll()`](https://javascript.info/searching-elements-dom#querySelectorAll) that returns an array of all the elements matching the selector, not only the first one. We will use that function to get all the fields with data.

> Learn more about `Arrays` [in this tutorial](https://javascript.info/array).

Don't forget that earlier we saved the parent element into a variable called `item`. We can use that variable to search only within that element.

```js
const fields = item.querySelectorAll('div.td')
console.log(fields)
```

[ screenshot of printed data ]

### Extracting data

The `fields` array now contains all the elements we need and we can access the data individually. Let's save the results into an [object](https://javascript.info/object). Those of you who know JavaScript will know that this is not the prettiest code ever written, but it is beginner friendly and that's important here. We will also use the `.trim()` function to remove unnecessary whitespace from the results.

```js
const result = {
    rank: fields[0].textContent.trim(),
    site: fields[1].textContent.trim(),
    dailyTimeOnSite: fields[2].textContent.trim(),
    dailyPageViews: fields[3].textContent.trim(),
    percentFromSearch: fields[4].textContent.trim(),
    totalLinkingSites: fields[5].textContent.trim(),
}
console.log(result)
```

[ screenshot of extracted data ]

If you were able to get the same result in your browser console, congratulations! You successfully extracted your first data. If not, don't worry and review your code carefully. You'll crush the bug soon enough.

## Next up
We have learned how to extract information of a single website from the Alexa Top Sites index. In the next chapter we will learn how to extract all of it.