# *CONFIDENTIAL - DO NOT SHARE*

# **[*Project Name*]** Incident Response Plan

<!--
Bold + Italics + Square Brackets: Fields to be filled in
-->

War Room Channel(s): **[Discord channel, Signal group, Zoom, etc.]**

War Room Participants: **[*Names and contact info*]**

### Immediate Steps

- [ ]  Review exploit transactions to identify vulnerability - **[*Person responsible*]**
    - Tools used:
        - [Tenderly Debugger](https://dashboard.tenderly.co/tx/mainnet/0xf427afc17bd30a84f4b47dc2eaa176115cf28bdea1110245d3b0948ca3b6595c/debugger)
        - [Foundry Debugger](https://book.getfoundry.sh/forge/debugger.html?highlight=debugger#debugger)
        - [ethtx.info](https://ethtx.info)
- [ ]  Pause contracts or take other defensive action - **[*Person responsible for coordinating*]**
    - Steps:
        - **[*Who, how, what addresses*]**
    - Review Transaction(s) - **[*Person responsible*]**
    - You do NOT want to be scrambling to figure out who can sign to take defensive actions. Use of [OpenZeppelin Defender](https://www.openzeppelin.com/defender) is highly recommended, as is having prepared defensive action scripts in advance that can be deployed as per the [Pre-Launch Security Checklist](https://github.com/nascentxyz/simple-security-toolkit/blob/main/pre-launch-security-checklist.md).
- [ ]  Review all contracts to identify knock-on vulnerabilities. Pause those as necessary - **[*Person responsible*]**
- [ ]  Update UI to reflect current status - **[*Person responsible*]**
- [ ]  Contact security partners - **[*Person responsible*]**
    - **[*List of past auditors and their contact info or location of shared channel*]**
        - Your auditors will want to assist to the extent they are able, even if primarily to protect their own reputations
    - DO NOT LET ANYONE OUTSIDE OF YOUR CIRCLE OF TRUST INTO THE WAR ROOM
- [ ]  Post message to users in Discord - **[*Person responsible*]**
    - Update regularly as meaningful new information or developments are available
        - Even if nothing new is known, updates every 4 to 12 hours will help reassure your community that you are working to address the situation
        - Run all messages by the vulnerability reviewer(s) to ensure no information is shared that inadvertently puts security at risk or commits to specific remediation before all facts are known
- [ ]  Post message to users on Twitter - **[*Person responsible*]**
    - Update regularly as meaningful new information or developments are available
        - Even if nothing new is known, updates every 4 to 12 hours will help reassure your community that you are working to address the situation
        - Run all messages by the vulnerability reviewer(s) to ensure no information is shared that inadvertently puts security at risk or commits to specific remediation before all facts are known

### After Immediate Steps Are Addressed

- [ ]  Draft and post full public postmortem
- [ ]  Prepare and deploy patch for contracts, ideally following [development process](development-process.md) guidelines
    - [ ]  Have it reviewed and signed off on by past auditors
    - [ ]  Have it reviewed by as many trusted members of the team and community as possible
    - [ ]  If the patch or potential interactions are complex enough to warrant it, strongly consider a short [Code4rena](https://code4rena.com/) contest (can be started within 48 hours)
