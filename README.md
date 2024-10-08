Problem Statement
In this assignment, you'll work on a simplified version of Shamir's Secret Sharing algorithm.

Consider an unknown polynomial of degree m. You would require m+1 roots of the polynomial to solve for the coefficients, represented as k = m + 1.
An unknown polynomial of degree m can be represented as:
f(x) = a_m x^m + a_{m-1} x^{m-1} + ... + a_1 x + c
Where:
f(x) is the polynomial function
m is the degree of the polynomial
a_m, a_{m-1}, ..., a_1, c are coefficients (real numbers)
a_m ≠ 0 (since it's the highest degree term, ensuring the polynomial is of degree m)
This representation shows that a polynomial of degree m is a sum of terms, where each term is a coefficient multiplied by a power of x. The highest power of x is m, and the powers decrease by 1 for each subsequent term until we reach the constant term c, which has no x.
The task is to find the constant term i.e, ‘c’ of the polynomial with the given roots. However, the points are not provided directly but in a specific format.
You need to read the input from the test cases provided in JSON format.
Sample Test Case:
{
    "keys": {
        "n": 4,
        "k": 3
    },
    "1": {
        "base": "10",
        "value": "4"
    },
    "2": {
        "base": "2",
        "value": "111"
    },
    "3": {
        "base": "10",
        "value": "12"
    },
    "6": {
        "base": "4",
        "value": "213"
    }
}
​
n: The number of roots provided in the given JSON
k: The minimum number of roots required to solve for the coefficients of the polynomial
k = m + 1, where m is the degree of the polynomial
Root Format Example:
"2": {
    "base": "2",
    "value": "111"
}
​
Consider the above root as (x, y):
x is the key of the object (in this case, x = 2)
y value is encoded with a given base
Decode y value: 111 in base 2 is 7
Therefore, x = 2 and y = 7
You can use any known method to find the coefficients of the polynomial, such as:
Lagrange interpolation
Matrix method
Gauss elimination
Solve for the constant term of the polynomial, typically represented as c.
Assignment Checkpoints:
1. Read the Test Case (Input) from a JSON file
Parse and read the input provided in JSON format from a separate file, which contains a series of polynomial roots
2. Decode the Y Values
Correctly decode the Y values that are encoded using different bases
3. Find the Secret (C)
Calculate the secret c using the decoded Y values and any known method
Output: Print ‘c’.
Hint: Although you can't test your code against the test case in a testing environment, you can double-check it manually by solving the polynomial on paper and comparing the outputs.
Good luck!

Find the second testcase here.
{
    "keys": {
        "n": 9,
        "k": 6
    },
    "1": {
        "base": "10",
        "value": "28735619723837"
    },
    "2": {
        "base": "16",
        "value": "1A228867F0CA"
    },
    "3": {
        "base": "12",
        "value": "32811A4AA0B7B"
    },
    "4": {
        "base": "11",
        "value": "917978721331A"
    },
    "5": {
        "base": "16",
        "value": "1A22886782E1"
    },
    "6": {
        "base": "10",
        "value": "28735619654702"
    },
    "7": {
        "base": "14",
        "value": "71AB5070CC4B"
    },
    "8": {
        "base": "9",
        "value": "122662581541670"
    },
    "9": {
        "base": "8",
        "value": "642121030037605"
    }
}

Code in C:
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

// Function to decode the value from the given base
long long decodeValue(const char *value, int base) {
    return strtoll(value, NULL, base);
}

// Function to perform Lagrange Interpolation
double lagrangeInterpolation(int x[], long long y[], int n) {
    double result = 0;

    for (int i = 0; i < n; i++) {
        double term = y[i];
        for (int j = 0; j < n; j++) {
            if (i != j) {
                term *= (0 - x[j]) / (double)(x[i] - x[j]);  // f(0) for constant term
            }
        }
        result += term;
    }

    return result;
}

int main() {
    // First test case data
    int n1 = 4;
    int k1 = 3;
    int x1[] = {1, 2, 3, 6};  // The keys
    long long y1[] = {
        decodeValue("4", 10),    // y1 decoded from base 10
        decodeValue("111", 2),   // y2 decoded from base 2
        decodeValue("12", 10),   // y3 decoded from base 10
        decodeValue("213", 4)    // y6 decoded from base 4
    };

    // Second test case data
    int n2 = 9;
    int k2 = 6;
    int x2[] = {1, 2, 3, 4, 5, 6, 7, 8, 9};  // The keys
    long long y2[] = {
        decodeValue("28735619723837", 10),  // y1 decoded from base 10
        decodeValue("1A228867F0CA", 16),   // y2 decoded from base 16
        decodeValue("32811A4AA0B7B", 12),  // y3 decoded from base 12
        decodeValue("917978721331A", 11),  // y4 decoded from base 11
        decodeValue("1A22886782E1", 16),   // y5 decoded from base 16
        decodeValue("28735619654702", 10), // y6 decoded from base 10
        decodeValue("71AB5070CC4B", 14),   // y7 decoded from base 14
        decodeValue("122662581541670", 9), // y8 decoded from base 9
        decodeValue("642121030037605", 8)  // y9 decoded from base 8
    };

    // Finding the constant term (c) for both test cases using Lagrange Interpolation
    double c1 = lagrangeInterpolation(x1, y1, k1);
    double c2 = lagrangeInterpolation(x2, y2, k2);

    // Output the results
    printf("The constant term (c) for the first test case is: %lf\n", c1);
    printf("The constant term (c) for the second test case is: %lf\n", c2);

    return 0;
}

