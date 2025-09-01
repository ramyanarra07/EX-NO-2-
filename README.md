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
DEVELOPED BY : NARRA RAMYA

REG NO : 212223040128

```
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define SIZE 5

void prepareText(const char *plaintext, char *out) {
    int i = 0, j = 0;
    int len = strlen(plaintext);
    while (i < len) {
        if (isalpha(plaintext[i])) {
            char c = toupper(plaintext[i]);
            if (c == 'J') c = 'I';
            out[j++] = c;
        }
        i++;
    }
    out[j] = '\0';
    // Insert X between duplicate pairs and pad if odd length
    int n = strlen(out);
    for (i = 0; i < n; i += 2) {
        if (out[i + 1] == '\0') {
            out[i + 1] = 'X';
            out[i + 2] = '\0';
            n++;
        } else if (out[i] == out[i + 1]) {
            for (int k = n; k > i + 1; k--)
                out[k] = out[k - 1];
            out[i + 1] = 'X';
            n++;
        }
    }
}

void generateKeyTable(const char *key, char keyTable[SIZE][SIZE]) {
    int dict[26] = {0};
    int i, j, idx = 0;
    char c;
    // Copy key characters
    for (i = 0; key[i] && idx < 25; i++) {
        c = toupper(key[i]);
        if (c == 'J') c = 'I';
        if (isalpha(c) && !dict[c - 'A']) {
            keyTable[idx / 5][idx % 5] = c;
            dict[c - 'A'] = 1;
            idx++;
        }
    }
    // Copy rest of alphabet
    for (c = 'A'; c <= 'Z' && idx < 25; c++) {
        if (c == 'J') continue;
        if (!dict[c - 'A']) {
            keyTable[idx / 5][idx % 5] = c;
            dict[c - 'A'] = 1;
            idx++;
        }
    }
}

void findPos(char keyTable[SIZE][SIZE], char c, int *row, int *col) {
    int i, j;
    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            if (keyTable[i][j] == c) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void encrypt(const char *pt, char keyTable[SIZE][SIZE], char *ct) {
    int i, row1, col1, row2, col2;
    for (i = 0; pt[i] && pt[i+1]; i += 2) {
        findPos(keyTable, pt[i], &row1, &col1);
        findPos(keyTable, pt[i+1], &row2, &col2);
        if (row1 == row2) {
            ct[i]   = keyTable[row1][(col1+1)%5];
            ct[i+1] = keyTable[row2][(col2+1)%5];
        } else if (col1 == col2) {
            ct[i]   = keyTable[(row1+1)%5][col1];
            ct[i+1] = keyTable[(row2+1)%5][col2];
        } else {
            ct[i]   = keyTable[row1][col2];
            ct[i+1] = keyTable[row2][col1];
        }
    }
    ct[i] = '\0';
}

int main() {
    char keyword[100], plaintext[200], prepared[200], keyTable[SIZE][SIZE], ciphertext[200];
    printf("Enter keyword: ");
    scanf("%s", keyword);
    printf("Enter plaintext: ");
    scanf("%s", plaintext);

    prepareText(plaintext, prepared);
    generateKeyTable(keyword, keyTable);
    encrypt(prepared, keyTable, ciphertext);

    printf("Encrypted ciphertext: %s\n", ciphertext);
    return 0;
}

```





Output:

<img width="801" height="334" alt="image" src="https://github.com/user-attachments/assets/890545c0-d34d-4d4e-8d7c-04997a75643a" />

RESULT :
The program was executed successfully.
