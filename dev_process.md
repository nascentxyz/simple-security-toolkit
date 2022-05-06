# Development Process
```
Feature Request received
 |
 └> Specification Writeup
     |
     └> Evaulation (Time + complexity + risks)
        |
        └> Implementation
```
## Specification
Make note of the kinds of variables that affect the feature:
1. User input?
2. Time?
3. Other protocols?
4. Existing state?

## Note on Evaulation
Make dedicated time to evaluate:
1. How long will this realistically take? Without specification most estimates will be wrong.
2. Is the specification overly complex? Complexity leads to bugs and worse overall code
3. What are the risks associated with this feature? Spend ample time evaluating this. Consider every module the feature may touch. Then go back through excluded modules and *ensure* they cannot be affected.

Now that you have millions of dollars at risk, you have to consider each feature may put *all* of it at risk. The cost of a misstep will likely be in the millions of dollars. 
## Implementation
1. Draft PR
  - Include the feature request + specification
2. Initial implementation pass
  - Get an initial implementation in place
3. Initial concrete tests
  - Write an initial test for the implementation. Each write to storage should be checked, each revert should be checked. Line-by-line of the implementation, look for storage writes
  - For non-state changing functions, good practice is have a test contract that inherits the contract that does the math 
4. cleanup implementation
5. improve test, then move back to 4
6. fuzz test
  - Now that you are pretty confident in your implementation, throw a monkey at it. A good fuzz test should consider all valid inputs, and include as many state transition assertions as possible (think: is this function monotonically in/decreasing, should it always be less than something else,  etc.)
7. integration test
  - Your feature now likely does exactly what you think it does. In a complex system, that is not enough. Ideally you have tested how it affects the entire system as well. Invariant tests are coming soon to foundry, which should help integration style tests, but make do with what you can with fuzz tests on a broader basis (not just for a single function)
8. pr review
  - The implementer is just the first line of defence. If you are a reviewer, confirm that the implementer followed the above principals (test-per-state-transition, test-per-revert, fuzz test, and integration test)
9. deployment script
  - Write the deployment script
10. deployment test
  - Ensure deployment goes exactly as planned by writing a test testing *every state transition* and make sure no changes unexpectedly happen. One way to accomplish this is the `record` cheatcode. Create a list of your entire protocol's addresses, call record, perform the upgrade, call `accesses` for each address of your protocol. Ensure there are no slots/addresses that unexpectedly changed. 
11. deploy
  - Congrats, you probably just crushed 99% of solidity devs in terms of a secure development +  deployment
12. Monitor next couple hours
13. Relax, have a beer