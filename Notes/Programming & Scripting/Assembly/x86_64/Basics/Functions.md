
**Things to consider before calling a function:** 

1. Save `registers` on the `stack`
2. Pass `function args `
3. Fix `stack` alignment
4. Get `function` return value (`in rax`)

**Things to consider when writing a function:** 

1. Saving `callee saved registers (rbx & rbp)`
2. Get args from 