###############
Probability
###############

Bayes rule:

- P(A|B) = (P(B|A)·P(A)) / P(B)

- P(A|B): Posterior
- P(B|A): Likelihood
- P(A): Prior
- P(B): Marginal Likelihood

- P(B): :math:`{\sum_a{P(B|A=a)·P(A=a)}}

0.50 = P(g|o1)·P(o1) /0.9
0.45 = P(g|o1)·P(o1)

0.05 = P(~g|o1)·P(o1) / 0.1
0.005 = P(~g|o1)·P(o1)

0.455 = P(o1)(P(~g|o1) + P(g|o1))
0.455 = P(o1)

0.15 = P(g|o2)·P(o2) /0.9
0.6 = P(g|o2)·P(o2)

0.25 = (P(~g|o2)·P(o2)) / 0.1
0.025 = P(~g|o2)·P(o2)

0.625 = p(o2)(P(~g|o2) + P(g|o2))
0.625 = p(o2)


P(o2|o1) = (P(o1|o2)·P(o2)) / P(o1)

P(o1) = P(o1|o2)·P(o2)

0.455 = P(o1|o2)·0.625
0.728 = P(o1|o2)
