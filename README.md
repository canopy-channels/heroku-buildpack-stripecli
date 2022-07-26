# heroku-buildpack-stripecli
Heroku buildpack that installs the Stripe CLI.

## Why is this useful?
Backends that use payments often must be reachable as a Stripe webhook endpoint in order to work correctly, especially when using asynchronous payment flows. For webhook endpoints with fixed addresses (e.g. example.org/stripe/events), this can be configured via the Stripe dashboard. However, for dynamically created backends like Heroku Review Apps, you need to run `stripe listen --forward-to=<your_local_backend>/stripe/events` in order to tie the webhook endpoint's lifetime to the lifetime of the backend.

Before `stripe listen` can be run from the Procfile as a worker process, the Stripe CLI needs to be installed. This buildpack does that.

## Procfile

Replace values enclosed in `<>` with your own values.

1. Add this line to your `Procfile`:

   ```procfile
   stripe: stripe listen --forward-to=${HEROKU_APP_NAME}.herokuapp.com/<your-webhook-path> --api-key ${STRIPE_API_KEY} <other optional args, e.g. --events checkout.session.completed>
   ```

2. Add the environment variables to Heroku:

   ```sh
   heroku config:set -a <app-name> STRIPE_API_KEY=<your-stripe-api-key> \
                                   STRIPE_CLI_VERSION=<https://github.com/stripe/stripe-cli/releases - optional>
   ```

3. Push/deploy your app

4. Enable the process in Heroku Dashboard > Configure Dynos

5. `heroku logs -t --dyno=stripe` and take note of the webhook signing secret

## Thanks
Thank you to [heroku/heroku-buildpack-awscli](https://github.com/heroku/heroku-buildpack-awscli), which served as a great reference while writing this.
