# Vim Guide

This is a quick summary and reference of helpful knowledge and my favorite tips in Vim that I've acquired over the years. This is not meant to be a thorough Vim tutorial. That is already provided at the operating system command prompt by typing `$ vimtutor`. I'll update and improve it from time to time, so feel free to bookmark, suggest improvements, and check back in the future!

## Table of Contents

* [Why another Vim tutorial?](#why-another-vim-tutorial)
* [Section 0: The legend](#section-0-the-legend)
* [Section 1: Working with files](#section-1-working-with-files)
* [Section 2: Modes, motions, and getting around](#section-2-modes-motions-and-getting-around)
* [Section 3: Speaking the "Vim" language](#section-3-speaking-the-vim-language)
    * [Verbs](#verbs)
    * [Nouns](#nouns)
    * [Modifiers](#modifiers)
    * [Some examples](#some-examples)
* [Section 4: Efficient movement](#section-4-efficient-movement)
* [Section 5: Efficient editing](#section-5-efficient-editing)
* [Section 6: Copying and pasting](#section-6-copying-and-pasting)
* [Section 7: Macros](#section-7-macros)
* [Section 8: Working with multiple windows](#section-8-working-with-multiple-windows)
    * [Splitting](#splitting)
    * [Shortcuts](#shortcuts)
* [Section 9: Working with many files in Vim](#section-9-working-with-many-files-in-vim)
* [Section 10: Some handy tips](#section-10-some-handy-tips)
* [Questions, comments, corrections, contributions](#questions-comments-corrections-contributions)

---

## Why another Vim tutorial?

<sup>_[back to top](#table-of-contents)_</sup>

After using Emacs for a couple of years, I decided to give [Vim](http://www.vim.org/) a try. As is the case with most editors I try, it took a few cycles of attempting to use it as my main editor, giving up, and then coming back and trying again. Approximately 8 cycles in, it finally stuck.

Once I had grasped the philsophy, or "idea", behind how Vim operates, what initially felt like nonsensical keyboard shortcuts and bindings made perfect sense. While my speed and proficiency in Emacs is due in large part to simple muscle memory, my proficiency in Vim is because of thinking in the "language" of the editor: Text objects are nouns, and actions are verbs. You can perform most verbs on most nouns. This concept alone allows you to perform complex actions by combining simple ones, and this means that even complex actions are intiutive.

Even so, there's a lot to learn, especially if you're starting a journey towards making Vim your [One True Editor](http://pragmatictips.com/22). Most of the time, when you're getting started, you really just want to know how to do specific things with the editor. And if you're coming from another editor, you'll probably wonder how to do things in Vim that the other editor could do.

That's what this guide is for. Not a lot of glam, but hopefully a lot of bang for your buck.

---

## Section 0: The legend

<sup>_[back to top](#table-of-contents)_</sup>

In this document, I'll have to convey some complex things. Here's how I'll represent them:

* Lines that begin with a colon mean that you type them at the Vim command line while in normal mode. Since the <kbd>:</kbd> key engages the Vim command line, these lines are simply meant to be typed exactly as listed.

    Example:

        :e ~/.vimrc

* Keypresses will be indicated like this: <kbd>p</kbd>, which means to press the <kbd>p</kbd> key.

* Two key presses in a row (for example, <kbd>y</kbd> <kbd>y</kbd>) means that you press and release each key in sequence quickly, one right after another. <kbd>y</kbd> <kbd>y</kbd> means to press and release the <kbd>y</kbd> key twice in rapid succession.

* Two key presses separated by a hyphen (for example, <kbd>Shift</kbd>-<kbd>y</kbd>) means that you press and hold each key in sequence. <kbd>Shift</kbd>-<kbd>y</kbd> means to press <kbd>Shift</kbd> and then press <kbd>y</kbd> while still holding down the <kbd>Shift</kbd> key.

* Pay attention to the capitalization of a key: <kbd>y</kbd> is different from <kbd>Y</kbd>. The first one means to simply press the <kbd>y</kbd> key, but the second one means <kbd>Shift</kbd>-<kbd>y</kbd>.

    _(Why not just use "shift-(something)" in all circumstances? Sometimes it helps alleviate confusion with sequences of "shift-key" combinations, like <kbd>Z</kbd> <kbd>Q</kbd>, which means to hold down shift while pressing and releasing <kbd>z</kbd> and <kbd>q</kbd> in sequence.)_

+ <kbd>g</kbd> <kbd>w</kbd> means to quickly press and release the <kbd>g</kbd> key followed immediately by pressing and releasing the <kbd>w</kbd> key.

---

## Section 1: Working with files

<sup>_[back to top](#table-of-contents)_</sup>

The first thing you'll want to know is how to open, save, and close files, and then [get out of the editor](https://twitter.com/iamdevloper/status/435555976687923200).

+ `:e README.md` to open README.md in the current directory. (File will be created if it doesn't already exist)
+ Can open interactively if you supply a directory name:

    `:e .` to start exploring the current directory.

+ `:w` to write (save) the file.
+ `:w <filename>` the equivalent of "Save As" (but keep working on the current file!)
+ `:sav <filename>` the equivalent of "Save As" (and switch to the new file)
+ `:q` to quit the current buffer, `:q!` to force-quit, discarding changes
+ Pressing <kbd>Z</kbd> <kbd>Q</kbd> in normal mode is the same as typing `:q!`
+ `:qa` to quit ALL buffers, `:qa!` to force-quit ALL buffers, discarding changes!
+ To actually remove a buffer from Vim, use `:bd` (this doesn't delete it from the filesystem though)

(More on working with multiple files at once in the **Working with many files in Vim** section!)

---

## Section 2: Modes, motions, and getting around

<sup>_[back to top](#table-of-contents)_</sup>

Being proficient with Vim means thinking about editing text in a new way. Vim is a modal editor, which means that the keys you press will do different things depending on which mode you're in.

There are 2 basic modes that you will encounter: **Normal mode** and **Insert mode**. Normal mode is aimed primarily at navigating text. Insert mode is for typing text. You should think of normal mode as the default mode.

> #### Analogy: Driving a car
> Let's pretend you are a landscaper in charge of maintaining a neighborhood. Sometimes you need to do things like transplant a tree, plant some flowers, or clear out some brush. Your car is super efficient and fast at getting around, but if you need to actually remove, change, or add something to the landscape, you have to get out of your car. Once you're done, you get back in your car, and move on. In Vim, normal mode is like driving around in your car, and insert mode is like getting out of the car at a location to make a change. You would never park your car, make a change, and then walk 10 blocks away to make another change when your car is much more efficient. Similarly, don't try to move around in insert mode using the arrow (or other directional keys). Your car (normal mode) is much faster!

The takeaway here is that you want to spend most of your time in normal mode, dropping into insert mode only when you need to input text. In general, we spend most of our time navigating around and manipulating (deleting, copying pasting, shifting, etc) text, and not typing it.

> #### Tip: Avoiding the Esc key
> The <kbd>Esc</kbd> key will always exit insert or visual mode and return you to normal mode. Reaching up to press <kbd>Esc</kbd> all the time really sucks, though. You can press <kbd>Ctrl</kbd>-<kbd>[</kbd>, which is the same thing on U.S. keyboards. <kbd>Ctrl</kbd>-<kbd>c</kbd> also works, and is my preferred way of doing it. There is one instance that I know of where <kbd>Ctrl</kbd>-<kbd>c</kbd> is not equivalent to pressing <kbd>Esc</kbd>. For more on why, see this [Vim Gotcha](#vim-gotcha-differences-between-ctrl-c-and-esc) below.

---

## Section 3: Speaking the "Vim" language

<sup>_[back to top](#table-of-contents)_</sup>

Probably the most helpful method for remembering how to do some of the seemingly non-intuitive (at least for users of modern-day text editors) keystrokes that you'll be using in Vim is to understand _why_ you're doing them. If you understand the philosophy by which Vim operates, suddenly you'll find that you're not simply memorizing a series of cryptic key-presses, but putting together simple concepts and principles to accomplish complex and powerful results.

To understand Vim's philosophy, let's look at the Vim "language".

### Verbs:

<sup>_[back to top](#table-of-contents)_</sup>


A _verb_ is something that you can _do_ to text, such as changing, selecting, replacing, or yanking.


> #### Tip: Repeat keypress to apply to a line
> Often times, for verbs that await a modifier/noun after pressing the verb key, you can apply the verb to the current line by simply pressing the verb key again.

+ Visual
    + By character: <kbd>v</kbd>
    + By line: <kbd>Shift</kbd>-<kbd>v</kbd>
    + By rectangle (aka by column): <kbd>Ctrl</kbd>-<kbd>v</kbd>
+ Change
    + With modifier and noun: <kbd>c</kbd>, `<modifier>`, `<noun>`
    + From cursor to line end: <kbd>Shift</kbd>-<kbd>c</kbd>
+ Delete
    + From cursor to end of the current word: <kbd>d</kbd> <kbd>w</kbd>
    + With modifier and noun: <kbd>d</kbd>, `<modifier>`, `<noun>`
    + From cursor to line end: <kbd>Shift</kbd>-<kbd>d</kbd>
    + Entire line: <kbd>d</kbd> <kbd>d</kbd>
+ Yank (Copy)
    + With modifier and noun: <kbd>y</kbd>, `<modifier>`, `<noun>`
    + Entire line: <kbd>Shift</kbd>-<kbd>y</kbd> (or <kbd>y</kbd> <kbd>y</kbd>)

### Nouns:

<sup>_[back to top](#table-of-contents)_</sup>

A _noun_ is some subset of the text which Vim can recognize and understand.

+ Word (a combination of letter(s), number(s), and/or underscore(s) in sequence): <kbd>w</kbd>
+ Word, including surrounding space: <kbd>W</kbd>
+ Paragraph: <kbd>p</kbd>
+ Sentence: <kbd>s</kbd>
+ Block: <kbd>b</kbd>
+ Tag: <kbd>t</kbd> (An HTML tag)
+ Angle brackets: <kbd><</kbd> and <kbd>></kbd>
+ Square brackets: <kbd>[</kbd> and <kbd>]</kbd>
+ Parentheses: <kbd>(</kbd> and <kbd>)</kbd>

### Modifiers:

<sup>_[back to top](#table-of-contents)_</sup>

These are used when applying certain verbs to a noun, in order to specify more advanced or complex behavior. For example, if we wanted to delete a word, we could use <kbd>d</kbd> <kbd>w</kbd> with the cursor placed on the first character of the word. What if we're not on the first character of the word, but somewhere in the middle? This is when a modifier would come in handy. We could type <kbd>d</kbd> <kbd>i</kbd> <kbd>w</kbd> to "delete inside word", and no matter which character in the word our cursor was on, we would delete the entire word.

Some other modifiers are:

+ In: <kbd>i</kbd>
+ Around (includes the whitespace character on either side of the noun being operated on): <kbd>a</kbd>
+ Till: <kbd>t</kbd>
+ Find: <kbd>f</kbd>
+ Search: <kbd>/</kbd>

### Some examples:

<sup>_[back to top](#table-of-contents)_</sup>

<kbd>c</kbd> <kbd>i</kbd> <kbd>w</kbd>: "Change inside word". Change the word at the cursor (deletes the entire word, and places you in insert mode at the beginning of where the word used to be).

<kbd>v</kbd> <kbd>i</kbd> <kbd>)</kbd>: "Visual inside parentheses". Select (visual mode) all text _inside_ the inner-most enclosing parentheses that contain the cursor.

<kbd>v</kbd> <kbd>a</kbd> <kbd>)</kbd>: "Visual around parentheses". Select (visual mode) all text _inside_ the inner-most enclosing parentheses, _including the parentheses themselves_, that contain the cursor.

<kbd>d</kbd> <kbd>t</kbd> <kbd>s</kbd>: "Delete to s". Deletes all text from the current position up to (but NOT including) the next occurrence of the character "s" _in the current line_.

<kbd>d</kbd> <kbd>f</kbd> <kbd>s</kbd>: "Delete find s". Deletes all text from the current position up to (INCLUDING) the next occurrence of the character "s" _in the current line_.

<kbd>c</kbd> <kbd>/</kbd> `<search string>` `Return`: "Change up to `<search string>`". Change all text from the current position all the way up to (but NOT including) the next occurrence of the search string.

---

## Section 4: Efficient movement

<sup>_[back to top](#table-of-contents)_</sup>

Now that you know how to perform some sweet verbs on those nouns, let's make sure you're doing the elementary movements as efficiently as possible!

+ Move the cursor using the <kbd>H</kbd> <kbd>J</kbd> <kbd>K</kbd> <kbd>L</kbd> keys.
    + Fast.
    + You don't have to take your fingers off the home row.
    + This is hard to get used to, but it seriously pays off later!
+ <kbd>^</kbd> to go to the beginning of the text of a line.
+ <kbd>0</kbd> to go to the HARD beginning of the line.
+ <kbd>$</kbd> to go to the end of a line.
+ <kbd>{</kbd> and <kbd>}</kbd> to move up and down by blank space.
+ Go to the top of the file with <kbd>g</kbd> <kbd>g</kbd>.
+ Go to the bottom of the file with <kbd>G</kbd>.
+ Going to a line (in this case, 13) with `:13`, or <kbd>1</kbd> <kbd>3</kbd> followed by either <kbd>g</kbd> <kbd>g</kbd> or <kbd>G</kbd>.
+ <kbd>z</kbd> <kbd>z</kbd> to center the current line on the screen (type a line number to perform this action for for that line number instead of the current line).
+ <kbd>z</kbd> <kbd>t</kbd> to recenter the view so the current line is at the top (type a line number to perform this action for for that line number instead of the current line).
+ <kbd>z</kbd> <kbd>b</kbd> to recenter the view so the current line is at the bottom (type a line number to perform this action for for that line number instead of the current line).

---

## Section 5: Efficient editing

<sup>_[back to top](#table-of-contents)_</sup>

Here are some tips and new ways to perform common editing tasks efficiently.

+ In insert mode, you can delete backwards word with <kbd>Ctrl</kbd>-<kbd>w</kbd>.
+ Search with <kbd>/</kbd> to get around, <kbd>n</kbd> and <kbd>N</kbd> for next and previous match.
    + Often the quickest way to get to a specific position in the file.
    + Enable incremental searching by adding `set incsearch` to your **.vimrc** file!
+ Search then change in word, around tag, etc.
    + Trying to change some text in an HTML file that's sitting inside that **&lt;strong&gt;** tag over there?
    + Search for part of text to get your cursor inside the **&lt;strong&gt;** tag.
    + Type <kbd>c</kbd> <kbd>i</kbd> <kbd>t</kbd> to delete the text inside the tag, and place the cursor in insert mode!
+ Repeat action - the <kbd>.</kbd> key.
+ <kbd>I</kbd> goes to the beginning of a line and switches to Insert mode.
+ <kbd>A</kbd> goes to the end of a line and switches to insert mode.
+ <kbd>r</kbd> to replace current character only and return to normal mode after typing a single replacement character.
+ <kbd>s</kbd> to replace current character and stay in insert mode until you manually exit it.
+ <kbd>S</kbd> or <kbd>c</kbd> <kbd>c</kbd> to replace the entire current line.
+ <kbd>D</kbd> deletes to the end of a line.
+ <kbd>C</kbd> changes to the end of a line.

---

## Section 6: Copying and pasting

<sup>_[back to top](#table-of-contents)_</sup>

Copying and pasting is done a little differently in Vim than in most modern editors. Understanding the registers and learning to use them to your advantage can let you do some really cool things in Vim.

+ <kbd>Y</kbd> to copy an entire line.
+ <kbd>p</kbd> to paste after the current cursor position. If you have an entire line copied, this pastes AFTER the current line.
+ <kbd>P</kbd> to paste before the current cursor position. If you have an entire line copied, this pastes BEFORE the current line.
+ Use `:reg` to see what is currently in your registers.
+ <kbd>0</kbd> is the default register. Copying text makes it go here.
+ <kbd>"</kbd> is the quickchange register. When you delete or change text, it goes here.

> #### Tip: Pasting in insert mode
> If you need to paste in insert mode (or at the command line/search prompt), press <kbd>Ctrl</kbd>-<kbd>r</kbd>. You will see a <kbd>"</kbd> appear at the cursor position. This indicates that Vim is waiting for you to specify the register to paste from. <kbd>0</kbd> is the default register, and will have your most recently-copied text.

---

## Section 7: Macros

<sup>_[back to top](#table-of-contents)_</sup>

Sometimes you need to repeat an action that is too complex for the <kbd>.</kbd> key. This is where macros come in.

* Press <kbd>q</kbd> then the key for the register you want to store in.
* Press <kbd>q</kbd> again to stop recording.
* To play back, use <kbd>@</kbd> followed by the key of the register you stored your macro in.
* You can run a macro multiple times: <kbd>1</kbd><kbd>0</kbd>@<kbd>a</kbd> to run the macro stored in the `a` register 10 times.

---

## Section 8: Working with multiple windows

<sup>_[back to top](#table-of-contents)_</sup>

### Splitting:

<sup>_[back to top](#table-of-contents)_</sup>

`:vs` - Open a vertical split showing whatever buffer the current window is showing.

`:vnew` - Open a vertical split beside the current window with a new empty buffer.

`:sp` - Open a horizontal split showing whatever buffer the current window is showing.

`:only` or `:on` - Closes all window splits except for the one the cursor is in.


### Shortcuts:

<sup>_[back to top](#table-of-contents)_</sup>

<kbd>Ctrl</kbd>-<kbd>w</kbd> followed by:

* <kbd>w</kbd> - Next window.
* <kbd>v</kbd> - Vertical split with current file.
* <kbd>s</kbd> - Horizontal split with current file.
* <kbd>h</kbd> <kbd>j</kbd> <kbd>k</kbd> or <kbd>l</kbd> - Go to the window to the left, below, above, or the right, respectively (same directions as cursor movement).
* <kbd>q</kbd> - Close the current window (like typing `:q`).
* <kbd><</kbd> or <kbd>></kbd> - Increase or decrease (respectively) the width of the current window split.
* <kbd>-</kbd> or <kbd>+</kbd> - Increase or decrease (respectively) the height of the current window split.
* <kbd>=</kbd> - Automatically re-size (re-arrange) your window splits. Useful for if your vertical split is not evenly-sized after dragging to resize your Vim window!

---

## Section 9: Working with many files in Vim

<sup>_[back to top](#table-of-contents)_</sup>

Some people say that

+ I use tabs. I have the following set in my **.vimrc** file in order to always show the tab bar:

    `set showtabline=2`

+ `:tabe <filename>` is like `:e <filename>`, but it opens the file/directory explorer in a new tab.
+ `:tabnew` opens a blank new tab.
+ While in explore mode, you can hit <kbd>Shift</kbd>-<kbd>t</kbd> to open the file in a new tab!
+ Got many files open, but not in tabs? Use `:tab ba` to make each buffer be a tab!
+ <kbd>g</kbd> <kbd>t</kbd> goes to the next tab.
+ <kbd>g</kbd> <kbd>T</kbd> goes to the previous tab.
+ I remapped <kbd>Shift</kbd>-<kbd>h</kbd> in normal mode to go to previous tab.
+ I remapped <kbd>Shift</kbd>-<kbd>l</kbd> in normal mode to go to next tab.
+ `:tabdo` allows you to perform an action to all open tabs. For example, `:tabdo %s/foo/bar/g` will replace all occurrences of the word foo with the word bar, in all open tabs.

---

## Section 10: Some handy tips

<sup>_[back to top](#table-of-contents)_</sup>

+ **Block commenting**:

    Vim doesn't have a built-in block commenting feature, but here's how I comment multiple lines (it's wordy, but it's not that complicated once you get used to it... I promise):

    1. Place your cursor at the beginning of the first line in the block to comment. This can be done by pressing <kbd>^</kbd> if your cursor is already on the correct line. Invoking the motion to go to a specific line number will automatically put the cursor at the beginning of that line.

    1. Press <kbd>Ctrl</kbd>-<kbd>v</kbd> to enter visual mode by rectangle/column.

    1. Go to the last line in the block to comment.

    1. Press <kbd>Shift</kbd>-<kbd>i</kbd> to place yourself in insert mode at the beginning of the first line only.

    1. Type the appropriate comment character(s) (for example the # character, for Python). I also like to add a space after the comment character, but that's a personal preference.

    1. Press <kbd>Esc</kbd> or <kbd>Ctrl</kbd>-<kbd>[</kbd> (NOT <kbd>Ctrl</kbd>-<kbd>c</kbd>!) to exit insert mode and replicate the inserted character across all rows that you previously selected.

> #### Vim Gotcha: Differences between Ctrl-c and Esc
> Why doesn't <kbd>Ctrl</kbd>-<kbd>c</kbd> always do the same thing as <kbd>Esc</kbd>? In Vim, <kbd>Ctrl</kbd>-<kbd>c</kbd> means to stop the thing you're currently doing. In insert mode, this means you stop inserting text and return to command mode. It also means that, unlike <kbd>Esc</kbd> (or the equivalent <kbd>Ctrl</kbd>-<kbd>[</kbd>),  it won't check to see if you've entered any abbreviations, or autocmds. Vim simply assumes you want to stop whatever you were doing (in this case, inserting text at the beginning of multiple lines), and go back to normal mode.

+ **Diff two files**:

    1. Open the two files to be diff'd in a vertical split.

    1. `:windo diffthis`

    1. to exit the diff, `:windo d¿iffoff`

+ **Setting Vim as your difftool**:

    In your `.gitconfig` file:

        [diff]
            tool = vimdiff

+ **Wrap text**:
    + To wrap a line, type <kbd>g</kbd> <kbd>w</kbd> <kbd>w</kbd> with your cursor on the line to be wrapped.
    + This will wrap the line at whatever your `textwidth` parameter is, inserting newlines as needed.
    + For example, to configure Vim to wrap at 80 characters, use `:set textwidth=80`

+ **Pasting into Vim without messing up the indentation of the copied text**:

    1. In normal mode, `:set paste` to temporarily turn off auto- and smart-indenting functionality in Vim.

    1. Paste your text.

    1. Make sure to use `:set nopaste` after you've finished pasting in order to turn the auto- and smart-indenting functionality back on.

---

## Questions, comments, corrections, contributions

<sup>_[back to top](#table-of-contents)_</sup>

This guide is a perpetual work-in-progress as I continue to learn and become more familiar with Vim. I'll be cleaning it up, adding more detail, and just making general improvements as time goes on.

If you have any questions or comments, corrections to anything I've written, or contributions you want to add, hit me up on Twitter at [@wahoowilky](http://twitter.com/wahoowilky), or just submit a pull request!
