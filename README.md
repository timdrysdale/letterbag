# letterbag
Server side code for sharing a virtual bag of letters, e.g. for playing real-world word-based games.

## Background

Board games based on randomly drawing letter tiles from a bag present particular difficulties for players in separate geographic locations who wish to play together. An excellent light-weight client-side only solution is available (here)[https://github.com/Tugzrida/virtual-scrabble-bag], although users must perform repetitive tasks such as entering how many tiles their opponent just drew (which ordinarily you need not know). Making relatively minor mistakes can lead to abandoning the game. 

## Server side code

A server-side implementation has one major drawback - someone needs to pay for a server for it to run on. Other than that, it has significant user experience advantages which for some may outweigh the cost. For example, users need only enter their own moves, rather than also keeping track of their opponent's. It's also relatively straightforward to implement letter swaps and undo.

## Usage 

The API is loosely based on RESTFUL principles. 

```
list all games
/api/v1/games GET

create new game
/api/v1/games PUT <secret>

delete game 
/api/v1/games PUT <secret>

draw letters:
/api/v1/games/<id>/draw PUT <n>,<secret>

undraw letters (randomly chooses which ones you should return):
/api/v1/games/<id>/undraw PUT <n>,[<letters>],<secret>

swap letters:
/api/v1/games/<id>/swap PUT [<letters>],<secret>

number of tiles remaining:
/api/v1/games/<id> GET 

history
/api/v1/games/<id>/history GET 

```

Since the letterbag cannot track which tiles you have played, validation of letters returned is limited to checking that the letters match letters previously drawn. Enhanced validation would require keeping track of the game state, but the most convenient user interface for that would potentially involve showing a board design, and is therefore undesirable. An alternative would be using image recognition to track letter tiles played in the field of the view of the camera, and this would have the advantage of working for any letter-based game (for which the fonts were suitably distinguishable). This would allow the system to infer the letters in the hands of the players, but not which letters are in which players hands, Even if you know which letters a player has drawn, if they have at some point both drawn the same letter, due to their being duplicates, and if only one of them has been played, it cannot be inferred from looking at the board which player played the letter, unless the board is imaged after each turn. It is not possible to infer the end of a turn from the drawing requests, because during fast play the opponent can be playing their letters before the other has drawn. On the other hand, integrating a scoring feature would allow values to appreciate the value proposition of pressing the "automatic score" button after completing their turn.

Such automatic scoring features are currently available in [this app](https://play.google.com/store/apps/details?id=com.scorabble.app&hl=en_GB&gl=US)

## Security

Ideally, the player starting the game is able to share a secret with the other players, which is unguessable by 

## Architecture

