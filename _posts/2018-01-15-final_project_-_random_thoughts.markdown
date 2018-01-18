---
layout: post
title:      "Final Project - random thoughts"
date:       2018-01-15 16:05:11 -0500
permalink:  final_project_-_random_thoughts
---


I just finished my final project, which was just a quick fun attempt at making Connect Four happen. See the code [here](https://github.com/burrahey/connect-four) and watch a quick 45 second demo [here](https://youtu.be/l0gtrrWDkz4 ).

After getting to play with React for a little bit, I definitely wanted to build a game with it. I knew that my dad would love a good game of Bejeweled but I knew I had only a little more than a day to build it. So, I went for Connect Four which seems simpler.

The React portion was fairly straightforward except for the asynchorous bit with timing: I already did a post on JS timing so I'll skip that for now. It was interesting and challenging to figure out how states/routes/components to relate to one another. 

But the meatiest bit of this project was checking the Connect Four board for winners. In the future, I'd love to implement a Connect Four AI because there is so much theory online about the ideal game. The perfect moves are actually already mapped out!

Well, there are about two trillion ways to win according to this [Numberphile](https://www.youtube.com/watch?v=yDWPi1pZ0Po&t=36s) video and because my app was clickable the entire real time, I had to make sure that after each player played, the board was checked as quickly and efficiently as possible. A Connect Four board is traditionally 6x7, so that's a lot of cells to check for each turn. Obviously a nested array structure seemed most conducive to the nested loops I could see myself writing.

Well, the first thing you have to realize is that you only need to check the spot where the newest token was placed, and you only need to check to see if the most recent player has won. However, in a given move, a player could win in the following ways:

1) **Horizontal:** there are up to four ways to win here, with the most recent token being the left-most place to the right-most place (e.g. [000XXXX], [00XXXX0], [0XXXX00], [XXXX00]

2) **Vertical:** if your token ended up at least four rows high, you could win by getting four down (that's only one way to win)

3) **Diagonal:**  there are up to eight ways to win here (four ways on each diagonal: imagine an 'X' shape with our token in the center). 

Okay, cool, so what's the most efficient way to test all of them? I wondered, should I go for a checksum? E.g if the values of the rows added up to a certain number, then you know it's a win. However, this involved the same number of operations as simply checking two values but was potentially more confusing with more chance of bugs. I also briefly experimented with entering in "undefined" values rather than zeros and it felt super wrong so I nixed that idea. I also wondered if I should I represent my arrays as zeros or should I fill the arrays as users enter in their tokens? In other words, should my default game look like this:

```
const defaultGame = [
  [0,0,0,0,0,0,0],
  [0,0,0,0,0,0,0],
  [0,0,0,0,0,0,0],
  [0,0,0,0,0,0,0],
  [0,0,0,0,0,0,0],
  [0,0,0,0,0,0,0]
];
``` 

Or this?

```
const defaultGame = [
  [],
  [],
  [],
  [],
	[],
  []
];
```

I ended up going with the first one because it made displaying information easier, and again, if I was checking length every time to make sure we don't go out of bounds, I wasn't sure if it was going to be an "optimization" at all. In the final implementation, you'll notice that I don't waste time looking for out of bounds values for the most part, but I do check some empty arrays, which this method may have avoided.

Finally, I also thought about what I could hold in my state or even in the function itself in order to make computation more simple. Would storing a hash somewhere with the values of the relationships reduce the number of checks I do? For example, we check a lot of the values multiple times for similarity. Ultimately, I knew that it was best to keep your state lean, and I didn't see that much benefit to increasing the amount of information I stored. 

Well, my implementation was fairly straightforward after all that. You can find it [here](https://github.com/burrahey/connect-four/blob/master/client/src/BoardCheckingFunctions.js).

First, I check horizontals because they're generally possible first:
```
  //checking horizontals
  for(let i = 0; i < 4; i++){
    if(gameState[row][column + i - 3] === gameState[row][column + i - 2] && gameState[row][column + i - 2] === gameState[row][column + i - 1] && gameState[row][column + i - 1] === gameState[row][column + i - 0]){
      console.log("win horizontal row: " + row)
      return true;
    }
  }
```

As you can see, it's three comparisons, up to four times, so 12 operations total (if you don't count all the adding/subtracting we do).

Then, diagonals, because those are most probable after that:
```
  //checking diagonals
  for(let i = 0; i < 4; i++){
    if(((row + i - 3) >= 0) && ((row + i) < maxLength)){
      //left to right
      if(gameState[row + i][column - i] === gameState[row + i - 1][column - i + 1] && gameState[row + i - 1][column - i + 1] === gameState[row + i - 2][column -i + 2] && gameState[row + i - 2][column - i + 2] === gameState[row + i - 3][column - i+ 3]){
        console.log("diagonal win left to right row:" + row + " column: " + column + " i: " + i)
        return true;
      } else if(gameState[row + i][column + i] === gameState[row + i - 1][column + i - 1] && gameState[row + i - 1][column + i - 1] === gameState[row + i - 2][column + i - 2] && gameState[row + i - 2][column + i - 2] === gameState[row + i - 3][column + i - 3]){
        //right to left
        console.log("diagonal win right to left row:" + row + " column: " + column + " i: " + i)
        return true;
      }
    }
  }
```

The key phrase is `if(((row + i - 3) >= 0) && ((row + i) < maxLength)` which makes sure I don't look for values out of bounds. In this case, we're looping four times and making eight operations (i.e. 32 total operations per play), but if your token is at the bottom or top or by the corners, we won't loop through every single time. We'll likely just make 1-2 checks per loop, so ~8 checks per play.

Finally, the vertical is just three comparisons:
```
  // checking vertical wins
  if(row < 3){
   if(gameState[row][column] === gameState[row+1][column] && gameState[row+1][column] === gameState[row+2][column] && gameState[row+2][column] === gameState[row+3][column]){
     console.log("win vertical: " + column)
     return true
   }
 }
```

I thought this implemention would take forever but I'm still new to how fast work is for computers because it ended up being fine. I decided to stop by optimization here because it was working fast enough for a good play (see video linked above for a demo). 

It was a really fun project and I hope to do more like this in the future! If you have any tips/tricks for optimization, please don't hesitate to reach out!

