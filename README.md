# challenge-roadef-2020
This challenge is about solving a Mixed Integer Non Linear problem: MINLP



#Methods used 
La recherche de méthodes de résolution de pro
Dans la littérature, les problèmes NLP (Non linear Problem) et MIP (Mixed Intégrer Problem ) sont des problèmes NP-difficiles. Il existe des méthodes de résolution de ces problèmes en utilisant des solveurs tels que AlphaECP [GAMS], ANTIGONE [GAMS], BARON [AMPL] [GAMS], Bonmin [AMPL] [GAMS] et d’autres.( 1). Ces solveurs résolvent généralmement de problèmes de type NLP puis utiilisent la méthode brach&bound pour trouver une solution entière ou binaire.
Dans le but de faciliter l’implémentation, nous avons utilisé une librairie disponible sous python. Parmi les librairies existantes, il y a Pyomio, Gekko
Approche1 : Tester tous les cas possibles. La complexité de ce programme est exponentielle, ce qui ne sera pas du tout optimal en temps si les nombres d'interventions ou de trimestre dépasse 3. Il n'est pas nécessaire de tester cette méthode pour savoir que le temps de calcul sera très grand. En effet, il y a un objectif non linéaire alors qu'il faut tester chaque solution possible. Il est plus efficace de trouver une méthode qui génère une solution puis l'utiliser dans une méta-heuristique que de faire cette énumération.
Approche 2 : Résoudre le problème MINLP
A l'aide du soldeur Python utilisé, on définit les variables de décision comme des variables binaires, on écrit les contraintes et l'objectif sous un format adapté à la résolution ( cf la documentation).
Cette méthode fait de la résolution en nombre réels et utilise la méthode Branch&Bound afin de trouver une bonne solution. Comme il y a des contraintes non-linéaires et non-régulières, il y a des fonctions alternatives à utiliser. Ces fonctions sont au moins de classe C1 en réalisant des prolongements. Mais, compte tenu de la complexité du second terme de l’objectif, l’algorithme atteint un nombre important d’itérations sans converger.
Approche3 : Ne pas tenir compte du second terme de la fonction objectif
En effet, ce terme présente une non-linéarité assez forte. En effet, il y a des fonctions tels que min, max qui sont non linéaires.
En tenant compte de cet objectif, il y a des fonctions à régulariser. De plus, il y a plus de variables intermédiaires, ce qui augmente le nombre d'itérations du solveur. Ainsi, comme il y a des limites sur le nombre d'itération, la méthode ne converge pas toujours. En revanche, enlever un objectif modifié le problème. Mais, le second terme de la fonction objective est un écart-type utilisé pour capturer les valeurs des risques trop élevés. Donc, ce terme est généralement faible par rapport au premier objectif, ce qu’on peut vérifier sur les exemples présentés en dessous. Donc on peut laisser le deuxième terme à conditions que les valeurs des risques ne contiennent pas des valeurs trop élevées par rapport à la moyenne.
Pratiquement, sur les deux premiers exemples, en tenant compte de toutes les contraintes et en négligeant le second terme de la fonction objectif, le soldeur en MINLP trouve la solution optimale. Un avantage de cette méthode est qu'on peut rapidement trouver une solution qui vérifie les contraintes. Cette solution sera un input pour notre méta-heuristique.
Sur les instances de la classe A, l’algorithme atteint un nombre limité d’itération sans trouver une solution faisable et donc s’arrête sauf l’instance n°9 qui contient le moins de variables de décisions.
Approche4 : Utiliser un solveur NLP : Conserver l'objectif et relaxer la contrainte entière.
Comme il n'est pas possible de trouver une formulation équivalente du problème comme un problème à variables réelles sans contraintes entières, on effectue une relaxation de la contrainte et on résout le problème non linéaire en nombre réels. On ajoute alors comme contrainte, la bernique des variables de décision (entre 0 et 1).
L'intérêt de cette méthode est qu'on trouve une borne inférieure à notre optimum.

#Read more
- Resolution with python
	- NLP
		- http://www.pyopt.org/
		- https://docs.scipy.org/doc/scipy/reference/tutorial/optimize.html#constrained-minimization-of-multivariate-scalar-functions-minimize
	- Pyomo: https://pyomo.readthedocs.io/en/stable/pyomo_overview/simple_examples.html
		
	- gekko:
		- https://gekko.readthedocs.io/en/latest/model_methods.html?highlight=array#pre-built-objects
		- https://apmonitor.com/wiki/index.php/Main/GekkoPythonOptimization
		- handle bug: 
			- https://stackoverflow.com/questions/56671888/how-to-retrieve-the-infeasibilities-txt-from-the-gekko
			- https://stackoverflow.com/questions/56942615/how-to-fix-solution-not-found-error-in-python-gekko-optimal-control-code
	- nomad: https://www.gerad.ca/nomad/

- More about solving MINLP problems
	- overview:
		- NLP:
			- https://www.researchgate.net/post/What-types-of-Optimization-Problems-are-supported-by-AMPL-AIMMS-GAMS-Pyomo
		- integer
			- https://scicomp.stackexchange.com/questions/19870/python-solvers-for-mixed-integer-nonlinear-constrained-optimization
			- bonnin:
				https://www.researchgate.net/post/Can-you-suggest-best-solver-for-the-mixed-integer-nonlinear-programming
				https://stackoverflow.com/questions/42391945/solving-minlp-with-pyomo-and-bonmin
		- MINLP: 
			- https://sci-hub.st/https://doi.org/10.1007/978-1-4614-1927-3_1
			- https://link.springer.com/book/10.1007/978-1-4614-1927-3
			
	- https://hal.archives-ouvertes.fr/tel-01450981/document



