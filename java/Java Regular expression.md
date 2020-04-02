# java Regular Expression

[freeformatter.com/java-regx-tester.html](https://www.freeformatter.com/java-regex-tester.html#ad-output)


## Meta character
| Character   | What does it do ? |
| ----------  | :---------------- |
| $           | Matches __end of input line__ .if in multiline mode, it also matches __before a line break character__, hence every end end of line |
| (x)         | Match 'x' and remember the match. also known as __capturing parenthesis__ |
| (?:x)       | matches 'x' but __does Not remember the match__. Also known as __NON-capturing parenthesis__ |
| x(?=y)      | Matches 'x' __only if 'x' is followed by 'y'__ . Also known as __lookahead__. |
| x(?!y)      | Match 'x' __only if 'x' is NOT followed by 'y__. Alsp known as __negative lookahead__. |
| ^           | Matches the __begining of the input__. If in multiline mode, it also matches after a line break character, Hence every new line. When used in set pattern ([^abc]), it negates the set; matching anything not enclosed in the bracket. | 
| \           | * Used to indicate that the next character should NOT be interpreted literally. For example, the character 'w' by itself will be interpreted as 'match the character w', but using '\w' signifies 'match an alpha-numeric character including underscore'. * Used to indicate that a metacharacter is to be interpreted literally. For example, the '.' metacharacter means 'match any single character but a new line', but if we would rather match a dot character instead, we would use '\.'. |
