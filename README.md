# Stavera on Vercel

Static site (one `index.html`). Wallet connection, Send, Swap and Bridge all run in the visitor's browser through MetaMask.

## Environment variables

**None are required.** Do not add any.

- Swap uses Circle App Kit, whose Swap capability accepts an *optional* Circle API key (Circle docs: without one, requests share a rate limit).
- Bridge (CCTP) needs no key.
- This is a static site: Vercel env vars are not readable by browser code, and any key the SDK used would end up in the browser anyway. So the site ships keyless and no secret is stored in it.

## Deploy

Option A, Vercel CLI (from this folder):

    npx vercel login
    npx vercel --prod

Option B, Git: push this folder to GitHub, then Vercel -> Add New -> Project -> import it.
Framework preset: **Other**. Build command: empty. Output directory: `.` (leave default).

## Test after deployment (use the https://… URL, in Chrome/Brave/Edge with MetaMask)

1. Open the site. Click **Connect wallet**. MetaMask should open, ask to add/switch to **Arc Testnet** (chain 5042002), then ask for the account. The button turns to "Connected".
2. Get test funds: https://faucet.circle.com -> Arc Testnet -> USDC (and EURC). Arc uses USDC for gas.
3. **Send** a small amount (0.1 USDC) to a second address. MetaMask confirms; the result links to https://testnet.arcscan.app.
4. **Swap**: choose USDC -> EURC, amount 1, click Swap. Approve (if asked) and confirm each MetaMask prompt. Check the tx on arcscan. Arc Testnet swap liquidity can be unstable; if it fails, retry with a smaller amount.
5. **Bridge Arc -> Sepolia**: have a little Sepolia ETH for gas, choose Arc Testnet -> Ethereum Sepolia, USDC 1, click Bridge. Confirm every MetaMask prompt (approve, burn, then mint on Sepolia; it can take several minutes). The destination step needs gas on Sepolia.
6. **Bridge Sepolia -> Arc**: get Sepolia USDC from the same faucet plus Sepolia ETH, then repeat in the other direction.

If a step fails, open the browser console (F12) and copy the error text.
If MetaMask never opens, check: HTTPS URL (not file://), MetaMask installed and unlocked, no ad/privacy blocker on the site.
