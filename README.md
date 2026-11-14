# adamdoherty.com

A static site: a home page, a privacy policy and terms of service. Google needs these public pages before Wave's
OAuth app (the customer-ops personal console's Google sign-in) can be published to production, which ends the
7-day expiry on its refresh tokens.

Pages: `index.html`, `privacy.html`, `terms.html`, `style.css`. `.htaccess` forces HTTPS, drops `www`, and maps
`/privacy` and `/terms` on Apache (Namecheap hosting). `CNAME` is only for the GitHub Pages option.

The privacy policy describes what the console actually does with Google data (`console/google.py`,
`config/personal/gmail.json` in customer-ops). If Wave's scopes or behaviour change, update `privacy.html` and its
effective date in the same change.

## Deploy option A: Namecheap Stellar hosting (cPanel)

1. Build the upload: `powershell -NoProfile -Command "Compress-Archive -Path index.html,privacy.html,terms.html,style.css,.htaccess -DestinationPath dist/adamdoherty-site.zip -Force"`
2. Namecheap, Hosting List, cPanel, File Manager, `public_html`: upload `dist/adamdoherty-site.zip`, Extract, then
   delete the zip. Turn on "Show Hidden Files" to confirm `.htaccess` is there.
3. Namecheap, Domain List, adamdoherty.com, Manage: Nameservers "Namecheap Web Hosting DNS" (or Advanced DNS
   A `@` and CNAME `www` pointing at the hosting server). Remove the URL Redirect / parking records.
4. cPanel, SSL Status: run AutoSSL so `https://adamdoherty.com` has a valid certificate.

## Deploy option B: GitHub Pages (free) with Namecheap DNS

1. Create a public repo `adamdoherty.com` on your personal GitHub account, then push:
   `git remote add origin https://github.com/<you>/adamdoherty.com.git && git push -u origin main`
2. Repo Settings, Pages: Source "Deploy from a branch", branch `main`, folder `/ (root)`. The custom domain
   reads `adamdoherty.com` from `CNAME`.
3. Namecheap, Domain List, adamdoherty.com, Manage, Advanced DNS. Remove the URL Redirect / parking records, then add:
   - A `@` 185.199.108.153
   - A `@` 185.199.109.153
   - A `@` 185.199.110.153
   - A `@` 185.199.111.153
   - CNAME `www` `<you>.github.io.`
4. When the Pages settings show the DNS check passing, tick "Enforce HTTPS".

## After either deploy: Google

1. Prove ownership: Google Search Console, Add property, Domain `adamdoherty.com`. Copy the `google-site-verification=`
   TXT value, add it in Namecheap Advanced DNS as TXT `@`, then Verify. Use the same Google account that owns the
   Cloud project `zero-486712`.
2. Cloud console, Google Auth Platform, Branding (project `zero-486712`): home page `https://adamdoherty.com/`,
   privacy policy `https://adamdoherty.com/privacy.html`, terms `https://adamdoherty.com/terms.html`, authorized
   domain `adamdoherty.com`. Save.
3. Audience: Publish app. This alone ends the 7-day refresh-token expiry; branding verification only removes the
   "unverified app" warning. Then reconnect Google in Wave's Settings to get a non-expiring token.
