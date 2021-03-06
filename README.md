# Bubble Trouble 2 Emulator

### Statement
Our project is a racket remake of the flash game "Bubble Trouble". It's interesting because we learn how to use racket libraries and implement and manipulate different objects with a functional language. It's interesting to us because we both played it in elementary/middle school so it's nostalgic. We hope to learn how to apply some of the concepts we learned in class to a project that has a clear result of what we want to see in the end.

### Analysis
We used object orientation to hold our "bubble" objects, "player" object, "hook" object that will be shot from the player to pop bubbles. To manipulate these objects, we used data abstraction to update the x/y variables, and filter to delete objects from the universe. To check whether collisions occurred (either hook hits bubble, or bubble hits player) we mapped over the objects x/y positions. state modification will be used within each object to update the location, for example. if the left arrow key is held, the player will move 10 units to the left (set! x (- x 10)). 

### Filter
Filter was used to delete "popped" bubbles on the list by checking a flag that was set within each bubble object during hook-bubble collision detection.
```racket
(define (delete-popped-bubbles)
  (set! bubble-list (filter (lambda (x) (not (x 'popped?))) bubble-list)))
```

### Map
Map was used to update the location of each bubble in the list, as well as check for player-bubble and hook-bubble collisions.

```racket
(define (update-bubbles)
  (map update-bubble bubble-list)
  )

(define (update-player-collision)
  (map check-collisions bubble-list))

(define (update-hook-collision)
  (map check-collisions-hook bubble-list))

```

### Fold
Fold was used to build the list of images that place-images takes. Place-images takes a list of images, then a list of their corresponding x/y coordinates.

```racket
(define (posn-bubble-list my-bubbles)
  (foldl (lambda (bubble rest-list) (cons (bubble 'my-posn) rest-list)) '() my-bubbles))

(define (obj-list)
  (foldl cons (draw-bubble-list bubble-list)
         (list (p1-sprite) (draw-chain) (hook-sprite) (draw-HUD) (num-list))))

(define (draw-bubble-list my-bubbles)
  (foldl (lambda (bubble rest-list) (cons (bubble 'draw) rest-list)) '() my-bubbles))
     
(define (posn-list)
  (foldl cons (posn-bubble-list bubble-list)
         (list (p1 'my-posn) (chain-posn) (my-hook 'my-posn) (HUD-posn) (make-posn 100 100))))

```

### External Technologies
Our project generated sound, because the original flash game produces sounds as well. When the hook is deployed, 
a bubble is popped, or the player is hit by a bubble a specific sound that is associated with each action is prodcued. The sounds do 
not differ when the same action is performed. 

### Data Sets or other Source Materials
We did not use data sets or other source materials. We were attempting to emulate the classic flash game 'Bubble trouble 2'
from scratch in order to understand and reinforce the concepts learned in this class in an applicative way. 

### Deliverable and Demonstration

We now have a playable demo of the first five levels desgined. There is a replay button that allows the user to play the level again 
in case they die. There is no way to go back to a previous level without restarting the game, though.

### Evaluation of Results

We know we are successful since we were able to recreate the first level in "Bubble Trouble".

## Architecture Diagram
![software flow chart](https://github.com/oplS17projects/Bubble-Trouble-2/blob/master/software%20flow%20chart.png)

As noted in the Architecture diagram, our propgram starts through updating each frame. This is because we are using the universe library
and in order to animate our game, we had to pass objects into the library functions that allow us to do so. Every time the world that we
created gets update, it updates our 3 object classes: the player, the balls, and the hook. These object's relative positions are
updated, and by analyzing where each object is we are able to tell what is going on and how to react to key situations. 

The position of certain objects are checked every time the world updates. The position of the hook and all bubbles is checked every 
update cycle because if the hook and a bubble intersect, the bubble should either split into two or disappear. The position of the 
player and the position of all bubbles is checked every cycle because if the player and a bubble intersect, the player should lose a
life. These bounds that intesect are determined by the size of each bubble, the bounded box of the player, and the dimensions of the
deployed hook.

If a bubble and the hook intersect, there are 2 possible outcomes. If this bubble is the last bubble in our defined bubble-list list,
the level has been cleared and the player may advance to the next level. If the bubble is not the last bubble in the list, we then must
split it or remvoe it from the list. The smallest bubbles, of defined 'size 1' are the smallest bubbles that can exist in this game. If
a bubble of 'size 1' is popped, it is removed from the bubble list. If a bubble greater than that of 'size 1' is popped, that bubble is
removed from the bubble list and replaced with two bubbles of one lesser size. This proceess is repeated until the level is cleared or 
until the player has lost.

If a bubble and the player intersect, the display must reset to it's starting coordinates. If the player has more than 0 lives left, 
they can keep playing the level. If they are hit with a bubble and they have lost their last life, the player is shamed with a 
humiliating loss screen and promted to retry. The retry does not send the player back to level 1, because the shame of getting the loss 
screen is punishment enough. 


## Schedule
From this proposal, we took the code that we have already written in our first and second explorations and committed them to one
repository (this repository). We then combined the concepts that we have worked on in our explorations into one file that is our deliverable file, which we worked on together at a high level so that we both knew what needed to be done and what was going on. 

From our combined explorations, we then worked towards our first milestone - getting multiple objects drawn to the sceen as well as
player controls. 

From our first milestone, we worked on object collison detection and creating a head-up display (HUD) until we have completed 
those tasks for our second milestone. 

From our second milestone, we worked on fixing bugs, outlying errors, and time permitting the creation of multiple levels to 
create our deliverable project. 

### First Milestone (Sun Apr 9)
We have multiple objects drawn to the screen as well as player controls implemented.

### Second Milestone (Sun Apr 16)
We have object collison detection, basic bubble physics (when a bubble pops, it splits into 2 and bounces at a lower/higher height, etc.) and creating a head-up display (HUD) implemented.   

### Public Presentation (Mon Apr 24, Wed Apr 26, or Fri Apr 28 [your date to be determined later])
We have bugs fixed, outlying errors fixed, and time permitting multiple levels implemented. 

## Group Responsibilities

### Molly McGuire @mollyelizabethmcguire11
- Displayed objects to the screen
- Implemented hook-bubble detection
- Initially bounded bubbles to the constraints of the screen
- Implemented initial left and right bubble movements 
- Implemented multiple levels
- Randomn bug fixing
### Michael Danino @mdanino94
- Polished the player controls I already implemented in my exploration, added multiple sprites for when walking left/right
- Created a HUD that shows a level indicator and the number of lives left. 
- Implemented basic bubble physics
- Helped with player-bubble collision detection.
- Changed original implementation with hard-coded bubble objects to a list of bubble objects that can be mapped/filtered/folded on.
- Random bug fixing

lost screen image from http://4vector.com/i/free-vector-laughing-and-pointing-emoticon_133428_Laughing_and_pointing_emoticon.jpg

win screen image from http://gclipart.com/wp-content/uploads/2017/02/Smiling-sun-with-sunglasses-clipart.jpeg
