# Variational_Auto_Encoder
## Basic knowledge to understand VAE in better way 
The key is to notice that any distribution in d dimensions can be generated by taking a set of d variables that are normally distributed and mapping them through a sufficiently complicated function. For example, say we wanted to construct a 2D random variable whose values lie on a ring. If z is 2D and normally distributed, g ( z ) = z/10 + z/ || z || is roughly a ring

![Density_approximation](https://user-images.githubusercontent.com/21220616/58219328-08202900-7d28-11e9-8a35-29a1659ffcc2.png)

Our model is representative of our dataset, we need to make sure that for every datapoint X in the dataset, there is one (or
many) settings of the latent variables which causes the model to generate something very similar to X.Formally, say we have a vector of latent variables z in a high-dimensional space Z which we can easily sample according to some probability density function (PDF) P ( z ) defined over Z.

![Euclids_Perception](https://user-images.githubusercontent.com/21220616/58311378-73502500-7e26-11e9-8c68-e8e6dbba8236.png)


Then, say we have a family of deterministic functions f ( z; θ ) , parameterized by a vector θ in some space Θ, where 
f : Z × Θ → X . f is deterministic, but if z is random and θ is fixed, then f (z; θ) is a random variable in the space
X . We wish to optimize θ such that we can sample z from P(z) and, with high probability, f ( z; θ ) will be like the X’s in our dataset

## Objective function 
![](http://latex.codecogs.com/gif.latex?P%28X%29%20%3D%20%5Cint%20P%28X%7Cz%3B%5CTheta%20%29P%28z%29dz)
The intuition behind this framework—called “maximum likelihood”— is that if the model is likely to produce training set samples, then it is also likely to produce similar samples, and unlikely to produce dissimilar ones.

## Variational Auto Encoder
we first sample a large number of z values { z 1 , ..., z n } , and compute ![Image](http://latex.codecogs.com/gif.latex?P%28X%29%20%5Capprox%20%5Cfrac%7B1%7D%7Bn%7D%20%5Csum%20P%28X%7Cz%29)
In practice, for most z, P ( X | z ) will be nearly zero, and hence contribute almost nothing to our estimate of P ( X ).
The key idea behind the variational autoencoder is to attempt to sample values of z that are likely to have produced X, and compute P ( X ) just from those. This means that we need anew function Q ( z | X ) which can take a value of X and give us a distribution over z values that are likely to produce X. Hopefully the space of z values that are likely under Q will be much smaller than the space of all z’s that are likely under the prior P ( z )

![derivation](http://latex.codecogs.com/gif.latex?%5Cboldsymbol%7BD%7D%5BQ%28z%29%7C%7CP%28z%7CX%29%5D%20%3D%20%5Csum%20Q%28z%29log%28Q%28z%29%29-Q%28z%29log%28P%28z%7CX%29%29%20%5Chspace%7B2cm%7D..eqn%281%29%20%5C%5C%20%5C%5CP%28z%7CX%29%20%3D%20%5Cfrac%7BP%28X%7Cz%29P%28z%29%7D%7BP%28X%29%7D%5Chspace%7B8.8cm%7D..eqn%282%29%20%5C%5C%20%5C%5CUsing%20%5Chspace%7B0.2cm%7D%20eqn%282%29%20%5C%5C%20%5C%5CD%5BQ%28z%29%7C%7CP%28z%7CX%29%5D%20%3D%20%5Csum%20Q%28z%29log%28Q%28z%29%29-Q%28z%29log%28%5Cfrac%7BP%28X%7Cz%29P%28z%29%7D%7BP%28X%29%7D%29%29%29%20%5C%5C%20%5C%5C%5Csum%20Q%28z%29log%28Q%28z%29%29-Q%28z%29log%28P%28z%29%29%20&plus;%20Q%28z%29log%28P%28X%29%29%20-Q%28z%29log%28P%28X%7Cz%29%29%20%5C%5C%20%5C%5CD%5BQ%28z%29%7C%7CP%28z%7CX%29%5D%3Dlog%28P%28X%29%29%20-%20%5Cmathbf%7BD%7D%5BQ%5Bz%7CX%5D%7C%7CP%28z%7CX%29%5D%20&plus;Q%28z%29log%28P%28X%7Cz%29%29%20%5C%5C%20%5C%5Clog%28P%28X%29%29%20-%20%5Cmathbf%7BD%7D%5BQ%5Bz%7CX%5D%7C%7CP%28z%7CX%29%5D%20%3D%20-%5Cmathbf%7BD%7D%5BQ%28z%29%7C%7CP%28z%7CX%29%5D%20&plus;%5Csum%20Q%28z%29log%28P%28X%7Cz%29%29)
This equation serves is the core of the variational autoencoder,
the left hand side has the quantity we want to maximize: log P(X) (plus an error term, which makes Q produce z’s that can reproduce a given X
