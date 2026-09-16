# clone-lin-geth-eip7918

**Class: EXPERIMENTAL.** This is not Geth, not a consensus client, and not Ethereum mainnet.

LIN scalar kernel of Geth Osaka [`calcExcessBlobGas`](https://github.com/ethereum/go-ethereum/blob/24dd23631661017452cfc7bcd07c114d3796cd65/consensus/misc/eip4844/eip4844.go) EIP-7918 reserve-price path.

- Upstream: [ethereum/go-ethereum](https://github.com/ethereum/go-ethereum) commit `24dd23631661017452cfc7bcd07c114d3796cd65` (LGPL-3.0)
- Results and proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open): `src/lin_geth_eip7918.lin`, `test/prove_geth_eip7918_external.py`, `docs/events/EVENT_GETH_EIP7918_CLONE_LIN.rulel`

## What this clone contains

| File | Role |
|---|---|
| `lin_geth_eip7918.lin` | LIN kernel (Compiler 0 / C11 host) |
| `geth_eip7918_c11.c` | gcc oracle (`unsigned __int128` + limb clone) |
| `eip7918_scalar.c` | pointer-free C subset for `lin_from_c` v2 |
| `eip7918_scalar.go` | pointer-free Go subset for `lin_from_go` |

## Honest limits

- uint64-scale: Geth `big.Int` fakeExponential is not claimed. Intermediates that do not fit uint64 fail-closed to 0 (min blob fee is 1).
- `*types.Header` / `*ChainConfig` become explicit arguments: parent_excess, parent_used, base_fee, blob_fee, target, max.
- Goldens (Geth tests): Prague BelowReservePrice=`262144`; BPO1=`5617366`; BPO3=`20107103`; fakeExponential(1,50000000,2225652)=`5709098764`.
- No P2P, no EVM, no blob sidecars.

## Reproduce (from lin-open)

```bash
make -C transpile/c c0
python3 test/prove_geth_eip7918_external.py
```
