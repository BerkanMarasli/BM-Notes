







What is the best possible solution?

- Minimum amount of code?
- Best performance?
- Least memory usage?
- Personal preference?



How do we measure performance? If we can't measure it, we cant judge it.



Example:

![Screenshot 2023-03-24 at 18.43.15](/Users/Berkan.Marasli/Downloads/Screenshot 2023-03-24 at 18.43.15.png)

Use constant time algorithm: (n/2) * (1 + n)



1. Measure the start time, end time and find the difference to know how long it takes time complexity wise. performance.now() gives the current timestamp in JS. Lots of influencing factors using this approach that are independent of your algorithm : computer specs, how many processes are running in background, some caching occurs with the browser, etc. Unreliable.



Constant time O(1)

Linear time O(n)

quadratic time O(n^2)

cubic time O(n^3)



General approach to find time complexity of an algorithm is to:

- Assume all expressions take the same amount of time to execute
- count the number of expression executions for each execution.

![Screenshot 2023-03-24 at 19.05.08](/Users/Berkan.Marasli/Downloads/Screenshot 2023-03-24 at 19.05.08.png)

We don't care about the exact number of executions but the general pattern of the execution count (growth rate). Therefore find the fastest growing term in the equation and remove the coefficient from the term as you don't care about the number itself (process known as Asymptotic Analysis).

![Screenshot 2023-03-24 at 19.08.59](/Users/Berkan.Marasli/Downloads/Screenshot 2023-03-24 at 19.08.59.png)

for constant time:

![Screenshot 2023-03-24 at 19.13.33](/Users/Berkan.Marasli/Downloads/Screenshot 2023-03-24 at 19.13.33.png)

![Screenshot 2023-03-24 at 19.14.35](/Users/Berkan.Marasli/Downloads/Screenshot 2023-03-24 at 19.14.35.png)



In theory, any mathetmical construct is possible but these are some of the common ones:

![Screenshot 2023-03-24 at 19.21.46](/Users/Berkan.Marasli/Downloads/Screenshot 2023-03-24 at 19.21.46.png)





















