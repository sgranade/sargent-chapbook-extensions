# Sargent's Chapbook Extensions

A collection of inserts and modifiers for [Chapbook](https://klembot.github.io/chapbook/), a story format for [Twine](https://twinery.org/).


## Summary

The extentions are focused on manipulating text to show later or display something different every time a player visits a passage.

- The `[collect]` modifier saves text to a variable instead of showing it, letting you build up longer descriptions that you can then show using the `{show collected}` insert.
- The `{if [condition]}` insert only shows text if the condition is true.
- The `{first time}` insert shows text only the first time a player visits the passage. 
- The `{one of}` insert shows a different text every time the player visits the passage.

Want to see them in action? [Check out the demo](https://sgranade.github.io/sargent-chapbook-extensions/).


## Installation

> [!WARNING]
>
> Chapbook version 2.0.0 has a bug in how it processes Javascript code. It's been fixed, but until the next version is released, you'll need to build the story format [from its source code](https://github.com/klembot/chapbook) to use these extensions.

The extensions are defined in the `text-manipulation.twee` file in the `src` directory. If you're writing your game using [Twee](https://dev.to/lazerwalker/a-modern-developer-s-workflow-for-twine-4imp), download that file and add it to your project. Then, in your starting passage, use `{embed passage: '_Add All Text Manipulation'}` to make all of the inserts and modifiers available.

If you're using the Twine visual editor, create a new passage called `_Add All Text Manipulation`. Open the `text-manipulation.twee` file on this site. Copy all of the `[Javascript]` blocks into that new passage. Then, in your starting passage, add `{embed passage: '_Add All Text Manipulation'}`.


## Use

### Text Collection

The `[collect]` modifier collects text to be shown later or manipulated. All text that follows the modifier is saved to be shown later using the insert `{show collected}`.

You can use `[collect]` more than once to keep adding new text to what's already been collected. By default, a space is added between each new collected text. If you don't want that space, use `[collect no-space]`.

The insert `{show collected}` empties the collection so that you can start over. To show the collected text while keeping it, use the insert `{show collected; keep=true}`. You can also guarantee that you're starting a fresh collection with the modifier `[collect new]`.

Finally, if you want access to the collected text, it lives in the Chapbook variable `__collected`.


### Conditional Insert

The `{if [condition]}` insert is like the `[if]` conditional display modifier, except that it's an insert that you can use in the middle of other text. For example, you might want to include a link to a new passage if the player has a key that will unlock a door.

```
The door is locked. {if hasKey: 'You could try [[unlocking it]].'
```

You can also show the player something different if the condition is false.

```
The door is locked. {if hasKey: 'You could try [[unlocking it]].', else: 'You need a key.'}
```


### First Time Insert

The `{first time}` insert shows text only the first time a player visits the passage. After that, the text isn't shown.

```
The floodlight illuminating you is painfully bright. {first time: "You shield your eyes, but can't see who's behind the light."}
```


### One Of Insert

The `{one of}` insert lets you display varying text each time a player reaches a passage. For example, say you wanted to show different flavor text every time a player entered a cave.

```
The cave is low and dark. {one of: ['Water splashes from the ceiling.', 'From ahead, a bat's squeak echoes off of the cave walls.', 'Your torch flickers.']}
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