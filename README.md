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

char keyMatrix[5][5];

// Function to check if character is already in key matrix
int isPresent(char ch, char keyMatrix[5][5], int size) {
    for(int i = 0; i < size; i++) {
        if(keyMatrix[i/5][i%5] == ch)
            return 1;
    }
    return 0;
}

// Function to generate key matrix
void generateKeyMatrix(char key[]) {
    int i, j, k = 0;
    char temp[25];

    // Remove duplicates
    for(i = 0; key[i] != '\0'; i++) {
        char ch = toupper(key[i]);
        if(ch == 'J') ch = 'I';

        if(!isPresent(ch, keyMatrix, k) && isalpha(ch)) {
            temp[k++] = ch;
        }
    }

    // Fill remaining letters
    for(char ch = 'A'; ch <= 'Z'; ch++) {
        if(ch == 'J') continue;

        if(!isPresent(ch, keyMatrix, k)) {
            temp[k++] = ch;
        }
    }

    // Fill 5x5 matrix
    k = 0;
    for(i = 0; i < 5; i++) {
        for(j = 0; j < 5; j++) {
            keyMatrix[i][j] = temp[k++];
        }
    }
}

// Function to find position of character
void findPosition(char ch, int *row, int *col) {
    if(ch == 'J') ch = 'I';

    for(int i = 0; i < 5; i++) {
        for(int j = 0; j < 5; j++) {
            if(keyMatrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

// Function to prepare plaintext
void prepareText(char input[], char output[]) {
    int i, j = 0;

    for(i = 0; input[i] != '\0'; i++) {
        if(isalpha(input[i])) {
            output[j++] = toupper(input[i]);
        }
    }
    output[j] = '\0';

    // Insert X for repeated letters
    char temp[100];
    int k = 0;

    for(i = 0; i < j; i++) {
        temp[k++] = output[i];

        if(output[i] == output[i+1]) {
            temp[k++] = 'X';
        }
    }

    if(k % 2 != 0) {
        temp[k++] = 'X';
    }

    temp[k] = '\0';
    strcpy(output, temp);
}

// Encryption function
void encrypt(char text[]) {
    int i, r1, c1, r2, c2;

    for(i = 0; text[i] != '\0'; i += 2) {
        findPosition(text[i], &r1, &c1);
        findPosition(text[i+1], &r2, &c2);

        // Same row
        if(r1 == r2) {
            text[i]     = keyMatrix[r1][(c1 + 1) % 5];
            text[i + 1] = keyMatrix[r2][(c2 + 1) % 5];
        }
        // Same column
        else if(c1 == c2) {
            text[i]     = keyMatrix[(r1 + 1) % 5][c1];
            text[i + 1] = keyMatrix[(r2 + 1) % 5][c2];
        }
        // Rectangle
        else {
            text[i]     = keyMatrix[r1][c2];
            text[i + 1] = keyMatrix[r2][c1];
        }
    }
}

int main() {
    char key[50], plaintext[100], preparedText[100];

    printf("Enter the keyword: ");
    scanf("%s", key);

    printf("Enter the plaintext: ");
    scanf("%s", plaintext);

    generateKeyMatrix(key);

    printf("\nKey Matrix:\n");
    for(int i = 0; i < 5; i++) {
        for(int j = 0; j < 5; j++) {
            printf("%c ", keyMatrix[i][j]);
        }
        printf("\n");
    }

    prepareText(plaintext, preparedText);

    encrypt(preparedText);

    printf("\nCipher Text: %s\n", preparedText);

    return 0;
}

```

Output:


<img width="509" height="486" alt="image" src="https://github.com/user-attachments/assets/be6442e9-9d08-41e8-9045-883a4cce61c8" />


RESULT:


Thus the implementation of Playfair Cipher had been executed successfully.
