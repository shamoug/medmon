# Robinhood Trading MCP — Setup

This repo registers the Robinhood trading MCP server in [`.mcp.json`](./.mcp.json):

```json
{
  "mcpServers": {
    "robinhood-trading": {
      "type": "http",
      "url": "https://agent.robinhood.com/mcp/trading"
    }
  }
}
```

The config alone does **not** connect you. The server requires an interactive
OAuth login (Robinhood credentials + 2FA), which can only be completed in a
**local** Claude Code session with a browser — not in a cloud/web session.

## Steps (run on your own machine)

1. **Get the branch with the config**

   ```bash
   git pull
   git checkout claude/robinhood-trading-mcp-akcr9c   # or merge it into your branch
   ```

2. **Start Claude Code in this repo and approve the server**

   ```bash
   claude
   ```

   On first launch Claude Code detects the project-scoped server in `.mcp.json`
   and asks you to trust it. Approve `robinhood-trading`.

   (You can also pre-approve non-interactively: `claude mcp list` shows its
   status; project servers are approved/reset via `claude mcp reset-project-choices`.)

3. **Authenticate**

   Inside Claude Code, run:

   ```
   /mcp
   ```

   Select `robinhood-trading`, choose **Authenticate**. A browser window opens
   to Robinhood's login. Sign in and complete 2FA. The OAuth token is stored
   locally by Claude Code.

4. **Verify**

   ```bash
   claude mcp list
   ```

   `robinhood-trading` should report **✓ Connected** instead of
   `⏸ Pending approval`. The Robinhood tools (e.g. account/positions/orders)
   are now available to Claude in that local session.

## Notes

- **Cloud/web sessions can't do this.** The endpoint is reachable from the
  cloud, but unauthenticated calls return `HTTP 401`. Keep your brokerage login
  on your own machine.
- **To remove the server:** `claude mcp remove robinhood-trading -s project`
  (and revert the `.mcp.json` change).
- **Trading is real money.** Once connected, tools can place/cancel real orders.
  Review every action Claude proposes before approving it.
