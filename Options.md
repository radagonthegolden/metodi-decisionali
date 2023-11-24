Le options ti permettono di comprare/vendere (oppure non farlo, da qui il nome *Options*) un certo titolo ad una data futura per un prezzo concordato adesso.

- $S =$ Sottostante, ovvero il titolo che segue la options
- $X =$ Prezzo di esercizio

Noi seguiamo un **modello binario**, ovvero il titolo in futuro potra avere uno tra soli due prezzi. Chiaramente in reala il prezzo futuro e' un continuo. 
- $u =$ up factor, ovvero di quanto va su l'opzione
- $d =$ down factor, ovvero di quanto va giù l'opzione

**Esempio:** se un titolo ora vale 50$ e in futuro puo valere o 100$ oppure 25$, allora $u = 2$ e $d = 0.5$

## Call
Si tratta di opzioni dove noi paghiamo la possibilita di comprare il titolo in futuro, ad un prezzo che decidiamo ora.

Consideriamo un titolo che ora vale 50 e in futuro vale:
$$\begin{cases}
	55 \quad \text{con probabilita} \quad pu \\
	48 \quad \text{con probabilita} \quad pd 
\end{cases}$$

Se la call costa 50 (quindi $X=50$) allora il payoff della call e': $\max(S-X,0)$. Questo deriva dal fatto che se il prezzo sale noi compriamo e ci intaschiamo la differenza di prezzo. Se invece e' sceso noi non compriamo. Quindi il payoff della call e':
$$\begin{cases}
	5 \quad \text{con probabilita} \quad pu \\
	0 \quad \text{con probabilita} \quad pd 
\end{cases}$$
Consideriamo anche un titolo **risk free** con un ritorno assicurato ad un tasso $r = 6\%$  

L'equazione **risk neutral valuation** del titolo sottostante e':
$S = \frac{1}{1+r}(S\cdot u \cdot pu + S\cdot d \cdot pd)$ 

Mentre quella del titolo risk free e':
$1 = \frac{1}{1+r}[(1+r)\cdot pu + (1+r)\cdot pd]$ 

Da notare che in entrambe facciamo uno sconto con un fattore di $1/(1+r)$

Possiamo semplificarle nel sistema:
$$\begin{cases}
1+r = u\cdot pu + d\cdot pd \\
1 = pu + pd
\end{cases}$$
Notare che le incognite sono $pu,pd$, mente la condizione di esistenza e' $u > 1+r > d$, che ha del tutto senso. Se $u<1+r$ significherebbe che il titolo risk free e' sempre meglio, $1+r < d$ invece indica che il titolo rischioso e' sempre da preferire. 

Risolvendo questo sistema possiamo con:
$$\begin{cases}
pu = \frac{(1+r)-d}{u-d} \\
pd = 1 - pu
\end{cases}$$
trovare il prezzo della call:
$C = \frac{1}{1+r}(pu\cdot 5 + pd\cdot 0)$
Dove 5 e 0 sono i due possibili payoff della call

