# adamdoherty.com

A static site: a home page, a privacy policy and terms of service. Google needs these public pages before Wave's
OAuth app (the customer-ops personal console's Google sign-in) can be published to production, which ends the
7-day expiry on its refresh tokens.

Pages: `index.html`, `privacy.html`, `terms.html`, `style.css`. `CNAME` tells GitHub Pages the custom domain.

The privacy policy describes what the console actually does with Google data (`console/google.py`,
`config/personal/gmail.json` in customer-ops). If Wave's scopes or behaviour change, update `privacy.html` and its
effective date in the same change.

## Deploy (GitHub Pages)

1. Create a public repo `adamdoherty.com` on your personal GitHub account, then push:
   `git remote add origin https://github.com/<you>/adamdoherty.com.git && git push -u origin main`
2. Repo Settings, Pages: Source "Deploy from a branch", branch `main`, folder `/ (root)`. The custom domain
   reads `adamdoherty.com` from `CNAME`.
3. Namecheap, Domain List, adamdoherty.com, Manage, Advanced DNS. Remove the parking records, then add:
   - A `@` 185.199.108.153
   - A `@` 185.199.109.153
   - A `@` 185.199.110.153
   - A `@` 185.199.111.153
   - CNAME `www` `<you>.github.io.`
4. When the Pages settings show the DNS check passing, tick "Enforce HTTPS".
