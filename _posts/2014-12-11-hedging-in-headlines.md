---
layout: post
title: 'Hedging in headlines'
date: 2014-12-11 10:35:00
categories: Text
---

At [Emergent](http://www.emergent.info), we shine a light on news agencies. We're figuring out who's right, who's wrong, and who's ... well ... not saying much of anything.

This text analysis post uses [OpenRefine](http://openrefine.org/) and [Overview](https://www.overviewproject.org) to analyze ambiguous headlines. And yay, it uses a new and exciting data corpus.

## The problem

Let's pick apart an example headline from September 10: "[Reports of missing Libyan planes raise 9/11 terror fears](http://www.usatoday.com/story/news/world/2014/09/04/libya-missing-planes-sept-11/15059169/)"

There are zero facts in this headline. There isn't even a *hint* of a genuine who, what, when, where or why.

I won't wander into journalistic ethics. The news agency knows that if it didn't publish this non-story, somebody else would. News is a dirty business; I don't blame all the Chicken Littles out there for convincing people to click ads.

I *do* blame that magical word, "reports".

## The question

At Emergent, we noticed some similarities among headlines. When one of these "hedging words" appears, a headline tends to avoid claiming a fact:

```
allege
alleged
allegedly
alleges
claim
claimed
claims
report
reported
reportedly
reports
rumor
rumored
rumors
rumour
romoured
rumours
said
say
says
```

Let's start with a simple question: how common are these hedging words in online news headlines?

## The data

Emergent tracks news websites.

When we see a questionable claim online, we immediately and continually search online for news articles that relate. We poll them every so often to check if the headline, byline or body text changed.

We rank each version of each article as either `for`, `against`, `observing` or `ignoring` the claim. And since the headline sometimes takes a different stance than the body, we rank the headline separately, too.

This is a bit subjective, but usually there's a clear ranking for every article. We often miss claims or articles completely, and that impacts the conclusions you can draw from our data.

Okay, here's the data I promised: [our complete list of article versions](http://downloads.emergent.info/emergent-article-versions-2014-12-11--01.csv).

Let's open it together.

## The process

### 1. Open OpenRefine

When journalists hear "CSV", they should think "OpenRefine". It's a spreadsheet program with many features journalists need.

[Download OpenRefine](https://github.com/OpenRefine/OpenRefine/releases/tag/2.6-beta.1), then:

* On Windows, unzip and double-click `refine.bat`.
* On Mac, open, drag the icon into the Applications folder and double-click it.
* On Linux, extract and execute `refine`.

Then browse to [http://127.0.0.1:3333](http://127.0.0.1:3333).

{% include image.html src="2014-12-11-hedging-in-headlines/openrefine.png" %}

### 2. Create a new project with Emergent data

Click "Create Project" in the left-hand pane, then choose "Web Addresses (URLs)" and enter this: `http://downloads.emergent.info/emergent-article-versions-2014-12-11--01.csv`

Click "Next"

Click "Create project"

### 3. Breathe

There's a lot going on here.

{% include image.html src="2014-12-11-hedging-in-headlines/openrefine-article-versions.png" %}

First off, OpenRefine says you're dealing with 4,307 rows of data, but it only lists 10. That keeps OpenRefine fast. Click `50` to show 50 rows at a time instead.

Next, notice the column headers. These came with the dataset, and they're part of how Emergent works:

* `slug`: uniquely identifies a claim
* `truthiness`: whether the claim is true, false, or unknown
* `url`: the URL of an article
* `versionNumber`: whether this is the first, second or third (or so on) distinct set of headline/byline/body text Emergent saw at the given URL
* `source`: name of the publication
* `headline`: article headline
* `headlineStance`: stance of the headline (one of `for`, `against`, `ignoring`, `observing`; blank if Emergent editors haven't classified the version yet)
* `stance`: same, for the body text

### 4. Zoom in on "observing" headline stance

Click on the little triangle next to the `headlineStance` column header. Choose "Facet" and then "Text Facet".

Now the facet shows up in the left-hand pane. Click on "observing".

{% include image.html src="2014-12-11-hedging-in-headlines/openrefine-headline-stance-facets.png" %}

We've learned that 1,673 headlines are ranked as `observing`, out of the total of 4,307.

### 5. Just one version per article, please

You can see that many articles (uniquely identified by `url`) have many versions. That happens when the headline, body or byline changed.

We don't want to even *think* about this complication for today's question. We want [one row per url](https://github.com/OpenRefine/OpenRefine/wiki/Recipes#removing-duplicate-rows-when-exact-values-are-found-in-a-column):

1. Click the down-arrow next to `url`, then "Sort..." and "OK".
2. Click the down-arrow next to `url`, then "Edit Cells" and "Blank down".
3. Click the down-arrow next to `url`, then "Facet", "Customized Facets" and "Facet by blank". Choose the `false` facet.

{% include image.html src="2014-12-11-hedging-in-headlines/openrefine-no-duplicate-urls.png" %}

We've learned there are 655 unique articles, and we have their headlines.

### 6. Export from OpenRefine

OpenRefine is great at cleaning and slicing data, but it isn't great at text analysis. Let's export it for [Overview](https://www.overviewproject.org).

Overview can import a CSV file, but it expects certain columns: "text", "title" and "url". We already have a "url" column, but we're missing the other two.

1. Click the down-arrow next to `headline`, then "Edit Column" and "Add column based on this column...". Enter `text` as the new column name, and click OK.
2. Do the same to add a `title` column which also just contains the headline.
3. Click "Export" and "Comma-separated value".

Success: you've just downloaded the headlines we want to analyze.

{% include image.html src="2014-12-11-hedging-in-headlines/openrefine-text-and-title-columns.png" %}

### 7. Import into Overview

Overview is made for text analysis. It can analyze headlines just fine. It's a website, so you don't need to install it.

1. Register for an account at [https://www.overviewproject.org](https://www.overviewproject.org)
2. Click "Import from a CSV file"
3. Choose the file you just downloaded from OpenRefine (it's just under 250kb)
4. Click "Upload"

{% include image.html src="2014-12-11-hedging-in-headlines/overview-importing.png" %}

{% include image.html src="2014-12-11-hedging-in-headlines/overview-document-set-list.png" %}

### 8. Breathe

Click your new document set in Overview.

{% include image.html src="2014-12-11-hedging-in-headlines/overview-document-set.png" %}

The default view is a "Tree" view. It organizes these headlines into topics.

That's interesting, but it says more about Emergent's choice of claims than it does about the language in the headlines.

### 9. Word cloud

Click "Add view" and "Word cloud". You'll get what you'd expect:

{% include image.html src="2014-12-11-hedging-in-headlines/overview-word-cloud.png" %}

Notice that there are plenty of words like "report", "reportedly", "says" and "reports".

The word cloud helps guide our research. It's how I came up with the list of hedging words at the top of this post.

### 10. Multi-search

Let's focus on hedging words.

Click "Add view" and "Multisearch".

{% include image.html src="2014-12-11-hedging-in-headlines/overview-multi-search.png" %}

You _could_ add one search at a time, but it's easier to add them all at once:

1. Click "Edit entire list as text"
2. Copy the list of words at the top of this blog post and paste it into the text box
3. Click "Use these searches"
4. Sort by document count

{% include image.html src="2014-12-11-hedging-in-headlines/overview-multi-search-hedging-words.png" %}

So now we know the top five hedging words:

1. "Report"
2. "Reportedly"
3. "Claims"
4. "Says"
5. "Reports"

### 11. Stem the terms

It's a bit annoying that "report" and "reports" are separate, since they're basically the same word. Let's combine their counts.

1. Click "Edit entire list as text"
2. Replace the entire text box with this content:
```
report,report*
say,say OR says OR said
claim,claim*
allege,allege*
rumor,rumor* OR rumour*
```
3. Click "Use these searches"

{% include image.html src="2014-12-11-hedging-in-headlines/overview-multi-search-stemmed-hedging-words.png" %}

This is called "stemming". Instead of searching for words, we're searching for *stems* of words. (Some search indexes stem automatically. Overview doesn't.)

### 12. ... Not

We've seen that 191 headlines include "report" and 190 include other hedging words.

Let's analyze the other headlines to see what we've missed.

1. Enter "No hedging words" and click "Create new Search"
2. Click "Edit" to edit the query. Enter this query: `NOT(report* OR say OR says OR said OR claim* OR allege* OR rumor* OR rumour*)`
3. Click "Save"
4. Click the document count to populate the list on the right with headlines that don't use our hedging words

{% include image.html src="2014-12-11-hedging-in-headlines/overview-multi-search-no-hedging-words.png" %}

From scanning the list, we can spot these patterns:

* Other hedging words, like "could", "may" or "believed"
* The word "suspect" (hedging to avoid libel)
* Quotation marks around key phrases
* Question marks
* Endings like ": [source]"

Given enough time, you should be able to learn more about all these patterns, too.

### Conclusion

All that research, and what have we learned?

* About 30 per cent of wishy-washy headlines include "report".
* Headlines achieve ambiguity by including hedging words or special punctuation.

When you read news online, be wary when you spot these patterns. They indicate that the article you're reading is missing key facts.
