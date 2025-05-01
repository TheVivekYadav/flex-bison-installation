Here's a detailed Markdown (`.md`) document explaining how to set up **Flex** and **Bison**, along with a simple demo program that uses both to build a basic calculator.

---

```markdown
# Flex and Bison Setup & Demo Program

This guide explains how to set up **Flex** and **Bison** on your system and create a basic calculator program using these tools.

## 🧰 What are Flex and Bison?

- **Flex** is a lexical analyzer generator (lexer), used to identify tokens (e.g., numbers, operators).
- **Bison** is a parser generator (like `yacc`), used to generate parsers from grammar rules.

## 🖥️ Installation

### On Linux (Ubuntu/Debian-based)
```bash
sudo apt update
sudo apt install flex bison build-essential
```

### On Arch-based Systems
```bash
sudo pacman -S flex bison
```

### On macOS (using Homebrew)
```bash
brew install flex bison
```

You may need to add the following to your `.zshrc` or `.bashrc` on macOS:
```bash
export PATH="/opt/homebrew/opt/flex/bin:/opt/homebrew/opt/bison/bin:$PATH"
export LDFLAGS="-L/opt/homebrew/opt/flex/lib -L/opt/homebrew/opt/bison/lib"
export CPPFLAGS="-I/opt/homebrew/opt/flex/include -I/opt/homebrew/opt/bison/include"
```

## 🧪 Demo: Simple Calculator

We’ll create a calculator that supports `+`, `-`, `*`, `/` and parentheses.

### 1. `calc.l` (Flex Lexer)
```c
%{
#include "calc.tab.h"
%}

%%
[0-9]+      { yylval = atoi(yytext); return NUMBER; }
[ \t\n]     ; // ignore whitespace
"+"         return PLUS;
"-"         return MINUS;
"*"         return MUL;
"/"         return DIV;
"("         return LPAREN;
")"         return RPAREN;
.           return yytext[0];
%%
```

### 2. `calc.y` (Bison Parser)
```c
%{
#include <stdio.h>
#include <stdlib.h>
%}

%token NUMBER
%token PLUS MINUS MUL DIV LPAREN RPAREN

%left PLUS MINUS
%left MUL DIV

%%
program:
    expr '\n'  { printf("= %d\n", $1); }
;

expr:
    expr PLUS expr   { $$ = $1 + $3; }
  | expr MINUS expr  { $$ = $1 - $3; }
  | expr MUL expr    { $$ = $1 * $3; }
  | expr DIV expr    { $$ = $1 / $3; }
  | LPAREN expr RPAREN { $$ = $2; }
  | NUMBER           { $$ = $1; }
;
%%

int main() {
    printf("Enter an expression:\n");
    yyparse();
    return 0;
}

int yyerror(const char *s) {
    fprintf(stderr, "Error: %s\n", s);
    return 1;
}
```

## ⚙️ Compilation Steps

```bash
bison -d calc.y     # Generates calc.tab.c and calc.tab.h
flex calc.l         # Generates lex.yy.c
gcc -o calc calc.tab.c lex.yy.c -lfl
```

## ▶️ Running the Calculator

```bash
./calc
Enter an expression:
3 + 4 * (2 - 1)
= 7
```

## 📁 Final File Structure
```
calc.l       # Flex file (lexer)
calc.y       # Bison file (parser)
calc.tab.c   # Bison-generated parser
calc.tab.h   # Bison-generated header
lex.yy.c     # Flex-generated lexer
calc         # Final compiled binary
```

## 🧩 Tips

- You can debug using `yydebug` or print intermediate rules.
- Use `%debug` in `.y` file to enable debugging info.
- Add error handling using `yyerror`.

## 📚 References

- [Flex Manual](https://westes.github.io/flex/manual/)
- [Bison Manual](https://www.gnu.org/software/bison/manual/)

---

Happy parsing! 🧠⚙️
```

---

Would you like me to generate the files (`calc.l`, `calc.y`) for download or bundle it in a `.zip`?