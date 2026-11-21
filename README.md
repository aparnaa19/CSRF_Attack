# Cross-Site Request Forgery (CSRF) Lab

How a logged-in user can be tricked into doing things they never meant to, just by visiting the wrong page. Using the Elgg social network as the target, I built an attacker site that made "Alice" add "Samy" as a friend and rewrite her own profile without her knowing. Then I switched on Elgg's defences and tested how SameSite cookies stop this kind of attack.

The full write-up with screenshots is in [report.pdf](report.pdf).

## Lab setup

- Elgg social network at `www.seed-server.com` (the victim site)
- Attacker site at `www.attacker32.com`
- Both running in Docker containers on a SEED Ubuntu VM, with the hostnames mapped in `/etc/hosts`
- Firefox with the HTTP Header Live extension to capture requests

## Task 1: Looking at the requests

I captured Elgg's GET and POST requests to see what a legitimate request carries: the session cookie (`Elgg=...`), the Referer, and two hidden fields on forms, `__elgg_token` and `__elgg_ts`. Those two fields turn out to be the whole defence later on.

## Task 2: CSRF with a GET request (add friend)

- Logged in as Samy, sent a friend request, and captured the URL. It contains the friend's GUID, which is how I found that Samy is 59 and Alice is 56.
- Built a fake "Easy money" page on the attacker site. Behind it, `addfriend.html` shows a harmless "404, try refreshing" message, while an `<img>` tag quietly loads the add-friend URL with Samy's GUID.
- Posted a link to the page in a blog on Elgg. When Alice clicked it while logged in, her browser sent the request with her own session cookie, and Samy appeared in her friends list.

## Task 3: CSRF with a POST request (edit profile)

- Captured the POST request that Elgg sends when a profile is saved
- Built `editprofile.html` with a hidden form that auto-submits to `/action/profile/edit`, with Alice's GUID and the description "Samy is my hero"
- When Alice opened the link, her profile was updated instantly

**Questions from the assignment**
- *How would the attacker find Alice's GUID without her password?* It's visible in her public profile's page source and in the requests her profile generates.
- *Could the attack work against whoever visits, without knowing them in advance?* Not here, because the attacker's page can't read Elgg's pages, so it has no way of learning the visitor's GUID.

## Task 4: Turning on Elgg's countermeasure

Elgg's CSRF check was disabled for the lab by an early `return` in `Csrf.php`. With that line commented out, both attacks failed with "token and timestamp are missing". The attacker's site can't read the secret `__elgg_token`, which is a random per-session value, so it can't put a valid one in a forged request.

## Task 5: SameSite cookies

Visiting `www.example32.com` set three cookies: `cookie-normal`, `cookie-lax` and `cookie-strict`.

| Request | normal | lax | strict |
|---|---|---|---|
| Same-site link | sent | sent | sent |
| Cross-site link (top-level GET) | sent | sent | not sent |
| Cross-site form POST | sent | not sent | not sent |

So a server can tell a request is cross-site when the Lax or Strict cookie is missing. For Elgg, I'd set session cookies to **Strict** (or Lax where normal navigation needs them) and keep the anti-CSRF tokens as a second layer.

## What I took away from it

- CSRF works because browsers attach cookies automatically, whichever site triggered the request
- A secret token the attacker can't read is what actually breaks the attack
- SameSite cookies are a simple, strong extra layer, and Lax by default already blocks the POST version of this attack

## Tools

Elgg, Firefox with HTTP Header Live, HTML and JavaScript, Docker, SEED Ubuntu 20.04

This was a college assignment for my Master's in Cybersecurity at Illinois Tech. All testing was done in an isolated lab on machines I controlled.
