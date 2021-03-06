## [[Linear First-Order ODEs]]

A first-order ODE is said to be **linear** if it can be brought into the form

> $$y' = p(x)y = r(x) \tag{1}$$

Equation $(1)$ is linear with respect to both $y$ and its derivative $y'$, however, $p(x)$ and $r(x)$ may be any given function of $x$.

If the first term is $f(x)y'$, divide the equation by $f(x)$ to get the **Standard Form**.

In engineering, $r(x)$ is frequently called the **input**, and $y(x)$ the **output** or the **response** to the input and initial conditions.

### [[Homogeneous Linear ODE]]

 In a simple case of the Linear First-Order ODE, $r(x)$ is equal to zero, thus $(1)$ becomes
$$y' + p(x)y = 0 \tag{2}$$

and is called **homogeneous**. By using [[Separation of Variables]] and integrating we can obtain a general solution of the homogeneous ODE $(2)$,

$$y(x) = ce^{-\int p(x)dx} \tag{3}$$

> **Standard Form of Homogeneous Linear ODE**
> > $$y' + p(x)y = 0 $$
>
> **General Solution**
> > $$y(x) = ce^{-h}$$
> > where $h = \int p(x) dx$

### [[Nonhomogeneous Linear ODE]]

Now considering the case of $(1)$ where $r(x)$ is not equal to zero over the entire interval we are solving for. When this is the case, the ODE is called **nonhomogeneous**. We can find the general solution using integration factors. Conveniently, the integration factor relies only on $x$.

> **Standard Form of Nonhomogeneous Linear ODE**
> > $$y' + p(x)y = r(x) $$
>
> **General Solution**
> > $$y(x) = ce^{-h}(\int e^h r dx + c)$$
> > where $h = \int p(x) dx$

The general solution can be rewritten as the sum of two terms

$$y(x) = e^{-h}\int e^h r dx + ce^{-h}$$

to show the following:

> Total Output = Response to the Input $r$ + Response to the Initial Data