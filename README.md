# ✨ So you want to sponsor a contest

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos:

- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted. (We'll set that one up later.)

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest is over and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Contest scoping

Sherlock is a protocol on the Ethereum blockchain that protects Decentralized Finance (DeFi) users from exploits with proprietary security analysis and protocol-level coverage.

There are 3 main participants in the Sherlock ecosystem:

- Protocols accrue debt to the solution over time, in return, their end users are compensated in case of an exploit at protocol level.
- Stakers provide capital to the solution, their capital is used in case of an exploit payout. In return for the risk they are rewarded SherX, a token which can be redeemed for underlying protocol premium tokens.
- Security team (watsons) is a decentralized group incentivized to keep protocols safe and signal protocol premium pricing to the Sherlock DAO.

## Smart Contracts

| Contract                      | Lines | External Contracts | Libraries (OpenZeppelin)     | Libraries (other) | Libraries (internal)             | Info                                                                                 |
| ----------------------------- | ----- | ------------------ | ---------------------------- | ----------------- | -------------------------------- | ------------------------------------------------------------------------------------ |
| `ForeignLock.sol`             | 2     | X                  | Ownable                      |                   |                                  | ERC20 tokens used to represent tokens staked in the solution                         |
| `NativeLock.sol`              | 5     | X                  | Ownable                      |                   |                                  | ERC20 token used to represent SHERX staked in the solution                           |
| `facets/Gov.sol`              | 142   | X                  | SafeMath, SafeERC20          | LibDiamond        |                                  | Facet containing the logic used to govern the solution.                              |
| `facets/GovDev.sol`           | 6     | X                  |                              | LibDiamond        |                                  | Facet used for remove/add/update solidity code in the solution.                      |
| `facets/Manager.sol`          | 124   | X                  | SafeMath                     |                   | LibPool, LibSherX                | Facet used to mananage protocol premiums and the amount of SHERX being minted        |
| `facets/Payout.sol`           | 66    | X                  | SafeMath, SafeERC20          | LibDiamond        | LibPool, LibSherX, LibSherXERC20 | Facet used to initiate payouts in case of an exploit                                 |
| `facets/PoolBase.sol`         | 143   | X                  | SafeMath, SafeERC20          |                   | LibPool                          | Facet used for every token in the solution. For staker actions and protocol actions. |
| `facets/PoolDevOnly.sol`      | 2     | X                  | SafeERC20                    | LibDiamond        | LibPool                          | Facet used to whitelist a certain address to access the staking function.            |
| `facets/PoolOpen.sol`         | 12    | X                  | SafeERC20                    |                   | LibPool                          | Facet used to for staking without whitelist.                                         |
| `facets/PoolStrategy.sol`     | 36    | X                  | SafeMath, SafeERC20          |                   |                                  | Facet used for yield strategies for staker tokens                                    |
| `facets/SherX.sol`            | 124   | X                  | SafeMath, SafeERC20          |                   | LibPool, LibSherX, LibSherXERC20 | Facet used for SHERX related functions, like redeeming underlying                    |
| `facets/SherXERC20.sol`       | 44    | X                  | SafeMath                     | LibDiamond        | LibSherXERC20                    | Facet used for the ERC20 function of SHERX                                           |
| `libraries/LibPool.sol`       | 35    | X                  | SafeMath, SafeERC20          |                   |                                  | Internal libary used for token pool related functions                                |
| `libraries/LibSherX.sol`      | 52    | X                  | SafeMath, SafeERC20          |                   | **LibPool, LibSherXERC20**       | Internal libary used for SHERX related functions                                     |
| `libraries/LibSherXERC20.sol` | 10    | X                  | SafeMath                     |                   |                                  | Internal libary used for ERC20 related functions                                     |
| `strategies/AaveV2.sol`       | 26    | Aave V2            | SafeMath, SafeERC20, Ownable |                   |                                  | Strategy used to deposit tokens into Aave V2.                                        |

Sherlock V1 (solution) is built using Diamonds (EIP 2535), basically meaning all the contracts in the `facets` folder live at the same address and share the same storage. The solution is ERC20 compatible, the ERC20 exposed functions are defined in `facets/SherXERC20.sol`.

Natspec comments can be read from the `interfaces` folder.

## Areas of concern

- The payout function is long and complex
- Redeeming SHERX
- `_doYield()` function in the SherX facet.
- The different functions for updating premiums and token pricing. Focussing on economic exploits.

# Contest prep

## 🐺 C4: Contest prep

- [x] Rename contest H1 below
- [x] Add link to report form in contest details below
- [x] Update pot sizes
- [x] Fill in start and end times in contest bullets below.
- [ ] Move any relevant information in "contest scope information" above to the bottom of this readme.
- [x] Add matching info to the [code423n4.com public contest data here](https://github.com/code-423n4/code423n4.com/tree/main/data/contests))
- [ ] Delete this checklist.

# Sherlock contest details

- $72,000 USDC main award pot
- $8,000 USDC gas optimization award pot
- Join [C4 Discord](https://discord.gg/EY5dvm3evD) to register
- Submit findings [using the C4 form](https://code423n4.com/2021-07-sherlock-contest/submit)
- [Read our guidelines for more details](https://code423n4.com/compete)
- Starts 2021-07-22 00:00 UTC
- Ends 2021-07-28 23:59 UTC

This repo will be made public before the start of the contest.

## More info

- [Conceptual overview (YouTube)](https://youtu.be/yMSPLfgt9To)
- [Code overview (YouTube)](https://youtu.be/lWkTmi--Ehg)
- [Sherlock Docs](https://docs.sherlock.xyz)
- [Sherlock Goerli](https://goerli.sherlock.xyz); **WARNING** code doesn't match 100% with the code in this repo.
