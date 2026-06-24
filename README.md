# Kerington Design & Marketing — website (first draft)

A static site, three pages:

- `index.html` — logo + services
- `shopping-cart-advertising.html` — pricing graphic, Polly's locations graphic, and the reservation form with a live price summary
- `privacy.html` — starter privacy policy
- `styles.css` — shared styling
- `images/` — logo, pricing graphic, locations graphic (the services screenshot is included as `services.png` but is not used on the site; see note below)

Everything is plain HTML/CSS/JS. No build step. It will run on Cloudflare Pages as-is.

---

## 1. Put it on GitHub + Cloudflare Pages

1. Keri (or you) creates a free GitHub account, then a new repository (for example `kerington-site`).
2. Upload every file in this folder to the repo, keeping the `images/` folder intact. (GitHub's "Add file > Upload files" works fine, no command line needed.)
3. In your Cloudflare account: **Workers & Pages > Create > Pages > Connect to Git**, pick the repo.
4. Build settings: framework preset **None**, build command **blank**, output directory **/** (root). Deploy.
5. Cloudflare gives you a `*.pages.dev` URL right away. Point Keri's custom domain at it later under the Pages project's **Custom domains** tab.

---

## 2. The form already emails Keri (Formspree)

The form is connected and ready. Endpoint: `https://formspree.io/f/xzdldqqz` (created under **keringtonllc@gmail.com**). It submits in the background, so the visitor sees an on-page "Reservation received" confirmation instead of being sent off to Formspree's own thank-you screen.

What's left to do once:

1. The very first real submission triggers a one-time "confirm your email" message from Formspree to keringtonllc@gmail.com. Click it once and the form is live for good.
2. In Formspree, open the form's settings and confirm the notification email reads **keringtonllc@gmail.com**. The recipient is controlled there, not in the page code (that's Formspree's anti-spam design).

After that, every reservation lands in the keringtonllc@gmail.com inbox with the store, package, carts, term, monthly price, estimated total, and the person's contact info. Because the form has an "email" field, Keri can reply to the notification and it goes straight to the customer.

Formspree's free tier covers a modest number of submissions per month. If volume grows, the paid tier is inexpensive.

---

## 3. Collecting payment — read this first

You flagged payment as the unknown. Here's the honest landscape, and what this draft does.

### The one hard rule
Never let card numbers be typed into this website or into any form we build. Card data has to go straight to a payment company (Stripe, Square, PayPal). That is what keeps Keri off the hook for PCI compliance and the legal/security burden of holding card data. Every option below follows that rule.

### What this draft does (recommended for launch): capture the order, invoice separately
The form collects the full order and contact info and emails it to Keri. No money moves on the site. Keri then sends a secure invoice or subscription link to start monthly billing.

Why start here:
- Zero backend, no secret keys, nothing to break, fastest path to live.
- The pricing is variable (store, carts, term) and someone should confirm availability and the exact carts before charging anyway.
- Smallest possible privacy/legal footprint while she gets going.

How Keri sends the invoice (pick one, all free to start, fee per transaction):
- **Stripe** — best for recurring monthly billing. She can create a customer and a monthly subscription right in the Stripe dashboard, or send an invoice. Recommended.
- **Square** — very easy invoicing, good if she ever wants in-person too.
- **PayPal** — simplest, widely recognized, supports recurring billing.

### When she wants real checkout on the site (phase 2)
Once orders are flowing, we can add self-serve checkout. The clean options:
- **Stripe Payment Links / Pricing Table** — no code. Make one monthly link per package and drop it on the page. Simple, but it's the five fixed tiers only.
- **Stripe Checkout via a Cloudflare Pages Function** — a small serverless function (lives in the same repo) builds a checkout session for the exact package the customer picked and hands off to Stripe's hosted page. More flexible, handles the "billed monthly for 1/3/6 months" logic. The Stripe **secret key is stored as a Cloudflare environment variable, never in the repo or the page.**

Tell me which processor Keri leans toward and I'll wire up phase 2 when you're ready.

> One thing I can't do for either of you: create the Stripe/Square/PayPal/Formspree/GitHub accounts or enter Keri's banking or payment details. Those have to be set up by her directly. I'll give exact steps and any code.

---

## 4. Privacy policy

`privacy.html` is a **starting template, not legal advice.** Before going fully live, especially once payments are on:
- Fill in the bracketed items (business address; confirm the form provider is Formspree, host is Cloudflare, processor is whatever she picks).
- Have an attorney review it. Michigan doesn't currently have its own broad consumer privacy law, but if she markets to or gets visitors from other states/countries, other rules can apply.
- The reservation form already links to the policy and has a required consent checkbox, which is good practice when collecting contact info.

---

## Note on the services image
The services screenshot Keri sent (`images/services.png`) had a cut-off heading and an AI-generated storefront sign reading "SOOHAIGE" (garbled text), so I rebuilt those three services as clean text on the home page instead of embedding the image. The three blurbs match her wording. If she'd rather use an actual photo there, send a clean one and I'll place it.

## Things to confirm with Keri
- Business phone number (none was provided; the site uses email only right now).
- Business mailing address for the privacy policy footer.
- Whether the five packages on the graphic are the only options, or she truly wants free-form cart counts (the form supports both: fixed packages plus a "Custom / not sure" path).
- Whether she has rights/agreement in place with Polly's for selling this cart advertising (assumed yes).
