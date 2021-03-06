# Functions of Continuous Distributions

## Probability density functions

### Summation Doesn't Work
Let $X$ be a random variable representing the height in feet of a randomly selected person. What is the probability that $5.9 \leq X \leq 6.1$? If $X$ was discrete we could calculate the probability as a sum: 
$$\sum_{5.9 \leq x \leq 6.1} p(x)$$
This sum doesn't make sense for a continuous random variable because $p(x)=0$ for all values of $x$. Intuitively, if there are infinitely many possible heights than having a height of exactly $6$ feet is just one of these infinite outcomes and the probability of it happening is $\frac{1}{\infty}=0$.

### Using Integrals
Let's look at a graph of the probability density function of heights. The **probability density function (or PDF)** is a special function where the area under the curve represents a probability. This means we use integrals instead of summations for continuous random variables. The area below represents $P(5.9 \leq X \leq 6.1)$.
```{r, include=FALSE}
#Code from https://www.r-bloggers.com/how-to-shade-under-a-normal-density-in-r/
shadenorm = function(below=NULL, above=NULL, pcts = c(0.025,0.975), mu=0, sig=1, numpts = 500, color = "gray", dens = 40,
                     justabove= FALSE, justbelow = FALSE, lines=FALSE,between=NULL,outside=NULL){

  if(is.null(between)){
    below = ifelse(is.null(below), qnorm(pcts[1],mu,sig), below)
    above = ifelse(is.null(above), qnorm(pcts[2],mu,sig), above)
  }
  
  if(is.null(outside)==FALSE){
    below = min(outside)
    above = max(outside)
  }
  
  lowlim = mu - 4*sig
  uplim  = mu + 4*sig
  
  x.grid = seq(lowlim,uplim, length= numpts)
  dens.all = dnorm(x.grid,mean=mu, sd = sig)
  
  if(lines==FALSE){
    plot(x.grid, dens.all, type="l", xlab="X", ylab="Density")
  }
  
  if(lines==TRUE){
    lines(x.grid,dens.all)
  }
  
  if(justabove==FALSE){
    x.below    = x.grid[x.grid<below]
    dens.below = dens.all[x.grid<below]
    polygon(c(x.below,rev(x.below)),c(rep(0,length(x.below)),rev(dens.below)),col=color,density=dens)
  }
  
  if(justbelow==FALSE){
    x.above    = x.grid[x.grid>above]
    dens.above = dens.all[x.grid>above]
    polygon(c(x.above,rev(x.above)),c(rep(0,length(x.above)),rev(dens.above)),col=color,density=dens)
  }

  if(is.null(between)==FALSE){
    from = min(between)
    to   = max(between)
    x.between    = x.grid[x.grid>from&x.grid<to]
    dens.between = dens.all[x.grid>from&x.grid<to]
    polygon(c(x.between,rev(x.between)),c(rep(0,length(x.between)),rev(dens.between)),col=color,density=dens)
  }
}
```
```{r, results = "asis", echo=FALSE}
shadenorm(mu=5.8, sig=1, between=c(5.9, 6.1), col="blue")
```
Approximating the area above with a trapezoid: 
\begin{align}
P(5.9 \leq X \leq 6.1) &= \int_{5.9}^{6.1} f(x) dx \notag \\
&= \text{base} \cdot \frac{h_1 + h_2}{2} \notag \\
&= (6.1 - 5.9) \cdot \frac{f(5.9) + f(6.1)}{2} \notag \\
&= .2 \cdot \frac{.4+.38}{2} \notag \\
&= .078 \notag
\end{align}

### Defining Probability Density Functions
The function $p(x)$ we sum in a discrete distribution is a probability mass function and the function $f(x)$ we integrate in a continuous distribution is a probability density function. This is similar to physics where a single point has no mass but we can integrate the density to find the mass of a region. Note that we us $p(x)$ for discrete distributions and $f(x)$ for continuous distributions.

Probability density functions have some special properties. A probability density function can't be negative because probabilities aren't negative. The total area under the curve must be $1$ since the probability of all possible outcomes is $1$. The integral from $a$ to $b$ must calculate $P(a \leq X \leq b)$.

***
```{definition, pdf, name="Properties of PDF"}


1. $f(x) \geq 0$
    
2. $\int_{- \infty}^{\infty}f(x) = 1$
    
3. $\int_{a}^{b}f(x) = P(a \leq X \leq b)$

```
***

### Cumulative Distribution Function
As before we define the cumulative distribution function $F(x) = P(X \leq x)$. For continuous random variables:
$$F(b) = \int_{-\infty}^{b}f(x)dx$$
It doesn't matter if inequalities are strict or not for continuous variables because $P(X \leq x) =P(X<x)$. This is because $P(X=x)=0$:
$$P(X \leq x) = P(X < x) + P(X=x) = P(X<x)+0=P(X<x)$$
Here are some useful properties of the CDF:

***
```{definition, cdf, name="Properties of CDF"}


1. $0 \leq F(x) \leq 1$

2. $F(x)$ is an increasing function because the area under the curve increases as the right bound increases.

3. $P(a \leq X \leq b) = F(b) - F(a)$
    
4. $P(X \geq a) = 1 - F(a)$

```
***
