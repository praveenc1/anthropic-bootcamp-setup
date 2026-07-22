

# Setup Issues and Resolutions

Issues and fixes reported by bootcamp participants during setup and exercises. Keep this current as new issues come up — post in the shared chat first, then fold the resolution back in here.

## Windows-only issues

### Python SSL/certificate errors (corporate proxy)

Windows-only — exercises ran fine on Mac throughout. Participants on Windows hit certificate errors from Python (things like `UNABLE_TO_GET_ISSUER_CERT_LOCALLY`) when a corporate proxy intercepts outbound connections — for pip installs and for API calls. This is also the root cause behind API key errors some participants saw: the proxy was breaking the underlying HTTPS connection, which surfaced as an API key error even though the key itself was fine. Same fix resolves both. Results varied by machine: some resolved it once and for all, others had to repeat the fix at the start of every exercise (15-20 minutes lost each time).

Resolution that worked for multiple people:

1. Remove all existing Python installations and reboot.
2. Install the latest Python version, making sure the "Add python.exe to PATH" option is checked. Leave the "install for all users / admin" option unchecked if it fails.
3. From a terminal, run `pip install pip-system-certs` (this patches Python to use the Windows certificate store instead of its bundled cert list). A related variant seen in chat: `pip install --trusted-host files.pythonhosted.org pip-system-certs`.
4. Reboot again so the change takes effect — skipping the reboot leaves you hitting the same error because environment variables aren't reloaded.
5. If you use WSL, apply the fix separately there too, and separately again inside any Python virtual environment — the local machine, WSL, and each venv have their own isolated trusted CA stores, so fixing one doesn't fix the others.
6. Validate across a few exercises and a VS Code restart before assuming it's fixed.

Tips that helped resolve it faster:
- If asking Claude to fix it, temporarily switch the model to Opus — Sonnet was seen "doubting itself" and stalling on this one, while Opus resolved it in a few minutes.
- Ask Claude to "resolve by executing commands/tools" directly rather than just suggesting steps — it patched the Windows certificate store itself after a few iterations.
- Use the VS Code Claude extension rather than the VS Code chat panel.

### Windows machines unable to connect to Claude

Windows-only. Several Windows users could not connect to Claude at all and couldn't complete exercises as a result. Suspected cause: corporate network/firewall blocking the connection. Suggested fix: contact IT and ask them to whitelist `api.anthropic.com`, or check whether the organization is blocking the endpoint outright.

