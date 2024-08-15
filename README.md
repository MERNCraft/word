# Find the Longest Word #

[Demo](https://MERNCraft.github.io/word)

You have 100 seconds to find the longest word!

This CSS-only game uses one immensely long nested CSS ruleset that starts like this:

```css
main {
  &:has(.layer-1 .a :checked) {
    p#word span:nth-child(4)::after {
      counter-set: c4 1;
      width: var(--size);
      border: var(--border);
    }

    [for=undo-1] {
      left: var(--left4);
    }
    /* hundreds of lines skipped */
  }
}
```

There are 6 nested `<div>`s with classes from `layer-6` down to `layer-1`. In each `<div>` is a set of radio buttons which share the same name. The first button in each set is checked by default. Its `id` is of the form `undo-X` where `X` is the number of the layer.

The other buttons in the set are wrapped in separate labels, each with a `<span>` with a different letter: A, I, N, R, S or T. When you select a radio button for a given letter, all radio buttons associated with that letter in all the layer `<div>`s are hidden, and so is the current layer `<div>`, so:

* You can only choose each letter once
* You cannot select a different letter in the same radio button set.

The CSS extract above shows what happens when you select the letter `A` as your first letter. 

Selected letters are added to the `::after` pseudo-element of a `<span>` in the element `<p id="word">`. This `<p>` element contains seven spans, and the central one is chosen for the first letter, because the different words that you can create can have three letters before or three letters after the `A`.

An `::after` pseudo-element can set its `content` to a hard-coded string, a custom CSS property or a CSS `counter`. There are six counters, named `c1` to `c6`. Each of them uses a `@counter-style` called `letters` with the hard-coded characters `"A" "I" "N" "R" "S" "T"`.

The rule `counter-set: c4 1;` tells the CSS to set the value of the `c4` counter to `1`. The rule for the `p#word span nth-child(4)::after` pseudo-elemnt, tells it to display the `c4` counter using the `letters` counter style. That is: it should show the letter `A`. And it should draw a white border around it.

```css
p#word span {
  text-align: center;

  &::after {
    display: inline-block;
    box-sizing: border-box;
  }
  
  /* lines skipped */

  &:nth-child(4)::after {
    content: counter(c4, letters);
  }
  
  /* more lines skipped */
}
```

A label with hard-coded co-ordinates is placed over the newly populated `::after` element. This label is connected to the `undo-X` radio button in the layer that defines the letter that has just been placed. Clicking on this label deselects the letter, and allows you to move one step back, so that you can try a different path going forward.

All colors and dimensions are stored in custom CSS properties at the beginning of the CSS file, so that it is easy to make adjustments during development.

The word "STRAIN" was chosen for this demo, because it can be built up from a single letter, one word at a time. It is possible to reach the same five-letter word "RANTS" by following six different paths:

1. A   AN   RAN   RANT   RANTS
2. A   AN   ANT   RANT   RANTS
3. A   AN   ANT   ANTS   RANTS
4. A   AT   RAT   RANT   RANTS
5. A   AT   ANT   RANT   RANTS
6. A   AT   ANT   ANTS   RANTS

Each of these paths requires its own specific CSS code.

There are two possible 6-letter words: TRAINS and STRAIN.

This seems a good metaphor for writing code: There are many ways to achieve the same result, and there are many blind alleys that you need to follow before you can find the most satisfying solution.

Writing this game in CSS only is not an efficient or robust process. To change the winning word (to TINGLES or HONESTLY for instance) would require rewriting hundreds of lines of a single CSS ruleset. The same game written in JavaScript would allow the game to be played with many different words, using a single code base.

However, using CSS in ways for which it was never intended reminds you that problems in other domains that might at first seem impossible to solve can be [treated with careful thought and just the tools that you have to hand](https://www.nasa.gov/history/50-years-ago-houston-weve-had-a-problem/#:~:text=a%20procedure%20to%20solve%20this%20%E2%80%9Csquare%20peg%20in%20a%20round%20hole%E2%80%9D%20problem.).

 