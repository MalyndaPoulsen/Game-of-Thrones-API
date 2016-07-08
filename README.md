# Game-of-Thrones-API
# API
How to "consume" an api.

1-First you need to find an api. Simply run a google search for a free api. 
For our example, we are going to use the 'An API of Ice and Fire' @ https://anapioficeandfire.com/
This site gives you http://anapioficeandfire.com/api/ as the base API. 
If you simply copy and paste this into the browser's url, you will see the possible extensions. 
In this case, you can capture characters, books, or houses. 
We can simply start with characters, and then we can include more later if we so choose.

2-Lets begin by creating a repository named "GameOfThrones" on github. Initialize the gitrepository, then clone/copy the address. 
Inside your terminal, cd into "projects" and then type `git clone <github-url>` and paste in your url. 
Remember to include a space between clone and your url.
Now type `cd GameOfThrones` and press the `enter` key.  
Now you are in your file, type `http-server .` in your terminal, and press the `enter` key.
In your Google Chrome browser type `local: 8080` and press `enter`.
This way you can view your progress on your local server.

3-Now open your `vsCode` and open your "GameofThrones" file.
Once you have it open, create a new file and name it `index.html`.
On this page, type `doc` and press `tab`. This will set up the body of your file.
Add a `<script>` tag above the second <body> tag with a CDN for Bootstrap and a seperate `<script>` tag for `jQuery` and a `<script>` tag for `app.js` and `service.js` and `style.css`. 
It will look like this: 
```javascript
<script src="https://code.jquery.com/jquery-2.2.4.js"   integrity="sha256-iT6Q9iMJYuQiMWNd9lDyBUStIq/8PuOW33aOqmvFpqI="   crossorigin="anonymous"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js" integrity="sha384-0mSbJDEHialfmuBBQP6A4Qrprq5OVfW37PRR3j5ELqxss1yVqOtnepnHVP9aJ7xS" crossorigin="anonymous"></script>
<script src="service.js"></script>
<script src="app.js"></script>
    ```

Now add a link to the `Bootstrap CSS file` and your `style sheet`.  
These should be included in the head of your index file. Include them right before the second `<head>` tag. 
It should look like:
```javascript
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" integrity="sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7" crossorigin="anonymous">
<link rel="stylesheet" href="style.css">
```



4- We need to write a function (which will make an ajax call via `$.get` function) that will call the API. 
Every time you write this kind of function, you need to include a callback function otherwise, your call is left hanging.
The callBack function says- *"Once you have found this information, do (new function) with it."

You will probably want to put the following into your *'service file' and it will look like: 
```javascript
    function CharacterService (url, callWhenDone){
            if(typeof callWhenDone !== 'function'){
      return 'Error: you must provide a function to call when done'
  }
}
```
5- Two things you should take note of 
    is the fact that `Character` is capitalized. That indicates it's a `constructor function`. 
    It will still function the same if it is lowercased, but this is a way to indicate your intentions. 
    Also the if statement `if(typeof callWhenDone !== 'function')` is saying, "If the callBack isn't a function, I can't help you."

6- Now we need a `goGetData function`. This is where we use our `$.get function`, and we also include the API address.
This function is basically a mailman. It delivers the request, and brings it back in a little `JSON` package. 
This function lives inside your `CharacterService function`.

It will look something like this: 

```javascript
function CharacterService (url, callWhenDone){
    if(typeof callWhenDone !== 'function'){
        return 'Error: you must provide a function to call when done'
    }
    function goGetData(){
  
    //url = "http://anapioficeandfire.com/api/characters/";
      
    $.get(url, function(response){
        callWhenDone(response.data)
    })
  
  goGetData()

}
```

7- We are awesome so far! You have your function, it is making a call, and it has a callBack. But you haven't determined what you want to get back.
If you simply ask for all the data, it will be a mess of text. 
So you need to figure out what you want to target.
Lets type `http://anapioficeandfire.com/api/characters` into the browser's url and see what we have access to.

8- The first thing you should note of, it is an `array of objects`.
Also, it is only showing `10` characters. Well, we know there are a lot more characters in the Game of Thrones. 
So this API has a hold on the number of items it will return at one time. It usually comes in groups of ten.
So you will want to add a filter to your url. 
Add `?page=2` to the end of `http://anapioficeandfire.com/api/characters`
As you can see you have a second page. You can try any number such as `?page=30` or `?page=50` to see how many pages are available.
Ok, so a lot!!

9-Lets decide what information we want to harvest or mine.
Here are your choices for this api:
```javascript
  {
    "url": "http://anapioficeandfire.com/api/characters/1894",
    "name": "Rolfe",
    "gender": "Male",
    "culture": "",
    "born": "",
    "died": "",
    "titles": [],
    "aliases": [],
    "father": "",
    "mother": "",
    "spouse": "",
    "allegiances": [],
    "books": [
      "http://anapioficeandfire.com/api/books/2"
    ],
    "povBooks": [],
    "tvSeries": [],
    "playedBy": []
  },
  ```
  We might want to create a card game using character cards... Lets get as much information as we can.

  10- So how do we target the properties of an object within an array?
  A `for loop" is for sorting through an array`. A `forEach` function also loops through arrays. 
  We do need to loop through an array, but we also want to target the properties of the objects. 
  So we have to use bracket notation.
  Inside our callBack function we need to define what we need and the path to get it.

    For this project it will look like:
    