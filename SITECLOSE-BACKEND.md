# SiteClose backend

Separate from BuildCrete. BuildCrete is the live business system and is not
touched by anything here.

## Supabase project "SiteClose"

    Project ref  vnckxbtmcxczseiviujy
    API URL      https://vnckxbtmcxczseiviujy.supabase.co
    Region       us-west-1
    Cost         $0 / month

The publishable key is safe to ship in the browser; row-level security is what
protects the data, not the key.

## Tables

    companies      one per contractor who signs up; holds their defaults
    profiles       links an auth user to their company
    catalog_items  the rate book, per trade
    quotes         header, dimensions, totals, status, public token
    quote_lines    the priced line items on a quote
    payments       deposits and final payments

RLS is enabled on all six. Every policy is scoped to `authenticated` only —
there are no `anon` policies, so an anonymous caller reads zero rows.

## Customer-facing quote link

Two SECURITY DEFINER functions, granted to `anon`, are the only anonymous
access to any data:

    get_public_quote(token)                      -> price, deposit, scope, company
    approve_public_quote(token, signature, name) -> signs and approves

`get_public_quote` deliberately omits cost, rates and profit: the customer sees
the price and what is included, never the contractor's margin.
`approve_public_quote` rejects a malformed signature or an unknown token, and
will not overwrite a signature once someone has signed.

## Verified

    anon SELECT on all six tables            0 rows
    get_public_quote with valid token        price + scope, no cost or rates
    get_public_quote with unknown token      null
    approve with junk signature              rejected
    approve with unknown token               rejected
    approve with valid signature             approved, timestamps set
    second approve attempt                   ignored, first signer preserved

## Not built yet

    Stripe        needs a Stripe account and bank details from the owner
    Vercel        deploy target; connector was unreachable at build time
    App wiring    the app still stores to the browser, not to Supabase
