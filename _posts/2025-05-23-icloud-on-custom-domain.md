# Set Up Two Personalized iCloud Email Addresses with a Custom Domain via Cloudflare

To use a custom domain with two personalized iCloud email addresses, you'll need an active **iCloud+ subscription**. You can use a domain you already own (e.g., via Cloudflare) and configure it for Apple Mail.

---

## 🔑 1. Subscribe to iCloud+

You'll need an **active iCloud+** subscription to use custom email domains.

---

## 📨 2. Add Your Domain to iCloud Mail

1. Go to [icloud.com/icloudplus](https://icloud.com/icloudplus) and sign in to your Apple ID.
2. Click **Custom Email Domain** in the toolbar.
3. Follow the on-screen instructions to **add your domain**.

---

## 🌐 3. Configure DNS Records in Cloudflare

1. Log in to your [Cloudflare dashboard](https://dash.cloudflare.com).
2. Select your domain and go to the **DNS** settings.
3. Add the DNS records Apple provides, such as:
   - **MX Records**:
     - `mx01.mail.icloud.com`
     - `mx02.mail.icloud.com`
   - **TXT, SPF, DKIM, and CNAME** records for verification and email security.
4. Make sure **proxy status** is set to **"DNS only"** (⚠️ no Cloudflare proxy) for all Apple-related mail records.

---

## 👤 4. Create Personalized Email Addresses

Once DNS is verified:

- You can create up to **three** personalized email addresses per domain (e.g., `yourname@yourdomain.com`).
- Set these up directly in the **iCloud Mail dashboard**.

---

## 📱 5. Set Up Mail on Apple Devices

1. On your **iPhone, iPad, or Mac**, go to:
   - `Settings` > **[Your Name]** > **iCloud** > **Custom Email Domain**
2. Follow the instructions to **add and activate your personalized addresses**.
3. Use the addresses in the **Mail app** on any Apple device signed into your iCloud account.

---

✅ Done! Your domain is now configured to send and receive email through iCloud Mail using your custom addresses.