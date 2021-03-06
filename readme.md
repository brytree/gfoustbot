![Those darn grandchildren](./GFoustBot.jpg?raw=true "Graham Foust Bot in action")

# GFoustBot

A bot that riffs on the poem "And the ghosts they own everything" by [Graham Foust](http://twitter.com/finedistraction)

## Bot in the Wild
[G Foust Bot](http://twitter.com/GFoustBot)

### Prerequisities

You'll need:

* Node installed on your command line
* A Twitter account with its own phone number & API keys
* A Wordnik account & API key
* The foustbot.js file

### Installing

Feel free to clone this repo and edit the foustbot.js file to create your own Twitter poet bots. Edit the foustbot.js file's requests to the Wordnik API. Run from your command line using 

```
node foustbot.js
```

Then refine the random words and tweets you're making.

### Poem Template

CONJUNCTION the NOUN-PLURAL they VERB-PRESENT everything/something/nothing/anything


### Code sample
I puleld the getPresentTense function to talk about briefly. Taking the botData from previous functions and callback for the async waterfall, this function uses the node-rest-client to make a query to the WordNik API and inputs the current verb to get it's verb stem. This allows the bot to tweet in the present tense, when possible, in keeping with Foust's urgency of 'they own everything.'

```
getPresentTense = function(botData, cb){
  var client = new Client();
  var wordnikPresentURL = 'http://api.wordnik.com:80/v4/word.json/' 
  var wordnikPresentURLPart2 = '/relatedWords?useCanonical=false&relationshipTypes=verb-stem&limitPerRelationshipType=10&api_key=';
  var args = {headers: {'Accept':'application/json'}};
  var wordnikURL = wordnikPresentURL + botData.verb + wordnikPresentURLPart2 + wordnikKey;

  console.log("wordnikurl: ", wordnikURL);

  client.get(wordnikURL, args, function (data, response) {
    if (response.statusCode === 200) {
      if(!data.length){
        botData.present.push(botData.verb);
      } else {
        for(var i = 0; i < 1; i++){
          var temp = data[i];
          botData.present.push(temp.words);
        }
      }
      if (botData.present.length) {
        cb(null, botData);
      } else {
        cb(null, null);
      }
    } else {
      cb(null, null);
    }
  });
};
```

## Deployment

This bot was deployed to Heroku with a simple Procfile and uses the Heroku scheduler to tweet every hour.

Procfile content: 
```
main: node foustbot.js
```

## Built With

* Twitter API - for tweeting
* Wordnik API - helps us check for parts of speech and isolate adjectives and nouns
* NPM - to install various node packages that made the process much easier
* async - to create an array of separate functions called in order that pass along data to each other
* dotenv -  makes our env variables easier to bring in to our js
* node-rest-client -returns API requests as JavaScript objects
* twit - a simple npm module that makes search and retweeting easier
* wordfilter - keeps our bot family friendly with a 'blacklist' of inappropriate language
* Heroku - for deployment
* Heroku Scheduler - to create a cron job

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details

## Acknowledgments
* This bot was built using [ursooperduper](https://github.com/ursooperduper)'s [PickTwoBot](https://twitter.com/picktwobot) tutorial as base code
* Profile image and banner image for Twitter using [Unsplash](https://unsplash.com) images

