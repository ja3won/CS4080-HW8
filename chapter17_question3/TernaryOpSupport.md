I would add `PREC_CONDITIONAL` between `PREC_ASSIGNMENT` and `PREC_OR`. The `TOKEN_QUESTION` row would use `ternary` as its infix function, with `PREC_CONDITIONAL` precedence. `TOKEN_COLON` would have no parsing function.

```c
static void ternary(bool canAssign) {
  parsePrecedence(PREC_CONDITIONAL);

  if (!match(TOKEN_COLON)) {
    error("Expected ':' before the false branch.");
    return;
  }

  expression();
}
```