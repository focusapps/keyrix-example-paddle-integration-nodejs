# Example Keyrix + Paddle integration
The following web app is written in Node.js and shows how to integrate
[Keyrix](https://keyrix.focusapps.app) and [Paddle](https://paddle.com) together
using webhooks. This example app utilizes Paddle subscriptions to showcase
how to create a concurrent/per-seat licensing system, where the customer
is billed for each active machine that is associated with their license.

The licensing model is implemented by utilizing Paddle's subscription
quantity feature. Each time a new machine is associated with a customer's
license, the subscription's quantity is incremented by 1; likewise, each
time a machine is removed, the subscription's quantity is decremented by 1.

> **This example application is not 100% production-ready**, but it should
> get you 90% of the way there. You may need to add additional logging,
> error handling, as well as listening for additional webhook events.

## Running the app

First up, configure a few environment variables:
```bash
# Your Paddle vendor ID (available at https://vendors.paddle.com/account under "Integrations")
export PADDLE_VENDOR_ID="YOUR_PADDLE_VENDOR_ID"

# Your Paddle API key (available at https://vendors.paddle.com/account under "Integrations")
export PADDLE_API_KEY="YOUR_PADDLE_API_KEY"

# Your Paddle public key (available at https://vendors.paddle.com/account under "Public Key")
export PADDLE_PUBLIC_KEY=$(printf %b \
  '-----BEGIN PUBLIC KEY-----\n' \
  'zdL8BgMFM7p7+FGEGuH1I0KBaMcB/RZZSUu4yTBMu0pJw2EWzr3CrOOiXQI3+6bA\n' \
  # …
  'efK41Ml6OwZB3tchqGmpuAsCEwEAaQ==\n' \
  '-----END PUBLIC KEY-----')

# Paddle plan ID to subscribe customers to
export PADDLE_PLAN_ID="YOUR_PADDLE_PLAN_ID"

# Keyrix product token (don't share this!)
export KEYRIX_PRODUCT_TOKEN="YOUR_KEYRIX_PRODUCT_TOKEN"

# Your Keyrix account ID
export KEYRIX_ACCOUNT_ID="YOUR_KEYRIX_ACCOUNT_ID"

# The Keyrix policy to use when creating licenses for new customers
# after they successfully subscribe to a plan
export KEYRIX_POLICY_ID="YOUR_KEYRIX_POLICY_ID"
```

You can either run each line above within your terminal session before
starting the app, or you can add the above contents to your `~/.bashrc`
file and then run `source ~/.bashrc` after saving the file.

Next, install dependencies with [`yarn`](https://yarnpkg.comg):
```
yarn
```

Then start the app:
```
yarn start
```

## Testing webhooks locally

For local development, create an [`ngrok`](https://ngrok.com) tunnel:
```
ngrok http 8080
```

Next up, add the secure `ngrok` URL to your Paddle and Keyrix accounts to
listen for webhooks.

1. **Paddle:** add `https://{YOUR_NGROK_URL}/paddle-webhooks` to https://vendors.paddle.com/account under
   "Alerts", subscribe to `subscription_created`, `subscription_updated`,
   and `subscription_cancelled`
1. **Keyrix:** add `https://{YOUR_NGROK_URL}/keyrix-webhooks` to https://app.keyrix.sh/webhook-endpoints

## Testing the integration

Visit the following url: http://localhost:8080 and fill out the Paddle
checkout form to subscribe.

## Questions?

Reach out at [keyrix@focusapps.app](mailto:keyrix@focusapps.app) if you have any
questions or concerns!
