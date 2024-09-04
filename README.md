# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.


## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26


## PROGRAM:
```
#include <stdio.h>
#include <stdlib.h>
// Function to perform Caesar Cipher encryption
void caesarEncrypt(char *text, int key) {
 for (int i = 0; text[i] != '\0'; i++) {
 char c = text[i];
 // Check if the character is an uppercase letter
 if (c >= 'A' && c <= 'Z') {
 text[i] = ((c - 'A' + key) % 26 + 26) % 26 + 'A';
 }
 // Check if the character is a lowercase letter
 else if (c >= 'a' && c <= 'z') {
 text[i] = ((c - 'a' + key) % 26 + 26) % 26 + 'a';
 }
 // Ignore non-alphabetic characters
 }
}
// Function to perform Caesar Cipher decryption
void caesarDecrypt(char *text, int key) {
 // Decryption is the same as encryption with a negative key
 caesarEncrypt(text, -key);
}
int main() {
 char message[100]; // Declare a character array to store the message
 int key;
 printf("Enter the message to encrypt: ");
 fgets(message, sizeof(message), stdin); // Read input from the user
 printf("Enter the Caesar Cipher key (an integer): ");
 scanf("%d", &key); // Read the key from the user
 // Encrypt the message using the Caesar Cipher
 caesarEncrypt(message, key);
 printf("Encrypted Message: %s", message);
 // Decrypt the message back to the original
 caesarDecrypt(message, key);
 printf("Decrypted Message: %s", message);
 return 0;
```
}


## OUTPUT:
![image](https://github.com/user-attachments/assets/53abaf17-290c-4d39-95f9-370fdacbb007)

OUTPUT:
Simulating Caesar Cipher


Input : Anna University
Encrypted Message : Dqqd Xqlyhuvlwb Decrypted Message : Anna University

## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
'''
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void generateKeyTable(char key[], int keyLength, char keyTable[SIZE][SIZE]) {
    int used[26] = {0};  // To track used characters
    int i, j, k = 0;

    for (i = 0; i < keyLength; i++) {
        if (key[i] != 'j') {
            if (used[key[i] - 'a'] == 0) {
                used[key[i] - 'a'] = 1;
                keyTable[k / SIZE][k % SIZE] = key[i];
                k++;
            }
        }
    }

    for (i = 0; i < 26; i++) {
        if (i + 'a' != 'j' && used[i] == 0) {
            keyTable[k / SIZE][k % SIZE] = i + 'a';
            k++;
        }
    }
}

void search(char keyTable[SIZE][SIZE], char a, char b, int *row1, int *col1, int *row2, int *col2) {
    int i, j;

    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            if (keyTable[i][j] == a) {
                *row1 = i;
                *col1 = j;
            } else if (keyTable[i][j] == b) {
                *row2 = i;
                *col2 = j;
            }
        }
    }
}

void encryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
    int row1, col1, row2, col2;

    search(keyTable, a, b, &row1, &col1, &row2, &col2);

    if (row1 == row2) {
        *x = keyTable[row1][(col1 + 1) % SIZE];
        *y = keyTable[row2][(col2 + 1) % SIZE];
    } else if (col1 == col2) {
        *x = keyTable[(row1 + 1) % SIZE][col1];
        *y = keyTable[(row2 + 1) % SIZE][col2];
    } else {
        *x = keyTable[row1][col2];
        *y = keyTable[row2][col1];
    }
}

void decryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
    int row1, col1, row2, col2;

    search(keyTable, a, b, &row1, &col1, &row2, &col2);

    if (row1 == row2) {
        *x = keyTable[row1][(col1 + SIZE - 1) % SIZE];
        *y = keyTable[row2][(col2 + SIZE - 1) % SIZE];
    } else if (col1 == col2) {
        *x = keyTable[(row1 + SIZE - 1) % SIZE][col1];
        *y = keyTable[(row2 + SIZE - 1) % SIZE][col2];
    } else {
        *x = keyTable[row1][col2];
        *y = keyTable[row2][col1];
    }
}

void encrypt(char keyTable[SIZE][SIZE], char plainText[], char cipherText[]) {
    int i, j = 0;
    char a, b;

    for (i = 0; plainText[i] != '\0'; i += 2) {
        a = plainText[i];
        if (plainText[i + 1] != '\0') {
            b = plainText[i + 1];
        } else {
            b = 'x';
        }

        if (a == b) {
            b = 'x';
            i--;
        }

        encryptPair(keyTable, a, b, &cipherText[j], &cipherText[j + 1]);
        j += 2;
    }
    cipherText[j] = '\0';
}

void decrypt(char keyTable[SIZE][SIZE], char cipherText[], char plainText[]) {
    int i, j = 0;
    char a, b;

    for (i = 0; cipherText[i] != '\0'; i += 2) {
        a = cipherText[i];
        b = cipherText[i + 1];

        decryptPair(keyTable, a, b, &plainText[j], &plainText[j + 1]);
        j += 2;
    }
    plainText[j] = '\0';
}

int main() {
    char key[100], plainText[100], cipherText[100], decryptedText[100];
    char keyTable[SIZE][SIZE];
    int i, keyLength;

    printf("Enter the key: ");
    scanf("%s", key);

    for (i = 0; key[i] != '\0'; i++) {
        key[i] = tolower(key[i]);
    }

    keyLength = strlen(key);
    generateKeyTable(key, keyLength, keyTable);

    printf("Enter the plaintext: ");
    scanf("%s", plainText);

    for (i = 0; plainText[i] != '\0'; i++) {
        plainText[i] = tolower(plainText[i]);
    }

    encrypt(keyTable, plainText, cipherText);
    printf("Encrypted Text: %s\n", cipherText);

    decrypt(keyTable, cipherText, decryptedText);
    printf("Decrypted Text: %s\n", decryptedText);

    return 0;
}
'''

## OUTPUT:
Output:![image](https://github.com/user-attachments/assets/cc3a2337-d174-48ea-b3fc-131cdc061445)

Key text: Monarchy Plain text: instruments Cipher text: gatlmzclrqtx

## RESULT:
The program is executed successfully


---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.


## PROGRAM:
'''
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Hill cipher key matrix and its inverse
int keymat[3][3] = { { 1, 2, 1 }, { 2, 3, 2 }, { 2, 2, 1 } };
int invkeymat[3][3] = { { -1, 0, 1 }, { 2, -1, 0 }, { -2, 2, -1 } };

// Key array for mapping
char key[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";

// Encode function
void encode(char a, char b, char c, char *ret) {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;

    x = posa * keymat[0][0] + posb * keymat[1][0] + posc * keymat[2][0];
    y = posa * keymat[0][1] + posb * keymat[1][1] + posc * keymat[2][1];
    z = posa * keymat[0][2] + posb * keymat[1][2] + posc * keymat[2][2];

    ret[0] = key[x % 26];
    ret[1] = key[y % 26];
    ret[2] = key[z % 26];
    ret[3] = '\0';
}

// Decode function
void decode(char a, char b, char c, char *ret) {
    int x, y, z;
    int posa = (int) a - 65;
    int posb = (int) b - 65;
    int posc = (int) c - 65;

    x = posa * invkeymat[0][0] + posb * invkeymat[1][0] + posc * invkeymat[2][0];
    y = posa * invkeymat[0][1] + posb * invkeymat[1][1] + posc * invkeymat[2][1];
    z = posa * invkeymat[0][2] + posb * invkeymat[1][2] + posc * invkeymat[2][2];

    ret[0] = key[(x % 26 < 0) ? (26 + x % 26) : (x % 26)];
    ret[1] = key[(y % 26 < 0) ? (26 + y % 26) : (y % 26)];
    ret[2] = key[(z % 26 < 0) ? (26 + z % 26) : (z % 26)];
    ret[3] = '\0';
}

int main() {
    char msg[1000];
    char enc[1000] = "";
    char dec[1000] = "";
    int n;

    strcpy(msg, "AATHI");

    printf("Simulation of Hill Cipher\n");
    printf("Input message : %s\n", msg);

    // Convert message to uppercase
    for (int i = 0; i < strlen(msg); i++) {
        msg[i] = toupper(msg[i]);
    }

    // Remove spaces (if needed)
    n = strlen(msg) % 3;

    // Append padding text 'X' if needed
    if (n != 0) {
        for (int i = 1; i <= (3 - n); i++) {
            strcat(msg, "X");
        }
    }

    printf("Padded message : %s\n", msg);

    // Encoding
    for (int i = 0; i < strlen(msg); i += 3) {
        char a = msg[i];
        char b = msg[i + 1];
        char c = msg[i + 2];
        char ret[4];
        encode(a, b, c, ret);
        strcat(enc, ret);
    }

    printf("Encoded message : %s\n", enc);

    // Decoding
    for (int i = 0; i < strlen(enc); i += 3) {
        char a = enc[i];
        char b = enc[i + 1];
        char c = enc[i + 2];
        char ret[4];
        decode(a, b, c, ret);
        strcat(dec, ret);
    }

    printf("Decoded message : %s\n", dec);
    return 0;
}
'''
## OUTPUT:
OUTPUT:![image](https://github.com/user-attachments/assets/cf6a53e7-4b38-4fc6-89ae-4427ccd1ee50)

Simulating Hill Cipher


Input Message : SecurityLaboratory
Padded Message : SECURITYLABORATORY Encrypted Message : EACSDKLCAEFQDUKSXU Decrypted Message : SECURITYLABORATORY
## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:
'''
#include <stdio.h>
#include <string.h>

// Function to perform Vigenere encryption
void vigenereEncrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];

        if (c >= 'A' && c <= 'Z') {
            // Encrypt uppercase letters
            text[i] = ((c - 'A' + key[i % keyLen] - 'A') % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Encrypt lowercase letters
            text[i] = ((c - 'a' + key[i % keyLen] - 'A') % 26) + 'a';
        }
    }
}

// Function to perform Vigenere decryption
void vigenereDecrypt(char *text, const char *key) {
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (int i = 0; i < textLen; i++) {
        char c = text[i];

        if (c >= 'A' && c <= 'Z') {
            // Decrypt uppercase letters
            text[i] = ((c - 'A' - (key[i % keyLen] - 'A') + 26) % 26) + 'A';
        } else if (c >= 'a' && c <= 'z') {
            // Decrypt lowercase letters
            text[i] = ((c - 'a' - (key[i % keyLen] - 'A') + 26) % 26) + 'a';
        }
    }
}

int main() {
    const char *key = "YUV"; 
    char message[] = "saveethaengineeringcollege"; 
    
    printf("Simulating Vigenere Cipher:\n");
    // Print the original plain text
    printf("Original Message: %s\n", message);
    
    // Print the key used
    printf("Key: %s\n", key);

    // Encrypt the message
    vigenereEncrypt(message, key);
    printf("Encrypted Message: %s\n", message);

    // Decrypt the message back to the original
    vigenereDecrypt(message, key);
    printf("Decrypted Message: %s\n", message);

    return 0;
}
'''
## OUTPUT:
OUTPUT :![image](https://github.com/user-attachments/assets/33da5309-a652-4a60-af8f-3a6a7b287bac)


Simulating Vigenere Cipher


Input Message : SecurityLaboratory
Encrypted Message : NMIYEMKCNIQVVROWXC Decrypted Message : SECURITYLABORATORY
## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:
'''

#include <stdio.h>
#include <string.h>

int main() {
    int i, j, len, rails, count, dir;
    char str[1000];
    int code[100][1000] = {0};  // Initialize the entire array to 0

    printf("Enter a Secret Message:\n");
    gets(str);  
    len = strlen(str);

    printf("Enter number of rails:\n");
    scanf("%d", &rails);

    count = 0;
    i = 0;
    dir = 1;  

    for (j = 0; j < len; j++) {
        code[i][j] = str[j];
        
        // Change direction if we reach the top or bottom rail
        if (i == 0) {
            dir = 1;
        } else if (i == rails - 1) {
            dir = -1;
        }

        i += dir;
    }

    printf("Encrypted Message:\n");

    // Print the encrypted message
    for (i = 0; i < rails; i++) {
        for (j = 0; j < len; j++) {
            if (code[i][j] != 0) {
                printf("%c", code[i][j]);
            }
        }
    }

    printf("\n");
    return 0;
}
'''
##Output:
output:![image](https://github.com/user-attachments/assets/99672c71-7461-4a56-9162-e504dc2b3f1b)

## RESULT:
The program is executed successfully
