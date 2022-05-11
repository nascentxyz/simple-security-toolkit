# **[*Project Name*]** Incident Response Plan

<!--
Bold + Square Brackets: Fields to be filled in
Italics: Explanatory text
-->

War Room Channel(s): **[Discord channel, Signal group, Zoom, etc.]**

War Room Participants: **[*Names and contact info*]**

### Immediate Steps

- [ ]  Review exploit transactions to identify vulnerability - **[*Person responsible*]**
    - Tools used:
        - [Tenderly Debugger](https://dashboard.tenderly.co/tx/mainnet/0xf427afc17bd30a84f4b47dc2eaa176115cf28bdea1110245d3b0948ca3b6595c/debugger)
        - [Foundry Debugger](https://book.getfoundry.sh/forge/debugger.html?highlight=debugger#debugger)
        - [ethtx.info](https://ethtx.info)
- [ ]  Pause contracts - **[*Person responsible for coordinating*]**
    - You do NOT want to be scrambling to figure out who can sign to take defensive actions. Use of [OpenZeppelin Defender](https://www.openzeppelin.com/defender) is highly recommended.
    - **[*List steps for who/how/what addresses*]**
- [ ]  Review all contracts to identify knockon vulnerabilities. Pause those as necessary - **[*Person responsible*]**
- [ ]  Update UI to reflect current status - **[*Person responsible*]**
- [ ]  Contact security partners - **[*Person responsible*]**
    - DO NOT LET ANYONE OUTSIDE OF CIRCLE OF TRUST INTO THE WAR ROOM
    - **[*List of past auditors and their contact info or location of shared channel. Your auditors will want to assist to the extent they are able, even if primarily to protect their own reputation.*]**
- [ ]  Post message to users in Discord - **[*Person responsible*]**
    - Update regularly as meaningful new information or developments are available
- [ ]  Post message to users on Twitter - **[*Person responsible*]**
    - Update regularly as meaningful new information or developments are available

### After Immediate Steps Are Addressed

- [ ]  Draft and post full public post-mortem
- [ ]  Prepare and deploy patch for contracts, ideally following [dev process](https://github.com/nascentxyz/simple-security-toolkit/dev_process) guidelines
    - [ ]  Have it reviewed and signed off on by past auditors
    - [ ]  Have it reviewed by as many trusted members of the team and community as possible
    - [ ]  If the patch or potential interactions are complex enough to warrant it, strongly consider a short [Code4rena](https://code4rena.com/) contest (can be started within 48 hours)
