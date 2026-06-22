## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char m[5][5];

int p(char c, int k) {
    for(int i = 0; i < k; i++) if(m[i/5][i%5] == c) return 1;
    return 0;
}

void gk(char *k) {
    char t[26]; int n = 0;
    for(int i = 0; k[i]; i++) {
        char c = toupper(k[i]) == 'J' ? 'I' : toupper(k[i]);
        if(isalpha(c) && !p(c, n)) t[n++] = c;
    }
    for(char c = 'A'; c <= 'Z'; c++) {
        if(c == 'J') continue;
        if(!p(c, n)) t[n++] = c;
    }
    for(int i = 0; i < 25; i++) m[i/5][i%5] = t[i];
}

void fp(char c, int *r, int *col) {
    c = (c == 'J') ? 'I' : c;
    for(int i = 0; i < 5; i++) for(int j = 0; j < 5; j++) if(m[i][j] == c) { *r = i; *col = j; }
}

void pt(char *in, char *out) {
    char t[100]; int j = 0, k = 0;
    for(int i = 0; in[i]; i++) if(isalpha(in[i])) t[j++] = toupper(in[i]);
    char f[100];
    for(int i = 0; i < j; i++) {
        f[k++] = t[i];
        if(t[i] == t[i+1]) f[k++] = 'X';
    }
    if(k % 2) f[k++] = 'X';
    f[k] = 0; strcpy(out, f);
}

void enc(char *t) {
    for(int i = 0; t[i]; i += 2) {
        int r1, c1, r2, c2;
        fp(t[i], &r1, &c1); fp(t[i+1], &r2, &c2);
        if(r1 == r2) { t[i] = m[r1][(c1+1)%5]; t[i+1] = m[r2][(c2+1)%5]; }
        else if(c1 == c2) { t[i] = m[(r1+1)%5][c1]; t[i+1] = m[(r2+1)%5][c2]; }
        else { t[i] = m[r1][c2]; t[i+1] = m[r2][c1]; }
    }
}

int main() {
    char k[50], pt_text[100], out[100];
    printf("Keyword: "); scanf("%s", k);
    printf("Plaintext: "); scanf("%s", pt_text);
    gk(k); pt(pt_text, out); enc(out);
    printf("Cipher: %s\n", out);
    return 0;
}

```

Output:


<img width="509" height="486" alt="image" src="https://github.com/user-attachments/assets/be6442e9-9d08-41e8-9045-883a4cce61c8" />


RESULT:


Thus the implementation of Playfair Cipher had been executed successfully.
