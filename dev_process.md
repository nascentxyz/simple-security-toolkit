# Development Process
One of the most crucial factors in having a secure codebase is a solid development process. "An ounce of prevention is worth a pound of cure". This document gives an *example* development process that we, at Nascent, have found works well. In addition to getting better over code quality, this process serves as a companion to our [audit readiness checklist](https://github.com/nascentxyz/nascent-security/blob/main/audit-readiness-checklist.md). If you follow the below process, you should naturally check off most of the items on the checklist.

```
Feature Request received
 |
 └> Specification Write up
     |
     └> Evaluation (Time + complexity + risks)
        |
        └> Implementation
           |
           └> Test
              |
              └> Deployment
                 |
                 └> Monitor
```
## Specification
Make note of the kinds of variables that affect the feature:
1. User input?
2. Time?
3. Other protocols?
4. Existing state?

## Note on Evaluation
Make dedicated time to evaluate:
1. How long will this realistically take? Without specification most estimates will be wrong.
2. Is the specification overly complex? Complexity leads to bugs and worse overall code
3. What are the risks associated with this feature? Spend ample time evaluating this. Consider every module the feature may touch. Then go back through excluded modules and *ensure* they cannot be affected.

Now that you have millions of dollars at risk, you have to consider each feature may put *all* of it at risk. The cost of a misstep will likely be in the millions of dollars. 
## Implementation -> Monitor
1. Draft PR
  - Include the feature request + specification
2. Initial implementation pass
  - Get an initial implementation in place
  - document all functions' intended behavior + inline documentation
3. Initial concrete [tests](https://book.getfoundry.sh/forge/tests.html)
  - Write an initial test for the implementation. Each write to storage should be checked, each revert should be checked. Line-by-line of the implementation, look for storage writes
  - For non-state changing functions, good practice is have a test contract that inherits the contract that does the math 
4. Cleanup implementation
5. Improve tests, then move back to 4 based on any found bugs
6. [Fuzz test](https://book.getfoundry.sh/forge/fuzz-testing.html)
  - Now that you are pretty confident in your implementation, throw a monkey at it. A good fuzz test should consider all valid inputs, and include as many state transition assertions as possible (think: is this function monotonically in/decreasing, should it be always less than something else,  etc.)
7. Move back to step 4
8. Integration test
  - Your feature now likely does exactly what you think it does. In a complex system, that is not enough. Ideally you have tested how it affects the entire system as well. Invariant tests are coming soon to [Foundry](https://github.com/foundry-rs/foundry), which should help integration style tests, but make do with what you can with fuzz tests on a broader basis (not just for a single function)
9. Run [slither](https://github.com/crytic/slither) & implement fixes, return to step 4 if needed
10. Cleanup documentation
11. PR review
  - The implementer is just the first line of defense. If you are a reviewer, confirm that the implementer followed the above principals (test-per-state-transition, test-per-revert, fuzz test, and integration test)
  - Review the documentation and ensure the implementation matches the documented behavior. If it does not, touch base with the implementer and confirm which needs to be updated.
12. Deployment script
  - Write the deployment script
  - Most likely Foundry has scripting ready when you are reading this. Check out this [PR](https://github.com/foundry-rs/foundry/pull/1208)
13. Deployment test
  - Ensure deployment goes exactly as planned by writing a test testing *every state transition* and make sure no changes unexpectedly happen. One way to accomplish this is the `record` cheatcode. Create a list of your entire protocol's addresses, call record, perform the upgrade, call `accesses` for each address of your protocol. Ensure there are no slots/addresses that unexpectedly changed.
14. Audit
  - Consider if this feature/contract needs an audit. Always lean towards being safe rather than sorry.
  - Have the complexity & code size inform if and who should audit your contracts
  - At Nascent, we have an internal tier list of quality of auditors. You should check with other developers about who are good auditors and who aren't. Some auditors are just there to check a box for a protocol, others actually care about finding vulnerabilities.
15. Implement audit fixes
16. Setup monitoring service
  - Have an internal tool that monitors important aspects of your system.
  - Use tools like [Check the Chain](https://github.com/fei-protocol/checkthechain), [Grafana](https://grafana.com/), or use an off-the-shelf monitoring tool like [tenderly](https://tenderly.co/alerting)
17. Deploy
  - Congrats, you probably just crushed 99% of solidity devs in terms of a secure development +  deployment
18. Monitor next couple hours
  - Use the monitoring service you set up to watch carefully for unexpected behaviors and be ready to take action
19. Relax, have a beer, you earned it.