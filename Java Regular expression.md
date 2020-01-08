# java Regular Expression

[freeformatter.com/java-regx-tester.html](https://www.freeformatter.com/java-regex-tester.html#ad-output)


## Meta character
| Character   | What does it do ? |
| ----------  | :---------------- |
| $           | Matches __end of input line__ .if in multiline mode, it also matches __before a line break character__, hence every end end of line |
| (x)         | Match 'x' and remember the match. also known as capturing parenthesis |
| (?:x)       | matches 'x' but __does Not remember the match__. Also known as NON-capturing parenthesis |
| x(?=y)      | Matches 'x' __only if 'x' is followed by 'y'__ . Also known as __lookahead__. |
| x(?!y)      | Match 'x' __ only if 'x' is NOT followed by 'y'__. Alsp known as __negative lookahead__. |
