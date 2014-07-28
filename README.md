# chess.js

Chessboard manipulation in JavaScript. Used in [JoahG/chs.xyz](https://github.com/JoahG/chs.xyz).

## Dependencies

chess.js has no dependencies. It works out of the box with 0 configuration.

## Initialization

After loading chess.js into your project, simply create a new `Board` object by assigning it to a variable:

```javascript
  var board = new Board();
```

The `Board` object constructor takes a single optional parameter, `move_callback`, which will be called every time a move is made in the game (could be used to push data to a server, etc.). The `move_callback` function takes a single parameter, which is a [Log Object](#log-objects);

```javascript
  var board = new Board(function(move){
    // Do something here
  })
```

## Attributes

The created `Board` object will contain several attributes. Here is a description of each of them:

  - `board.spaces`: will be an `Array` of [Space Objects](#space-objects);
  - `board.turn`: will be the current player turn in a `String`.
  - `board.playing_with_fairies`: will be a `Boolean` turning on/off playing with [fairies](http://en.wikipedia.org/wiki/Fairy_chess_piece).
  - `board.log`: will be an `Array` of [Log Objects](#log-objects);
  - `board.move_callback`: will be the `move_callback` function included on initialization.

## Methods

The created `Board` object will also contain several methods for manipulating the board. They are profiled as follows:

  - `board.space_at(coordinates)`: will return a [Space Object](#space-objects) existing at the inputted `coordinates` (in the form of `'HV'` - aka `'b3'`).
    - returns: [Space Object](#space-objects) existing at the inputted `coordinates`
    - parameters: (1)
      - `coordinates` (`String`): desired coordinates in the form of `'HV'` - aka `'b3'`
  - `board.update_guards()`: will update all the pieces' spaces they are guarding.
    - returns: `undefined`
    - parameters: (0)
  - `board.kings_in_check(army)`: will determine whether or not the King is in check.
    - returns: 
      - with inputted `army`
        - `Boolean` (whether or not `army`'s king is in check)
      - without inputted `army`
        - `Array` consisting of two `Boolean`s (`[white_in_check, black_in_check]`).
    - parameters: (1)
      - `army` (`String`): will restrict the return to the inputted army **optional**
  - `board.is_checkmate(army)`: will determine whether or not an army is in checkmate.
    - returns: `Boolean` (whether or not `army` is in checkmate)
    - parameters: (1)
      - `army` (`String`): which army to check for checkmate
  - `board.next(str)`: will increment the inputted `str` by one value (aka input `'b'` will return `'c'`)
    - returns: `String`
    - parameters: (1)
      - `str` (`String`): string to be incremented.
  - `board.prev(str)`: will decrement the inputted `str` by one value (aka input `'b'` will return `'a'`)
    - returns: `String`
    - parameters: (1)
      - `str` (`String`): string to be decremented.
  - `board.restore(Board)`: will restore the current `Board` object to the inputted `Board` state.
    - returns: `undefined`
    - parameters: (1)
      - `Board` (`Object`): board state to be restored
  - `board.export()`: will prep the board for exporting
    - returns: `String` (`JSON` `stringified` Board object)
    - parameters: (0)

## Other Object Types

There are also two Object types that are children of the `Board` object constructor. They are used and included within the `Board` methods.

### Space Objects

The `Space` objects will represent each square (or "space") on the board. They are contained in `board.spaces`.

#### Attributes

The `Space` objects have a few attributes, which are as follows:

  - `space.hor`: `String` (`space`'s horizontal position (aka `'d'`))
  - `space.ver`: `String` (`space`'s vertical position (aka `'4'`))
  - `space.is_occupied`: `false` if unoccupied, otherwise a [Piece Object](#piece-objects)
  - `space.is_guarded`: `Array` of pieces guarding the space.

#### Methods

The `Space` objects also have a single method, which is the following:

  - `space.guarded_by(army)`: returns whether or not an army is guarding a space
    - returns: `Boolean`
    - parameters: (1)
      - `army` (`String`)

### Piece Objects

The `Piece` objects represent the individual pieces on the board. 

#### Attributes

The `Piece` objects have a few attributes, which are as follows:

  - `piece.type`: `String` type of piece (aka `'knight'`)
  - `piece.space`: `String` coordinates of piece (aka `'b2'`)
  - `piece.army`: `String` piece's army (aka `'white'`)

#### Methods

The `Piece` objects also have several methods:

  - `piece.possible_moves()`: returns all the possible moves for a given piece
    - returns `Array`:
      - `Array[0]`: All the possible *moves* for the piece (`Array` of coordinates)
      - `Array[1]`: All the possible *captures* for the piece (`Array` of coordinates)
      - `Array[2]`: All the *moves* and *captures* combined (`Array` of coordinates)
      - `Array[3]`: All the spaces the piece is guarding (`Array` of coordinates)
    - parameters: (0)
  - `piece.can_take(other_piece)`: returns `Boolean` whether or not `piece` can take `other_piece`
    - returns: `Boolean`
    - parameters: (1)
      - `other_piece` (`Object`): [Piece Object](#piece-objects)
  - `piece.takes(space, other_piece, is_en_passant)`: `piece` takes `other_piece`
    - returns: `Boolean` (whether or not take was successful)
    - parameters: (3)
      - `space` (`String`): coordinates of capture
      - `other_piece` (`Object`): [Piece Object](#piece-objects)
      - `is_en_passant` (`Boolean`): whether or not capture is [En Passant](http://en.wikipedia.org/wiki/En_passant)
  - `piece.can_move()`: 
    - returns: `Boolean` (whether or not piece can move)
    - parameters: (0)
  - `piece.can_move_to(coordinate)`: 
    - returns: `Boolean` (whether or not piece can move to `coordinate`)
    - parameters: (1)
      - `coordinate` (`String`): `String` coordinates of space (aka `'e1'`)
  - `piece.move_to(coordinate, is_cap, is_en_passant)`: move `piece` to `coordinate`
    - returns: `Boolean` (whether or not move was successful)
    - parameters: (3)
      - `coordinate` (`String`): coordinate to move `piece` to.
      - `is_cap` (`Boolean`): whether or not move is capture.
      - `is_en_passant` (`Boolean`): whether or not capture is [En Passant](http://en.wikipedia.org/wiki/En_passant)
  - `piece.can_castle(side)`:
    - returns: `Boolean` (whether or not `piece` can castle to `side`)
    - parameters: (1)
      - `side` (`String`): side to castle to (either `'kingside'` or `'queenside'`)
  - `piece.can_en_passant()`:
    - returns: `Boolean` (whether or not `piece` can move via [En Passant](http://en.wikipedia.org/wiki/En_passant)
    - parameters: (0)
  - `piece.move_is_en_passant(coordinate)`: test whether move would be [En Passant](http://en.wikipedia.org/wiki/En_passant)
    - returns: `Boolean` (whether or not move is [En Passant](http://en.wikipedia.org/wiki/En_passant))
    - parameters: (1)
      - `coordinate` (`String`): coordinate to test
  - `piece.update_guard()`: update a piece's guarded spaces
    - returns: `undefined`
    - parameters: (0)

### Log Objects

Log Objects are inserted into the `board.log` array of logged actions.

Every log object is an `Array` containing two items:

  - `Array[0]` (`String`): The action in [Algebraic Chess notation](http://en.wikipedia.org/wiki/Algebraic_notation_(chess))
  - `Array[1]` (`Object`): Object with details of the action, containing the following attributes:
    - `type` (`String`): action type (either `'castle'`, `'move'`, or `'capture'`)
    - `from` (`String`): coordinate of the space the piece is *leaving*
    - `to` (`String`): coordinate of the space to which the piece is *arriving*
    - `piece` (`Object`): [Piece Object](#piece-objects) with piece performing action
    - `cap_piece` (`Object`): (only if `type` is `'capture'`) [Piece Object](#piece-objects) that was taken
    - `check` (`Boolean`): whether or not the action put the other army in check
    - `pawn_promotion` (`Boolean`): whether or not the action produced a Pawn Promotion
    - `old_type` (`String`): (only if `pawn_promotion` is `true`) the `type` of the piece that was changed to `piece` in Pawn Promotion


##Contributing

If you find a bug, or would like to help out with development, just follow some simple steps:

  1. [Make an issue.](https://github.com/JoahG/chess.js/issues/new)
  2. [Fork the repo.](https://github.com/JoahG/chess.js/fork)
  3. Make your changes.
  4. Commit and create a pull request.

##Author

chess.js is written and maintained by [Joah Gerstenberg](http://www.joahg.com), copyright 2014. All code contained within these files are licensed under an [MIT license](https://github.com/JoahG/chess.js/blob/master/MIT-LICENSE).

