---
layout: post
title:  "Implementation of SMO Algorithm in Python: SVMs Simplified"
date:   2020-12-27 09:12:17 +0330
categories: svm
---

# Hard and Soft SVM

Imagine two linearly separable points on a two-dimensional xy-coordinate system:

![Linearly Separable](/assets/img/hard-svm.png)

*Hard-SVM* is the learning rule in which we return an Empirical risk minimization (ERM) hyperplane that sperates the training set with the *largest margin possible*. This ERM is hard to solve, even using the best Quadratic programming algoirthms.

Quadratic programming or QP is concerned with solving *optimization problems* which are about n-planes (lines, planes, and hyperplanes) --- basically, function which its graph is a parabola in many dimensions.

On the other hand, we have *Soft-SVM*. SVMs which their ERM is not concerned with the linearly separable data being separated by *that wide of a margin*. Errors are allowed. 

![Linearly Separable](/assets/img/soft-svm.png)

In short, Soft-SVM is defined as:

> for features X of size m which are *somewhat* linearly separable, there exists a m-1-dimensional plane which seperates *most* of them into two classes. The vectors from the plane to the highest margin are called *Support Vectors*.

# Duality and Lagrange Multipliers
SVMs are linear classifiers in the way that their *objective function* looks like:

![Linearity](/assets/latex/svm_linear.png)

In which if f(x) >= 0, then y = - 1, else, y = 1.

In optimization, *duality* means that optimization problems may be viewed from eitherof two prespectives, the primal problem or the dual problem. The solution ot the dual problem provides a lower bound to the solution of the primal problem.

Our ERM is optimizing this linear f(x). We state this minimization problem as:

![Minimization](/assets/latex/minimization.png)

The g and h functions define the *equality* and *inequality* contraints respectively. But how do we find these constraints?

In optimization, the method of Lagrange multipliers is a strategy of finding the local maxima and minima of a function subject to equality contraints.

Imagine we have this minimization problem:

![Minimization Problem](/assets/latex/min_problem.png)

We can solve this minimization problem by finding the minima of g(x, y). We do this using gradient descent (read more about gradient descent in other posts) in a way that:

![Lagrange Multiplier](/assets/latex/lar_multi.png)

This is what we call a *Lagrange multiplier*. They are *critical points*, not *local extrema*, despite the fact that they find the *minimum* of the function (remember, it's min, not *argmin*!)

# The SMO Algorithm

SMO stands for Sequential minimal optimization and is a QP problem. It was proposed by John Platt in 1998. It's basically a dual optimization problem which can be stated as:

![SMO Problem](/assets/latex/smo_problem.png)


We're going to explain this algorithm, all the while we implement it in Python.

## Imports, Hyperparameters and Data Input

We're going to import Numpy, random, time and math.

```
import numpy as np
import random
import time
import math
```

And the hyperparameters are:

```
C: regularization parameter
tol: numerical tolerance
max-passes: maximum number of iterations over Larange multipliers without changing
```

Data inputs are `X` and `y`. `X` is a multidimensional Numpy array. `y` is a one-dimensional array of the same type.

## The Linear Classifier

The linear classifier we're going to use is:

![Linear Classifier](/assets/latex/lin_classifier.png)

We can replace the inner product (the *angled brackets* stand for the inner product of the ith X and the entire X) with a *kernel function* such as RBF or Sigmoid.

The Pythonic code for this is:

```
def linear_classifier(alpha, X, X_i, y_i, b):
    return alpha * y_i * np.inner(X_i, X).sum() + b
```

Here `alpha_i` is the current Lagrange multiplier.

As we said, `np.inner(x_i, X)` can be replaced with a kernel function. This can be done with Logistic Regression as well. In case of using a Kernel function to aide the linear classifier, it is called a *Kernel method*. The subject of this blog's next post will be kernel methods. 

## Computing the Constraints

Imagine Lagrange multiplier alpha having an upper bound constraint H and a lower bound constraint L in such way that ![Lower Upper Bound](/assets/latex/lower_upper_alpha.png). Thusly, these constraints are computed according to this conditional equation:

![Compute L and H](/assets/latex/compute_l_h.png)

Which we implement in Python as:

```
def compute_L(alpha_i, alpha_j, y_i, y_j, C):
    if y_i != y_j:
        return max(0, alpha_i - alpha_j)
    else:
        return max(0, alpha_i + alpha_j - C)


def compute_H(alpha_i, alpha_j, y_i, y_j, C):
    if y_i != y_j:
        return min(C, C + alpha_i + alpha_j)
    else:
        return min(C, alpha_i + alpha_j)
```

As we said, `C` here is the regularization parameter.

## Calculating Error and Eta

You can think of error as `f(x_i) - y_i`. `eta`, on the other hand, is a parameter in the algorithm that helps us assess if we wish to continue to the next iteration. These two are equated as:

![Error and Eta](/assets/latex/error_eta.png)

And are implemented as:

```
def calculate_eta(X_i, X_j):
    i_j_inner = np.inner(X_i, X_j)
    i_i_inner = np.inner(X_i, X_i)
    j_j_inner = np.inner(X_j, X_j)

    return 2 * (i_j_inner - i_i_inner - j_j_inner)


def calculate_error(X_i, y_i, alpha, X, b):
    f_x_i = linear_classifier(alpha, X, X_i, y_i, b)
    E_i = f_x_i - y_i
    
    return E_i

```

## Calculating the Lagrange Multipliers

We need two Lagrange multipliers for this algorithm (alpha_i and alpha_j in the dual optimization problem), and as we'll see, we'll iterate until they converge. To calculate these two multipliers, we need to implement three equations:



![Alpha i](/assets/latex/alpha_i_eq.png)


![Alpha J](/assets/latex/alpha_j_eq.png)

![Alpha J](/assets/latex/alpha_j_clip.png)


As you can see, we need to clip the second multiplier based on H and L. you'll see why in the algorithm.

```
def clip_alpha_j(alpha_j, H, L):
    if alpha_j > H:
        return H
    elif alpha_j < L:
        return L
    else:
        return alpha_j


def calculate_alpha_j(alpha_j, E_i, E_j, eta):
    return alpha_j - ((y_j * (E_i - E_j)) / eta)


def calculate_alpha_i(alpha_i, y_i, y_j, alpha_j_old, alpha_j):
    return alpha_i + (y_i * y_j) * (alpha_j_old - alpha_j)

```

## Calculating the thresolhd

As you can see in the optimization problem, we have a threshold called b. To calculate this b, we need to first calculate two bs, b1 and b2, and calculate the final threshold based on them.

![b1](/assets/latex/b1.png)

![b2](/assets/latex/b2.png)

![b](/assets/latex/b.png)

We implement these in Python as:

```
def calculate_bs(b, X_i, y_i, alpha_i, alpha_j, E_i, E_j, X_j, y_j, alpha_i_old, alpha_j_old):

    b_one = (b - E_i - y_i) * (alpha_i - alpha_i_old) * np.inner(X_i, X_i) - (y_j * (alpha_j - alpha_j_old)) * np.inner(X_i, X_j) 
    b_two = (b - E_j - y_i) * (alpha_i - alpha_i_old) * np.inner(X_i, X_i) - (y_j * (alpha_j - alpha_j_old)) * np.inner(X_i, X_j) 

    return b_one, b_two

def compute_b(b_one, b_two, alpha_i, alpha_j, C):
    if alpha_i > 0 and alpha_i < C:
        return b1
    elif alpha_j > 0 and alpha_j < C:
        return b2
    else:
        (b_one + b_two) / 2
```

## Random j

In our algorithm, i is the iterator, but j is a random value. We calculate this random number using this function:

```
def rand_j(m, i):
    random.seed(time.time())
    
    j = random.randint(0, m)

    while i == j:
        j = random.randint(0, m)

    return j
```


## SMO Algorithm

The goal of SMO is to calculate two things, the Lagrange multipliers, which is an array of numbers in the size of the inpput features, and the threshold b. This function looks like:

```
def Simplified_smo(C=C, tol=tol, max_passes=max_passes, X=X, y=y):
    m = X.shape[0]    
    b = 0
    alphas = np.zeros(m)
    alphas_old = np.zeros(m)
    passes = 0

    while passes < max_passes:
        num_changed_alphas = 0
        for i in range(1, m):
            E_i = calculate_error(X[i], y[i], alphas[i], X, b)
            y_i_E_i = y[i] * E_i

            if (y_i_E_i < (-1 * tol) and alpha[i] < C) or (y_i_E_i > tol and alpha[i] > 0):
                j = rand_j(m, i)
                E_j = calculate_error(X[j], y[j], alphas[j], X, b)
                alphas_old[i], alphas_old[j] = alphas[i], alphas[j]

                L = compute_L(alphas[i], alphas[j], y[i], y[j], C)
                H = compute_H(alphas[i], alphas[j], y[i], y[j], C)                

                if L == H:
                    continue

                eta = calculate_eta(X[i], X[j])

                if eta >= 0:
                    continue

                alphas[j] = clip_alpha_j(calculate_alpha_j(alpha[j], E_i, E_j, eta), H, L)

                if math.abs(alphas[j] - alphas_old[j]) < 0.000005:
                    continue

                alphas[i] = calculate_alpha_i(alphas[i], y[i], y[j], alphas_old[j], alphas[j])

                b_one, b_two = calculate_bs(b, X[i], y[i], alphas[i], alphas[j], E_i, E_j, X[j], y[j], alphas_old[i], alphas_old[j])

                b = compute_b(b_one, b_two, alphas[i], alphas[j], C)

                num_changed_alphas += 1
        
        if num_changed_alphas == 0:
            passes += 1
        else:
            passes = 0

        return alphas, b

```

***

This algorithm is explained in detail in [this](/assets/pdf/smo.pdf) paper. Enjoy coding!

