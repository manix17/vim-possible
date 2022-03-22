# Converting a Space (or any character) separated list to other forms

## To Comma separated list

### Input text

> one two three four five six seven eight nine

### Output text

> one, two, three, four, five, six, seven, eight, nine

### If you are running vim, you can use the following code-block to test your approach

``` plain

one two three four five six seven eight nine

```

## Solution 1 (Using find command)

---

`0f<space>cw,<space><Esc>;.;,;.`

### Explanation - Solution 1

`0` Go to the first character in the line, can be replaced with `^` to go to the first non-empty character.

`f<space>` search for space (in the line) in forward direction and jump on it.

`cw,<space><esc>` delete the word and jump to the --INSERT-- Mode. Type `,` and `<space>` character. Jump back to the --NORMAL-- mode.

`;` Repeat the `f` command in forward direction, moves the cursor to the next occurrence of space. use `,` to go backwards.

`.` Repeat the last operation, in this case `cw, <esc>` command which change the the " " to ", "

`;.` Keep replacing the next space characters.

> Remember you don't need to remember this command, you just need to know the building blocks which makes this command, after some practice you can create/write command like this on-the-fly. If you are new to vim I highly recommend you to learn the basics first and then come here. ��

## Solution 2 (Using substitute command)

---

`:s/\s/,<space>/g`

### Explanation - Solution 2

Command Format: `:[range]s/{pattern}/{string}/[flags] [count]`

`:s` substitution command on the line where cursor is at, use `:%s` to search in entire file.
`/` separator
`\s` pattern, matches 1 empty character, use `\s+` to match more than 1 empty character.
`,<space>` string, character which will be replaced with.
`g` flag, global flag to replace all occurrence in the line.

> Tip 1: Learning a little bit of regex (Regular Expressions) can help a lot with substitution command.

> Tip 2 : Substitute or Search command uses search highlighting to show the matching results. To keep showing the search highlights even after the search use `:set hlsearch` command. To toggle search highlighting use `:set hlsearch!`, to disable it completely use `:set nohlsearch`, to clear the last search highlights without disabling search highlighting use `:noh`

## Solution 3 (Using a Macro)

---

> use Macro when normal solution is not possible

`0qrf<space>i,<esc>wq100@r`

### Explanation - Solution 3

`0` Go to the first character in the line.

`qr` start recording a macro `@r` in register r.

`f<space>` jump to next occurrence of space in that line.

`i,<space><esc>` go to --INSERT-- mode, type ", " and jump back to --NORMAL-- mode.

`w` move to the next word, so that the macro is not applied to the same space character again & again.

`q` stop recording the macro

`100@r` execute the macro 100 times(It will replace 100 space characters).

> It'll need some practice to become better at using macro.

> Tip: If you frequently do the same text transformation, you can define a function instead of using the macro.

## To other different formats

---

1. Converting to quoted & comma separated

    > 'one', 'two', 'three', 'four', 'five', 'six', 'seven', 'eight', 'nine'

    Substitution: `I'<esc>:s/\s+/', '/g<enter>A'<esc>`

    Macro: `I'<esc>qqf<space>cw', '<esc>q100@qA'<esc>`

    > This is useful to use in an array or in SQL query.

2. Splitting into words separated by new line
    
    ### Output

    ``` plain
    one
    two
    three
    four
    five
    six
    seven
    eight
    nine
    ```

    `0qsf<space>r<enter>q100@s`

3. Joining newline delimited list of words into a single line

    ### Input

    ``` plain
    The
    quick
    brown
    fox
    jumps
    over
    the
    lazy
    dog.
    ```

    ### Output

    >  The quick brown fox jumps over the lazy dog.

    `J` This key allows to combine the just below line into the current line, you can use `.` after the `J` to continue combining the lines.

    `V/dog:s/\n\s+/ /g` Substitution

    `0qjJq7@j` Merge below 8 lines into a single with macro.

    > Tip: you can view the value stored in your registers using the `:reg` command.
