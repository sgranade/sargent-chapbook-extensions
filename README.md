# Sargent's Chapbook Extensions

A collection of inserts and modifiers for [Chapbook](https://klembot.github.io/chapbook/), a story format for [Twine](https://twinery.org/).


## Summary

The extentions are focused on manipulating text to show later or display something different every time a player visits a passage.

- The `[collect]` modifier saves text to a variable instead of showing it, letting you build up longer descriptions that you can then show using the `{show collected}` insert.
- The `{if [condition]}` insert only shows text if the condition is true.
- The `[first time]` modifier and `{first time}` insert show text one time only. 
- The `{one of}` insert shows different text every time.
- The `{^ [expression]}` insert capitalizes the string value of the variable or expression.

Want to see them in action? [Check out the demo](https://sgranade.github.io/sargent-chapbook-extensions/).


## Installation

> [!WARNING]
>
> Chapbook version 2.0.0 has a bug in how it processes Javascript code. These extensions only work with Chapbook 2.1.0 or higher.

The extensions are defined in the `text-manipulation.twee` file in the `src` directory. If you're writing your game using [Twee](https://dev.to/lazerwalker/a-modern-developer-s-workflow-for-twine-4imp), download that file and add it to your project. Then, in your starting passage, use `{embed passage: '_Add All Text Manipulation'}` to make all of the inserts and modifiers available.

If you're using the Twine visual editor, create a new passage called `_Add All Text Manipulation`. Open the `text-manipulation.twee` file on this site. Copy all of the `[Javascript]` blocks into that new passage. Then, in your starting passage, add `{embed passage: '_Add All Text Manipulation'}`.


## Use

### Text Collection

The `[collect]` modifier collects text to be shown later or manipulated. All text that follows the modifier is saved to be shown later using the insert `{show collected}`.

You can use `[collect]` more than once to keep adding new text to what's already been collected. By default, a single space is added between each new collected text. If you don't want that space, use `[collect no-space]`. If you want a paragraph between them instead, use `[collect new-paragraph]`.

The insert `{show collected}` empties the collection so that you can start over. To show the collected text while keeping it, use the insert `{show collected; keep=true}`. You can also guarantee that you're starting a fresh collection with the modifier `[collect new]`.

Finally, if you want access to the collected text, it lives in the Chapbook variable `__collected`.


### Conditional Insert

The `{if [condition]}` insert is like the `[if]` conditional display modifier, except that it's an insert that you can use in the middle of other text. For example, you might want to include a link to a new passage if the player has a key that will unlock a door.

```
The door is locked. {if hasKey: 'You could try [[unlocking it]].}'
```

You can also show the player something different if the condition is false.

```
The door is locked. {if hasKey: 'You could try [[unlocking it]].', else: 'You need a key.'}
```


### First Time Modifier and Insert

The `[first time]` modifier and `{first time}` insert only show their text the first time. After that, the text isn't shown. The modifier is useful for paragraph-sized portions of text.

```
[first time]
A flashlight rakes across the bushes where you're hidden, but the guard doesn't see you and, satisfied, walks on.
```

The insert is better for smaller snippets of text, or text that appear in the middle of a paragraph or sentence.

```
The floodlight illuminating you is painfully bright. {first time: "You shield your eyes, but can't see who's behind the light."}
```

Note that these aren't tied to the player visiting the passage with the modifier or insert, but to the player being _shown the text_. Most of the time that's a distinction without a difference, except when you use the modifier or insert in a conditonal block.

Let's extend the insert example above.

```
[if floodlightOn]
The floodlight illuminating you is painfully bright. {first time: "You shield your eyes, but can't see who's behind the light."}
```

If the floodlight is off the first time the player visits the passage, the `{first time}` insert isn't triggered. That visit doesn't count as the "first time" as far as the insert is concerned.


### One Of Insert

The `{one of}` insert lets you display varying text each time it's triggered. For example, say you wanted to show different flavor text every time a player entered a cave.

```
The cave is low and dark. {one of: ['Water splashes from the ceiling.', 'From ahead, the squeak of a bat echoes off of the cave walls.', 'Your torch flickers.']}
```

Each time the player visits the passage, one of the choices in the list will be shown at random.

You can control the order that the choices will be shown using the `order` parameter.

```
{one of: ['choice 1', 'choice 2', 'choice 3'], order: 'order'}
```

`order` can be any of the following:

- `'random'`: Show a random choice each visit without showing the same thing twice in a row. The default.
- `'pure random'`: Like random, but choices can be shown twice in a row.
- `'shuffled'`: The choices are shuffled, then shown in that shuffled order. Once all of them have been shown, they’re re-shuffled.
- `'stopping'`: Show the choices in order, stopping with the last one and thereafter showing it forever.
- `'cycling'`: Show the choices in order, repeating the order after the last one’s shown.


### Capitalization Insert

The `{^ [variable]}` insert capitalizes the variable's contents and shows it. For example, say you had variables corresponding to an NPC's pronouns and you wanted to use one at the start of a sentence.

```
{^ companion.his} clothes are folded neatly at the foot of {companion.his} bed.
```

This could display "Their clothes are folded neatly at the foot of their bed" or "His clothes are folded neatly at the foot of his bed."


## Credits

- Thanks to [oddevan](https://github.com/oddevan) for contributing the `[first time]` modifier.