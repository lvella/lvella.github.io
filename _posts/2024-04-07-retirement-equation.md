---
title: "The Retirement Equation"
date: 2024-04-07
---

One day I wondered: how much money do I need to have in order to retire?

I don’t know if I was bored or something, but instead of googling or writing an Excel spreadsheet (and by Excel, I really mean LibreOffice Calc), I developed a single differential equation to give me the answer. I left it on the side for a while and then lost the piece of paper it was written on. When I found it, I wrote a blog post so I wouldn’t need the paper anymore.

Then I found the equation was wrong. I’m not sure why I initially thought it was right, but it didn’t simplify to the know formula for compound interest, so I took the post down. Then I had an epiphany while sleeping, and figured out what I now think to be the right equation. I am revising this blog post to correct that mistake.

It goes like this: given a time period, let’s say, a year (it works with any time period, you just have to keep it consistent), the inputs are:

$E$
: your yearly expenses.

$j$
: the yearly yield factor you can get by investing your money, i.e. $1 + \text{APY}$. E.g. if you can get an investment that gets you 10% per year, then $j = 1 + 10\% = 1.1$.

$k$
: the yearly inflation factor; i.e. the same as above, if inflation is $3\%$ per year, then $k = 1.03$.

A note on converting multiplicative factors between different time periods: these factors don’t accumulate by sum, they accumulate by multiplying, so you can’t just multiply or divide, you have to exponentiate or take roots. For instance, imagine you can get $0.7\%$ per month in some investment. To get the annual factor, you start with the monthly factor of $1.007$ and apply it 12 times, $1.007 \times 1.007 \times \ldots \times 1.007$, or, in a more concise way, raise $1.007$ to the 12th power, and you get $1.0873$, or $8.73\%$ per year. If you multiply by 12 you get $8.4\%$, which is an approximation, but not quite right. To convert yearly to monthly you take the 12th root, or raise to $1/12$.

Back to the retirement equation, we can approach it in two ways: as the time $t$ passes, we can either adjust the expenses to the inflation, or we adjust the yield to the inflation. Let’s do the second because it greatly simplifies the differential equation. We introduce a new symbol $J$, the inflation-adjusted yield factor, which can be calculated by dividing the multiplicative investment yield by the multiplicative inflation in the period:

$$J = j / k$$

$J$ is your real gain factor, in today’s currency value. So we won’t be working in literal amount of money, but inflation corrected money to the value of today!

Finally, let’s define the principal as $P$, i.e. the total amount of money you have at a given time.

Now, we want to calculate how $P$ varies with $t$. In the fashion of differential modelling, for every small time increment, $dt$, we want to determine the corresponding change in the principal, $dP$. We are assuming you are retiring from your savings only, i.e. no pension, so there are only two components: your investment returns and your expenses.

The expenses part is easy: it is proportional to the time, so it is just $-E \times dt$. In another words, for some infinitesimal time variation $dt$, you lose a proportional amount of money, scaled by your “fixed” expenses $E$ (“fixed” in a sense of value, because in absolute value it raises with inflation, but this is already considered when we choose to work in today’s value of money).

The tricky part is the yield. We need to find some $f(P) \times dt$ so that this is how much money you get by investing your principal in the very tiny time of $dt$. We can start by taking the derivative of the compound interest formula:

$$C = C(0) \times J^t$$

thus

$$\frac{dC}{dt} = C(0) \times J^t \times \log(J)$$

$$dC = C(0) \times J^t \times \log(J) \times dt$$

This is going in the direction we need, we have a rate of change to integrate, but it is not a function of the current principal. We need to decouple this slope from its starting condition, and get it for any instantaneous value of $C$. Fortunately for us, $C(t)$ appears right inside $dC$, and we can substitute:

$$dC = C \times \log(J) \times dt$$

Now we have the exact money increment $dC$ we gain for every infinitesimal time $dt$, for an instantaneous amount of money $C$. So this must be the same infinitesimal money increment we get for our investment yield component, but in relation to $P$ instead of $C$, as $P$ is our total amount of money: $P \times \log(J) \times dt$.

Putting everything together, we have:

$$dP = P \log(J) \, dt - E \, dt$$

If we ask Wolfram Alpha or ChatGPT to solve this for us (because I promptly forgot how to solve differential equations as soon as the course was over), we get:

$$P(t) = K J^t + \frac{E}{\log(J)}$$

where $K$ is the integration constant.

Now we can ask all kinds of questions to this equation. One of the easiest is: “If I retire now, with $P(0)$ money, how much money will I have after $t$ years?” To answer this, we have to figure out $K$, so we plug $t = 0$, and solve this equation where the only unknown is $K$:

$$P(0) = K J^0 + \frac{E}{\log(J)}$$

I don’t even need WolframAlpha to see that:

$$K = P(0) - \frac{E}{\log(J)}$$

then, to calculate $P(t)$, i.e. your principal after living on it for $t$ years, just plug the numbers in the formula:

$$P(t) = \frac{E}{\log(J)} + \left(P(0) - \frac{E}{\log(J)}\right) J^t$$

or, if you prefer in a form that resembles the compound interest formula:

$$P(t) = P(0) J^t - E \frac{J^t - 1}{\log(J)}$$

Example: I have ¥ 54,321 in a liquid investment with an inflation-corrected APY of $6\%$. My expenses are ¥ 4,444 per year. I live off this money. How much, in today’s ¥, corrected by inflation, will I have after 5 years? Well, just plug the numbers. The time scale (year, week) doesn’t matter, as long as all values are given over the same time scale. Obviously, the currency doesn’t matter either.

$$P(5) = 54321 \times 1.06^5 - 4444 \times \frac{1.06^5 - 1}{\log(1.06)} \approx 46898.27$$

The answer turns out to be ¥ 46,898.27. With this starting value, and keeping my inflation-adjusted expenses fixed, which theoretically would give me the same living standards, I would have a limited amount of time until I had to un-retire (or be dead). This is always the case when the investment yield is not enough to cover both the expenses and the inflation (i.e. $J \times P(0) - 1 < E$).

We can easily figure out how long I have left until I run out of money, or, in our equation terms, $P(t) = 0$:

$$54321 \times 1.06^t - 4444 \times \frac{1.06^t - 1}{\log(1.06)} = 0$$

Solve it for $t$, and you should agree with ChatGPT, who used sympy to figure out I have at most 21.38 years until I have to un-retire. Out of curisity, this is the Python code it used:

```python
from sympy import symbols, log, solve

# Define the symbols
J, P0, E, t = symbols('J P0 E t')

# Define the expression
expr = P0 * J**t - (E * (J**t - 1))/log(J)

# Values given
P0_val = 54321
E_val = 4444
J_val = 1.06

# Substitute the values into the expression
expr_substituted = expr.subs({P0: P0_val, E: E_val, J: J_val})

# Solve for t when the expression equals zero
t_solution = solve(expr_substituted, t)
print(t_solution)
```

But what about my original question? My inflation-adjusted APY is still $6\%$, but I plan to live more 52 years, and I want to live my best life, live well, so I still have the yearly expenses of ¥ 4,444, in current ¥, and I plan to keep increasing it with the inflation. How much money do I have to have in order to retire? Or, in another words, what must be $P(0)$ so that $P(52) = 0$?

Well, the equation to solve is:

$$P(52) = P(0) \times 1.06^{52} - 4444 \times \frac{1.06^{52} - 1}{\log(1.06)} = 0$$

Using sympy to solve this, I found that $P(0) = 72582.13$, thus I have to amass the incredible sum of ¥ 72,582.13 in order to retire.

That is it! Problem solved!

But there are some flaws in this model. The first one is that it is a continuous approximation to a discrete process, because real expenses and investment yields are discrete events. Nevertheless, given the stated premises, I believe this to be a reasonably accurate average description.

Another problem is that the premises are too simple. It doesn’t take into consideration taxes, pensions, health expenses that tends to increase with age, and possible many more factors that may apply to a particular situation. If you really want to use a differential equation for your retirement (instead of, say, Excel (LibreOffice Calc, really)), you may need to adapt it to your specific circumstances.

But the real problem of this or any other model is that we can’t predict the future. This model is based on a fixed inflation, fixed APY, fixed expenses, fixed date of death, etc, but we we can’t really know or control any of those things (except maybe date of death, if you are not dead by then already).

Such a model can still be useful, though, but you would have to once in a while plug in the numbers with the best available data, and correct course by either adjusting your expenses or finding a job before you are too old for it.

*originally posted on the old blog*
