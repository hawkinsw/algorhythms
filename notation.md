# ~~99~~  Approximately 100 Red Balloons

> We shall now make one more simplifying abstraction: it is the rate of growth, or order of growth, of the running time that really interests us. We therefore consider only the leading term of a formula (e.g., $an^2$), since the lower-order terms are relatively insignificant for large values of $n$.

I want to make sure that come the end of the semester, we all have a very solid grasp of the vocabulary required to talk about the design of algorithms and, especially, their analysis.

So far we have used terms like _incremental_ and _divide and conquer_ and _invariant_ when talking about the design of algorithms.

As for the analysis of algorithms, we have learned several terms but _rate of growth_ and _order of growth_ seem to be the most important.

Before we dig into the precise meaning of these interchangeable terms, which is the point of this Algorhythm, let's remember our goal in analyzing the runtime of an algorithm and the tools that we have at our disposal.

## Where Were We

We have an algorithm -- one that we defined and one that computes the solution to a computational problem. We can look at its pseudocode and determine the _basic operations_ of which it is comprised. Because we are _not_ concerned about the idiosyncrasies of a given piece of hardware, our analysis of how long our algorithm will take to run on an input of a given size depends on the number of those basic operations that must be completed for the algorithm to compute a solution to a computational problem.

From that base we continue our analysis by defining a function (call it, $f$) that models our algorithm's runtime. The only parameter of this function is the size of the algorithm's input. Normally we call that value -- the size of the input to our algorithm and, subsequently, a vital bit of information necessary to calculate its runtime -- $n$. The output of the time-modeling function is the number of basic operations that must be completed for our algorithm to compute a solution to some instance of the computational problem (that is defined by $n$).

For example, it might be the case that, given an input of size $n$, an algorithm we design will need $n^2 + \frac{1}{2}n$ basic operations to compute a solution. In this case, we model the runtime of our algorithm using $f$:

$$f(n) = n^2 + \frac{1}{2}n $$

Or, it might be the case that, given an input of size $n$, an algorithm we design will need $n \  lg \  n + 2n$ basic operations to compute a solution. In this case, we model the runtime of our algorithm using this $f$:

$$f(n) = n \  lg \   n + 2n $$

Let's say, for purposes of exposition, that the two different algorithms modeled by these time functions both solve the same computational problem. Which one should we implement? Which one should we add to our [Killer App](https://en.wikipedia.org/wiki/Killer_application)? And do our $`f`$s help us decide?

> ### Sidebar: What do you mean, "size of the input"?
> &#x21F0; The phrase "size of the input" is just vague enough to be useful and just vague enough to be confusing. The mystery is a result of the fact that we are attempting to capture a characteristic of an instance of a computational problem so that it is applicable to as many algorithms as possible. When I was learning to analyze algorithms, some examples really helped me: For an algorithm that sorts lists of inputs, the "size of the input" is the length of the list of items to sort; For an algorithm that factors prime numbers, the "size of the input" is the number of digits in the number to factor; For an algorithm that predicts the weather in the future, the "size of the input" might be the number of days from now when we are concerned about precipitation. Notice a common theme: As the "size of the input" grows, the instance of the computational problem gets harder and harder.

Of course, if our application only uses the algorithm to compute solutions for inputs whose size is, maybe, $10$ or $20$, the solution will be ready in less than the blink of an eye. So, it really doesn't matter which algorithm we choose. On the other hand, if we are worried about our application scaling to the billions of users we expect, the choice of which algorithm to implement might make a difference.

## Rated $O$

So, which of the two algorithms will compute a solution the fastest? We have two functions that model the number of basic computations required to compute a solution given an input of a particular size -- one for each of the algorithms. And, remember, these functions compute a running time that is independent of the vagaries of individual CPUs and hardware (they calculate the [basic operations](#where-were-we)!). These functions are the tools we can use to do the analysis of the orders of growth of the runtime of our algorithms.

## Order of Growth

Forget, for a second, that we are using these functions for a purpose (to model the number of basic operations required for our algorithm(s) to compute) and just think about them as any old, ordinary mathematical functions (with the restriction that they are asymptotically nonnegative -- that is, their outputs are non-negative for sufficiently large input values).

We can talk about the rate of growth of a function. As the function's input gets larger, how does the output change? If the input gets $1$ bigger, does the output get $1$ bigger? $2$ bigger? $8$ bigger?

For functions that diverge (i.e., functions whose output does not near a fixed, constant value as the magnitude of their inputs goes to infinity), as their input increases in magnitude, the magnitude of the output of the function will go to infinity!

Going back to the context of measuring the runtime of an algorithm, think about the typical situation in computer science: As an algorithm is asked to do more and more work,[^inputsize] the time it takes to do that work keeps growing. Functions that model the runtime of such algorithms seem to fit the description just given (i.e., they are divergent!). 

[^inputsize]: [Recall](#sidebar-what-do-you-mean-size-of-the-input) that we said it is very common for an increased input size to be the way that we ask an algorithm to do an increased amount of work.

Because it will be common for our algorithm-runtime-modeling functions to be divergent, in order to decide which one is better[^better] will mean that we have to compare two functions whose magnitudes go to infinity? Comparing infinities? 

Big-$`O`$ no!

Are we stuck or do we just have to use calculus? Neither one sounds particuarly enjoyable, but at least one gets us to an answer. We will need tools that help us _somehow_ compare whether the magnitude of one function approaches infinity faster than another -- tools to analyze the order of growth!

[^better]: Where _better_ usually means faster, but could mean something with respect to memory usage.

## It All Goes Back to Knuth

And now we are back to _order of growth_ or _order of rate of growth_ and making comparisons. The section of CLRS quoted above is the _only_ place in the book where the authors define those terms. In fact, at the start of Chapter 3, the authors write:

> The order of growth of the running time of an algorithm, defined in Chapter 2, gives a simple characterization of the algorithm’s efficiency and also allows us to compare the relative performance of alternative algorithms.

Intuitively we all "know" what the term "order" means -- it means that we are waving our hands and omitting some details without losing what is important. But, what is the _real_ definition when we see it used in _order of growth_ in the textbook and when we are analyzing algorithms?

Like most things in computer science, all roads lead back to Knuth. In Volume 1[^v1] of his (still incomplete) tomb on computer science, Knuth gives a little history of the Big-$`O`$ notation:

> A very convenient notation for dealing with approximations was introduced by P. Bachmann in the book _Analytische Zahlentheorie_ in 1892. This "big-oh" notation ...

[^v1]: D. E. Knuth, _The Art of Computer Programming_. Addison-Wesley Professional, 1997.

It's pretty neat that we could[^bachmann] (if we spoke German),[^google] find our way all the way back to the source of this odd notation. After his first use of the Big-$`O`$ notation and his application of it to the analysis of the runtime of computer algorithms, Knuth wrote a letter to the editor in 1976 advocating for $\Theta$ and $\Omega$:

> Sometimes we also need a corresponding notation for lower-bounded functions, i.e., those functions which are at least as large as a constant times $f(n)$ for all large $n$.[^sigact]

[^bachmann]: Bachmann, Paul (1837-1920). 1872. Zahlentheorie : Versuch Einer Gesammtdarstellung Dieser Wissenschaft. Die Analytische Zahlentheorie / von Paul Bachmann.. B. G. Teubner (Leipzig). http://gallica.bnf.fr/ark:/12148/bpt6k994750.

[^google]: But, Google does and I tried to translate parts of it -- really, _really_ cool!

What do these letters have to do with our attempt at finding a better definition for _order of growth_? In his letter, as Knuth gives the precise definitions for Big-$`O`$, Big-$`\Omega`$ and Big-$`\Theta`$ that we have seen in class, he says that each can be "read" as 

> order at most ...

> order at least ...

and

> order exactly ...

I read these statements as Knuth saying that Big-$`O`$, Big-$`\Omega`$ and Big-$`\Theta`$ _are_ the definitions of the term _order of growth_. That reading is supported by the fact that Bachmann chose the $O$ in Big-$`O`$ to stand for the "o" in "order of approximation". 

Whew. I think we got to the bottom of _that_ mystery.

## Rising Interest Rates

Before we can be completely done with our research, we should make sure that we are completely clear with how we can compare these orders. Because the magnitudes of the (interesting) functions that we want to analyze all go to infinity, we are really concerned with the relative rate at which two different functions go to infinity! 

In other words, 

> The notion of the 'order' or the 'rate of increase' of a function is essentially a relative one. If we wish to say that 'the rate of increase of $f(x)$ is so and so' all we can say is that it is greater than, equal to, or less than that of some other function $\phi(x)$.[^infinities]

With that, I think we have seen all that we need to see: The sets defined by $O(g(n))$ (and the others) contain functions that are comparable (in some way -- less, greater, exact) to $g(n)$. In that way, we can say that, e.g., all the functions in the set $O(n^2)$ are the functions whose rate of growth is no worse than $n^2$.

I feel far more comfortable, now, using the notation! I hope that you do, too!

[^infinities]: Hardy, G. H. (Godfrey Harold), 1877-1947. Orders of infinity, the "infinitärcalcül" of Paul Du Bois-Reymond, Cambridge, University press, 1910

[^sigact]: Donald E. Knuth. 1976. Big Omicron and big Omega and big Theta. SIGACT News 8, 2 (April-June 1976), 18–24. https://doi-org.uc.idm.oclc.org/10.1145/1008328.1008329

## Put the Rhythm In Algorithms

["99 Luftballons"](https://en.wikipedia.org/wiki/99_Luftballons) from _Nena_ by Nena.
