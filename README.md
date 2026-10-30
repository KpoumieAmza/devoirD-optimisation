# Methodes de descente et quasi-Newton

Projet d'optimisation sans contrainte : implementation et comparaison de quatre algorithmes
de minimisation d'une fonction de plusieurs variables.

## Contenu du dossier

- descente_et_quasi_newton.ipynb : notebook principal (theorie, code commente, experiences, graphiques)
- requirements.txt : bibliotheques Python necessaires
- README.md : ce fichier

## Algorithmes etudies

1. Descente du gradient a pas fixe
   x_{k+1} = x_k - alpha * grad_f(x_k), avec alpha constant.

2. Descente du gradient a pas variable
   Meme direction, mais le pas est calcule a chaque iteration par une recherche lineaire d'Armijo (backtracking).

3. Descente du gradient a pas conjugue (gradient conjugue)
   Les directions de descente sont conjuguees entre elles. Deux versions :
   - version quadratique, avec pas optimal explicite (convergence en au plus n iterations),
   - version non lineaire, avec les formules de Fletcher-Reeves et de Polak-Ribiere.

4. Quasi-Newton (BFGS)
   Approximation de l'inverse de la hessienne mise a jour a chaque iteration a partir des gradients,
   avec recherche lineaire d'Armijo.

## Fonctions de test

- Fonction quadratique en dimension 2 : f(x) = 1/2 x^T A x - b^T x (conditionnement 11).
- Fonction de Rosenbrock : f(x, y) = (1 - x)^2 + 100 (y - x^2)^2, minimum en (1, 1).

## Installation et lancement

Python 3.8 ou plus recent est necessaire.

    pip install -r requirements.txt
    jupyter notebook descente_et_quasi_newton.ipynb

Puis executer toutes les cellules (menu Cell, Run All).
Le temps d'execution total est de quelques secondes.

## Structure du notebook

1. Outils communs : historique et recherche lineaire d'Armijo
2. Les quatre algorithmes
3. Fonctions de test et fonctions d'affichage
4. Experience 1 : fonction quadratique (trajectoires, convergence, influence du pas fixe)
5. Experience 2 : fonction de Rosenbrock
6. Conclusion

## Resultats obtenus (tolerance 1e-6 sur la norme du gradient)

Fonction quadratique :

    Methode                     Iterations
    Gradient pas fixe (0.1)            138
    Gradient pas variable               72
    Gradient conjugue                    2
    Quasi-Newton BFGS                    5

Fonction de Rosenbrock :

    Methode                     Iterations
    Gradient pas fixe (0.001)        32076
    Gradient pas variable            13756
    Gradient conjugue (PR)            1126
    Gradient conjugue (FR)             191
    Quasi-Newton BFGS                   34

## Utiliser les fonctions sur votre propre probleme

Chaque algorithme prend la fonction f, son gradient et un point de depart, et renvoie
le point final et un historique (positions, valeurs de f, normes du gradient).

    x, historique = quasi_newton_bfgs(f, grad, x0=np.array([0.0, 0.0]))

## Remarques

- Les nombres d'iterations peuvent varier legerement selon les parametres (pas, tolerance, point de depart).
- Pour la descente a pas fixe, le pas doit verifier 0 < alpha < 2/L sur une fonction quadratique,
  ou L est la plus grande valeur propre de la hessienne.
- BFGS stocke une matrice n x n : en grande dimension, on prefere la variante L-BFGS.
