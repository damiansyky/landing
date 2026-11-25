# Hosted DsxID SDK

The production files are published with the landing page:

```
https://dsxid.org/sdk/dsxid.js
https://dsxid.org/sdk/dsxid.css
```

Include them on an e-shop page:

```html
<link rel="stylesheet" href="https://dsxid.org/sdk/dsxid.css">
<script src="https://dsxid.org/sdk/dsxid.js"></script>
```

The landing page shows the exact URL and a copy button. You can also upload
these two files to your own static host; the SDK then loads from your domain
instead. It still talks only to the local DsxID bridge on `127.0.0.1:41991`.

`dsxid.js` is the obfuscated browser build. The readable source lives in
`../../web-sdk/src/dsxid.js`; the Node verifier stays server-side in
`../../web-sdk/dsxid-verify.js` and must not be served to browsers.

After changing the SDK, rebuild it from the repository root; the script updates
`landing/sdk/` automatically:

```powershell
powershell -ExecutionPolicy Bypass -File web-sdk/build.ps1
```

Hosting the script on dsxid.org serves static files only. No identity, profile,
or approval data is sent to that server.
