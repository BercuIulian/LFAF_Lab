# NFA to DFA Conversion

### Course: Formal Languages & Finite Automata
### Author: Bercu Iulian

----

## Theory
In NFA, when a specific input is given to the current state, the machine goes to multiple states. It can have zero, one or more than one move on a given input symbol.
On the other hand, in DFA, when a specific input is given to the current state, the machine goes to only one state. DFA has only one move on a given input symbol.


## Objectives:

a. Create a local && remote repository of a VCS hosting service (let us all use Github to avoid unnecessary headaches);

b. Choose a programming language, and my suggestion would be to choose one that supports all the main paradigms;

c. Create a separate folder where you will be keeping the report. This semester I wish I won't see reports alongside source code files, fingers crossed;

According to your variant number (by universal convention it is register ID), get the grammar definition and do the following tasks:

a. Implement a type/class for your grammar;

b. Add one function that would generate 5 valid strings from the language expressed by your given grammar;

c. Implement some functionality that would convert and object of type Grammar to one of type Finite Automaton;

d. For the Finite Automaton, please add a method that checks if an input string can be obtained via the state transition from it;

## Implementation description

*  The 'Lexer' class takes in a string 'text' in its constructor and 'tokenizes' it using the tokenize method, which returns a vector of 'Token' objects.
   Each 'Token' object contains a 'TokenType' enum value and a 'string' value
   
   ```
   enum TokenType {
    Number,
    Plus,
    Minus,
    Multiply,
    Divide,
    LParen,
    RParen,
    };

    struct Token {
      TokenType type;
      string value;

    Token(TokenType type, const string& value)
        : type(type), value(value) {}
    };
   ```


* The 'advance' method increments the current position in the input string, while 'current_char' returns the current character at that position.The 'skip_whitespace'
  method moves the current position past any whitespace characters, while 'collect_digits' collects a sequence of digits starting from the current position.

   ```
    class Lexer {
    public:
      explicit Lexer(const string& text) : text_(text), pos_(0) {}

    void advance() { pos_++; }

    char current_char() { return pos_ < text_.size() ? text_[pos_] : '\0'; }

    void skip_whitespace() {
      while (isspace(current_char())) {
        advance();
      }
    }

    string collect_digits() {
      string result = "";
      while (isdigit(current_char())) {
        result += current_char();
        advance();
      }
      return result;
    }
   ```

* The 'collect_token' method collects the next token by checking the current character and either advancing the position and returning a corresponding 'Token' object
  or throwing an error if the character is invalid.
  
  ```
    Token collect_token() {
    char current = current_char();
    switch (current) {
      case '+':
        advance();
        return Token(Plus, "+");
      case '-':
        advance();
        return Token(Minus, "-");
      case '*':
        advance();
        return Token(Multiply, "*");
      case '/':
        advance();
        return Token(Divide, "/");
      case '(':
        advance();
        return Token(LParen, "(");
      case ')':
        advance();
        return Token(RParen, ")");
      default:
        if (isdigit(current)) {
          return Token(Number, collect_digits());
        } else {
          throw runtime_error("Invalid character");
        }
    }
  }

  vector<Token> tokenize() {
    vector<Token> tokens;
    while (pos_ < text_.size()) {
      skip_whitespace();
      if (pos_ >= text_.size()) break;
      tokens.push_back(collect_token());
    }
    return tokens;
  }

   private:
    const string text_;
    size_t pos_;
  };
  ```
  
* Finally, in the 'main' function, we create a 'Lexer' object with a sample input string, tokenize it, and print out each token's type and value using a 'for' loop.
  
  ```
    string formula = "3 + 4 * 2 / ( 1 - 5 )";
    Lexer lexer(formula);
  vector<Token> tokens = lexer.tokenize();

  for (const Token& token : tokens) {
    cout << "Token(" << token.type << ", " << token.value << ")" << endl;
  }
  ```


## Conclusions / Screenshots / Results

![image](https://user-images.githubusercontent.com/113422203/227787900-f0f917cf-73cf-4aec-9aa5-65125314dcd5.png)




The grammar I had for this Lab is Variant 4
VN={S, L, D}, 
VT={a, b, c, d, e, f, j},
P={ 
    S → aS
    S → bS
    S → cD
    S → dL
    S → e
    L → eL
    L → fL
    L → jD
    L → e
    D → eD
    D → d
}

With the following objectives:




In the implementation I created a loop in main that will access grammar class 5 times in order to generate 5 random string. In grammar class
there are 3 string functions for each valid character. In order for the string to be random I used the time function, since the time will not be the same.
The last void function in class grammar is check. This function will output true if the string coresponds tothe grammar and false if the string does not corespond.