
## Before the audit
### Bare minimum quality checklist

- [ ] Use the latest major version of Solidity.
- [ ] Use known/established libraries where possible. OpenZeppelin contracts are preferred because they prioritize security above all else, and most auditors are already familiar with them.
- [ ] Contracts should compile without any errors or warnings from the compiler.
- [ ] All public/external functions should have [NatSpec documentation](https://docs.soliditylang.org/en/develop/natspec-format.html). Consider this part of the public interface of the contract.
- [ ] Any `public` function that can be made `external` should be made `external`. This is not just a gas consideration, but it also reduces the cognitive overhead for auditors because it reduces the number of possible contexts in which the function can be called.
- [ ] Have tests for all "happy path" user stories. All tests should be passing.
- [ ] Use the [Checks-Effects-Interactions pattern](https://docs.soliditylang.org/en/v0.8.13/security-considerations.html#use-the-checks-effects-interactions-pattern) everywhere possible. Otherwise use reentrancy guards. Treat all token and ETH transfers as 'interactions'.
- [ ] Focus on the readability and understanability of your code. E.g., choose variable and function names that are human-readable. It should be easy for any developer to read your code and understand what it's doing. Readability is a security consideration.
- [ ] Avoid using assembly as much as possible. Use of assembly increases audit times because it throws away Solidity's guardrails and must be checked much more carefully.
- [ ] Have good documentation and/or code comments for (i) what your contracts are trying to accomplish at a high level and (ii) what all the primary functions (external/public mutable functions) are trying to accomplish. The faster auditors can onboard to your code, the more time they can spend trying to break it.
- [ ] Run the code through a spellchecker.
- [ ] Have at least one trusted Solidity dev or security person outside your organization sanity check your contracts. If your code is a tire fire and in need of major changes, you want to hear about that early and from a trusted friend (for free) rather than after an expensive audit.

### Nice to haves
- [ ] Run a static analysis tool (e.g., [Slither](https://github.com/crytic/slither) or [MythX](https://mythx.io/)) on your code and consider what it tells you. They very often raise flags for non-issues, but they can sometimes catch low-hanging fruit, so it's worth doing.
- [ ] Write negative tests. E.g., if users should NOT be able to withdraw within 100 blocks of depositing, then write a test where a users tries to withdraw early and make sure the user's attempt fails.
- [ ] Write invariants and fuzz test them.
- [ ] You can also try to use formal verification tools to verify the invariants, but be aware that -- in practice -- current formal verification tools are rarely useful for anything non-trivial.
- [ ] Write down your security assumptions. This doesn't have to be super formal. E.g., "We assume that the `owner` is not malicious, that the Chainlink oracles won't lie about the token price, that the Chainlink oracles will always report the price at least once every 24 hours, that all tokens that the `owner` approves are ERC20-compliant tokens with no transfer hooks, and that there will never be a chain reorg of more than 30 blocks". This helps you understand how things could possibly go wrong _even if your contracts are bug-free_. Auditors may also be able to tell you whether or not your assumptions are realistic. They may also be able point out assumptions you're making that you didn't realize you were making.
- [ ] If you're unsure about something in your own code, or there are areas where you'd like auditors to spend more time, make a list of these to share with the auditors.

## After the audit
- [ ] Have a security contact email displayed prominently in your GitHub `README.md` file. Make sure someone on your team actually receives those emails.
- [ ] Make sure your GitHub repo lists the addresses where your contracts are deployed.
- [ ] Make sure your UI links to your GitHub repo.
- [ ] If you received a large number of recommended changes to your code during the audit, you should strongly consider getting a second audit after you make the changes, ideally from another auditor. Audits with long lists of issues often indicate that the auditors would have found even more issues given more time.
- [ ] Verify your contracts on Etherscan.
- [ ] Set up a bug bounty program. [Immunify](https://immunefi.com/) or [HackerOne](https://www.hackerone.com/) can help coordinate it.
- [ ] Set up monitoring and alerting. For example, you can have a script to monitor for new governance proposals and be alerted when they occur. Or, if you're using a TWAP, have a script that checks the TWAP every block, compares it against the price feed from a CEX, and alerts you if it's every different by more than 10%. Or an alert to let you know when more than 20% of the tokens in your contract have been removed in a single tx. Things like that. You want to be on top of what's happening with your project so you can respond quickly to security incidents.
- [ ] Create an incident response plan. In the event of a hack, know ahead of time who will be in the war room, which platform(s) (e.g., Discord, Signal, etc) and which channels you'll use to communicate. For the core response group, having voice comms is very helpful. Know who you will reach out to for help. Know what your first steps will be (e.g.: pause contracts, reach out to security partners, alert users that an issue is happening via Twitter/Discord, etc). Know who will take on what roles: Who will communicate with the public and outside partners? Who will start digging into the attacker's txs to find the vulnerability that was exploited? Who will be responsible for submitting the txs to (for example) pause the contracts? Having this all in a doc somewhere will be helpful so you can rely on it when the adrenaline is clouding your judgment.

