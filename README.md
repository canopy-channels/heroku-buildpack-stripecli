# heroku-buildpack-stripecli
Heroku buildpack that installs the Stripe CLI.

## Why is this useful?
Backends that use payments often must be reachable as a Stripe webhook endpoint in order to work correctly, especially when using asynchronous payment flows. For webhook endpoints with fixed addresses (e.g. example.org/stripe/events), this can be configured via the Stripe dashboard. However, for dynamically created backends like Heroku Review Apps, you need to run `stripe listen --forward-to=<your_local_backend>/stripe/events` in order to tie the webhook endpoint's lifetime to the lifetime of the backend.

Before `stripe listen` can be run from the Procfile as a worker process, the Stripe CLI needs to be installed. This buildpack does that.
