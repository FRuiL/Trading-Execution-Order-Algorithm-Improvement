# Trading-Execution-Order-Algorithm-Improvement

**Input**:

Trading Strategy: Short/Long

Total amount: 10,000 USD

Trading Speed: 1 (high speed) duration: 7 hours, pakage size: 3360/2
Trading Speed: -1 (normal) duration: 14 hours pakage size: 3360

Currency pair: Price Currency/Base Currency


**Trading Logic**:

Percentage change rate:

Short: -(ex_check_point - ex_last_checkpoint)/ex_last_checkpoint

Long : +(ex_check_point - ex_last_checkpoint)/ex_last_checkpoint

**Currency Conversion**:

1. USDXXX

profit in USD = USD * percentage change of USDXXX

2. XXXUSD

Tot USD ---> package-XXX  (USD / rate between XXXUSD)

profit-list = []

profit in package-XXX = package-XXX * percentage change of XXXUSD
profit-list.append(profit in package-XXX)

tot-profit in XXX = sum(profit-list)
tot-profit in USD = tot-profit in XXX * ex_rate bewtween XXXUSD

1. GBPCHF

USD ---> GBP  (USD / rate between GBPUSD)

profit in GBP = GBP * percentage change of GBPCHF

profit in USD = profit in GBP * ex_rate bewtween GBPUSD

4. AUDCAD

USD ---> AUD  (USD * rate between USDAUD)

profit in AUD = AUD * percentage change of AUDCAD

profit in USD = profit in AUD / ex_rate bewtween USDAUD

**Check Points**:

**Short Condition**:

1. 18:00 (normal checkpoint)

calculate profits 

Average > real: continue

Average < real: hold

2. 23:55 (calculate checkpoint)

calculate profits

3. 00:00 (last checkpoint)

calculate profits

if hold:

Average > real: resume

Average < real: break 

if continue:

Average > real: continue 

Average < real: break -> Ending 

4. 04:00 (Ending)
Post process
    - sum profit
    - return a Dataframe: timestamp, currency_pair, ex_rate


**Long Condition**:

1. 18:00

calculate profits 

Average < real: continue

Average > real: hold

2. 23:55

calculate profits

3. 00:00 

calculate profits

if hold:

Average < real: restart

Average > real: break 

if continue:

Average < real: continue 

Average > real: break 

**Finalize**:

calculate total after break


**How to calculate average prices and profits if there are several fails during the trading period?**

1. Since we are missing values for these 15-second-interval, we just replace these missing position with the average for the last period.

2. We only save the founds and do not proceed with the check points and average calculation.
