int main() {

char a[8] = {1, 2, 3, 4, 5, 6, 7, 8};

char b[8] = {1, 1, 1, 1, 1, 1, 1, 1);

char c[8];

asm volatile (

"movq (%1), %%mm0\n"

/* Load the first array into mm0 */

"movq (%2), %%mm1\n"

/* Load the second array into mm1 */

"paddb %%mm1, %%mm0\n"

/* Add the bytes in mm1 to mm0 */

"movq %%mm0, (%0)\n"

/* Store the result back into the c array */

"emms\n"

/* Clear the MMX state to allow FP operations

afterwards */

:

: "r"(c), "r"(a), "r"(b)

: "%mm0", "%mm1"

/* Clobber list */

);

for(int i = 0; i < 8; i++) {

if (i>0) printf(",");

printf("%d", c[i]);

}

printf("\n");

return 0;

}
