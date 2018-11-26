## Bond Valuation - Payment Schedule

We now graduate to doing something really serious... I mean, really really serious things that are relevant to our business - bond valuation. By the end of this week, you will know how to price a vanilla treasury bond and be able to challenge the QARM team's calculation if they ever get it wrong! ^_^

When valuating fixed income instruments, we must first identify future cash flows.  Today's challenge is to build a payment schedule for a bond.

1. Create a new Julia package called `BondPricing` (or Python module) for this marathon

2. Define a struct (or class in Python) with
  - cusip
  - coupon (e.g. 0.03 means 3%)
  - maturity date
  - coupon frequency (e.g. 2 means semi-annual)

3. Write a function `payment_schedule(bond, as_of_date)` that returns a DataFrame with the following columns:
  - `date`: date of the cash flow
  - `amount`: coupon payment, or coupon+principal for the last payment
  - `days`: number of days between as of date and payment date
  
Requirements:
- Your code must be able to handle coupon frequency of 1, 2, 4, and 12.

Notes:
- You can assume par amount of $1,000 (e.g. you get $20 for every period for a 4% semi-annual coupon bond)
- Typically, a semi-annual coupon bond will pay coupon every 6 months on the same day of month.  For example, a bond maturing on 15-Aug-2028 will make coupon payments on 15-Feb and 15-Aug.

Submit your solution by running the following code:

```julia
bond = Bond("TESTBOND2", 0.04, Date(2028, 8, 15), 2)
as_of_date = Date(2018, 8, 15)
@show payment_schedule(bond, as_of_date)
```

The reuslt should look like this:
```
│ Row │ date       │ amount  │ days  │
├─────┼────────────┼─────────┼───────┤
│ 1   │ 2019-02-15 │ 20.0    │ 184   │
│ 2   │ 2019-08-15 │ 20.0    │ 365   │
│ 3   │ 2020-02-15 │ 20.0    │ 549   │
...
│ 19  │ 2028-02-15 │ 20.0    │ 3471  │
│ 20  │ 2028-08-15 │ 1020.0  │ 3653  │
```

Just FYI:
1. In many cases, we are not so certain about future cash flows. For example, for a mortgage-backed security, the borrower may prepay the loan and we end up receiving more cash than expected. Likewise, a corporate bond may be called before maturity date. 

2. In practice, if the payment date falls on a weekend or holiday then it needs to be adjusted to the following buisness date but for this exercise we can ignore that.
