---
title: "How much do I need to retire?"
date: 2026-06-19
---

You need this much to retire:

$$\frac{E}{\ln\left(\frac{1 + r}{1 + i}\right)}$$

where

$E$
: your yearly expenses.

$r$
: the expected annual return on your investments.

$i$
: the expected annual inflation rate.

This doesn't mean you can't retire with less, but if you have at least this much (and your inflation-adjusted expenses, investment returns, and inflation rate don't change), you can retire for sure. With less, you risk running out of money before dying.

Practical example: suppose I need ₺ 10,000 per month to live, and get 1% return per month on my investments, and the inflation is 0.6% per month (notice I am using month instead of year, because the time period doesn't matter, as long as you stay consistent). Then, I need at least:

$$\frac{10,000}{\ln\left(\frac{1 + 0.01}{1 + 0.006}\right)} \approx ₺ \, 2,519,996.69.$$

This post is kind of a follow-up to [The Retirement Equation]({% post_url 2024-04-07-retirement-equation %}), where we derived the following ordinary differential equation for the evolution of your principal $P$ over time, given only investment returns and expenses:

$$\frac{dP}{dt} = P \ln\left(\frac{1 + r}{1 + i}\right) - E$$

where $P$ (for principal) is the amount of money you have.

The first term is the returns on your investments (which hopefully is positive). The second term is your expenses, which is inexorably negative. The epiphany is that the formula leading this post follows immediately from this ODE, because:

 * If $P \ln\left(\frac{1 + r}{1 + i}\right) < E$, you lose money over time,
 * If $P \ln\left(\frac{1 + r}{1 + i}\right) > E$, you gain money over time, and
 * If $P \ln\left(\frac{1 + r}{1 + i}\right) = E$, that's exactly where your inflation-adjusted gains and expenses stay balanced.

Juggling this last equation around, we get that $P$ equals the formula at the beginning of this post, for the absolute minimum you need to retire.

I encourage you to read the original post to get the assumptions behind this derivation. Beyond the obvious (you can't predict future rates), this is a continuous approximation to what in reality happens discretely (i.e. returns happen monthly, or daily, or quarterly, etc., and expenses happen in small lumps over time). It is good enough, but is an approximation nonetheless.

PS: the meaning of the negative result when $r < i$ is left as an exercise to the reader, because it is too late for me
to try to make sense of it. 
