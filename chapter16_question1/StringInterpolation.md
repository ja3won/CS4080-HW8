I would add `TOKEN_INTERPOLATION` for string text that comes before an embedded expression. `TOKEN_STRING` would represent an ordinary string or the final segment of an interpolated string. The scanner would read each string segment up to `${`, then produce the expression inside the braces as regular Lox tokens. After the closing `}`, it should resume scanning the surrounding string. The `${` and `}` delimiters are consumed by the scanner and do not become tokens.

For:

```lox
"${drink} will be ready in ${steep + cool} minutes."
```

The tokens would be:

```text
TOKEN_INTERPOLATION ""
TOKEN_IDENTIFIER    "drink"
TOKEN_INTERPOLATION " will be ready in "
TOKEN_IDENTIFIER    "steep"
TOKEN_PLUS          "+"
TOKEN_IDENTIFIER    "cool"
TOKEN_STRING        " minutes."
```

- The first `""` means there is no text before `${drink}`.
- `drink` is the variable name. Its value, `Tea`, is inserted when the program runs.
- The next `TOKEN_INTERPOLATION` contains the text between the expressions.
- `steep + cool` is tokenized like any other Lox expression, so the usual expression parser handles the identifiers and addition.
- The final `TOKEN_STRING` contains the remaining text and marks the end of the string.

For the nested example:

```lox
"Nested ${"interpolation?! Are you ${"mad?!"}"}"
```

The tokens would be:

```text
TOKEN_INTERPOLATION "Nested "
TOKEN_INTERPOLATION "interpolation?! Are you "
TOKEN_STRING        "mad?!"
TOKEN_STRING        ""
TOKEN_STRING        ""
```

The first token contains the outer string's text before its interpolation. The scanner then enters the nested string and produces its text as another interpolation token. The two empty `TOKEN_STRING` tokens end the middle and outer strings as neither has remaining text after its interpolation. A stack can keep track of the active strings and interpolations. When the scanner reaches a nested `${...}`, it saves the surrounding string's position and state. After scanning the nested string, it resumes the outer string and produces its final segment.
