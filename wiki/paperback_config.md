# Paperback Config

Certain objects in our mod can include a `paperback` table within its definition, this is used to provide certain effects and avoid repetition.

## Centers and Tags

```lua
paperback = {
  requires_custom_suits = true, -- Makes this object only available in pool if Custom Suits are enabled in config
  requires_crowns = true, -- Makes this object require at least one card to be a Crown to be in the pool (ignores wild cards)
  requires_stars = true, -- Makes this object require at least one card to be a Star to be in the pool (ignores wild cards)
  requires_spectrum_or_suit = true, -- Makes this object require a Spectrum to be played (allows wild cards) or at least one card to be any modded suit (ignores wild cards)
  requires_enhancements = true, -- Makes this object only available in pool if Enhancements are enabled in config
  requires_paperclips = true, -- Makes this object only available in pool if Paperclips are enabled in config
  requires_minor_arcana = true, -- Makes this object only available in pool if Minor Arcana are enabled in config
  requires_tags = true, -- Makes this object only available in pool if Tags are enabled in config
  requires_editions = true, -- Makes this object only available in pool if Editions are enabled in config
  requires_ranks = true -- Makes this object only available in pool if Ranks are enabled in config
  requires_ego_gifts = true -- Makes this object only available in pool if E.G.O. Gifts are enabled in config
}
```

> [!NOTE]
> `requires_crowns` and `requires_stars` automatically set `requires_custom_suits = true`

## Jokers

```lua
paperback = {
  ignores_the_world = true, -- Whether this joker ignores the effect applied by The World joker, used by B-Soda
}
```

## Back

```lua
paperback = {
  create_crowns = true, -- Makes this Back create a full suit of Crowns
  create_stars = true, -- Makes this Back create a full suit of Stars
}
```

## Extra Button

Allows cards to contain an additional button located on the middle left of the card when highlighted.

```lua
paperback = {
  extra_button = {
    text = 'b_use', -- The localization key for the button text. This key should be in `misc.dictionary` within the localization file.
    colour = G.C.WHITE, -- Background color of the button, default is G.C.PAPERBACK_MAIN_COLOR
    text_colour = G.C.BLACK, -- Text color of the button, default is G.C.UI.TEXT_LIGHT

    -- Run when the button is clicked. `self` refers to the table `extra_button` and `card` is the card this button is attached to.
    click = function(self, card)
      SMODS.calculate_effect({ message = "You're rich!" }, card) -- shows a message when clicked
    end,

    -- Must return a boolean value. Decides whether the button can be used, if this returns false,
    -- the button will be greyed out and the click function will not run when clicked.
    -- NOTE: This function is called every frame that the button UI element exists on screen.
    can_use = function(self, card)
      return G.GAME.dollars > 10 -- this button can only be used when you have more than 10 dollars
    end,

    -- Must return a boolean value. Decides whether the button will appear on a specific card.
    -- It will appear on any highlighted card by default, any additional conditions can be specified here.
    should_show = function(self, card)
      return card.area == G.jokers -- this button will only show up on highlighted cards in the Jokers area
    end
  }
}
```

### Dynamically changing text and colors

At any point in time, you can change what localization key, background color or text color the extra button will use.

For text, it's as simple as modifying the value to a different localization key. For example, within the button click function.

```lua
click = function(self, card)
  self.text = 'another_loc_key'
end
```

As for colors it's not as simple, first you need to assign the button a copy of the color, rather than a direct reference to one:

```lua
extra_button = {
  colour = copy_table(G.C.WHITE)
}
```

And then, to modify it you need to individually change each of the RGBA values:

```lua
click = function(self, card)
  self.colour[1] = 0 -- red
  self.colour[2] = 0 -- green
  self.colour[3] = 0 -- blue
  self.colour[4] = 1 -- alpha
end
```

> [!NOTE]
> Changing these values does not necessarily have to be done within the button functions, as long as you have access to the center you can modify the config values at any time.
