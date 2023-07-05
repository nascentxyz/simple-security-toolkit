# Audit Readiness Checklist

### Bare minimum quality checklist

- [ ]  Use the [latest](https://docs.soliditylang.org/en/latest/) major version of Solidity.
- [ ]  Use known/established libraries where possible. [OpenZeppelin contracts](https://github.com/OpenZeppelin/openzeppelin-contracts/) are preferred because they prioritize security above all else, and most auditors are already familiar with them. [Solmate contracts](https://github.com/Rari-Capital/solmate) can be a good alternative for functions where gas optimization is paramount.
- [ ]  Have tests for "happy path" user stories and tests that expect reverts for actions that are supposed to fail. All tests should be passing.
- [ ]  Incorporate fuzz tests. Fuzzing has proven to be highly effective in uncovering bugs. Tools like [Foundry](https://github.com/foundry-rs/foundry) and [Echidna](https://github.com/crytic/echidna) allow both stateless and stateful fuzzing. The [Foundry Invariants reference guide](https://book.getfoundry.sh/forge/invariant-testing?highlight=invariant#invariant-testing) provides an overview of how to set up your contract invariants for fuzzing. If you're using [Hardhat](https://github.com/NomicFoundation/hardhat), it is worthwhile to also include one of the above tools to enhance your testing capabilities.
- [ ]  Run a static analysis tool ([Slither](https://github.com/crytic/slither) preferred, [MythX](https://mythx.io/) is an alternative) on your code and consider what it tells you. They very often raise flags for non-issues, but they can sometimes catch low-hanging fruit, so it's worth doing.
- [ ]  Prepare the deploy script and mock upgrade scripts (if applicable) and include them as part of audit scope. Deployments and upgrades are as important as runtime code and require the same amount of security attention.
- [ ]  Document all functions. Use [NatSpec documentation](https://docs.soliditylang.org/en/develop/natspec-format.html) for `public`/`external` functions. Consider this part of the public interface of the contract.
- [ ]  Contracts compile without any errors or warnings from the compiler.
- [ ]  Run the code through a spellchecker.
- [ ]  Avoid using assembly as much as possible. Use of assembly increases audit times because it throws away Solidity's guardrails and must be checked much more carefully.
- [ ]  Document use of `unchecked`. Concretely describe *why* it is safe to not perform arithmetic checks on each code block. Preferably for each operation.
- [ ]  Any `public` function that can be made `external` should be made `external`. This is not just a gas consideration, but it also reduces the cognitive overhead for auditors because it reduces the number of possible contexts in which the function can be called.
- [ ]  Use the [Function Requirements-Effects-Interactions-Protocol Invariants (FREI-PI) pattern](https://www.nascent.xyz/idea/youre-writing-require-statements-wrong) everywhere possible. Treat all token and ETH transfers as 'interactions'. Verify your system-level protocol invariants still hold at the end of each interaction.
- [ ]  Have at least one trusted Solidity dev or security person outside your organization sanity check your contracts. If your code is a tire fire and in need of major changes, you want to hear about that early and from a trusted friend (for free) rather than after an expensive audit.


### Nice to haves

- [ ]  Use formal verification tools to verify the invariants, but be aware that -- in practice -- current formal verification tools aren't a silver bullet and have some edge cases that aren't handled. [Certora](https://www.certora.com/) and [Runtime Verification](https://runtimeverification.com/) are examples of commonly used (paid) tools in this category.
- [ ]  Write down your extraneous security assumptions. This doesn't have to be super formal. E.g., "We assume that the `owner` is not malicious, that the Chainlink oracles won't lie about the token price, that the Chainlink oracles will always report the price at least once every 24 hours, that all tokens that the `owner` approves are ERC20-compliant tokens with no transfer hooks, and that there will never be a chain reorg of more than 30 blocks." This helps you understand how things could possibly go wrong *even if your contracts are bug-free*. Good auditors will be able to help you understand you whether your assumptions are realistic. They may also be able point out assumptions you're making that you didn't realize you were making.
- [ ]  If you're unsure about something in your own code, or there are areas where you'd like auditors to spend more time, make a list of these to share with the auditors.
- [ ]  Add scoping details for auditors. The form used in preparation for [Code4rena](https://code4rena.com/) is provided as an example in the collapsible section below.
<details> <summary>Audit Scoping Details</summary>
  
  - If you have a public code repo, please share it here:
  - How many contracts are in scope?:
  - Total SLoC for these contracts?:
  - How many external imports are there?:
  - How many separate interfaces and struct definitions are there for the contracts within scope?:
  - Does most of your code generally use composition or inheritance?:
  - How many external calls?:
  - What is the overall line coverage percentage provided by your tests?:
  - Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?:
  - If so, please describe required context:
  - Does it use an oracle?: 
  - Does the token conform to the ERC20 standard?: 
  - Do you expect ERC721, ERC777, FEE-ON-TRANSFER, REBASING or any other non-standard ERC will interact with the smart contracts?:
  - Are there any novel or unique curve logic or mathematical models?: 
  - Does it use a timelock function?:
  - Is it an NFT?: 
  - Does it have an AMM?: 
  - Is it a fork of a popular project?: 
  - Does it use rollups?:
  - Is it multi-chain?:
  - Does it use a side-chain?:
  - Describe any specific areas you would like addressed. E.g. Please try to break XYZ.":
</details>
  