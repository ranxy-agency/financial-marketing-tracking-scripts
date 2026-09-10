# ⚡ Financial Marketing Tracking & Offline Conversion Scripts

A collection of server-side Google Tag Manager (sGTM) recipes, Meta Conversions API (CAPI) payloads, and webhook handlers engineered to track deeper down-funnel milestones (KYC completions, demo requests, and funded accounts).

Maintained by the performance growth engineers at [Ranxy](https://ranxy.com/).

---

## 🎯 Full-Service Financial Growth Marketing
Looking for end-to-end paid media management and tracking architecture for forex, crypto, prop firms, or fintech?
* **[Book a Strategy Consultation with Ranxy](https://ranxy.com/)**

---

## 💻 Sample Meta CAPI Payload for Funded Trading Accounts (Node.js)

When a trader makes their first deposit, push the offline conversion back to Meta with user parameter hashing:

```javascript
import crypto from 'crypto';

function hashParam(value) {
  return crypto.createHash('sha256').update(value.trim().toLowerCase()).digest('hex');
}

export async function sendFundedAccountEvent(email, depositAmount, currency = 'USD') {
  const payload = {
    data: [
      {
        event_name: 'FundedAccount',
        event_time: Math.floor(Date.now() / 1000),
        action_source: 'website',
        user_data: {
          em: [hashParam(email)],
        },
        custom_data: {
          currency: currency,
          value: depositAmount,
          lead_type: 'LiveTrader'
        }
      }
    ]
  };

  // POST payload to [https://graph.facebook.com/v19.0/](https://graph.facebook.com/v19.0/){PIXEL_ID}/events?access_token={ACCESS_TOKEN}
  return payload;
}
