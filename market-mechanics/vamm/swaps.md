# Swaps

#### Constant-product pricing <a href="#constant-product-pricing" id="constant-product-pricing"></a>

Users trade across the invariant:

* **Base-in → Quote-out**: _Pay fixed, receive float_
* **Quote-in → Base-out**: _Receive fixed, pay float_

Fees are charged on the **output** leg; fees stay in reserves (slightly growing k).

#### Deterministic decay <a href="#deterministic-decay" id="deterministic-decay"></a>

The fixed leg decays linearly over the term. Define:&#x20;

$$
\alpha(t) = \frac{t - t_0}{T - t_0},\qquad 0 \le \alpha(t) \le 1,
$$

and the decayed expected price: &#x20;

$$
p_t = p_0 \times (1 - \alpha(t)).
$$

This ensures the fixed leg trends toward 0 at maturity.



