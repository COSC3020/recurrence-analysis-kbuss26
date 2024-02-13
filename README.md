[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-24ddc0f5d75046c5622901739e7c5dd533143b0c8e959d652212380cedb1ea36.svg)](https://classroom.github.com/a/OlW38W4k)
# Recurrence Analysis -- Mystery Function

Analyze the running time of the following recursive procedure as a function of
$n$ and find a tight big $O$ bound on the runtime for the function. You may
assume that each operation takes unit time. You do not need to provide a formal
proof, but you should show your work: at a minimum, show the recurrence relation
you derive for the runtime of the code, and then how you solved the recurrence
relation.

```javascript
function mystery(n) {
    if(n <= 1)
        return;
    else {
        mystery(n / 3);
        var count = 0;
        mystery(n / 3);
        for(var i = 0; i < n*n; i++) {
            for(var j = 0; j < n; j++) {
                for(var k = 0; k < n*n; k++) {
                    count = count + 1;
                }
            }
        }
        mystery(n / 3);
    }
}
```

Add your answer to this markdown file. [This
page](https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/writing-mathematical-expressions)
might help with the notation for mathematical expressions.

### Response

The one if-else statement and the recursive calls indicate a recurrence relation with
a base case, where $n <= 1$, and the recursive case, with ``mystery(n / 3)``  being called
3 times. 

Furthermore, there exists a sequentially double-nested loop alongside the recursive calls.
The first loop terminates at $n^2$ iterations, the second at $n$ iterations, and the
third at $n^2$ iterations. The sequential for loop is as follows:<br>

$(n ^ 2) * (n * (n ^ 2))$<br>
$= n * n * n * n * n$, by properties of exponents<br>
$= n^5$, by properties of exponents<br>

The nested loop is sequential to the 3 recursive calls, so ignoring constant operations
(i.e. declaring the count var), the recursive relation exists as
$3T(n/3) + n^5$, where $T(n)$ represents the time for input size $n$.

The base case only has one statement when $n <= 1$, so for the base case, $T(n) = 1$.

Putting this together, we get that <br>
$T(n) = 1$, when $n <= 1$<br>
$T(n) = 3T(n/3) + n^5$, otherwise<br>

We can now solve for $\Theta(n)$ by substitution for a general idea of $O(n)$.

$T(n) = 3T(n/3) + n^5$<br>
$= 3(3T(n/9) + (n/3)^5) + n^5$<br>
$= 9T(n/9) + n^5 + 3(n/3)^5$<br>
$= 9(3T(n/27) + n^5/9) + n^5 + n^5/3^4$<br>
$= 27T(n/27) + n^5 + n^5/3^4 + n^5/9^4$<br>
$= ...$<br>
$= 3^iT(n/3^i) + n^5 + n^5/3^4 + n^5/9^4 + ... + n^5/3^{4(i-1)}$<br>

Suppose $i = log{_3}{n}$. Then we can say, using the base case, that<br>

$T(n) = 3^iT(n/3^i) + n^5 + n^5/3^4 + n^5/9^4 + ... + n^5/3^{4(i-1)}$<br>
$= nT(1) + n^5 + n^5/3^4 + n^5/9^4 + ... + n^5/3^{4(log{_3}{n}-1)}$<br>
$= n + n^5 + n^5/3^4 + n^5/9^4 + ... + n^5/3^{4log{_3}{n}-4}$<br>

With $n^5$ being the biggest growth rate in the final solution, we can say that this
algorithm fits in $O(n^5)$.
