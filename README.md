# EX.-NO-2-IMPLEMENTATION-OF-PLAYFAIR-CIPHER

## Name: Arunachalam
## Reference Number: 212225230019

# AIM:
To write a C program to implement the Playfair Substitution technique.

# DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:

  1.  If both letters are the same (or only one letter is left), add an "X" after the first letter
  2.  If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
  3.  If the letters appear on the same column of your table, replace them with the letters immediately below respectively
  4.  If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.

# EXAMPLE:

<img width="558" height="380" alt="image" src="https://github.com/user-attachments/assets/c714d913-cd1b-4d99-b78f-d3eeb6ebcea1" />

# ALGORITHM:

STEP-1: Read the plain text from the user. 
STEP-2: Read the keyword from the user. 
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.

# PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char matrix[5][5];

void createMatrix(char key[])
{
    int used[26] = {0};
    int i, j, k = 0;
    char ch;

    used['J' - 'A'] = 1;

    for (i = 0; key[i] != '\0'; i++)
    {
        ch = toupper(key[i]);

        if (ch >= 'A' && ch <= 'Z')
        {
            if (ch == 'J')
                ch = 'I';

            if (!used[ch - 'A'])
            {
                matrix[k / 5][k % 5] = ch;
                used[ch - 'A'] = 1;
                k++;
            }
        }
    }

    for (ch = 'A'; ch <= 'Z'; ch++)
    {
        if (!used[ch - 'A'])
        {
            matrix[k / 5][k % 5] = ch;
            k++;
        }
    }
}

void findPosition(char ch, int *row, int *col)
{
    int i, j;

    if (ch == 'J')
        ch = 'I';

    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++)
            if (matrix[i][j] == ch)
            {
                *row = i;
                *col = j;
                return;
            }
}

int main()
{
    char key[100], text[500], plain[1000], cipher[1000];
    int i, n = 0, k = 0;
    int r1, c1, r2, c2;

    printf("Enter plaintext: ");
    fgets(text, sizeof(text), stdin);

    printf("Enter key: ");
    fgets(key, sizeof(key), stdin);

    createMatrix(key);

    for (i = 0; text[i] != '\0'; i++)
    {
        char ch = toupper(text[i]);

        if (ch >= 'A' && ch <= 'Z')
        {
            if (ch == 'J')
                ch = 'I';

            plain[n++] = ch;
        }
    }

    plain[n] = '\0';

    i = 0;

    while (i < n)
    {
        char a = plain[i];
        char b;

        if (i + 1 >= n)
        {
            b = 'X';
            i++;
        }
        else if (plain[i] == plain[i + 1])
        {
            b = 'X';
            i++;
        }
        else
        {
            b = plain[i + 1];
            i += 2;
        }

        findPosition(a, &r1, &c1);
        findPosition(b, &r2, &c2);

        if (r1 == r2)
        {
            cipher[k++] = matrix[r1][(c1 + 1) % 5];
            cipher[k++] = matrix[r2][(c2 + 1) % 5];
        }
        else if (c1 == c2)
        {
            cipher[k++] = matrix[(r1 + 1) % 5][c1];
            cipher[k++] = matrix[(r2 + 1) % 5][c2];
        }
        else
        {
            cipher[k++] = matrix[r1][c2];
            cipher[k++] = matrix[r2][c1];
        }
    }

    cipher[k] = '\0';

    printf("\nKey Matrix:\n");

    for (i = 0; i < 5; i++)
    {
        for (int j = 0; j < 5; j++)
            printf("%c ", matrix[i][j]);

        printf("\n");
    }

    printf("\nCiphertext: %s\n", cipher);

    return 0;
}
```
# OUTPUT:

<img width="1227" height="356" alt="image" src="https://github.com/user-attachments/assets/836107d7-c526-4d7d-92da-962c5bf1a22e" />

# RESULT:
Thus, the C program to implement the Playfair substitution cipher was written and executed successfully. The plaintext was encrypted into ciphertext using a 5×5 key matrix.
