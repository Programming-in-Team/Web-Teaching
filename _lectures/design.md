---
layout: default
permalink: /html/
linkname: HTML
ordering: 2
---

# HTML: the language of the Web <!-- omit in toc -->

**An automatically generated PDF of this transcript is available [here](../generatedPDFs/design.pdf).**

## Table of Contents <!-- omit in toc -->
- [Required & recommended readings and activities](#required--recommended-readings-and-activities)
- [Learning goals](#learning-goals)
- [Web sites vs. web applications vs. the web as a platform](#web-sites-vs-web-applications-vs-the-web-as-a-platform)
- [Electron](#electron)
- [Web design basics](#web-design-basics)
  - [Rule: Don't make me think](#rule-dont-make-me-think)
  - [Rule: Minimize noise and clutter](#rule-minimize-noise-and-clutter)
  - [Rule: If you cannot make it self-evident, make it self-explanatory](#rule-if-you-cannot-make-it-self-evident-make-it-self-explanatory)
  - [Aesthetics matter (of course!)](#aesthetics-matter-of-course)
  - [Expectations vs. reality: usability testing](#expectations-vs-reality-usability-testing)
  - [Site navigation: the *trunk test*](#site-navigation-the-trunk-test)
  - [Entry page checklist](#entry-page-checklist)
  - [Performance metrics](#performance-metrics)
- [HTML5](#html5)
  - [The move towards HTML5](#the-move-towards-html5)
  - [Who decides the HTML standard](#who-decides-the-html-standard)
- [Self-check](#self-check)

## Required & recommended readings and activities

- Required readings:
  //TODO: update the required readings for HTML5
  - :warning: [Introduction to HTML forms](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Your_first_HTML_form) (ignore the section on *Basic form styling*, we will cover CSS in a later lecture) and [Sending form data](https://developer.mozilla.org/en-US/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data) (up to and including the section on *Viewing HTTP requests*).
- Recommended activity:
  - :headphones: Listen to [this podcast with Tom Dale](http://talkingcode.com/podcast/episode-11-tom-dale/) discussing modern web applications and the move from web sites to web apps and an almost *native* experience.
- Recommended readings:
  - :closed_book: Chapter 3 of the [Definite Guide to HTML5](https://www.apress.com/gp/book/9781430239604).
  - [Web Fundamentals by Google](https://developers.google.com/web/fundamentals/).
  - [Examples of what modern web technologies can achieve in Chrome](https://experiments.withgoogle.com/) (if you use another browser, not all examples may work as expected).
  - [Atomic Design by Brad Frost](http://atomicdesign.bradfrost.com/) 
- Relevant scientific publications:
  - Fernandez, A., Insfran, E. and Abrahão, S., 2011. [Usability evaluation methods for the web: A systematic mapping study](https://www.sciencedirect.com/science/article/pii/S0950584911000607). Information and Software Technology, 53(8), pp. 789-817.
  - Sonderegger, A. and Sauer, J., 2010. [The influence of design aesthetics in usability testing: Effects on user performance and perceived usability](https://www.sciencedirect.com/science/article/pii/S0003687009001148). Applied Ergonomics, 41(3), pp. 403-410.
  - Mathur, A., Acar, G., Friedman, M., Lucherini, E., Mayer, J., Ghetty, M. and Narayanan, A., 2019. [Dark Patterns at Scale: Findings from a Crawl of 11KShopping Websites](https://webtransparency.cs.princeton.edu/dark-patterns/), ACM Human-Computer Interaction, Vol. 3, CSCW, Article 81.


## Learning goals

- Apply web design principles during the design stage of a web app.
- Explain the ideas behind usability testing and employ it.
- Employ HTML to create web pages.

<sup>Note that this lecture does not cover **how to write HTML**, this is covered in the required reading for this lecture. At the end of this lecture we provide historic context to understand/appreciate the current state of HTML.</sup>

## Web sites vs. web applications vs. the web as a platform

The W3C has many working groups, among them (until 2016) the *Web Applications (WebApps) Working Group* whose [goal](https://www.w3.org/2014/06/webapps-charter.html) was as follows:

```
As web browsers and the web engine components that power them become ubiquitous across a range
of operating systems and devices, developers are increasingly using web technologies to build
applications and are relying on web engines as application runtime environments. Examples of
applications now commonly built using web technologies include reservation systems, online
shopping / auction sites, games, multimedia applications, maps, enterprise-specific applications,
interactive design applications, and PIM (email, calendar, etc) systems.
```

In 2017 this group was superseded by the [*Web Platform Working Group*](https://www.w3.org/2017/08/webplatform-charter.html) which has a similar goal (but not written up as nicely). The working group is responsible for a number of web technologies that move us closer towards the vision of the *browser as the operating system*, including:

- client-side database and offline applications;
- file and filesystem APIs;
- WebSockets;
- Web Workers (enables web applications to spawn background processes);
- DOM & HTML;
- Canvas (for drawing);
- Web components (a component model for the web).

## Electron

Do web technologies also help you to create desktop apps? Indeed, they do! [Electron](https://electronjs.org/) is an open-source project that enables you to build **cross-platform** desktop apps (for Windows, Mac and Linux) with HTML, JavaScript and CSS - the very technologies you learn about in this course.

Electron itself uses [Node.js](https://nodejs.org/en/), the server-side JavaScript runtime we cover in a later lecture together with [Chromium](https://www.chromium.org/), an open-source web runtime (i.e. browser).

The major benefit of Electron should be clear: instead of writing three separate desktop variants (one for each operating system) you only have to write and maintain one. New features are integrated in one application instead of three, which reduces feature delivery time. For these reasons, many well-known applications today are built on Electron, including [Visual Studio Code](https://code.visualstudio.com/) (the IDE we recommend you to use), the [Slack](https://slack.com) app, [Atom](https://atom.io/) and [many, many more](https://electronjs.org/apps).

If you ever wanted to know how *Windows 95* looks like, there is an [Electron app for that as well](https://github.com/felixrieseberg/windows95). It looks like this:

![Windows 95](../img/design-windows95.png)

On the downside, such a cross-platform approach usually brings with it considerable overhead - each application for instance bundles Chromium, which means that even if your app is not doing anything else besides `Hello World` it will be at least 30MB large. More concretely, the unzipped Windows95 Electron app is more than 454 MB in size, while the original Windows 95 operating system required about [40 MB of disk space](https://www.technologytips.com/windows-system-requirements/).

## Web design basics

Web design is not trivial. However, a few basic rules go a long way. Most of these principles seem obvious, but in practice are often ignored. This lecture is based on the book [Don't Make Me Think, Revisited (2014)](http://www.sensible.com/dmmt.html) by Steve Krug. In the following sections we go over a number of Krug's rules with concrete examples.

### Rule: Don't make me think

The way a web site or web application (I tend to use the terms interchangeably here; the rules apply to both apps and sites) works should be self-evident; the user should not have to expend **cognitive effort** to understand what she can do.

Consider this example of [ebay.com](https://www.ebay.com/) :point_down:

![ebay web site](../img/design-ebay.png)

<sup>Screenshot taken on October 11, 2019</sup>

:point_up: Here, it is very clear for the user what she can do: search for products, browse through the available items via product categories and shop for discounted electronics.

Contrast ebay with the following older example from [koopplein](https://web.archive.org/web/20140207233217/http://koopplein.nl/) :point_down:

![koopplein web site](../img/design-koopplein.png)

<sup>Screenshot taken February 8, 2014</sup>

:point_up: It is not self-evident for the user how to act to achieve her goals and major questions are raised:

- How do I get to the offers?
- What if I want to look at offers from Delft **and** Rijswijk instead of one or the other?
- What is all this text about?

**Good to know**: In case you wonder how it is possible to go back in time and look at older versions of a web site (as today's [koopplein.nl presence](https://koopplein.nl/) looks considerably better), head over to the [Wayback Machine](https://web.archive.org/), maintained by the Internet Archive whose mission is to **archive** the web. The Wayback Machine of course does not archive the entire web (this would be impossible), but it does take regular snapshots of more than **390 billion web pages**, including those of koopplein - [take a look](https://web.archive.org/web/*/koopplein.nl).

When naming and formatting links, buttons, section headers, etc. adhere to **established standards** and **be clear instead of clever**. For instance, a company's web site that has a link to its current job offers should use as link text `Jobs` or `Vacancies` (clear to the user what this link is about) instead of `Interested?` or `Join us!` (less clear).

Similarly, there are established style standards of how to format a link: in the early years of the web, blue underlined text was synonymous with a link and thus we are now stuck with the saying *10 blue links* as a synonym for web search results.

Users should also **not get lost within a web site**. A site should provide users with information on where they are and on how they arrived at that point. Ebay for instance leaves so-called **breadcrumbs**:

![ebay web site](../img/design-ebay2.png)

Lastly, it should be easy for the user to **distinguish different parts of a site** such as advertisement vs. content. Here :point_down: is an example from Google that does a poor job in this respect. It is not obvious on first sight that the first search result is indeed a paid advertisement instead of a so-called "organic" search result:

![Google web site recent](../img/design-google.png)

A few years ago ([August 15, 2013](https://web.archive.org/web/20130815204510/https://www.google.com/search?q=toyota) to be precise) Google was a lot more forthcoming when it came to notifying its users about advertisements :point_down:

![Google web site 2013](../img/design-google2.png)

It should be mentioned though that Google is not the only offender here, take this example :point_down: from Twitter which is similarly poorly designed in terms of content distinction between organic tweets and promoted (i.e. paid) ones:

![Twitter web site](../img/design-twitter.png)

The Google/Twitter examples above are of course not accidental: user interface designers are not accidentally trying to disguise the fact that those are ads (also known as promoted/sponsored content). This is done on purpose: we know that users are less likely to focus their attention on ads if it is obvious that they are indeed ads. Disguising them means a higher likelihood of users engaging with them. A higher engagement level means a higher financial return for the company. This is what is known as a **dark pattern**, more specifically the pattern of disguised ads. More formally, *"dark patterns are user interface design choices that benefit an online service by coercing, steering, or deceiving users into making unintended and potentially harmful decisions."* and can be found in more than 10% of shopping websites (the more popular the shopping site, the more likely the appearance of dark patterns) according to [this recent research study](https://webtransparency.cs.princeton.edu/dark-patterns/) from Princeton University. Dark patterns exist in different domains, not just social web platforms or search engines: in the past, researchers have looked at dark patterns [in the design of games](http://fdg2013.org/program/papers/paper06_zagal_etal.pdf), [privacy](https://www.degruyter.com/downloadpdf/j/popets.2016.2016.issue-4/popets-2016-0038/popets-2016-0038.pdf) and [home robots](https://ieeexplore.ieee.org/abstract/document/8673274).


### Rule: Minimize noise and clutter

The rule does not have to be explained, here are two examples that should make things clear :point_down:

ZDNet crawled on [March 31, 2001](https://web.archive.org/web/20010331202808/http://www4.zdnet.com:80/):

![ZDNet web site](../img/design-zdnet-2001.png)

vs. ZDNet crawled on [August 31, 2014](https://web.archive.org/web/20140831235129/http://www.zdnet.com/) :point_down:

![ZDNet web site](../img/design-zdnet-2014.png)

While it is not hard to go back to very old web designs and find faults in them, it should also be mentioned that in those times, every single HTTP request/response pair was time-consuming (the Internet was slow) and expensive. It made sense to push as much content as possible into a single page which could then be sent to the client in a single HTTP response.

### Rule: If you cannot make it self-evident, make it self-explanatory

Self-explanatory sites require users to expend a small amount of cognitive effort. When a site is not self-explanatory, a small amount of explanatory text can go a long way. In today's mobile world where a lot of content is accessed [*mobile first*](https://mayvendev.com/blog/mobilefirst), it is also vital to keep the mobile user in mind as well who has to deal with a small screen, a touch-based interface and possibly many distractions while surfing the web.

A positive example of this rule is the following Surveymonkey ([December 1, 2013](https://web.archive.org/web/20131201000510/https://www.surveymonkey.com/)) splash screen :point_down:

![Surveymonkey](../img/design-surveymonkey.png)

Lastly, **avoid happy talk**, that is text without any content for the sake of adding some text (e.g. a welcome message).

### Aesthetics matter (of course!)

This may be obvious, but the look and feel of an application of course also influences both the users' *perceived* usability as well as the *actual* usability. An insightful piece of research conducted by [Sonderegger & Sauer](https://www.sciencedirect.com/science/article/pii/S0003687009001148) has measured this influence. They conducted an experiment with an objectively ugly and an objectively good looking mobile phone prototype: not only did the study participants rate the ugly phone as being less usable, they actually performed worse in a given task than the participants receiving the good looking device.

### Expectations vs. reality: usability testing

Our expectations of web users are often *not* grounded in reality. We may expect users to be **rational**, **attentive** and having a **clear goal** in mind.

Instead, the average web user:

- quickly scans a web page (not even reading it);
- decides within seconds whether or not a site is worth their attention (research on so-called *dwell time* has shown this [again](https://dl.acm.org/citation.cfm?id=1835513) and [again](https://dl.acm.org/citation.cfm?id=2835833));
- clicks on the first link they find;
- depends a lot on the browser's back button (and not all web applications can deal with it);
- does not read instructions.

A web application should be designed based on **user reality**. **Usability testing** is an important step to create a well-designed web application. The development cycle consists of *designing*-*testing*-*reviewing*:

![Web app development cycle](../img/design-cycle.png)

In a **usability test**, a user is given a **typical task**, such as:

- Create a user account.
- Retrieve a lost password.
- Change the current credit card information.
- Delete a user account.
- Find an article in the archive.
- Edit a posting made in a forum.
- Start a game with three players.

While the user is busy with the task, her actions towards completing the task are being recorded. These actions are then translated into performance metrics. Performance metrics depend on the task, it could be the **number of clicks** required to complete the task, the **time taken** or the **number of wrongly clicked elements**.

Usability testers should be a mix of target audience and average web users; 2-3 testers per iteration tend to be sufficient.

A typical usability setup has the following roles:

- The **participant** (our tester) sits in front of the device (laptop, mobile phone, tablet).
- The **facilitator** sits next to her and guides her through the test.
- The **observers** (developers of the app, managers, etc.) watch the usability test and discuss the tester's performance (and how to improve it) afterwards.

This [blog post](https://asinthecity.com/2013/04/09/recording-mobile-device-usability-testing-sessions-guerrilla-style/) provides a good practical overview of usability testing with mobile devices, and includes imagery of the [participant and facilitator setup](https://benmelbourne.files.wordpress.com/2013/04/pop-up-mobile-usability-testing-lab-2.jpg) and the [observer setup](https://benmelbourne.files.wordpress.com/2013/04/pop-up-mobile-usability-testing-lab-5.jpg).

The result of a usability test are a set of issues. Each of those issues should be assigned a **priority** (low, medium, high) and the next iteration of the development should focus on the high priority problems. No new issues should be added to the list until the most severe issues are fixed.

### Site navigation: the *trunk test*

In order to determine whether a web site's navigation scheme is useful, Krug developed the so-called *trunk test*. Given a web site, pick a random page in it, print it and give it to a user who has never seen the site.

As quickly as possible, the user should find:

- the name of the web site;
- the name of the page she currently views;
- major sections of the page;
- possible navigation options at this point; and
- *You are here* indicators.

If the user is not able to find those elements, the site navigation scheme is considered sub-optimal.

### Entry page checklist

The home page (or entry page) of a web application should answer a number of essential questions:

- What **is** this?
- What can I **do** here?
- Why **should** I be here?
- What do they **have** here?

Surprisingly many home pages are not able to answer these questions. Take for example this entry page:

![Coffeecompany](../img/design-coffeecompany.png)

<sup>Screenshot taken October 11, 2019</sup>

<details> 
  <summary>What is the core business of this company? Click to find out the answer! </summary>
   This is the homepage of a Dutch cafe chain: https://coffeecompany.nl/ - not something easily guessable from the entry page.
</details>

Another serial offender of the entry page checklist are university home pages as immortalized in [this xkcd comic](http://xkcd.com/773/).

### Performance metrics

Once web applications become complex, besides general usability we also need to consider various **performance metrics**. A basic question to ask is: *how long does it take for the entire application to load?* This is of course a rather crude metric and it is not always clear what exactly this means, due to polling, etc. We thus need more fine-grained metrics. The [metrics section at Google's web.dev resource](https://web.dev/metrics/) provides a good overview of what questions to ask in order to measure aspects of an application's performance that are *relevant* to the user. This includes metrics such as:

- the time until the first content is rendered;
- the time until the user can successfully interact with the application;
- the time between the user's first interaction with the application and the time the browser responds to it.

:bug: While the Firefox dev tools do not provide these metrics out-of-the-box, its [performance panel](https://developer.mozilla.org/en-US/docs/Tools/Performance) allows a developer to explore where the browser spends a lot of its time: executing JavaScript, computing the stye (which may have changed due to a resizing of the browser window or some JavaScript function), computing the layout or rendering the page. 

For example, when we record the performance of the [tudelft.nl](https://www.tudelft.nl/) homepage, we see the following in the beginning of the loading process :point_down: :

![performance pane](../img/design-performance.png)

In this [waterfall](https://developer.mozilla.org/en-US/docs/Tools/Performance/Waterfall) visualization we see that initially the browser spends most of its time recalculating the style (based on for instance additional images that were loaded); few paint events occur. In general, the following sequence of events is an often recurring one: JavaScript computation :arrow_right: recompute style :arrow_right: recompute layout :arrow_right: repaint.


## HTML5

HTML5 is a set of related technologies (core HTML5, CSS, JavaScript) that together enable **rich web content**:

- **Core HTML5**: mark up content;
- **CSS**: control the appearance of marked-up content;
- client-side **JavaScript**: manipulate the contents of HTML documents and respond to user interactions.

Modern web application development requires knowledge of all three technologies. In practice, it also requires a whole set of additional frameworks and tools to go from prototype code to production code, such as build tools, transpilers, code coverage tools and so on. This is the case even "just" for [frontend coding](https://medium.com/the-node-js-collection/modern-javascript-explained-for-dinosaurs-f695e9747b70). We will introduce a few of those tools throughout the practical assignments.

//TODO: web stack examples

Before HTML5 we had **XHTML** and HTML 4.01. XHTML is a reformulation of HTML 4 as an XML 1.0 application and stands for **Extensible HyperText Markup Language**. It looks as follows (taken straight from the [W3C XHTML recommendation](https://www.w3.org/TR/xhtml1/)):

```html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE html
     PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
  <head>
    <title>Virtual Library</title>
  </head>
  <body>
    <p>Moved to <a href="http://example.org/">example.org</a>.</p>
  </body>
</html>
```

XHTML was designed to make processing of web pages **easier for machines** by having a very strict set of rules. The problem though was that (X)HTML is written by developers, not machines and it turned out to be too much hassle to write valid XHTML. Moreover, browsers were and are able to render invalid XHTML pages properly (so why even try to write valid XHTML?) and thus XHTML was eventually abandoned in favor of HTML5, which is not only less strict but also added a host of new features to the language.

With this introduction of new features **browser compatibility** issues returned: some browser vendors are faster than others in implementing W3C standards (in addition to implementing their own non-standardized features). A good resource to check which browser versions support which HTML5 feature and to what extent is https://caniuse.com/.

As a concrete example, here is the browser support overview of HTML5 form features as provided by [caniuse](https://caniuse.com/#search=form%20features):

![HTML5 form support](../img/design-html5-form.png)

<sup>Screenshot taken on December 16, 2019.</sup>

We can thus easily find out which browser versions do (not) support these features or only partially support them. This enables application developers to make choices - depending on their target population (and the most popular browsers among the target population), certain HTML5 features should (not) be employed.

### The move towards HTML5

The initial list of HTML tags (1991/92) was **static**: `<title> <a> <isindex> <plaintext> <listing> <p> <h1> <address> <hp1> <dl> <dt> <ul>`. JavaScript was created within 10 days (*which explains many JavaScript quirks*) in May 1995 by [Brendan Eich](https://twitter.com/BrendanEich), a co-founder of Mozilla who today is behind the browser [Brave](https://brave.com/). At the time, Eich worked at Netscape, which offered Netscape Navigator, a dominant browser in those years. It was the beginning of client-side **dynamic** scripting for the browser.

**Plugins** were created to go beyond what at the time was possible with HTML. Probably the most famous plugin remains Adobe Flash, which was introduced in 1996. HTML5 is a drive to return rich content **directly** into the browser, without the need for plugins or addons.

//TODO: use as semantic HTML example the cse-theme setup
HTML5 introduced a number of **semantic HTML elements** including `<article> <footer> <header> <main> <aside> <section> <output>`. As a guideline, when creating an HTML document, it is always best to select the **most specific** element to represent your content (instead of only using `<div>`'s). Semantic elements provide **meaning** but do not force a particular presentation. Older HTML elements (pre-HTML5) often do force a particular presentation, e.g. `<b>` or `<i>`. At the same time, those heavily used HTML elements cannot be moved to an obsolete state - as this would inevitably break a large portion of the web. For the browser vendors, backwards compatibility is a necessity, not an option. It should be pointed out that **semantic HTML** is quite different from the grand vision of the [Semantic Web](https://www.w3.org/standards/semanticweb/):

```
The Semantic Web is a Web of data — of dates and titles and part numbers and chemical
properties and any other data one might conceive of.
```

Let us lastly point to the unfortunately underutilized [`<kbd>` HTML tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/kbd) which represents keyboard input (e.g. <kbd>Shift</kbd>+<kbd>Ctrl</kbd>) and should be used by anyone writing tutorials/documentation in HTML.

### Who decides the HTML standard

HTML is widely used, which makes standardisation a slow process. Many different stakeholders are part of W3C's [Web Platform Working Group](https://www.w3.org/WebPlatform/WG/) (Microsoft, Google, Mozilla, Nokia, Baidu, Yandex, etc.). The standardization process of the W3C is elaborate, as a wide variety of stakeholders have to build consensus. Confusingly, a **W3C recommendation** is the highest level of standardization possible, before achieving it, a number of steps leading up to the recommendation are required:

1. **Working Draft**: *a document that W3C has published for review by the community, including W3C Members, the public, and other technical organizations.*
2. **Candidate Recommendation**: *a document that W3C believes has been widely reviewed and satisfies the Working Group's technical requirements. W3C publishes a Candidate Recommendation to gather implementation experience.*
3. **Proposed Recommendation**: *a mature technical report that, after wide review for technical soundness and implementability, W3C has sent to the W3C Advisory Committee for final endorsement.*
4. **W3C Recommendation**: *a specification or set of guidelines that, after extensive consensus-building, has received the endorsement of W3C Members and the Director. W3C recommends the wide deployment of its Recommendations. Note: W3C Recommendations are similar to the standards published by other organizations.*

<sup>Source: [W3C Recommendation Track Process](https://www.w3.org/2018/Process-20180201/#maturity-levels).</sup>

Informally, one could say that the W3C standardizes what the browser vendors have agreed upon and have chosen to implement or chosen to implement in the near future.

Consensus building takes time. HTML5 for instance was a candidate recommendation in Q4-2012 and became a W3C recommendation in Q4-2014. HTML5.1 was a candidate recommendation in Q1-2015 and became a recommendation in Q4-2016.

As of August 2018, [HTML5.3](https://www.w3.org/TR/html53/) has a *Working Draft* status; if you look at the standard text you will find it to be very elaborate (this is a 1,000+ pages document!) and precise, sufficiently so that any browser vendor can take the text and implement the features described in it without ambiguity.

In rare cases, features added to a web standard can also be removed again, the [AppCache](https://developer.mozilla.org/en-US/docs/Web/HTML/Using_the_application_cache) is a prime example of this: it was developed as technology to enable offline web applications in a simple manner (by adding a manifest file to a site containing no more than a few lines of text), but turned out to have so many [pitfalls](https://alistapart.com/article/application-cache-is-a-douchebag) that it was eventually abandoned in favor of another set of technologies ([Service Workers](https://developers.google.com/web/fundamentals/primers/service-workers/)). However, it is [still supported by all major browsers](https://caniuse.com/#search=appcache):

![AppCache](../img/design-appcache.png)

<sup>Screenshot taken on October 11, 2019.</sup>

The W3C writes the following about the AppCache: *This feature is in the process of being removed from the web platform. (**This is a long process that takes many years.**) Using the application cache feature at this time is highly discouraged. Use service workers instead.* 


## Self-check

Here are a few questions you should be able to answer after having followed the lecture and having worked through the required readings:

<details> 
  <summary>True or False? The `action` attribute of an HTML form defines where the data gets sent. Its value must be a valid relative or absolute URL.</summary>
  True.
</details>

<details> 
  <summary>True or False? If an HTML form is sent using the GET method, the data is appended to the body of the HTTP request.</summary>
  False.
</details>

<details> 
  <summary>True or False? The `action` attribute of an HTML form defines how the form is accessed on the client: either as a HTTP POST request or an HTTP GET request.</summary>
  False.
</details>

<details>
<summary>In a usability test, what is the task of the facilitator?</summary>
...
</details>

<details>
<summary>What does it mean for the W3C to produce a recommendation document?</summary>
</details>

//TODO: add more