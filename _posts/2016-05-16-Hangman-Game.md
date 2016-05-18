---
title:  "Hangman Game"
categories:
image: /img/hangman.png
---

I had a little downtime and decided to create a fun little project for myself, a hangman game!  I'll admit, I was inspired while watching an old episode of Homeland &mdash; morbid, I know.  Don't care how I made it? You can go straight to my project page to play a round of [hangman](http://vlhernandez.com/hangman/)! If you want to know how I made this continue reading!


I started out by looking for an api I could use to get a random word and found [wordnik](http://developer.wordnik.com/docs.html).  There are several parameters I can play with, but all I really needed was to set a max word length. Wordnik returns a random word as a json object.

Once I get the word, I make another api call to get the definition and part of speech (I save these and offer them to the user as a hint in exchange for a turn). I store all that data in an object that allows me to keep track of guessed letters.

When the page loads, I make my api call to get a random word and using [jquery](https://jquery.com/) create the markup to hold the word. I'm using a bottom border to indicate the blank space for each letter. I give each <div> an index that I'll use later when the user makes a guess. If the word contains a hyphen, an apostrophe or a space I give it an additional class named symbol that I use to eliminate the little underline and display the symbol.

![word with hyphen](img/hangman-hyphen.png)

{% highlight html linenos %}
  <div class="word">
    <div idx="0" class="letter"></div>
    <div idx="1" class="letter"></div>
    <div idx="2" class="letter"></div>
    <div idx="3" class="letter symbol">-</div>
    <div idx="4" class="letter"></div>
    <div idx="5" class="letter"></div>
    <div idx="6" class="letter"></div>
  </div>
{% endhighlight %}


When the user clicks on a letter, I use the onclick event to determine which letter was clicked, disable the click event for that particular div (keyboard key) and give it a class called disabled which allows me to style the div to look like an inactive key.

Next, I go to my word object and check whether the user made a correct guess by checking for the letter in my object.  If it was a good guess I reveal the all instances of the letter by adding it to the <div>.

![keyboard keys disabled](img/hangman-keyboard.png)

{% highlight html linenos %}
  <div class="word">
    <div idx="0" class="letter"></div>
    <div idx="1" class="letter"></div>
    <div idx="2" class="letter"></div>
    <div idx="3" class="letter symbol">-</div>
    <div idx="4" class="letter">o</div>
    <div idx="5" class="letter"></div>
    <div idx="6" class="letter"></div>
  </div>
{% endhighlight %}

To incrementally display the little man, I first used CSS to build him. Each body part has a class that matches the key value pairs in the man object I described earlier. I then use a counter to keep track of how many bad guesses the user has made. When they make a bad guess, I use the count to reveal the corresponding body part.

![bad guess](img/hangman-badguess.png)

{% highlight javascript linenos %}
// assume missedCnt = 3

var showPart = man[missedCnt];

var man = {
    0: "lt-leg",
    1: "rt-leg",
    2: "torso",
    3: "lt-arm", // <-- this body part will be revealed
    4: "rt-arm",
    5: "head"
  };

// using jquery I'll remove the class "hidden" from the
// div with class "lt-arm" to reveal the left arm
$("."+showPart).removeClass("hidden")

{% endhighlight %}


If the user choses to take a hint I increment the bad guess counter and call my revealBodyPart function to reveal a body part along with the hint.

![hint](img/hangman-hint.png)

##### Style Details: #####

* Headline: Google Fonts Cabin Sketch
* Body: Google Fonts Open Sans
* Colors: I just wanted something to get away from all the blues I'd been using recently.
* Word Letters: font-size: 1em (16px)
* Keyboard Letters: font-size: 1em (16px)
* Hint: font-size: .9em (14.4px), letter-spacing: .0625em (.9px)
