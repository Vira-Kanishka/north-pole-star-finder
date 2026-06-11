At time $T$

$\delta_S =$ declination of star $S$
$p_S = 90^{\circ} - \delta_S$, angular distance of star $S$ from pole 
$\epsilon =$ obliquity of ecliptic
$\beta_P= 90^{\circ} - \epsilon$, ecliptic latitude of celestial pole 
$\lambda_P= 90^{\circ}$, ecliptic longitude of celestial pole 
$(\beta_S \, , \, \lambda_S) =$ ecliptic coordinates of star $S$

$\tau$ = chosen max. angular distance from pole at which star $S$ may be a pole-star 
$m_S =$ apparent magnitude of star $S$, i.e., brightness of $S$ to observer on earth

As per spherical Law of Cosines, for any spherical triangle sides $a, b, c$ and interior angle $C$ opposite to $c$, the following holds: $\cos{c} = \cos{a} \cos{b} + \sin{a} \sin{b} \cos{C}$

Using this, we get the ecliptic to equatorial transformation:
$\cos({90^{\circ} - \delta}) = \cos({\epsilon}) \cos({90^{\circ}-\beta}) + \sin({\epsilon}) \sin({90^{\circ} - \beta}) \cos({90^{\circ}-\lambda})$
$\implies \cos({p_S}) = \cos({\epsilon}) \sin ({\beta}) + \sin({\epsilon}) \cos ({\beta}) \sin({\lambda})$

When $p_S$ is min., $\cos({p_S})$ is max, so: 
max$\{\cos (p_S)\} =  \cos({\epsilon}) \sin ({\beta_S}) + \sin({\epsilon}) \cos ({\beta_S}) = \cos({\beta_S - (90^{\circ}-\epsilon)})$ where $\lambda =90^{\circ}$ (i.e. $\lambda_P$)
$\implies$ max$\{\cos (p_S)\}= \cos({\beta_S - \beta_P}) \implies$  min$\{p_S\} = |\beta_S - \beta_P|$
For $S$ to be $S_P$, it should satisfy : 
min$\{p_S\} \leq \tau \implies |\beta_S - \beta_P| \leq \tau \implies \beta_P -\tau   \leq \beta_S \leq \beta_P + \tau$ 

take $\tau = 5^{\circ}$ (assumption)
$m_S \leq 6$ for visibility/detectability by naked eye (Weaver, 1947). 

So, a star $S$ is not a pole star if it doesn't satisfy: $\beta_S \in [\beta_P - 5^{\circ}  \,,\, \beta_P +5^\circ]$ and $m_S \leq 6$
On strict standards, a star $S$ is not a pole star if it doesn't satisfy: $\beta_S \in [\beta_P - 1^{\circ}  \,,\, \beta_P +1^\circ]$ and $m_S \leq 6$   ; where $\tau = 1^{\circ}$

We compute the pole-star $S_P$ as follows: 

$C = \{ S : β_S \in [β_P − τ, β_P + τ] \,\,\text{and} \,\, m_S ≤ 6 \}$ is the set of candidate pole stars but only one element of $C$ is pole star $S_P$
let $C^* = \{S\in C : p_S \leq p_{S^{'}} \, \forall \, S^{'} \in C\}$. So, $S_P = \text{argmin}_{\{S \in C^*\}} \, m_{S}$    
Here, we take the subset $C^*$ of $C$ that contains the stars with min. $p_S$, and then we take the star with min. $m_S$ from $C^*$. 

References 

- Weaver, H. F. (1947). The Visibility of Stars Without Optical Aid. *Publications of the Astronomical Society of the Pacific*, *59*(350), 
  232–243. https://doi.org/10.1086/125956

