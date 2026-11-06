For the expression:

```lox
(-1 + 2) * 3 - -4
```

The calls happen in the order shown below. Indentation shows which function calls which. Moving back to the left means the previous call returned. The comments show the token being handled, not another argument.

```text
expression()
  parsePrecedence(PREC_ASSIGNMENT)
    grouping(true)                         // (
      expression()
        parsePrecedence(PREC_ASSIGNMENT)
          unary(true)                      // first -
            parsePrecedence(PREC_UNARY)
              number(false)                // 1
          binary(true)                     // +
            parsePrecedence(PREC_FACTOR)
              number(false)                // 2
    binary(true)                           // *
      parsePrecedence(PREC_UNARY)
        number(false)                      // 3
    binary(true)                           // subtraction -
      parsePrecedence(PREC_FACTOR)
        unary(false)                       // negation -
          parsePrecedence(PREC_UNARY)
            number(false)                  // 4
```