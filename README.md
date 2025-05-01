## 🔤 Demo: Word, Number, and Operator Classifier

This Flex program classifies words, numbers, and symbols from input.

### 📄 `lexer.l`
```c
%{
#include <stdio.h>
%}

%%
[0-9]+              { printf("NUMBER: %s\n", yytext); }
[A-Za-z_][A-Za-z0-9_]* { printf("WORD: %s\n", yytext); }
[+\-*/=<>]          { printf("OPERATOR: %s\n", yytext); }
[\n\t ]+            ; // Ignore whitespace
.                   { printf("UNKNOWN: %s\n", yytext); }
%%

int main(void) {
    printf("Enter input (Ctrl+D to end):\n");
    yylex();
    return 0;
}

int yywrap(void) {
    return 1;
}
```

---

## ⚙️ Compilation Steps

```bash
flex lexer.l           # Generates lex.yy.c
gcc -o lexer lex.yy.c -lfl
```

---

## ▶️ Usage

```bash
./lexer
Enter input (Ctrl+D to end):
x = 42 + y
```

### Output:
```
WORD: x
OPERATOR: =
NUMBER: 42
OPERATOR: +
WORD: y
```

