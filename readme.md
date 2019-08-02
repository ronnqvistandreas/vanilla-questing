# Vanilla Questing
This is an open source webapp that presents quest related content for Classic World of Warcraft in a graphical and easy-to-follow format. The underlying algorithm parses a custom written JSON file and dynamically renders the page content based on the JSON block you’re viewing. There are no traditional backend components like a database being used, and the application functions completely on the client side. Profiles and settings are stored locally in your browsers localstorage.

**Disclaimer**: The content of the website represents the latest “gh-pages” branch which is a compiled and minified version of the master branch. The master branch might not fully represent the current iteration of the live website if something new is in development. If you're interested in routing rather than the code, read the section about routing locally.

## Table of Contents
* [Cloning the project](https://github.com/wickstjo/vanilla-questing#cloning-the-project)
* [Routing locally with hot-reloading](https://github.com/wickstjo/vanilla-questing#routing-locally-with-hot-reloading)
* [Writing your own route file](https://github.com/wickstjo/vanilla-questing#writing-your-own-route-file)
	* [Stick to lowercase](https://github.com/wickstjo/vanilla-questing#stick-to-lowercase)
	* [Borrow heavily](https://github.com/wickstjo/vanilla-questing#borrow-heavily)
* [The route wrapper](https://github.com/wickstjo/vanilla-questing#the-route-wrapper)
* [Required page properties](https://github.com/wickstjo/vanilla-questing#required-page-properties)
* [Required waypoint properties](https://github.com/wickstjo/vanilla-questing#required-waypoint-properties)
* [Optional waypoint properties](https://github.com/wickstjo/vanilla-questing#optional-waypoint-properties)
* [Quest type formatting](https://github.com/wickstjo/vanilla-questing#quest-type-formatting)
* [Samplecode](https://github.com/wickstjo/vanilla-questing#samplecode-for-a-finished-waypoint)

## Cloning the project
The application is entirely written in **ReactJS** and comes with automatic hot reloading. You need to have NodeJS and NPM installed in order to install the required node modules after cloning the project. This are the version I have installed:

* **Node** v11.12.0
* **NPM** 6.7.0

The following bash commands should install and boot up the project successfully:

```bash
$ git clone https://github.com/wickstjo/vanilla-questing
$ cd vanilla-questing/
$ npm install
$ npm start
```

## Routing locally with hot-reloading
I recently nuked the "development mode" from the main product and will instead be launching a separate repository for that purpose that does not have a multitude of unnecessary bells and whistles. This should not take very long so be on the lookout!

## Writing your own route file
JSON files read very much like text, so converting your leveling routes to a format the algorithm can understand is not a massive undertaking, even for people with no prior programming experience. The same patterns are repeated from start to finish, so after understanding the basics you end up mostly copy/pasting blocks and modifying small details here and there.

### Stick to lowercase
Many quests are “punny” and written with awkward capitalization and general spelling. Because of this, stick to writing everything in **lowercase**. If a mouseover tooltip for a quest comes back as undefined it’s either spelled incorrectly or doesn’t exist in the ID file. Consult the files [[horde](https://github.com/wickstjo/vanilla-questing/blob/master/src/routes/horde/quests.json) / [alliance](https://github.com/wickstjo/vanilla-questing/blob/master/src/routes/alliance/quests.json)] for spelling issues, and if the quest doesn’t exist at all, contact me and I’ll add it asap.

### Borrow heavily
Most of the heavy lifting is already done since you can borrow references for quests and NPCs from my completed route files with CTRL+F searching. You can read [this reddit post](https://www.reddit.com/r/classicwow/comments/ca6ud9/vanillaquestingme_last_minute_betanew_fresh/et971t5/) for a more detailed breakdown of JSON. You can view my [route files](https://github.com/wickstjo/vanilla-questing/tree/master/src/routes) for tips and references.


## The route wrapper:
Everything in your route needs to be wrapped neatly inside an **Array** with the name **path**:
```json
{
   "path": []
}
```

## Required page properties:

* **Zone**: Has to match the name of the background image file. [See list](https://github.com/wickstjo/vanilla-questing/blob/master/src/funcs/misc.js#L42).
* **Experience**: Your estimation of what level the user should be at this point.
	* Can be either an integer or float. 
* **Waypoints**: Needs to be of the type Array.

```json
{
   "zone": "elwynn",
   "experience": 5.36,
   "waypoints": []
}
```

## Required waypoint properties:

* **Coords**: The xy representation of where the waypoint is.
* **Header**: An arbitrary name for the waypoint.
* **Type**: Determined the color of the waypoint icon.
	* **Hub** is blue.
	* **Quest** is yellow.
	* **Objective** is red.
	* **Travel** is purple.
	* **Flightpath** is green.

```json
{
   "coords": { "x": 49, "y": 42 },
   "header": "this is a cool header",
   "type": "quest"
}
```
## Optional waypoint properties:

* **Starts**: A quest is picked up here.
	* Yellow background. 
* **Ends**: A quest is returned here.
	* Green background. 
* **Completed**: A quest is both picked up and finished here.
	* Purple background.
* **Objectives**: A quest objective is done here.
	* Red background. 
* **Special**: A custom note from the author.
	* Blue background.
* **Align**: How the waypoint number is aligned.
	* Top, Right, Bottom.
	* Defaults to Left is no align property is given.

```json
{
   "starts": ["this quest is picked up"],
   "ends": ["this quest is finished"],
   "objectives": ["this is a quest objective"],
   "completed": ["this quest is picked up and finished at once"],
   "special": ["this is a cool note"],
   "align": "bottom",
}
```

## Quest type formatting
For the engine to fetch IDs correctly, standalone quests only require the name of the quest while chain quests require a second property that defines  which part should be fetched. Elite, dungeon, escort and random drop quests should also be tagged with their corresponding letter for readability, but it's not mandatory.

* **P#**: Part # of a chain quest.
* **E**: Elite quest
* **D**: Dungeon quest
* **F**: Escort quest
* **R**: Random drop quest

```json
{
   "starts": [["this is a chain quest", "p1"]],
   "starts": [["this is an elite quest", "E"]],
   "starts": [["this is a dungeon quest", "D"]],
   "starts": [["this is an escort quest", "F"]],
   "starts": [["this is a random drop quest", "R"]],
}
```

Special notes work in a similar way, except that the note is turned into a link when a secondary property is provided.

```json
{
   "special": [["this is a cool link", "https://classicdb.ch/"]]
}
```

## Samplecode for a finished waypoint:

```json
{
   "coords": { "x": 30, "y": 89 },
   "header": "western pirate ships",
   "objectives": [["the bloodsail buccaneers", "p5"]],
   "starts": [["cortello's riddle", "p1"]],
   "special": ["look for a scroll inside the boats"],
   "type": "objective"
}
```

## Samplecode that's ready to be imported
```json
{
   "path": [
      {
         "zone": "tirisfal",
         "experience": 12.82,
         "waypoints": [
            {
               "coords": { "x": 61, "y": 59 },
               "header": "zeppelin station",
               "type": "travel"
            },
            {
               "coords": { "x": 62, "y": 52 },
               "align": "right",
               "header": "brill",
               "starts": ["delivery to silverpine"],
               "type": "hub"
            },
            {
               "coords": { "x": 62, "y": 65 },
               "align": "right",
               "header": "undercity gates",
               "type": "travel"
            }
         ]
      },
      {
         "zone": "undercity",
         "experience": 12.82,
         "waypoints": [
            {
               "coords": { "x": 63, "y": 48 },
               "align": "top",
               "header": "undercity flightpath",
               "special": ["learn the flightpath"],
               "type": "flightpath"
            },
            {
               "coords": { "x": 71, "y": 44 },
               "align": "top",
               "header": "undercity lifts",
               "type": "travel"
            }
         ]
      }
   ]
}
```
