# Assignment 4: Designing Decimal to Binary Number Converter
Used TypeScript to roughly emulate how the conversion would be done with pen and paper, as in this example:

$$
\begin{align*}
\text{Integer Part:} \\
        & 57 & = 28 & \times 2 + & \boxed{1} \\
         & 28 & = 14 & \times 2 + & \boxed{0} \\
         & 14 & = 7  & \times 2 + & \boxed{0} \\
         & 7  & = 3  & \times 2 + & \boxed{1} \\
         & 3  & = 1  & \times 2 + & \boxed{1} \\
         & 1  & = 0  & \times 2 + & \boxed{1} \\
\text{Fractional Part:} \\
         & 0.8125 & \times 2 & = & \boxed{1} & + 0.625 \\
         & 0.625  & \times 2 & = & \boxed{1} & + 0.25 \\
         & 0.25   & \times 2 & = & \boxed{0} & + 0.5 \\
         & 0.5    & \times 2 & = & \boxed{1} & + 0.0 \\
\end{align*}
$$

$$
57.8125_{10} = 111001.1101_2
$$

$$
\text{(Note that the digits of the integer part are inverted)}
$$

<br>

```typescript

// Return type = string as the result will be the binary number (potential leading zeros)
const convertDecToBin = (decimalInput: number): string => {

    if (decimalInput === 0) return '0';

    // For simplicity's sake, instead of using Two's Complement, negative inputs are treated as absolute.
    const isNegative = decimalInput < 0;
    decimalInput = Math.abs(decimalInput);

    let decIntegerPart: number = Math.trunc(decimalInput);
    let decFractionalPart: number = decimalInput - decIntegerPart;

    // The conditional initialization prevents that inputs > 1 are not returned as .xxx, without a 0 in the integer part.
    let binIntegerPart: string = decIntegerPart == 0 ? '0' : '';
    let binFractionalPart: string = '';


    // Convert integer decimal part to binary
    // -------------------------------------------------
    if (decIntegerPart > 0) {
        while (decIntegerPart > 0) {

            // The modulus operator (%) returns the remainder of the division.
            // The addition assignment operator (+=) assigns the value of the sum to the left operand, concatenating the binary digits.
            binIntegerPart += decIntegerPart % 2;
            decIntegerPart = Math.trunc(decIntegerPart / 2);
        }

        // Invert the digits as one would do with pen and paper.
        binIntegerPart = invertString(binIntegerPart)
    }


    // Convert fractional decimal part to binary
    // -------------------------------------------------
    if (decFractionalPart > 0) {

        let currentBinDigit: number = 0;

        do {
            decFractionalPart *= 2;
            currentBinDigit = Math.trunc(decFractionalPart);
            decFractionalPart -= currentBinDigit;
            binFractionalPart += currentBinDigit;

            // For simplicity's sake, limit to 7 fractional digits (roughly 0.01 decimal precision)
            if (binFractionalPart.length == 7) break;
        } while (decFractionalPart > 0)
    }

    // Concatenated integer and decimal binary parts if necessary and return signed result.
    // ------------------------------------------------------------------------------------
    let result = binFractionalPart ? `${binIntegerPart}.${binFractionalPart}` : binIntegerPart

    return isNegative ? `-${result}` : result;
}


// Done with a loop instead of array methods (reverse or reduce) for the sake of JS beginners.
const invertString = (input: string): string => {
    let invertedString: string = '';

    for (let i = input.length - 1; i >= 0; i--) {
        invertedString += input[i];
    }

    return invertedString;
}

// Run an example:
console.log(convertDecToBin(57.8125))

```

You can try this code at https://www.typescriptlang.org/play/
