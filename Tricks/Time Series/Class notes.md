Fonction de Corrélation (FAC)

Le coefficient de corrélation entre deux composantes d'une série temporelle à des dates différentes est appelé le coefficient d'autocorrélation. Il est défini par:7

$\rho_{X_t, X_{t-h}} = \frac{Cov(X_t, X_{t-h})}{\sqrt{V(X_t)V(X_{t-h})}}$

Lorsque le processus est stationnaire, le coefficient d'autocorrélation ne dépend que du décalage temporel h et est noté $\rho_h$:

$\rho_h = \frac{\gamma(h)}{\sigma_X^2}$

La fonction d'autocorrélation (FAC) d'un processus stationnaire $(X_t)$ est une fonction qui donne le coefficient d'autocorrélation pour chaque décalage temporel k

$\rho_k: \mathbb{Z} \rightarrow [-1, 1]$

$k \mapsto \rho_k = \frac{\gamma_k}{\gamma_0}$

L'autocorrélogramme est la représentation graphique de la FAC. Il permet de visualiser la mémoire d'un processus, c'est-à-dire comment ses valeurs passées influencent ses valeurs présentes.

L'autocorrélogramme peut aider à identifier le modèle approprié pour une série temporelle.

Un bruit blanc a une FAC nulle pour tous les décalages non nuls, ce qui signifie qu'il n'a pas de mémoire


## La Décomposition de Wold : Théorème Fondamental de l'Analyse des Séries Temporelles

La décomposition de Wold, formulée par Herman Wold en 1938, est considérée comme le théorème fondamental de l'analyse des séries temporelles stationnaires. Ce théorème affirme que **tout processus stochastique stationnaire (Xt) peut être représenté sous la forme d'une somme infinie pondérée de valeurs passées d'un bruit blanc**.

### Formulation Mathématique

La décomposition de Wold s'exprime mathématiquement comme suit:

$X_t = \mu_X + \sum_{k=0}^{\infty} \psi_k \epsilon_{t-k}$

où :

- **$X_t$** est la valeur du processus stochastique au temps t.
- **$\mu_X$** est une constante représentant la moyenne du processus.
- **$\psi_k$** sont des coefficients réels appelés "poids" ou "coefficients de Wold", avec $\psi_0 = 1$ et $\sum_{k=1}^{\infty} \psi_k^2 < \infty$.
- **$\epsilon_t$** est un bruit blanc centré de variance finie $\sigma^2$.

### Interprétation

Cette décomposition signifie que la valeur actuelle d'un processus stationnaire peut être décomposée en deux parties :

- **Une composante déterministe**: $\mu_X$, qui représente la moyenne du processus.
- **Une composante stochastique**: $\sum_{k=0}^{\infty} \psi_k \epsilon_{t-k}$, qui est une somme infinie pondérée de valeurs passées d'un bruit blanc.

La composante stochastique montre que la valeur actuelle du processus est influencée par des chocs aléatoires passés (représentés par le bruit blanc) dont l'impact diminue au fur et à mesure que l'on remonte dans le temps. La condition $\sum_{k=1}^{\infty} \psi_k^2 < \infty$ assure la convergence de la somme et garantit que l'influence des chocs passés finit par s'estomper.

### Importance

La décomposition de Wold a des implications importantes pour l'analyse des séries temporelles:

- **Représentation**: Elle fournit une représentation générale pour tout processus stochastique stationnaire.
- **Modélisation**: Elle sert de base pour la construction de modèles de séries temporelles, notamment les modèles ARMA (AutoRegressive Moving Average).
- **Prévision**: Elle permet de comprendre comment les valeurs passées influencent les valeurs futures et aide à la prévision des séries temporelles.

### Limitations

Bien que fondamentale, la décomposition de Wold a certaines limitations:

- **Infinité de paramètres**: La somme infinie de poids rend la décomposition difficile à utiliser directement en pratique. On utilise généralement des modèles avec un nombre fini de paramètres, comme les modèles ARMA.
- **Stationnarité**: La décomposition de Wold ne s'applique qu'aux processus stationnaires.

### Opérateur Retard et Décomposition de Wold

L'opérateur retard (L), défini par $LX_t = X_{t-1}$, peut être utilisé pour exprimer la décomposition de Wold de manière plus concise:

$X_t = \Psi(L)\epsilon_t + \mu$

où $\Psi(L) = \sum_{j=0}^{\infty} \psi_jL^j$ est un polynôme de degré infini en l'opérateur retard.

### Décomposition de Wold et Modèles ARMA

En pratique, la modélisation des séries temporelles repose sur le principe de parcimonie, qui consiste à privilégier les modèles les plus simples possibles. L'utilisation directe de la décomposition de Wold nécessiterait l'estimation d'une infinité de paramètres. Les modèles ARMA, en revanche, offrent une représentation parcimonieuse des processus stationnaires en utilisant un nombre fini de paramètres.

Les modèles ARMA combinent une composante autorégressive (AR) et une composante moyenne mobile (MA), ce qui permet de modéliser une grande variété de séries temporelles stationnaires. La décomposition de Wold fournit le cadre théorique pour justifier l'utilisation des modèles ARMA.

Les sources fournies n'abordent pas en détail l'estimation des coefficients de Wold ni les méthodes spécifiques pour appliquer la décomposition de Wold en pratique.