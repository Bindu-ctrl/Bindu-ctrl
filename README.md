#include <stdio.h>

typedef struct {
    int base;
    int value;
} Root;

// Function to convert a value from a given base to decimal
int decode_value(int base, int value) {
    int decimal_value = 0;
    int power = 1;

    while (value > 0) {
        decimal_value += (value % 10) * power;
        power *= base;
        value /= 10;
    }
    return decimal_value;
}

// Function to calculate the polynomial value at x using Lagrange interpolation
float lagrange_polynomial(int x, Root roots[], int n) {
    float result = 0.0;

    for (int i = 0; i < n; i++) {
        float term = roots[i].value;

        for (int j = 0; j < n; j++) {
            if (j != i) {
                term *= (float)(x - roots[j].base) / (roots[i].base - roots[j].base);
            }
        }
        result += term;
    }
    return result;
}

int main() {
    // Define roots and their decoded values
    Root roots[] = {
        {1, decode_value(10, 4)},  // (1, 4)
        {2, decode_value(2, 111)},  // (2, 7)
        {3, decode_value(10, 12)}   // (3, 12)
    };

    int n = sizeof(roots) / sizeof(roots[0]); // Number of roots

    // Calculate the constant term c = P(0)
    float c = lagrange_polynomial(0, roots, n);

    // Print the constant term
    printf("The constant term c is: %.2f\n", c);

    return 0;
}
