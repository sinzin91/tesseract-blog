---
title: "I built a semantic movie search app with GPT-4"
date: 2023-03-29T19:11:16-07:00
draft: false
# weight: 1
tags: ["gpt", "search", "project"]
author: "Tenzin Wangdhen"
showToc: true
TocOpen: false
hidemeta: false
comments: false
description: "Exploring the unreasonable effectiveness of building apps with GPT-4"
disableHLJS: false # to disable highlightjs
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: false
# cover:
#     image: "<image path/url>" # image path/url
#     alt: "<alt text>" # alt text
#     caption: "<text>" # display caption under cover
#     relative: false # when using page bundles set this to true
#     hidden: true # only hide on current single page
editPost:
    URL: "https://github.com/sinzin91/tesseract-blog/blob/master/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

TLDR:
I built a semantic movie search app with GPT-4. You can check it out here: https://moviesgpt.net/. And here's the Github repo: https://github.com/sinzin91/moviesGPT

![MoviesGPT Demo](/images/moviesgpt/moviesgpt-demo.gif)
_the hero keeps forgetting who he is_


I've been thinking a lot about potential products one can build around LLMs. One idea I mentioned jokingly to some friends was to use GPT as a natural language interface to a movies API.

In theory, I thought, you could ask GPT about movies semantically, meaning you search based on what the movie is about or its content, instead of just raw keyword search. For example, I could search for "movies where all the main characters die" and get back "The Departed". Then I could use the response to query a movies API like The Movies Database. Google is surprisingly bad at this, returning links to listicles instead of the movies themselves.

The current way to do this, and the one that even ChatGPT recommends, is gathering data on all the movies, generating [word embeddings](https://en.wikipedia.org/wiki/Word_embedding) on that data, storing it in a Vector database, and then querying using something like cosine similarity. But GPT-3, having been trained on all internet data, has "seen" every movie and discussion around every movie already. It already has the embeddings. And it already "knows" how to find those movies given the right prompt.

With this rough sketch of an idea, I sipped some coffee and entered this into GPT-4:
```text
You are a master professional front end React developer. 
Show how you would build a front end that includes a search bar at the top. 
Below the search bar are six cards which include the movie cover with the title below it. 
Searches hit The Movie Database api's search endpoint with the search term. 
The results from the API call are used to update the cards below the search bar. 
The site uses a black and blue, dark mode style theme, with minimal UI components.
```

I basically just wrote that off the cuff with no large amount of premeditation, and got back a very reasonable starting place for my app:
![MoviesGPT Create React App](/images/moviesgpt/gpt-create-moviesgpt.png)

> _Note_: I am a hobbyist front-end engineer at best and only know enough React and Javascript to be slightly dangerous. I absolutely _hate_ CSS, and can't remember the difference between `align-items` and `justify-content` to save my life. 

To my surprise, I was able to just copy paste the code from GPT-4 to get a decent looking site!

I then asked to change it so that it calls OpenAI first:

```text
Ok great! 
Now I want you to change it so that searches are sent to OpenAI's 
text-davinci-003 API. Hit The Movies Database API with each element 
from the returned array, and render those movies in the MovieGrid.
```

Interestingly, GPT generated a prompt from my prompt:

```javascript
const response = await openai.Completion.create(
  { 
    engine: "text-davinci-003", 
    prompt: `Find 6 movie titles related to "${searchTerm}"`
  })
```


This prompt is not precise enough though, so here I actually had to do some work:
```javascript
Return an array of movie titles that best match this search term, 
ordered from most to least relevant. Generate up to 16 titles.
If you are unable to answer the question, start your response with Sorry.

Example:
prompt: "movies with brando"

response: ["The Godfather", "The Godfather: Part II", "Apocalypse Now", "The Wild One"]

prompt: ${searchTerm}
response:
```

I'm giving GPT an example of the kind of response I expect. In this case, I want it to create a valid array of movie titles.

With this change, I had the first working POC of my original idea! The last time experienced such a sensation of god-like empowerment might have been unlocking robots in [Factorio](https://www.factorio.com/).

![Factorio Robots](/images/moviesgpt/factorio-robots.png)
_in Factorio, eventually you can have robots automatically build things based on blueprints_

Whew, ok great, now what? 

I showed the app to a friend, and he mentioned that it looked like crap on mobile. So I asked GPT-4: `change the css so the website works well on mobile phones`. Lo and behold, after copying over the CSS directly, the site worked flawlessly on mobile. No futzing with media queries for me!


I realized I also wanted to add some star ratings to these movies, to get a sense of their popularity. TMDb API returns `vote_ratings`, which is a number from 0 to 10. I found the distribution of `vote_ratings` here https://www.kaggle.com/code/erikbruin/movie-recommendation-systems-for-tmdb. 

Adding a stars component was also mostly copy/paste once I wrote the prompt:
```text
The movie database api returns a `vote_average` in the response. 
I want a "Stars" component below each movie title that gives gives 
the movie 1 star if `vote_average` is between 0 to 2, 2 stars if it 
is between 2 to 4, etc. If a movie got one star, there should be a 
single bright yellow star and four grey stars in the component.
```

I also wanted something to happen when I clicked on the cards. Maybe show the movie summary, release date, etc. Here's the prompt:
![Overlay](/images/moviesgpt/moviesgpt-overlay.png)

One thing that was bothering me in terms of practical usability was the lack of Rotten Tomatoes ratings. Often, that's the first thing I check when deciding whether to watch a given movie. Unfortunately, Rotten Tomatoes charges $60k/year (!!) for access to their API. Then I had the idea of asking GPT for the Rotten Tomatoes ratings. To my surprise, it does seem to have a sense for the ratings. Though they aren't fully accurate, they seem to be generally in the right ballpark.

I updated my prompt to generate a new data structure that includes the Rotten Tomatoes ratings:
```
Return a JSON object movie titles that best match this search term and their Rotten Tomatoes tomatometer score, 
ordered from most to least relevant. 
Get the most up to date and accurate Rotten Tomatoes tomatometer score.
Generate up to 12 titles.
If you are unable to answer the question, return a string that starts with Sorry.
The response must be a valid JSON.

Example:
prompt: "movies with brando"

response: [
  { "title": "The Godfather", "rottenTomatoesScore": 98 },
  { "title": "The Godfather: Part II", "rottenTomatoesScore": 97 },
  { "title": "Apocalypse Now", "rottenTomatoesScore": 96 },
  { "title": "The Wild One", "rottenTomatoesScore": 85 }
]

prompt: ${searchTerm}
response:
```

I also had GPT make me the new Rotten Tomatoes rating component:

![Rotten Tomatoes Prompt](/images/moviesgpt/moviesgpt-rottentomatoes-prompt.png)


Now I can safely avoid these movies!

![Rotten Tomatoes Rating](/images/moviesgpt/moviesgpt-rottentomatoes.png)

I was surprised by how well semantic search works out of the box with GPT. There are some limitations though. One is that it only knows about movies up to the date it was trained, which is late 2021. When asked about movies after that, it simply responds that it doesn't know. This issue may get addressed by the Plugins that OpenAI announced, which would allow GPT to call live APIs to fetch realtime data.

![Can't find movies from 2022](/images/moviesgpt/moviesgpt-2022-movies.png)
_can't find movies from 2022_

I tried using `gpt-3.5-turbo`, but that model seemed to be much more prudish, refusing to respond to searches on movies involving murder, or other subjects deemed unseemly by the moderators. So I switched back to `text-davinci-003`. 

To be clear, I still had to do some coding to get the functionality and styling to match exactly what I wanted. I don't think someone that has no experience with React or Javascript would be able to replicate this. GPT still gets some stuff wrong, like telling me to use outdated libraries or generating wonky styles (_who wants a blue background on a header??_). But it's getting pretty damn close. I got the feeling that I was a tech lead pair programming with a very knowledgeable junior engineer. It doesn't do well with the big picture decisions, but knows how to implement very well given the right direction. To be fair, I'm not doing anything ground-breaking here. There must be tons of sites like this in the pretraining data, so it's relatively easy for GPT to generate the relevant code.

It was also _much_ easier to get into that elusive flow state while hacking this out. The dopamine hits from thinking of a feature and seeing it in action just kept coming, without the usual frustrations of not knowing how a particular function works and having to look it up. This makes coding _fun_ again. 

It's clear that I am the bottleneck in this process. I'm sitting here copying and pasting from GPT with some slight modifications. It seems like the natural progression here is to have GPT directly create the code based on my prompts. I imagine you could build a more advanced "Create React App" that generates and updates the app based on your prompts.

I hope this inspires people to pursue building those apps they always wanted to build, but couldn't find time for. GPT still does not know _what_ app to build and _why_ it should build it, which is where we come in.
