# Popular ISO 20022 messages, by scheme

**Last reviewed:** 2026-11-11, by the owner of the plugin.

**What was checked.** On the review date the version notes below were compared with the schemes' public documents; the other columns (the schemes that use each message, and what each message replaced) were carried over from the table of the first release of this plugin's server (reviewed in the first quarter of 2026) and have not been re-verified. A cell that is not marked "checked" is a claim of ours, not a quotation of a scheme.

**Sources** (public): Swift CBPR+ usage guidelines and release notes; the Federal Reserve's Fedwire Funds Service and FedNow Service ISO 20022 documentation; Bank of England CHAPS and RTGS documentation; ECB T2 specifications; the European Payments Council's SEPA rulebooks and implementation guidelines (2025 edition); BIS CPMI harmonised data requirements (October 2023).

**Ranking.** Rank 1 is the message used on the most schemes of this table; payments first, then reporting, then securities and administration. "Used by" lists the schemes that use or require the message.

## Payments clearing and settlement between banks (`pacs`)

| Rank | Message | In plain words | Replaces | Used by |
|---|---|---|---|---|
| 1 | `pacs.008` | a customer's payment sent from bank to bank | MT 103 | Swift CBPR+, SEPA SCT, SEPA SCT Inst, T2, CHAPS, Fedwire, CHIPS, FedNow, RTP |
| 2 | `pacs.009` | a transfer between banks with no customer payment behind it (liquidity, cover) | MT 202, MT 205 | Swift CBPR+, T2, CHAPS, Fedwire, CHIPS |
| 3 | `pacs.002` | a bank's accept or reject reply to a payment | MT 012, proprietary acknowledgements | Swift CBPR+, SEPA SCT, SEPA SCT Inst, T2, CHAPS, FedNow, RTP |
| 4 | `pacs.004` | the return of a payment that has already settled | MT 202 used as a return | Swift CBPR+, SEPA SCT, SEPA SCT Inst, T2, CHAPS, FedNow, Fedwire |
| 12 | `pacs.003` | a direct debit collected from bank to bank | proprietary formats | SEPA SDD Core, SEPA SDD B2B |
| 13 | `pacs.028` | a question about the status of a payment sent earlier | MT 195, MT 295 (in part) | FedNow, Swift CBPR+, RTP |

## Payment initiation from a corporate to its bank (`pain`)

| Rank | Message | In plain words | Replaces | Used by |
|---|---|---|---|---|
| 5 | `pain.001` | the credit transfer instruction a company gives its bank; the common format for treasuries and ERP systems | MT 101, proprietary formats | SEPA SCT, SEPA SCT Inst, Swift CBPR+ (origination), CHAPS, FedNow |
| 6 | `pain.002` | the bank's status reply to a `pain.001` or `pain.008` | proprietary notifications | SEPA SCT, SEPA SCT Inst, SEPA SDD, Swift CBPR+ (origination) |
| 11 | `pain.008` | the direct debit instruction a company gives its bank | proprietary formats, EDIFACT | SEPA SDD Core, SEPA SDD B2B |
| 15 | `pain.013` | a request to pay, sent by the party to be paid before any transfer is made | proprietary formats | FedNow, RTP, SEPA Request-to-Pay |

## Cash management and account reporting (`camt`)

| Rank | Message | In plain words | Replaces | Used by |
|---|---|---|---|---|
| 7 | `camt.053` | the end-of-day statement of an account, used for reconciliation | MT 940, MT 950 | Swift CBPR+, SEPA, T2, CHAPS, Fedwire (reporting) |
| 8 | `camt.054` | a debit or credit notification for one transaction, sent as it happens | MT 900, MT 910 | Swift CBPR+, SEPA SCT Inst, T2, CHAPS |
| 9 | `camt.052` | an intraday report of balances and transactions for liquidity management | MT 942 | Swift CBPR+, SEPA, T2, CHAPS |
| 10 | `camt.056` | a request to cancel or recall a payment, in flight or settled | MT 192, MT 292 | Swift CBPR+, SEPA SCT, SEPA SCT Inst, T2, CHAPS, FedNow |
| 14 | `camt.029` | the answer to a cancellation request or an inquiry; it closes an exception or recall flow | MT 196, MT 296 | Swift CBPR+, T2, FedNow, CHAPS |

## Securities settlement (`sese`) and administration (`admi`)

| Rank | Message | In plain words | Replaces | Used by |
|---|---|---|---|---|
| 20 | `sese.023` | an instruction to settle a securities trade at a depository | MT 540 to MT 543 | Euroclear, Clearstream, DTCC |
| 21 | `sese.024` | a status update on a settlement instruction | MT 548 | Euroclear, Clearstream, DTCC |
| 22 | `admi.007` | an infrastructure-level receipt that confirms or rejects a message received | proprietary formats | FedNow |

## Versions the schemes pin (checked 2026-11-11)

| Scheme | Message | Version | Where it was checked |
|---|---|---|---|
| Swift CBPR+ | `pacs.008` | `pacs.008.001.08` (the usage guidelines of the 2025 and 2026 releases) | Swift's release documentation and the guidelines published by participants |
| SEPA SCT, SEPA SCT Inst | `pacs.008` | `pacs.008.001.08` (2025 implementation guidelines, on the 2019 message versions) | the EPC's 2025 inter-PSP implementation guidelines |
| SEPA SCT, SEPA SCT Inst | `pacs.004` | `pacs.004.001.09` (returns) | the EPC's 2025 inter-PSP implementation guidelines |
| SEPA SCT | `pain.001` | `pain.001.001.09` | the EPC's 2025 customer-to-PSP implementation guidelines |
| SEPA SDD | `pain.008` | `pain.008.001.08` | the EPC's 2025 customer-to-PSP implementation guidelines |
| Fedwire | `pacs.008` | `pacs.008.001.08` | the Federal Reserve's Fedwire ISO 20022 documentation |

Not checked, and therefore not stated: the versions of `pacs.009` and of the FedNow messages, and any version newer than those above. The server lists every version of a message; the scheme's own guidelines say which one it pins.

## Two facts about Swift's cutover

Since 22 November 2025, ISO 20022 is the required format for cross-border payment instructions on Swift (the coexistence with the older MT messages ended for instructions such as MT 103 and MT 202). Reporting messages such as MT 940 were not withdrawn then; they are still supported and deprecated. So the "replaces" column names what a message succeeds, not what is switched off.
