SITECLOSE - quote-to-cash for trades

SCREENS (bottom nav on phone, rail on desktop)
  Dashboard  KPIs: sent, approved, close rate, won $, collected $, outstanding $
  New Quote  site-visit flow -> CLOSE screen
  Quotes     pipeline list, filter by status
  Setup      trade preset + rate book + company

STATUS FLOW
  Draft -> Sent -> Approved -> Invoiced -> Paid   (+ Lost)

QUOTE ENGINE (generalizes concrete to any trade)
  Every catalog item has a BASIS that computes qty from the job dimensions:
    area       qty = sq ft
    perimeter  qty = 2*(L+W) linear ft
    volume     qty = cubic yards  (area * thickness / 27, + waste)
    basetons   qty = tons of base (area/100 * depth/2, + waste)
    rebar      qty = bars, grid formula w/ splices  (concrete's edge, kept)
    manual     qty = typed in
  Line total = qty * rate. Cost = sum. Price = cost / (1 - profit%).
  Deposit = price * deposit%.

TRADE PRESETS (seed the rate book so setup is ~60 seconds)
  Concrete Flatwork | Pavers & Hardscape | Landscaping | Artificial Turf | Fencing | Blank

WORKS TODAY (no backend)
  build quote, close on site, mark approved, record deposit, invoice,
  mark paid, KPIs, send via the phone's own mail/SMS app, print/PDF

NEEDS BACKEND (Supabase + Stripe)
  client taps Approve on their own phone, card/ACH collection,
  multi-device sync, team accounts, automated reminders
