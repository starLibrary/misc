# Accuracy rates of the test
- ### Test sensitivity : P(+|D)
> _**positive result when the patient has disease**_
- ### Test specificity : P(-|~D)
> _**negative test result when the patient doesn't have disease**_


- ### Prevalence of disease : P(D)


- ### Positive predictive value : P(D|+)
> _**the probability that a patient has the disease given a positive test result**_
- ### Negative predictive value : P(~D|-)
> _**the probability that a patient doesn't have the disease given a negative test result**_

<br/>

## By Bayes' Formula,
**P(D|+) &nbsp;&nbsp;&nbsp;= &nbsp;&nbsp;&nbsp;P(+|D)*P(D) &nbsp;&nbsp;&nbsp;/ &nbsp;&nbsp;&nbsp;( P(+|D)*P(D) &nbsp;&nbsp;&nbsp;+ &nbsp;&nbsp;&nbsp;P(+|~D)*P(~D) )**<br/>
<b>PPV = TS x PD / ( TS x PD + P(+|~D)*P(~D) )</b>

### Recall,
> _**P(+|~D) &nbsp;&nbsp;&nbsp;= &nbsp;&nbsp;&nbsp;1 - P(+|D) &nbsp;&nbsp;&nbsp;= &nbsp;&nbsp;&nbsp;1 - Test specificity &nbsp;&nbsp;&nbsp;= &nbsp;&nbsp;&nbsp;1 - TS**_<br/>
> _**P(~D) &nbsp;&nbsp;&nbsp;= &nbsp;&nbsp;&nbsp;1 - P(D) &nbsp;&nbsp;&nbsp;= &nbsp;&nbsp;&nbsp;1 - Prevalence of disease &nbsp;&nbsp;&nbsp;= &nbsp;&nbsp;&nbsp;1 - PD**_

### Substituting
**P(D|+) = P(+|D)*P(D) / ( P(+|D)*P(D) + [1-P(-|~D)]*[1-P(D)] )**<br/>
<b>PPV =   TSxPD / {TSxPD  +  1-TSx1-PD}</b>


- ### DLR_+ = P(+|D) / P(+|~D)
> _**The diagnostic likelihood (of the disease) ratio of a positive test**_

- ### DLR_- = P(-|D) / P(-|~D)
> _**The diagnostic likelihood (of the disease) ratio of a negative test**_

Recall that P(+|D) and P(-|~D), (test sensitivity and specificity respectively)
are accuracy rates of a diagnostic test for the two possible results. They should
be close to 1 because no one would take an inaccurate test

P(D|+) / P(~D|+) =
        P(+|D) * P(D) / (P(+|~D) * P(~D)) =
        P(+|D)/P(+|~D) * P(D)/P(~D) =
        DLR_+ * P(D)/P(~D)

The left side of the equation represents the post-test odds of disease
given a positive test result : P(D|+)/P(~D|+)

Pre-test odds of disease : P(D)/P(~D)

In other words, a DLR_+ value equal to N indicates that the hypothesis
of disease is N times more supported by the data than the hypothesis of no disease.

The equation P(D|-) / P(~D|-) = P(-|D) / P(-|~D) * P(D)/P(~D) says what about
the post-test odds of disease relative to the pre-test odds of disease given
negative test results?

post-test odds are less than pre-test odds





P(D|+) = P(+|D)*P(D) / ( P(+|D)*P(D) + P(+|~D)*P(~D) )




Random variables are said to be iid if they are independent and identically distributed
By independent we mean "statistically unrelated from one another".
Identically distributed means that "all have been drawn from the same population distribution".

