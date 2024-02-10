# Options
Le options ti permettono di comprare/vendere (oppure non farlo, da qui il nome *Options*) un certo titolo ad una data futura per un prezzo concordato adesso.

- $S =$ Sottostante, ovvero il titolo che segue la options
- $X =$ Prezzo di esercizio

Noi seguiamo un **modello binario**, ovvero il titolo in futuro potra avere uno tra soli due prezzi. Chiaramente in realta il prezzo futuro e' un continuo. 
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
\end{cases} \tag{1}$$
Notare che le incognite sono $pu,pd$, mente la condizione di esistenza e' $u > 1+r > d$, che ha del tutto senso. Se $u<1+r$ significherebbe che il titolo risk free e' sempre meglio, $1+r < d$ invece indica che il titolo rischioso e' sempre da preferire. 

Risolvendo questo sistema possiamo con:
$$\begin{cases}
pu = \frac{(1+r)-d}{u-d} \\
pd = 1 - pu
\end{cases}$$
trovare il prezzo della call:
$C = \frac{1}{1+r}(pu\cdot 5 + pd\cdot 0)$
Dove 5 e 0 sono i due possibili payoff della call

Possiamo riassumere il sistema (1) in un'equazione matriciale:
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

Consideriamo il punto finale più in alto, in esso il titolo vale $S\cdot u^4 \cdot d^0$, quindi il payoff della call e'  $\max(S\cdot u^4 \cdot d^0 -X,0)$

Per quello sotto e' $\max(S\cdot u^3 \cdot d^1 -X,0)$ e cosi' via. 

Dobbiamo pero' considerare la probabilita' di arrivare al primo nodo finale e' $qu^4\cdot qd^0$, per il secondo e' $qu^3\cdot qd^1$, e cosi' via. Notare che usiamo le $q$ in modo da non dover attualizzare ad ogni passo. 

L'ultima cosa di cui dobbiamo tenere conto e' che per arrivare ad alcuni nodi ci sono più percorsi possibili. Quindi La probabilita' di arrivare al secondo nodo e' da moltiplicare per 4 (numero di percorsi che ci arrivano). Il numero di percorsi e' calcolabile con il binomiale

Il prezzo della call sara' la sommatoria su tutti i prezzi dei singoli nodi per la probabilità di arrivarci per il numero di percorsi che ci arrivano:
$$C = \sum_{k=0}^n \binom{n}{k} qu^kqd^{n-k}\cdot \max(Su^kd^{n-k}-X,0)$$**Calcolo dei fattori:** come si calcolano i fattori $u,d$?
- $u = e^{\sigma \sqrt{\Delta t}}$ 
- $d =1/u =  e^{-\sigma \sqrt{\Delta t}}$
Se $T$ è il tempo di maturazione della call e $n$ è il numero di intervalli temporali, allora $\Delta t = T/n$. Mentre $\sigma$ è la **volatilità annua del titolo sottostante**, che non è altro che la radice quadrata della varianza (la deviazione standard).

Al posto di fare $(1+r)$ per attualizzare/capitalizzare, possiamo usare $e^{r\Delta t}$, considerando un $\Delta t$ negativo in caso di attualizzazione.

Infine quindi la funzione per il calcolo del prezzo di una call sarà del tipo `Call(S,X,T,rf,s,n)`. Dove `rf` è il tasso del risk free (praticamente basta sostituirlo a $r$), mentre `s` è $\sigma$  

Calcoliamo quindi:
1. $\Delta t = T/n$
2. $u = e^{\sigma \sqrt{\Delta t}} \quad d = 1/u$
3. $R = e^{rf\Delta t}$ 
4. $pu = \frac{R-d}{u-d} \quad pd = 1-pu \quad qu = \frac{1}{R}pu \quad qd = \frac{1}{R} pd$
5. $C = \sum_{k=0}^n \binom{n}{k} qu^kqd^{n-k}\cdot \max(Su^kd^{n-k}-X,0)$ 

**Prezzo della call come funzione.** Notiamo che:
- Vedendo il prezzo come funzione del tempo (crescente) questo tende ad un valore
- E' chiaramente una funzione crescente del sottostante e decrescente dello strike-price (X). Questo porta a due effetti:
	- Come funzione del tasso RF è crescente. Sembra strano guardando la funzione, ma risulta dal fatto che se il tasso RF cresce, cresce anche il valore del sottostante
	- Stessa cosa vale per la sigma, per cui il prezzo è una funzione crescente

**Put:** si tratta invece di option che mi danno il diritto a comparare un titolo allo strike price. Il calcolo del prezzo è lo stesso, basta cambiare il max da $\max(Su^kd^{n-k}-X,0)$ a $\max(X-Su^kd^{n-k},0)$

## Modello di Black-Scholes
Si tratta di un modello **a tempo continuo**, dove abbiamo:
- **Prezzo call:** $S\cdot N(d_1)-X\cdot e^{-r\cdot T}\cdot N(d_2)$
- **Prezzo put:** $X\cdot e^{-r\cdot T}\cdot N(d_2)- S\cdot N(d_1)$
Dove:
- $d1 = (\ln(S/X)+(r+\sigma^2/2)T)/(\sigma\sqrt{T}) \quad (2)$
- $d2 = d1 - \sigma \sqrt{T}$
- $N(d_1),N(d_2)$ sono invece **funzioni cumulative di distribuzioni normali** di $d_1,d_2$
	- In Excel si calcolano semplicemente con `NORMDIST(d1)`

Questa assunzione di usare una normale deriva da una verifica empirica che i **rendimenti** di un mercato si distribuiscano come una normale

>**Rendimenti:** presi i prezzi di un titolo o un mercato a vari istanti temporali si ottengono i rendimenti facendo $\ln(S_t/S_{t-1}$)

Da ciò si assume che il sottostante segua una **log-normale** (distribuzione sempre positiva, come il prezzo di un titolo dopotutto). Detto in parole povere: preso il prezzo di partenza di un titolo, il suo **percorso di prezzo** questo segue una crescita risk-free in aggiunta ad una parte stocastica:
$$S_{t+\Delta t} = S_t \cdot e^{\mu \Delta t + \sigma \varepsilon \sqrt{\Delta t}}$$
La parte $e^{\mu \Delta t}$ è detta **drift** ed è quella risk-free, mentre la parte $e^{\sigma \varepsilon \sqrt{\Delta t}}$ è quella stocastica, infatti $\varepsilon$ è estratta da una normale standard.

### Put-Call parity
Analizziamo due portafogli: uno con una put e il titolo e una con una call e il valore $X\cdot e^{rT}$, vediamone i payoff
$$\begin{align}
\text{Put} + S &: \quad \max(X-S_t,0) + S_t = \max(X,S_t) \\
	\text{Call} + X\cdot e^{-rT} &: \quad \max(S_t -X, 0) + X = \max (S_t,X)
\end{align}$$
Chiaramente il max tra i due valori sarà lo stesso, quindi i due portafogli hanno lo stesso payoff. Ma due portafogli con lo stesso payoff allo stesso tempo (scadenza dell'option) devono avere lo stesso prezzo. Quindi deve valere l'uguaglianza: 
$$\text{Put} + S = \text{Call} + X\cdot e^{-rT}$$
Che può essere usata per ottenere la Put data la Call (o viceversa) senza il bisogno di calcolarla esplicitamente.
### Volatilità implicita
Per ora abbiamo assunto di avere $\sigma$ come valore dato, ma come si ottiene? Ci sono due modi:
- Ottenibile tramite lo storico dei prezzi
- Invertire la formula BS (chiaramente dobbiamo conoscere il prezzo)

Notiamo che invertire (2) non è facile, quindi usiamo un metodo numerico in cui calcoliamo il prezzo BS al variare di $\sigma$ e ci fermiamo quando è uguale al prezzo conosciuto.

In realtà calcoliamo la differenza tra questi due prezzi come funzione di $\sigma$ e cerchiamo lo zero di questa nuova funzione. Questo perché ci sono molte tecniche conosciute per trovare lo zero di una funzione, come ad esempio il **metodo di bisezione**.

## Analisi rendimenti
Dato un vettore $S$ di prezzi di un certo titolo a vari istanti temporali, vogliamo analizzarne l'evoluzione nel tempo. 
1. La prima cosa da fare è passare dai prezzi ai rendimenti, costruendo il vettore $P_t = ln(S_{t+1}/S_t)$
2. Calcoliamo Min e Max dei valori di $P$ 
3. Calcoliamo la **lunghezza intervallo:** $(\text{Max}-\text{Min})/n$ 
4. Fatto ciò possiamo costruire il **vettore delle classi** la cui i-esima componente è $\text{Min}+i*(\text{Max}-\text{Min})/n$
5. Calcoliamo le frequenze tramite la funzione  `frquenze(dati,classi)`
6. Fatto ciò si può fare il grafico

### Simulazione del sentiero di Prezzo
Usando la formula
$$P_{t+\Delta t} = P_t \cdot e^{\mu \Delta t + \sigma z_t \sqrt{\Delta t}}$$
Dove $z$ è un vettore di estrazione di una $N(0,1)$, anche qui si può fare il grafico dopo aver costruito il vettore, questo è un **sentiero di prezzo simulato**.

### Distribuzione log-normale
Voglio fare un'analisi dei rendimenti ma usando rendimenti simulati. Part da un prezzo fisso $P_0$ e immagino di andare avanti di un singolo anno ($\Delta t=1$), Quindi ottengo il vettore $P$ che ha per componenti:
$$P_t = P_0*e^{\mu + \sigma z_t}$$
Faccio ora lo stesso procedimento che ho fatto nell'analisi dei rendimenti, noterò che il grafico che ne risulta è distribuito come una **log-normale**.

### Matrice varianza-covarianza
Vogliamo costruire la matrice di covarianza di vari titoli (da cui si estrae anche la varianza di un singolo titolo guardando la diagonale)
1. Calcolo i rendimenti di ogni singolo titolo
2. Calcolo la media di questi rendimenti
3. Costruisco una matrice che ha in ogni colonna il vettore dei rendimenti di un titolo - media dei rendimenti di quel titolo. Questa matrice la chiamo $A$
4. La matrice di covarianza sarà $(A^T*A)/n$ dove $n$ è il numero di osservazioni (per un singolo titolo), quindi la lunghezza dei vettori dei rendimenti. Per farlo faccio `MATR.PRODOTTO(MATR.TRASPOSTA(A),A)`

# TOPSIS
Poniamo di dover scegliere uno tra tante alternative, potrebbero essere opzioni, dottorandi, titoli, qualsiasi cosa. Abbiamo tanti criteri di scelta, ognuno assegna un voto ad ogni alternativa componendo la matrice $x$. Inoltre ad ogni criterio è assegnato un peso che saranno le componenti del vettore $w$. 
1. Normalizziamo la matrice $x$, visto che ogni criterio ha una scala diversa. Per farlo calcoliamo $s_i = \sqrt{\sum_{i=1}^m x_{ij}^2}$ per ogni colonna, dividendo poi ogni elemento $x_{ij}$ della colonna $j$ per $s_i$:
	   $r_{ij} = x_{ij}/s_i$
2. Ogni colonna è moltiplicata per il peso del criterio: $v_{ij} = w_j * r_{ij}$ 
3. Determino la soluzione **ideale** e quella **anti-ideale**:
	   $A^*_i = \max_j v_{ij}$
	   $A'_i = \min_j v_{ij}$
4. Determino la distanza dalla soluzione ideale e da quella anti-ideale per ogni alternativa (riga della matrice normalizzata e pesata), ottenendo per ciascuna due valori:
	   $D^*_i = \sqrt{\sum (v_{ij} - A^*_i)^2}$ 
	   $D'_i = \sqrt{\sum (v_{ij} - A'_i)^2}$ 
5. Calcolo la **relative closeness** per ciascun elemento:
	   $C_i = D'_i /(D'_i + D^*_i)$ 
6. Vince l'alternativa che è più vicina ad 1












