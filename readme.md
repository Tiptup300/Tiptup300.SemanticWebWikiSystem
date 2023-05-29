## Intro

This is just a project to build towards something bigger. Specifically having people write out stories in normal english, or even better, not normal english and then it just interprets it and tags it into data. Data that be hooked to other data. 

### Intro / tldr; how do i run it?

Just copy the prompt from prompt.txt and put it into a fresh window at chat.openai... 

### Intro / Improvements To Come

* Need to remove a lot of duplicate declarations in the prompt, get it shorter. As well as refine what I want to ask less of the ai bot. 
    * One thing at a time. The output will be data that can be processed and do the heavy lifting. 
* The other issue is that I have instance * _ needs to be culled down.
    * (underscore) for semanticscript defined attributes.
    * (full stop) for domain defined attributes.
    * (asterisk) for story defined attributes.
* Suffixes for datatype of fields
    * Dates with times could be condensed consistently always as utc format/timezone
    * string, int, double, float, datetime, date, time, dataElement
* Consistency is needed for refering to the names of the elements. 

## Example

### Example: ChatGPT Input

```
DOMAIN DATA:
The domain is music. In music there are artists, there are albums, artists release albums, on these albums are songs or tracks. There are also eps lps, but they're all albums.

STORY DATA:
Controversies
On June 16, 2011, journalist Andrea Swensson of City Pages negatively reviewed one of Green's performances, writing that it "failed to measure up to the fun factor of his recorded material. Green spent most of the set stationed in front of a microphone at the center of the stage, barely moving an inch while he sang, and flanked by two forgettable back-up singers and a DJ that was all but hidden behind a giant LCD display".[93] In response, the following day Green tweeted, "I respect your criticism but be fair! People enjoyed last night! I'm guessing ur gay? And my masculinity offended u? well fuck U!"[94] Green promptly received angered responses from some of his followers on Twitter, to which he replied, "Apologies gay community! what was homophobic about that?"[95] In a subsequent interview with magazine Us Weekly, Green stated that his comments were meant in good fun, adding that "I am not harboring any sort of negative feeling toward the gay community" and that "I am one of the most liberal artists that I think you will ever meet, and I pride myself on that."[96]

On December 31, 2011, Green sang John Lennon's "Imagine" just prior to the ball drop for New Year's Eve at New York City's Times Square. In his rendition, Green replaced the line "and no religion, too" with the words "and all religions true".[97] Many saw this as a substantial revision of the meaning behind Lennon's original lyrics.[97] Shortly after the performance, Green responded via Twitter: "Yo I meant no disrespect by changing the lyric guys! I was trying to say a world where u could believe what u wanted that's all".[98] Green deleted a series of tweets pertaining to this event shortly thereafter.[99]

On October 30, 2012, Green was accused of sexual battery by a woman with whom he had dined at a Los Angeles restaurant,[100][101] leading to a nearly year-long investigation by the Los Angeles Police Department.[102] On October 21, 2013, Green pleaded not guilty to a felony charge of furnishing a controlled substance after the Los Angeles District Attorney's Office declined to file a charge of rape of an intoxicated person, citing insufficient evidence.[103] On August 29, 2014, Green pleaded no contest to one felony count of furnishing ecstasy and was sentenced to three years of probation, 360 hours of community service, and ordered to complete 52 Alcoholics Anonymous or Narcotics Anonymous meetings.[104]

On August 31, 2014, Green was criticized for tweets relating to his sexual battery court case. Two of his specific tweets regarding rape were "People who have really been raped REMEMBER!!!" and "If someone is passed out they're not even WITH you consciously! so WITH Implies consent." After sending a number of tweets on the topic of rape, he temporarily deactivated his Twitter account. He re-activated it a number of hours later, tweeting the following apology: "I truly and deeply apologize for the comments attributed to me on Twitter. Those comments were idiotic, untrue and not what I believe."[105]
```

### Example: ChatGPT Output
```json
[
    {
        "_domain": "music"
    },
    {
        "_controversy": {
            "*date": "June 16, 2011",
            "*journalist": "Andrea Swensson",
            "*review": "negative",
            "*performance": "Green's",
            "*criticism": "failed to measure up to the fun factor of his recorded material. Green spent most of the set stationed in front of a microphone at the center of the stage, barely moving an inch while he sang, and flanked by two forgettable back-up singers and a DJ that was all but hidden behind a giant LCD display."
        }
    },
    {
        "_response": {
            "*date": "June 17, 2011",
            "*content": "I respect your criticism but be fair! People enjoyed last night! I'm guessing ur gay? And my masculinity offended u? well fuck U!",
            "*source": "Green's tweet"
        }
    },
    {
        "_response": {
            "*date": "June 17, 2011",
            "*content": "Apologies gay community! what was homophobic about that?",
            "*source": "Green's tweet"
        }
    },
    {
        "_interview": {
            "*date": "unknown",
            "*magazine": "Us Weekly",
            "*content": "Green stated that his comments were meant in good fun, adding that 'I am not harboring any sort of negative feeling toward the gay community' and that 'I am one of the most liberal artists that I think you will ever meet, and I pride myself on that.'"
        }
    },
    {
        "_performance": {
            "*date": "December 31, 2011",
            "*song": "Imagine",
            "*event": "New Year's Eve at New York City's Times Square",
            "*revisedLine": "and all religions true",
            "*originalLine": "and no religion, too",
            "*interpretation": "Many saw this as a substantial revision of the meaning behind Lennon's original lyrics."
        }
    },
    {
        "_response": {
            "*date": "unknown",
            "*content": "Yo I meant no disrespect by changing the lyric guys! I was trying to say a world where u could believe what u wanted that's all",
            "*source": "Green's tweet"
        }
    },
    {
        "_tweets": {
            "*date": "August 31, 2014",
            "*criticism": "related to sexual battery court case",
            "*tweets": [
                "People who have really been raped REMEMBER!!!",
                "If someone is passed out they're not even WITH you consciously! so WITH Implies consent."
            ]
        }
    },
    {
        "_response": {
            "*date": "August 31, 2014",
            "*content": "I truly and deeply apologize for the comments attributed to me on Twitter. Those comments were idiotic, untrue and not what I believe.",
            "*source": "Green's tweet"
        }
    }
]
```
## Prompt

```
YOU ARE A MACHINE THAT ONLY RESPONSES IN VALID JSON.

You are a machine that exports semanticscript from user stories. Sematicscript is a markup scripting language that can be always be processed as typical json, it only differs in that it's a way to define arbitrary data types with arbitrary sets of data. Almost semantic web sort of thing. The json will always appear as an array of objects each object has no nested data. All nested data should instead be it's own other objects with markers to the parent data.

The variable name is sometimes prefixed with special character signifiying  what that data actually is and concepts about it.Here's an example sets of data:

In all cases any matching variable name between multiple sets of data with or without a prefix are interpretted the same. Ex. _artist is artist is *artist. 

DO Duplicate data by seperating parts of elements into their own more detailed element.
DO Tag as many notable names, dates, times, or titles these are of as possible.
DO Tag as many pieces of content as possible such as names of people, names of events, names of pieces of work or content.
DO NOT try to nest elements.
DO duplicate nested elements to their own object on the top level array of the json.
DO NOT have arrays of objects within one data element.

SPECIAL CHARACTERS:

_ = This attribute prefix signifies that this attribute is a type identifier, There is to be ONLY ONE of these per data set.
. = This attribute prefix signifies that this attribute is describing a relationship a data attribute is a reference to a parent data point.
(none) = This signifies a data attribute thats specific to that type of data. 
* = This signifies a data attribute that's specific to that instance.

(parentType). = If a data object is nested it must have the parent type name prefixed with a period.

THE FOLLOWING DATA IS MEANT TO PROVIDE EXAMPLES AND SHOULD BE IGNORED OTHERWISE, NO ADDITIONAL INFORMATION OR PROCESSING IS NEEDED FOR THE FOLLOWING EXAMPLES

EXAMPLES WITH ARTISTS, SONGS, AND ALBUMS
---------------------------------------

DOMAIN DATA:
The domain is music. In music there are artists, there are albums, artists release albums, on these albums are songs or tracks. There are also eps lps, but they're all albums.

STORY DATA:
The artist (the) Smashing Pumpkins released two albums "Siamese Dream", and "Mellon Collie" (which was released in 2002).

SEMANTICSCRIPT:
[
   {
      "_artist": "Smashing Pumpkins",
      "*nickName": "The Smashing Pumpkins",
      "*usesThe": true,
      "albums": [
         { "_album":"Siamese Dream" },
         { "_album":"Mellon Collie", "releaseDate": 2002 }
      ]
   },
   { "_album": "Siamese Dream", "artist": "Smashing Pumpkins"},
   { "_album": "Mellon Collie", "artist": }
   {
      "_album": "Siamese Dream",
      ".artist": "Smashing Pumpkins"
   },
   {  "_artist": "Fall out boy" },
   { "_artist": "Slayer" },
   { "_album": "Silent Hill 2 Original Soundtrack" },
   { "_artist": "Black Sabbath" },
   { "_album": "Black Sabbath", "artist": "Black Sabbath" },
   { "_song": "Black Sabbath", "artist": "Black Sabbath", "album": "Black Sabbath" }
]
--------------------------------------
ALL DATA IS TO BE OUTPUTTED TO CORRECTLY FORMATTED AND DISPLAYED JSON WITH AN ARRAY OF OBJECTS
--------------------------------------
REQUEST:
--------------------------------------
PLEASE RESPOND Y IF YOU UNDERSTAND
--------------------------------------
ALWAYS RESPONSD AS AN ARRAY OF OBJECTS
```

