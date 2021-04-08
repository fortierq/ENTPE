# Exemple de programme linéaire (PL)

## Problème linéaire
$$
\left\{
    \begin{array}{ll}
        \max z = 4x + 5y\\
        2x + y \leq 8 \\
        x + 2y \leq 7 \\
        y \leq 3 \\
        x, y \geq 0
    \end{array}
\right.
$$

(sous forme canonique : seulement des inégalités $\leq$ à part pour le max et les contraintes de positivité de $x, y$)

## Cas de deux variables

[Rprésentation graphique des contraintes avec Geogebra](https://www.geogebra.org/m/jcjnzg9x)  
Avec  Geogebra, en dessinant le polytope, on trouve un optimum de $\boxed{z = 22}$ pour $\boxed{x = 3}$ et $\boxed{y = 2}$.  
De manière générale, l'**optimum d'un PL est atteint en un sommet du polytope des contraintes**.

# Algorithme du simplexe

## 1ère étape

Mettre le problème sous forme standard (avec des $=$ au lieu de $\leq$).  
On utilise des **variables d'écarts** $e_1$, $e_2$, $e_3$.

$$
\left\{
    \begin{array}{ll}
        \max z = 4x + 5y\\
        2x + y + e_1 = 8 \\
        x + 2y + e_2 = 7 \\
        y + e_3 = 3 \\
        x, y, e_1, e_2, e_3 \geq 0
    \end{array}
\right.
$$

## 2ème étape

Ecriture sous forme de tableau 

| $x$ |  $y$ | $e_1$ | $e_2$  | $e_3$  | $z$  | =  |
|---|---|---|---|---|---|---|
| 2  | 1  | 1  |  0 | 0  | 0  | 8  |
| 1  | 2  | 0  | 1  | 0  | 0 |  7 |
|  0 |  1 |  0 | 0  | 1  | 0  |  3 |
|  -4 |  -5 |  0 | 0  | 0  | 1  |  0 |

($z = 4x + 5y \Longleftrightarrow 4x + 5y - z = 0$)

## 3ème étape

À chaque instant de l'algo, on a une base (ensemble de variables parmi $x$, $y$, $e_1$, $e_2$, $e_3$ non nulles). Les variables qui ne sont pas dans la base valent 0.  
Remarque : cette base correspond à un sommet du polytope.

Dans la base initiale, on choisit en général les variables d'écarts, donc $e_1$, $e_2$, $e_3$.  Et donc $x = y = 0$.  
Il faut que cette base soit admissibles, c-a-d vérifie les conditions du PL (si ce n'est pas le cas, on applique la méthode du simplexe à 2 phases).  
$x = y = 0 \Longrightarrow$ $
\left\{
    \begin{array}{ll}
        \max z = 4x + 5y\\
        e_1 = 8 \geq 0\\
        e_2 = 7 \geq 0\\
        e_3 = 3 \geq 0\\
        x, y, e_1, e_2, e_3 \geq 0
    \end{array}
\right.
$  
On voit que (x, y, $e_1$, $e_2$, $e_3$) = (0, 0, 8, 7, 3) est une base admissible.

| $x$ |  $y$ | $e_1$ | $e_2$  | $e_3$  | $z$  | =  |
|---|---|---|---|---|---|---|
| 2  | 1  | 1  |  0 | 0  | 0  | 8  |
| 1  | 2  | 0  | 1  | 0  | 0 |  7 |
|  0 |  1 |  0 | 0  | 1  | 0  |  3 |
|  **-4** |  **-5** |  0 | 0  | 0  | 1  |  0 |

On choisit de rentrer une variable dans la base qui permette d'augmenter z (c'est à dire avec un **coeff négatif** sur la dernière ligne)
On choisit par ex. de rentrer $y$ dans la base.  
On utilise comme **pivot la ligne telle que Bp/p soit minimum** :
 
| $x$ |  $y$ | $e_1$ | $e_2$  | $e_3$  | $z$  | Bp/p |
|---|---|---|---|---|---|---|
| 2  | 1  | 1  |  0 | 0  | 0  | 8/1  |
| 1  | 2  | 0  | 1  | 0  | 0 |  7/2 |
|  0 |  1 |  0 | 0  | 1  | 0  |  3/1 |
|  **-4** |  **-5** |  0 | 0  | 0  | 1  |  0 |

min(8, 7/2, 3) = 3 (ce qui correspond à la **3ème ligne** $E_3$) donc on va mettre des 0 sur la colonne de $y$ sauf un 1 sur la **3ème ligne** (pivot).  Pour cela on fait des opérations sur des lignes, comme pour la méthode du pivot de Gauss.  
(Remarque : comme auparavant c'était $e_3$ qui avait un 1 sur la 3ème ligne dans la base, il va être enlevé de la base.)

| $x$ |  $y$ | $e_1$ | $e_2$  | $e_3$  | $z$  | = |
|---|---|---|---|---|---|---|
| 2  | 0  | 1  |  0 | -1  | 0  | 8 - $E_3$ = 5 |
| 1  | 0  | 0  | 1  | -2  | 0 |  7 - $2E_3$ = 1|
|  0 |  1 |  0 | 0  | 1  | 0  |  3 |
|  -4 |  0 |  0 | 0  | 5  | 1  |  0 + 5 $E_3$ = 15|

On rentre $x$ (qui a un coeff négatif)

| $x$ |  $y$ | $e_1$ | $e_2$  | $e_3$  | $z$  | Bp/p |
|---|---|---|---|---|---|---|
| 2  | 0  | 1  |  0 | -1  | 0  | 5/2 |
| 1  | 0  | 0  | 1  | -2  | 0 |  1/1|
|  0 |  1 |  0 | 0  | 1  | 0  |  3 |
|  -4 |  0 |  0 | 0  | 5  | 1  |  = 15|

min(5/2, 1/1) = 1 $\Longrightarrow$ on sort $e_2$  

| $x$ |  $y$ | $e_1$ | $e_2$  | $e_3$  | $z$  | = |
|---|---|---|---|---|---|---|
| 0  | 0  | 1  |  -2 | 3  | 0  | 5 - $2E_2$ = 3 |
| 1  | 0  | 0  | 1  | -2  | 0 |  1|
|  0 |  1 |  0 | 0  | 1  | 0  |  3 |
|  0 |  4 |  0 | 4  | -3  | 1  |  = 15 + 4 $E_2$ = 19|

On entre $e_3$ qui a un coeff négatif pour $z$.

| $x$ |  $y$ | $e_1$ | $e_2$  | $e_3$  | $z$  | Bp/p |
|---|---|---|---|---|---|---|
| 0  | 0  | 1  |  -2 | 3  | 0  | 3/3 |
| 1  | 0  | 0  | 1  | -2  | 0 |  $< 0$|
|  0 |  1 |  0 | 0  | 1  | 0  |  3/1 |
|  0 |  4 |  0 | 4  | -3  | 1  | 19|

min(3/3, 3/1) = 1 donc on va utiliser la 1ère ligne comme pivot (et on sort $e_1$). On commence par diviser $E_1$ par 3 pour avoir un 1 comme pivot puis on soustrait les lignes pour mettre des 0 :  
| $x$ |  $y$ | $e_1$ | $e_2$  | $e_3$  | $z$  | = |
|---|---|---|---|---|---|---|
| 0  | 0  | 1/3  |  -2/3 | 1  | 0  | 1 |
| 1  | 0  | 2/3  | -1/3  | 0  | 0 |  1 + 2$E_1$ = 3|
|  0 |  1 |  -1/3 | 2/3  | 0  | 0  |  3 - $E_1$ = 2 |
|  0 |  4 |  1 | 2  | 0  | 1  | 19 + 3$E_1$ = 22|

...

## Fin de l'algorithme

Quand tous les coeffs de la dernière ligne sont positifs, on ne peut plus augmenter la valeur de $z$ et donc on arrête l'algo et on en déduit l'optimum.  
Ici on retrouve bien un optimum $\boxed{z = 22}$, obtenu avec $e_1 = e_2 = 0$ (variables hors base) et $\boxed{x = 3}$ (ligne 2), $\boxed{y = 2}$ (ligne 3), $e_3 = 1$ (ligne 1).