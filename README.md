#include <iostream>
#include <stack>
#include <cctype>
using namespace std;

int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    if (op == '^') return 3;
    return 0;
}

string infixToPostfix(string expression) {
    stack<char> s;
    string output;
    for (size_t i = 0; i < expression.length(); i++) {
        char ch = expression[i];
        if (isalnum(ch)) {
            output += ch;
        } else if (ch == '(') {
            s.push(ch);
        } else if (ch == ')') {
            while (!s.empty() && s.top() != '(') {
                output += s.top();
                s.pop();
            }
            s.pop();
        } else {
            while (!s.empty() && precedence(s.top()) >= precedence(ch)) {
                output += s.top();
                s.pop();
            }
            s.push(ch);
        }
    }
    while (!s.empty()) {
        output += s.top();
        s.pop();
    }
    return output;
}

string postfixToInfix(string expression) {
    stack<string> s;
    for (size_t i = 0; i < expression.length(); i++) {
        char ch = expression[i];
        if (isalnum(ch)) {
            s.push(string(1, ch));
        } else {
            string op2 = s.top(); s.pop();
            string op1 = s.top(); s.pop();
            string temp = "(" + op1 + ch + op2 + ")";
            s.push(temp);
        }
    }
    return s.top();
}

int main() {
    string infix_expr = "A+B*(C^D-E)";
    string postfix_expr = infixToPostfix(infix_expr);
    cout << "Infix ke Postfix: " << postfix_expr << endl;
    cout << "Postfix ke Infix: " << postfixToInfix(postfix_expr) << endl;
    return 0;
}
