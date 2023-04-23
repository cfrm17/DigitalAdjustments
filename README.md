# Digital Adjustments

A callable range accrual swaps can be priced using a single factor linear Gaussian model (LGM). Additional digital adjustments are required to capture the implied volatility smile observed in the market. 

A range accrual swap is composed of two interest rate streams, a structured stream and a funding stream. The funding stream is a set of standard floating cashflows paying LIBOR + a spread. The index for the floating cashflow resets in advance and pays in arrear according to standard market conventions. 

The structured stream consists of a set of range accrual cashflows. Each of these pays an amount (which may be fixed or floating) multiplied by a counting fraction that represents the number of days a reference index lies within a pre-specified range. 

We consider the simpler case of a fixed spread only coupon first. In this case each term in the coupon payment can be replicated by a combination of delayed payment digital caps and floors.

This means that if we are a payer of the range accrual stream, as is typically the case for a swap house, then we are long a stream of digital caps and floors. Now digitals are very sensitive to volatility smile and skew, so a model for pricing range accrual swaps should incorporate the volatility smile dynamics. For non-callable range accrual swaps we can use a model for a single index, such as the SABR model, to fit the volatility smile, and then price each of the digitals in the range accrual as a call or put spread. 

As the range accrual swap is just a linear sum of each of the digital contributions it is not necessary to use a term structure model. Closed form approximations can be used to incorporate corrections arising from non-standard payment delays. The situation for callable range accrual swaps is not so simple. 

In this case we must use a term structure model to solve for the optimal exercise boundary. The ideal solution would be to have a term structure model that incorporated and could be calibrated to the entire term structure of skew and smile. This is difficult. As we currently do not have such a model we must make approximations to account for the effect of skew. This is where adjustments come in.

To capture the optionality in the call feature of a callable swap the LGM is calibrated to co-terminal European swaptions. There is a choice in the pricing templates to calibrate either to at-the-money swaptions, or to the strike. In the latter case, for each call date an effective strike is calculated for the swap starting at the next exercise start date and terminating at the callable swap termination date. 

The effective strike is used to account for term structures of coupons, notionals and funding spreads. This calibration ensures then that the LGM agrees with the prices of a selected set of vanilla swaptions either at-the-money or at the effective strike.  

However the prices of other options will not be calibrated, and as the skew and smile of the LGM is normal, we will almost certainly not match prices of options at strikes higher than that of those in the calibration set, or options on indices of significantly different tenor and in particular options on LIBOR (caplets and floorlets). 

The mean reversion parameter in the LGM can be used to reduce the mismatch in simultaneously pricing at-the-money caps and swaptions but in general it cannot eliminate it completely and it cannot compensate for pricing mismatches at different strikes resulting from volatility smile (and indeed it would be a mistake to use it to do so). 

This leaves us with a problem, as we know that the digital options in the underlying range accrual swap will in all likelihood be mispriced if we calibrate to at-the-money or at-the-strike European swaptions. 

There is no pretence that this is an elegant solution to the problem of pricing callable range accrual swaps and similar products. It is not. As mentioned earlier it is not a substitute for a proper term structure model that incorporates skew and smile, but that said it is a surprisingly effective method when applied to a select class of products. 

It is also very fast, a stochastic volatility term structure model would require at least one additional factor and some variations would require two. It is very clear how the adjustment is being applied and the magnitude of the adjustment is transparent – so this can and should be monitored. 

As a side effect the method provides a mechanism for measuring skew and smile risk for a term-structure model that has no free parameters for skew and smile, so that even if no adjustment were necessary (i.e. the market happened to be normal) the technique has a value as a risk measure.

Reference:

https://finpricing.com/knowledge.html
