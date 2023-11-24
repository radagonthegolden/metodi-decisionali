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
\end{cases} \tag{1} \label{sistem}$$
Notare che le incognite sono $pu,pd$, mente la condizione di esistenza e' $u > 1+r > d$, che ha del tutto senso. Se $u<1+r$ significherebbe che il titolo risk free e' sempre meglio, $1+r < d$ invece indica che il titolo rischioso e' sempre da preferire. 

Risolvendo questo sistema possiamo con:
$$\begin{cases}
pu = \frac{(1+r)-d}{u-d} \\
pd = 1 - pu
\end{cases}$$
trovare il prezzo della call:
$C = \frac{1}{1+r}(pu\cdot 5 + pd\cdot 0)$
Dove 5 e 0 sono i due possibili payoff della call

Possiamo riassumere il sistema $\eqref{sistem}$ in un'equazione matriciale:
$$
\begin{align}
\begin{bmatrix}
1,1 & 0.97 \\
1 & 1
\end{bmatrix}
\begin{bmatrix}
pu \\
pd
\end{bmatrix} &=
\begin{bmatrix}
1.06 \\
1
\end{bmatrix} = \\
= \begin{bmatrix}
u & d \\
1 & 1
\end{bmatrix}
\begin{bmatrix}
pu \\
pd
\end{bmatrix} &=
\begin{bmatrix}
1+r \\
1
\end{bmatrix}
\end{align}
$$
Chiamando la prima matrice $A$ e i due vettori dopo $x$ e $b$ possiamo risolvere il sistema con l'equazione $x = A^{-1}b$ 

>Le funzioni Excel per invertire una matrice e il prodotto matriciale sono `MATR.INVERSA` e `MATR.PRODOTTO`, entrambe richiedono `ctrl+shift+invio` per essere eseguite (ricordarsi di selezionare l'area adatta prima)

### Metodi alternativo
#### Ipotesi di investimento
Immagino di investire $N_1$ nel primo titolo (rischioso) e $N_2$ nel titolo risk free. Tutto deve eguagliare il payoff della call
$$\begin{cases}
N_1 \cdot 55 + N_2 \cdot 1.06 = 5 \quad \text{stato up} \\
N_1 \cdot 48 + N_2 \cdot 1.06 = 0 \quad \text{stato down}
\end{cases}$$
Anche questo sistema si puo ridurre ad equazione matriciale:
$$
\begin{bmatrix}
55 & 1.06 \\
48 & 1
\end{bmatrix}
\begin{bmatrix}
N_1 \\
N_2
\end{bmatrix} =
\begin{bmatrix}
1+r \\
1
\end{bmatrix}
$$
Anche qui si possono trovare $N_1,N_2$ invertendo la prima matrice. Il prezzo della call sara':
$C = N_1\cdot 50 + N_2 \cdot 1 = 3.27$
#### Delta
Immaginiamo di andare lunghi (coprire) con $\Delta$ azioni e andare corti (vendere) con 1 call

Scelgo ora $\Delta$ in modo che il portafoglio sia privo di rischio, ovvero devo eguagliare i due stati (up e down). 

Se va su ho $55 \cdot \Delta - 5$, se va giu ho $48\cdot \Delta - 0$. Per avere risk free devono essere uguali:
$55\Delta -5 = 48\Delta \implies \Delta = 0.77$ 

Prendiamo il caso vada giu, allora il portafoglio vale $48\cdot 0.77 = 37,3$, detto cio va scontato a oggi: $37.3/1.06 = 35.2$ 

Quindi se il "valore risk free" del portafoglio e' $35.2$, questo dovra essere uguale alle azioni investite meno il prezzo della call:
$35.2 = \Delta \cdot 50 - C \implies C = 3.27$
### Prezzo a tempi maggiori
Continuiamo a pensare che ad ogni unita' di tempo il titolo possa scendere o salire solo in due punti, ma ora consideriamo tempi maggiori:
![[alberi.png]]

Per evitare di dover attualizzare (dividere per $1+r$) ogni volta, introduciamo $qu = pu/(1+r)$ e $qd = pd/(1+r)$ 

Consideriamo il punto finale piu in alto, in esso il titolo vale $S\cdot u^4 \cdot d^0$, quindi il payoff della call e'  $\max(S\cdot u^4 \cdot d^0 -X,0)$

Per quello sotto e' $\max(S\cdot u^3 \cdot d^1 -X,0)$ e cosi' via. 

Dobbiamo pero' considerare la probabilita' di arrivare al primo nodo finale e' $qu^4\cdot qd^0$, per il secondo e' $qu^3\cdot qd^1$, e cosi' via. Notare che usiamo le $q$ in modo da non dover attualizzare ad ogni passo. 

L'ultima cosa di cui dobbiamo tenere conto e' che per arrivare ad alcuni nodi ci sono piu percorsi possibili. Quindi La probabilita' di arrivare al secondo nodo e' da moltiplicare per 4 (numero di percorsi che ci arrivano). Il numero di percorsi e' calcolabile con il binomiale

Il prezzo della call sara' la sommatoria su tutti i prezzi dei singoli nodi per la probabilita di arrivarci per il numero di percorsi che ci arrivano:
$$C = \sum_{k=0}^n \binom{n}{k} qu^kqd^{n-k}\cdot \max(Su^kd^{n-k}-X,0)$$  