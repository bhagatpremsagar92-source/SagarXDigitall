# Sagar X Digital — API + Payment Panel

## 1. Install
Node.js 18+ required.

```bash
npm install
```

## 2. Configure provider API
Copy `.env.example` to `.env` and put your Growtak API key:

```env
PROVIDER_API_URL=https://growtak.com/api/v2
PROVIDER_API_KEY=YOUR_REAL_KEY
ADMIN_PASSWORD=841239
```

Never put the provider API key inside `public/index.html`.

## 3. Start
```bash
npm start
```
Open `http://localhost:3000`.

## 4. Growtak API mapping from the supplied API documentation
- Service list: POST `action=services`
- Add order: POST `action=add`, `service`, `link`, `quantity`
- Status: POST `action=status`, `order`

The backend uses the API URL and key from `.env`.

## Payment workflow
1. Customer selects service and quantity.
2. Customer pays using the configured QR/UPI and uploads payment screenshot.
3. Order is stored locally on the server.
4. By default `autoSend=false`: admin approves/sends the order to the provider.
5. If `autoSend=true`, the server sends the order immediately after the screenshot is uploaded.

IMPORTANT: A screenshot is not proof of a successful payment. For real money, manual verification or a payment gateway/webhook should be used before sending an order to the SMM provider.

## Service mapping
Each customer-facing service must have the provider's `service` ID. For example, if Growtak's service list says:
`service: 1, name: Followers`
then the Sagar X Digital service should store providerServiceId `1`.

The included UI already has service add/edit/delete. The server endpoint `/api/provider-services` can fetch the provider catalog for mapping.

## Production
Use HTTPS, a real database, proper admin sessions/authentication, payment gateway verification/webhooks, rate limiting, backups, and do not commit `.env`.
