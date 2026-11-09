# 06 · Privacy and trust note (draft)

Update this as the build changes. It gets copied into the submission.

1. **What stays private**
   - **On-chain:** sender, receiver and amount of every shielded payment and prepare step (`utest1 → utest1`); memos are encrypted.
   - **Which amount the user checks:** "Check only" downloads the whole boundary index and scores locally in the browser, so our server never sees it. (A real send still reaches the server, because the server's wallet builds the transaction.)

2. **What the app reveals, to whom**
   - **Light-wallet server (`testnet.zec.rocks`):** sees our indexer's IP and request timing (public chain data only).
   - **Our index server:** sees that someone downloaded the index (hidden behind Nym mixFetch where enabled). It never receives amounts, addresses or keys.
   - **Light-wallet server, wallet side:** our local wallet engine syncs and broadcasts through `testnet.zec.rocks`, which sees the user's IP and timing unless that traffic goes over Nym.
   - **Chain observers:** see that shielded transactions happened, and their fees. Only if the user chooses a transparent exit do they see each piece as a public deshield (amount and time). That's unavoidable when leaving the pool, so Mirage's job is to make those pieces look like many others.
   - **Our watcher:** the demo watcher holds a viewing key only (no spending key). It can read incoming amounts and memos for the demo wallet.
   - **Our storage:** we keep the public index only; no user data, no logs of checked amounts.

3. **Who users must trust**
   - **Us:** to serve an honest index. The index is rebuildable from public chain data by anyone.
   - **The wallet engine (zkool_graphql):** it holds the spending keys and signs each scheduled exit piece automatically when it is due, so the user doesn't sign piece by piece. Whoever runs it has custody:
     - **Self-hosted:** it runs on the user's machine (localhost only), so keys stay with the user. The machine has to stay online for scheduled pieces to go out.
     - **Our hosted testnet demo:** we, the operators, hold the two demo wallets (TAZ only).
   - **The hold limit:** a piece that still scores red is held and re-checked up to 6 times, 2 hours apart, and then sent anyway so funds are never stuck (`MAX_HOLDS` in `server/src/jobs.ts`; the hosted demo scales the 2 hours down to minutes with `EXIT_DELAY_SCALE`). There is no button yet to cancel a scheduled exit; that's on the list.
   - **Nym:** for network privacy where used.

4. **What we'd fix next**
   - **Broadcast:** Nym for every broadcast path.
   - **Index:** a client-side indexer option (no index server at all).
   - **Model:** publish and peer-review the crowd-score model.
   - **Control:** let the user cancel a scheduled exit, and choose whether a still-red piece is sent or kept shielded.
