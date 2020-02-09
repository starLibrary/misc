Accuracy rates of the test
> -    Test sensitivity : P(+|D) :
            positive result when the patient has disease
> -    Test specificity : P(-|~D) :
            negative test result when the patient doesnt have disease
Prevalence of disease : P(D)
    
    
Positive predictive value : P(D|+) :
        the probability that a patient has the disease given a positive test result :
        
            
Negative predictive value : P(~D|-) :
        the probability that a patient does not have the disease given a negative test result :
            
    

By Bayes Formula,    P(D|+) = P(+|D)*P(D) / ( P(+|D)*P(D) + P(+|~D)*P(~D) )

Recall,   P(+|~D) = 1 - P(+|D) = 1 - Test specificity
            P(~D) = 1 - P(D)   = 1 - Prevalence of disease

Substituting         P(D|+) = P(+|D)*P(D) / ( P(+|D)*P(D) + [1-P(-|~D)]*[1-P(D)] )

Positive predictive value =   Test sensitivity * Prevalence of disease /
                            1-Test specificity * 1-Prevalence of disease


DLR_+ : The diagnostic likelihood ratio of a positive test
DLR_+ = P(+|D) / P(+|~D)

DLR_- = P(-|D) / P(-|~D)

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

