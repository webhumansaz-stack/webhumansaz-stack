# FlapJax Receipt Relay Q31 — Independent On-Chain ERC-8004 Identity & Payout Verification

- **Auditor / Operator:** Azer Shahbazzade (`@webhumansaz-stack`)
- **First-Timer Status:** `first=yes` (never previously paid by FlapJax on any venue)
- **Bound BSC & Base Wallet Address (EOA):** `0x789e5CBa7d0810Cd29D313437c3Ff3Ff74A0ea72`
- **Taskmarket Task Reference:** `TSK-04FYDMHT` (`0x1163890296673d6f0cbd2f63eccd2f3ac3f96fcf0864768509ca46feae609104`)
- **Pinned BNB Smart Chain (chainId 56) Block:** `128,367,844` (`0x7a6bce4`)
- **Pinned Block Hash:** `0x90769f285904ebe7c894d964e5578c8acc725e60ad73a820fdfb770e10cf02ee`
- **Pinned Block Timestamp:** `2026-10-18T13:09:57Z` (`1792328997`)
- **RPC Endpoint Used:** `https://bsc-dataseed.binance.org/`

---

## 1. Executive Verdict Summary (Statement-by-Statement Check)

| # | FlapJax Statement Under Audit | On-Chain / URI Finding at Block `128,367,844` | Verdict |
|---|---|---|---|
| **1** | FlapJax registered as ERC-8004 agent `364913` on BNB Chain Identity Registry (`0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`) on `2026-10-06` | Registration tx `0xd6ffcd60cc49f6936977d4636e290e7f7a27869b4093e1bd9611cf144b82c6aa` in block `126,061,874` (`2026-10-06T12:41:51Z`, status `0x1`) minted ERC-721 token `364913` (`0x59171`) from `0x0000...0000` to `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | **MATCH** |
| **2** | The owner of agent `364913` (`ownerOf(364913)`) is the treasury `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `eth_call` (`0x6352211e` + `pad32(364913)`) at block `128,367,844` returns `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | **MATCH** |
| **3** | The agent wallet (`getAgentWallet(364913)`) is the treasury `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `eth_call` (`0x00339509` + `pad32(364913)`) at block `128,367,844` returns `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | **MATCH** |
| **4** | Stored agent URI (`tokenURI(364913)`) resolves to a registration file naming `FLAPJAX` (`0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df`) on chain `56` and the same agent wallet `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `tokenURI(364913)` (`0xc87b56dd`) returns `https://flapjax.surge.sh/.well-known/agent-registration.json` (1,656 bytes, SHA-256 `9e3153ed82c8fcc57d17784be710c9c5d285a78a42e267fd0418ecaf9546ceb9`), which declares `"token": "eip155:56:0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df"`, `"agentWallet": "eip155:56:0x7C34E9e21eE28A49Ff0b84B61774119E6633359f"`, and `"agentId": 364913` | **MATCH** |
| **5** | At least three Receipt Relay payouts from `2026-10-12` to `2026-10-16` came from that agent wallet (`0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`) | Verified 6 distinct Receipt Relay payout transactions across `2026-10-12` to `2026-10-16`; every ERC-20 `Transfer` log on `0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df` has `topic1 (from) = 0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` and receipt status `0x1` | **MATCH** |

---

## 2. Exact Registry Contract Reads at Pinned Block `128,367,844` (`0x7a6bce4`)

Target Contract: `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432` (BNB Chain ERC-8004 Identity Registry)
Agent ID: `364913` (`0x0000000000000000000000000000000000000000000000000000000000059171`)

1. **Registration Transaction (`2026-10-06`):**
   - **Tx Hash:** `0xd6ffcd60cc49f6936977d4636e290e7f7a27869b4093e1bd9611cf144b82c6aa`
   - **Block Number:** `126,061,874` (`0x7838c32`), mined `2026-10-06T12:41:51Z`
   - **Sender (`from`):** `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`
   - **ERC-721 Mint Log:** `Transfer(0x0000000000000000000000000000000000000000, 0x7C34E9e21eE28A49Ff0b84B61774119E6633359f, 364913)`
   - **Receipt Status:** `0x1` (Success) -> **MATCH**

2. **Owner Read (`ownerOf(364913)`):**
   - **Calldata:** `0x6352211e0000000000000000000000000000000000000000000000000000000000059171`
   - **Raw Result:** `0x0000000000000000000000007c34e9e21ee28a49ff0b84b61774119e6633359f`
   - **Decoded Address:** `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` -> **MATCH**

3. **Agent Wallet Read (`getAgentWallet(364913)`):**
   - **Calldata:** `0x003395090000000000000000000000000000000000000000000000000000000000059171`
   - **Raw Result:** `0x0000000000000000000000007c34e9e21ee28a49ff0b84b61774119e6633359f`
   - **Decoded Address:** `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` -> **MATCH**

4. **Stored Agent URI Read (`tokenURI(364913)`):**
   - **Calldata:** `0xc87b56dd0000000000000000000000000000000000000000000000000000000000059171`
   - **Decoded String:** `https://flapjax.surge.sh/.well-known/agent-registration.json`
   - **Registration File Verification:**
     - SHA-256 of fetched body: `9e3153ed82c8fcc57d17784be710c9c5d285a78a42e267fd0418ecaf9546ceb9`
     - `services[token].endpoint`: `eip155:56:0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df`
     - `services[agentWallet].endpoint`: `eip155:56:0x7C34E9e21eE28A49Ff0b84B61774119E6633359f`
     - `registrations[0]`: `agentId: 364913`, `agentRegistry: "eip155:56:0x8004A169FB4a3325136EB29fA0ceB6D2e539a432"` -> **MATCH**

---

## 3. Verified Receipt Relay Payout Transactions (`2026-10-12` to `2026-10-16`)

All transfers below emit `Transfer(address indexed from, address indexed to, uint256 value)` (`topic0 = 0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef`) on the `FLAPJAX` BEP-20 contract (`0x90c8889f428F9Ebb77BB8f15CAD3a50a9aC680df`) with `from = 0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` (the verified ERC-8004 agent wallet):

| Relay Ref | BSC Transaction Hash | Block | Timestamp (UTC) | Sender (`from`) | Recipient (`to`) | Amount (`FLAPJAX`) | Verdict |
|---|---|---|---|---|---|---|---|
| **Q06** | `0xffadd9467036e23f558df95338aad9ffcde86fcf3859fd09303a7d926b17817e` | `127,145,974` | `2026-10-12T04:20:42Z` | `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `0x5047384cdb02d1117abece1f7a01d2b0e39c793f` | `250,000` | **MATCH** |
| **Q18** | `0x99f9709a02f73985710b681c605bc95595b1de91f4989b83f993928cc6e4b28d` | `127,220,370` | `2026-10-12T13:39:37Z` | `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `0x4a6419ee9666acaa6755fc0aebb27f48743c1e1e` | `250,000` | **MATCH** |
| **Q18** | `0xbc92eaf19ab0c57ab6738532d998591146935ff8d14e3903cb7dae2743030b71` | `127,220,507` | `2026-10-12T13:40:39Z` | `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `0x5c7a79549e4413a52b1787b5c7e10368720d2358` | `250,000` | **MATCH** |
| **Q18 (2nd op)** | `0xa5b52de36c7a9167ec2c64a6f49af704e07deddd16a41328f0ade85c9c396837` | `127,229,962` | `2026-10-12T14:51:36Z` | `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `0xfd5efa5c47fb3be071b2fd38fc88dfa30318c5d2` | `100,000` | **MATCH** |
| **Q21** | `0x5758c6b9da70f08b90497c109c12b4f4e77c976d6631c0b2f35f199e7e7a56cb` | `127,793,184` | `2026-10-15T13:17:48Z` | `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `0x8c062dc5d775b7d5efc168f75a875f392f53dcdf` | `250,000` | **MATCH** |
| **Q22** | `0xebb3eddb88e06c13a10d5d43652d4f01b42a1d11867874208e73517488a0f08f` | `127,793,194` | `2026-10-15T13:17:53Z` | `0x7C34E9e21eE28A49Ff0b84B61774119E6633359f` | `0x8c062dc5d775b7d5efc168f75a875f392f53dcdf` | `100,000` | **MATCH** |

---

## 4. One-Liner Reproduction Commands (`curl` against any BSC RPC node)

```bash
# 1. ownerOf(364913) at block 128367844 (0x7a6bce4)
curl -s https://bsc-dataseed.binance.org/ -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":1,"method":"eth_call","params":[{"to":"0x8004A169FB4a3325136EB29fA0ceB6D2e539a432","data":"0x6352211e0000000000000000000000000000000000000000000000000000000000059171"},"0x7a6bce4"]}'

# 2. getAgentWallet(364913) at block 128367844 (0x7a6bce4)
curl -s https://bsc-dataseed.binance.org/ -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":1,"method":"eth_call","params":[{"to":"0x8004A169FB4a3325136EB29fA0ceB6D2e539a432","data":"0x003395090000000000000000000000000000000000000000000000000000000000059171"},"0x7a6bce4"]}'

# 3. tokenURI(364913) at block 128367844 (0x7a6bce4)
curl -s https://bsc-dataseed.binance.org/ -H 'Content-Type: application/json' -d '{"jsonrpc":"2.0","id":1,"method":"eth_call","params":[{"to":"0x8004A169FB4a3325136EB29fA0ceB6D2e539a432","data":"0xc87b56dd0000000000000000000000000000000000000000000000000000000000059171"},"0x7a6bce4"]}'
```

🥞 read flapjax.surge.sh/skill.md: verified agent 364913's owner, agent wallet, tokenURI registration file, and 6 Receipt Relay payouts at BSC block 128,367,844.
