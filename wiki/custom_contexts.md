# Contexts

Every context added by Paperback can be easily accessed through the `context` parameter within your `calculate` function, they will all be located under the `context.paperback` field to avoid any conflicts with other mods.

### Cashing Out

This context is used for effects that happen when the "Cash Out" button is pressed.

```lua
context.paperback = {
  cashing_out = true
}
```

### Using Tag

This context is used for effects before a Tag is activated.

```lua
context.paperback = {
  using_tag = true,
  tag = self -- Tag: the tag that was activated
}
```

### Tag Acquired

This context is used for effects when a Tag is added.

```lua
context.paperback = {
  tag_acquired = true,
  tag = tag, -- Tag: the tag that was acquired
}
```

### Destroying Non-Playing Card

This context is used for effects after a card other than a playing card is destroyed.

```lua
context.paperback = {
  destroying_non_playing_card = true,
  destroyed_card = self -- Card: the card that was destroyed
}
```

### Destroying Joker

This context is used for effects after specifically a joker is destroyed. This is a subset of the above context.

```lua
context.paperback = {
  destroying_non_playing_card = true,
  destroyed_card = self -- Card: the card that was destroyed
  destroying_joker = true,
  destroyed_joker = self -- Card: the joker that was destroyed
}
```

> [!WARNING]
> When using this, ensure that your own joker isn't the one being destroyed: `card ~= context.paperback.destroyed_joker`

### Level Up Hand

This context is used for effects after a hand's level has been increased.

```lua
context.paperback = {
  level_up_hand = true
}
```

### Repetitions from playing card

This context is used to collect repetitions from other playing cards

```lua
context.paperback = {
  repetition_from_playing_card = true,
  other_card = card, -- the card that would be retriggered if at least 1 repetition was returned
  cardarea = card.area, -- the area where the card is
  scoring_hand = context.scoring_hand, -- the scoring hand, if any
}
```

> [!WARNING]
> This is a W.I.P and currently only supports calculating repetitions from Enhancements

### Modify final hand

This context is used for modifying cards in a hand right before they're highlighted. Currently used by Black Rainbows and Bismuth

```lua
context.paperback = {
  modify_final_hand = true,
  scoring_hand = final_scoring_hand,
  full_hand = G.play.cards
}
```

### Drawing cards

Allows adding to the amount of cards drawn at any time.

```lua
context.paperback = {
  drawing_cards = true,
  amount = hand_space, -- integer: the amount of cards drawn initially (does not take into account cards added by this context)
}
```

### Nichola

This context calculates after playing cards are done scoring, but before jokers trigger. This context comes with the usual during-scoring context values, like context.full_hand. Specifically used for Nichola.

```lua
context.paperback = {
  nichola = true
}
```

### Before Joker Effects

This context calculates before jokers trigger. Unlike context.paperback.nichola, this does not pass any info about the current played hand. Used for Journal.

```lua
context.paperback = {
  before_joker_effects = true
}
```

### Sold E.G.O. Gift

This context calculates to trigger an E.G.O. gift's sell effect (when you manually sell it or when the Corroded sticker force-sells it).

```lua
context.paperback = {
  sold_ego_gift = card -- Card: the E.G.O. gift being sold
}
```
