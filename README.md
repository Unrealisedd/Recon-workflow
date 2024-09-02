# Recon Workflow

How I approach recon on a new bug bounty target. Roughly in order, but I
jump around depending on what I find.

## 0. Scope check

Before anything else, read the program policy properly.

- What's in scope (domains, apps, APIs, wildcard?)
- What's explicitly out of scope
- Any restricted test types (no DoS, no social engineering, etc)
- Save the scope somewhere you can reference later, easy to forget mid-hunt

## 1. Subdomain enumeration

I run a few tools in parallel and merge the results.

- Passive first: `subfinder`, `amass enum -passive`, `crt.sh`
- Certificate transparency is huge - lots of internal/staging stuff leaks here
- Active brute-force: `amass enum -brute` with a decent wordlist
- Permutations: `dnsgen` piped into `massdns` to catch things like
  staging-api, api-staging, api2, newapi, etc
- Validate everything with `httpx` to see what's actually alive
- I usually end up with a list of live subdomains in a text file that I
  keep updating throughout the engagement

## 2. Port scanning

- Quick scan first: `nmap -sV --top-ports 1000` on all live hosts
- If the scope allows it, full port scan on interesting targets
- Anything running on a non-standard port is worth investigating
- Look for: debug ports, database ports exposed, admin panels on high ports

## 3. Tech fingerprinting

- `httpx` with `-tech-detect` gives you a quick overview
- Wappalyzer in the browser while manually browsing
- Check response headers, cookies, error pages for framework info
- Knowing the stack tells you what vulns to look for

## 4. Content discovery

This is where I spend most of my recon time honestly.

- `ffuf` or `gobuster` with multiple wordlists (SecLists is the baseline)
- Check for: backup files (.bak, .old, ~), config files, git repos (.git/HEAD)
- Different extensions: .php, .asp, .jsp, .json, .xml, .yml, .env
- Try the same wordlists on interesting subdomains too, not just the main domain
- Don't forget API paths: /api/, /v1/, /v2/, /graphql, /swagger, /api-docs

## 5. Parameter discovery

- `ParamSpider` or `gau` to find historical parameters from archives
- `Arjun` for brute-forcing hidden parameters on interesting endpoints
- Wayback Machine URLs often reveal params the app no longer links to
  but still accepts

## 6. GitHub and secrets

- Search the org's repos for hardcoded secrets, API keys, internal URLs
- Tools: `trufflehog`, `gitleaks`, or just manual GitHub dorking
- Check commit history, not just current code - people remove secrets
  but forget they're in old commits
- Google dorks: `site:github.com "target.com"` + password/secret/key/token

## 7. JS file analysis

- Pull all JS files from the target and grep through them
- Look for: API endpoints, internal domains, hardcoded tokens, comments
  with developer notes
- `LinkFinder` automates endpoint extraction from JS
- Minified JS can still reveal a lot when you search for strings

## 8. Putting it together

After all of this I usually have:

- A list of live subdomains with their tech stacks
- Interesting endpoints and hidden paths
- Parameters to fuzz
- Potential secrets or leaked info
- A prioritized list of what to actually attack

I keep everything in text files and revisit the recon whenever I get stuck.
New subdomains and endpoints show up over time, so I re-run the enum
periodically.
