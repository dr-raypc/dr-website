# drraypc.com — Personal Security Portfolio Site

Built with vanilla HTML, CSS, and JavaScript. No frameworks, no build tools, no dependencies.
Hosted on GitHub Pages with a custom domain.

---

## File Structure

```
/
├── index.html                        ← Main site (home, about, skills, repos, contact)
├── private/
│   └── raymond-mayer/
│       └── index.html                ← Secret resume page (unlisted, no login required)
└── README.md
```

---

## Deployment — GitHub Pages

### Step 1: Create the Repository

Create a new GitHub repository. You can name it anything — for a custom domain the name doesn't matter.

```
Repository name: portfolio   (or anything you want)
Visibility: Public (required for free GitHub Pages)
```

### Step 2: Push Your Files

```bash
git init
git add .
git commit -m "initial site"
git branch -M main
git remote add origin https://github.com/YOURUSERNAME/YOURREPO.git
git push -u origin main
```

### Step 3: Enable GitHub Pages

1. Go to your repo on GitHub
2. Click Settings → Pages (left sidebar)
3. Under "Source" — select: Deploy from a branch
4. Branch: main / (root)
5. Click Save

GitHub will build and deploy your site. It will be live at:
`https://YOURUSERNAME.github.io/YOURREPO`

### Step 4: Connect Your Custom Domain (drraypc.com)

**In GitHub:**
1. Settings → Pages → Custom domain
2. Enter: drraypc.com
3. Click Save
4. Check "Enforce HTTPS" (may take a few minutes to appear)

**At your DNS registrar (wherever drraypc.com is registered):**

Add these DNS records:

```
Type    Name    Value
A       @       185.199.108.153
A       @       185.199.109.153
A       @       185.199.110.153
A       @       185.199.111.153
CNAME   www     YOURUSERNAME.github.io
```

DNS propagation takes 5 minutes to 48 hours depending on your registrar.
GitHub will auto-provision a free Let's Encrypt SSL certificate once DNS resolves.

---

## Wiring Up the Contact Form

The form is built and ready — it just needs a backend to send to.
Two free options:

### Option A — Formspree (recommended, easiest)

1. Go to formspree.io and create a free account
2. Create a new form — they give you an endpoint URL like:
   `https://formspree.io/f/xabc1234`
3. In index.html, find the form tag and update the action:

```html
<!-- Find this line: -->
<form id="contactForm" action="#" method="POST">

<!-- Change to: -->
<form id="contactForm" action="https://formspree.io/f/YOUR_FORM_ID" method="POST">
```

Formspree handles the submission and emails you. Free tier = 50 submissions/month.

### Option B — Netlify Forms (if you switch from GitHub Pages to Netlify)

Add `netlify` attribute to the form tag:
```html
<form name="contact" method="POST" data-netlify="true">
```

---

## Updating Your GitHub Username in the Site

Search the index.html for `YOURGITHUBUSERNAME` and replace with your actual GitHub username.
The GitHub API call will then auto-populate your real repos.

```bash
# Quick find/replace from terminal:
sed -i 's/YOURGITHUBUSERNAME/youractualusername/g' index.html
```

---

## The Secret Resume Page

The resume lives at: `drraypc.com/private/raymond-mayer`

It is:
- Not linked from the main site
- Tagged with `<meta name="robots" content="noindex, nofollow">` so Google won't index it
- Has a "print / save PDF" button built in (uses browser print dialog — save as PDF)

To share it, just send the full URL directly. Only people with the URL can find it.

If you ever want to change the path, rename the folder and update the watermark text inside the file.

---

## Keeping Repos Updated

The site uses the GitHub API to fetch repos dynamically — no manual updates needed.
Every time someone visits the site, it pulls your latest repos automatically.

The API endpoint used:
```
https://api.github.com/users/YOURUSERNAME/repos?sort=updated&per_page=30
```

GitHub's unauthenticated API allows 60 requests/hour per IP which is plenty for a personal site.
If you ever need more, you can add a GitHub Personal Access Token.

---

## Local Development

No build step needed. Just open index.html in a browser.

For the GitHub API to work locally (avoids CORS issues), run a simple local server:

```bash
# Python 3
python -m http.server 8080

# Then visit:
http://localhost:8080
```

---

## Customization Checklist

Before going live, update these in index.html:

- [ ] Replace `YOURGITHUBUSERNAME` with your actual GitHub username (3 places)
- [ ] Update the contact form `action="#"` with your Formspree endpoint
- [ ] Update `available for work` in the nav if your status changes
- [ ] Adjust the "about" text in the About section if needed
- [ ] Update LinkedIn URL if different from `linkedin.com/in/ray-mayer`

And in private/raymond-mayer/index.html:
- [ ] No changes needed — it pulls from the same contact info

---

## Updating the Site

Any time you push to the main branch, GitHub Pages auto-deploys within ~60 seconds.

```bash
git add .
git commit -m "update repos section"
git push
```
