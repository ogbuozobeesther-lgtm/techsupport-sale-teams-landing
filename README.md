# TechSupport – Laptop Landing Page (direct to Make webhook)

A single-page, responsive landing page for **TechSupport**. Customers browse three laptops and submit an enquiry. The form posts **straight from the browser to a Make.com webhook**. There is no API, no server code and no authentication, so it works on any static host (GitHub Pages, Vercel, Netlify and others).

## Files

```
.
├── index.html   # The whole page (HTML, CSS, JavaScript)
├── images/      # Laptop images (SVG illustrations – replace with photos)
└── README.md
```

## Setting the webhook

Open `index.html` and find this line in the **EDIT HERE** block of the `<script>`:

```js
var WEBHOOK_URL = "https://hook.eu1.make.com/n7oj8udg1kxky1bvvafn3v6u44yt2bc1";
```

Replace the URL with your Make **Custom webhook** URL.

> **Note:** because the form posts directly from the browser, the webhook URL is visible to anyone who views the page source. That is expected for this setup. Anyone who finds it could send fake enquiries, so avoid putting sensitive actions behind this scenario, and regenerate the webhook in Make if it is ever abused.

## What is sent to Make

A JSON `POST` with these exact field names:

```json
{
  "Customer Name": "Ada Obi",
  "Email": "ada@example.com",
  "Phone": "+234 801 234 5678",
  "Product Interested In": "Dell Latitude",
  "Budget": "₦500,000 – ₦800,000",
  "Customer Message": "Need 5 laptops for my team.",
  "Lead Source": "Product Page",
  "Submission Date": "2026-11-17T09:03:00.000Z"
}
```

`Lead Source` is always `Product Page`. `Submission Date` is the visitor's current date and time (ISO 8601, UTC).

If Make cannot be reached or does not answer successfully, the form shows an error and does not show the success message.

## Editing content

In the **EDIT HERE** block of `index.html`:

| To change | Edit |
| --- | --- |
| Product names, categories, descriptions, specs | `PRODUCTS` |
| Prices (Naira, numbers only) | `priceNGN` in `PRODUCTS` |
| Product images | Add a file to `images/`, then update `image` in `PRODUCTS` |
| Email, phone, country, footer text | `SITE` |
| Budget dropdown options | `BUDGET_OPTIONS` |

Specifications, prices and contact details are **sample placeholders**. Replace them with verified information before going live.

## Deploying

Upload the repository to any static host. For GitHub Pages: **Settings → Pages → Deploy from branch → main / root**. No build step is needed.

## Testing the form

1. In Make, open your scenario with the **Custom webhook** and click **Run once**.
2. Open the page, click **Enquire About This Laptop** on a card and check the product is pre-selected.
3. Submit with empty fields to see validation, then fill it in and submit.
4. Confirm the success message appears and Make received the eight fields above.

## Checklist

- [ ] Replace the placeholder email and phone (`SITE`).
- [ ] Replace sample specs and prices (`PRODUCTS`).
- [ ] Replace the SVG illustrations in `images/` with real photos.
- [ ] Map the eight fields in your Make scenario and run a real test.
