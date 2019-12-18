**Topics**:

- Association rules
- Rule interestingness metrics
- Mining rules from non-binary data

 

## Transaction Data 

An item (I) is: 

- For market basket data: I is an item in the store, e.g. milk. 
- For relational data: I is an attribute-value pair (numeric attributes should be discretized), e.g. salary=high, gender=male. 

Formally, let I = {i1, i2, . . . , in } be a set of n binary attributes called items. Let D = {t1, t2, . . . , tm } be a set of transactions called the database. Each transaction in D has an unique transaction ID and contains a subset of the items in I. 

Note: Non-transaction data can be made into transaction data using binarization. 



## Association Rules

A rule takes the form X → Y  (LHS  → RHS)

- X, Y⊆I 

- X∩Y=∅
- X and Y are called itemsets. 
- X is the rule’s antecedent (left-hand side) 
- Y is the rule’s consequent (right-hand side) 

o The left hand side (LHS) contains a set of one or more ‘things’. 

o The right hand side contains a set of one or more ‘things’. 

SomeExamples
 o {Cereal}  → {Milk}
 o {Diapers, FridayEvening}  → {Beer}
 o {Holiday, Turkey}  →  {Cranberry Sauce} 



### How Do We Evaluate Rules?



<img src='https://annalyzin.files.wordpress.com/2016/04/association-rule-support-table.png?w=503&h=447' style="zoom:50%;" >

1. **Support:** “how often do these things appear together?” 

   supp(Z ) = nZ /n

→ share of transactions in the database that contains Z . 

Rules from Same Item set Will Have Same Support: Support(X→Y) = Support(Y→X) 

<img src="https://annalyzin.files.wordpress.com/2016/03/association-rule-support-eqn.png?w=248&h=68" style="zoom:67%;" >



2. **Confidence:** “given LHS, how often do we see RHS?”  

Confidence of a rule X → Y is defined as conf(X →Y) = supp(X ∪Y)/supp(X) 

→ share of transactions containing Y in all the transactions containing X . 

Confidence Gives Direction: Conf(X→Y) != Conf(Y→X) 

<img src="https://annalyzin.files.wordpress.com/2016/03/association-rule-confidence-eqn.png?w=527&amp;h=77" style="zoom:67%;" />



#### Probabilistic interpretation of Support and Confidence

supp(Z ) = $n_Z /n$ corresponds to an estimate for $P(\hat EZ) = n_Z /n$, the probability for the event that itemset Z is contained in a transaction. 

Confidence can be interpreted as an estimate for the conditional probability P(EY|EX ) = P(EX ∩EY) / P(EX ) This directly follows the definition of confidence: 

- A rule R is A ⇒ B where A and B are disjoint patterns. 

- Support(A ⇒ B) = P(A ∪ B) 

- Confidence(A ⇒ B)=P(B|A)=posterior probability 

  
#### Weaknesses of Support and Confidence

Support suffers from the ‘rare item problem’: Infrequent items not meeting minimum support are ignored which is problematic if rare items are important.
E.g. rarely sold products which account for a large part of revenue or profit. Typical support distribution (retail point-of-sale data with 169 items): 

One drawback of the confidence measure is that it might misrepresent the importance of an association. This is because it only accounts for how popular apples are, but not beers. If beers are also very popular in general, there will be a higher chance that a transaction containing apples will also contain beers, thus inflating the confidence measure. To account for the base popularity of both constituent items, we use a third measure called lift.



3. **Lift** “how often does LHS appear with RHS, compared to what chance would predict?” 

This says how likely item Y is purchased when item X is purchased, while controlling for how popular item Y is. In Table 1, **the lift of {apple -> beer} is 1,which implies no association between items. A lift value greater than 1 means that item Y is *likely* to be bought if item X is bought, while a value less than 1 means that item Y is *unlikely* to be bought if item X is bought.**

<img src="https://annalyzin.files.wordpress.com/2016/03/association-rule-lift-eqn.png?w=566&amp;h=80" alt="Association Rule Lift eqn" style="zoom: 67%;" />

**When Lift > 1, the occurrence of  X ⇒ Y together is more likely than what you would expect by chance** 



| Function            | Definition                                   |
| ------------------- | -------------------------------------------- |
| Support             | s(A ⇒ B) = P(A,B) and s(A) = P(A)            |
| Confidence          | c(A ⇒ B) = P(B)                              |
| Expected Confidence | EC(A ⇒ B) = P(B)                             |
| Lift                | L(A ⇒ B) = c(A ⇒ B)/P(B) = P(A,B)/(P(A)P(B)) |

Lift represents a measure of the distance between P(B|A) and P(B), or equivalently, the extent to which A and B are not independent. In other words, lift gives a measure of the extent to which the equation 

P(A,B) = P(A)P(B)  is not true. 

The expected confidence is the value that confidence would take if A and B were in fact independent.






## Apriori Algorithm

The *apriori principle* can reduce the number of itemsets we need to examine. Put simply, the apriori principle states that

> if an itemset is infrequent, then all its **supersets** must also be infrequent

This means that if {beer} was found to be infrequent, we can expect {beer, pizza} to be equally or even more infrequent. So in consolidating the list of popular itemsets, we need not consider {beer, pizza}, nor any other itemset configuration that contains beer.

This principle holds true because of the following property of support measure: 

∀𝑿,𝒀: 𝑿⊆𝒀 ⇒𝒔(𝑿) ≥𝒔(𝒀)

– The support of an itemset never exceeds that of its subsets 

– This is known as the anti-monotone property of support 



Two-step approach: 

1. Frequent Itemset Generation
    – Generate all item sets whose support ≥ 𝑚𝑖𝑛𝑠𝑢𝑝  (Expensive)

2. Rule Generation
    – Generate high confidence (strong) rules from each frequent itemset 



#### Finding itemsets with high support

Using the apriori principle, the number of itemsets that have to be examined can be pruned, and the list of popular itemsets can be obtained in these steps:

**Step 0**. Start with itemsets containing just a single item, such as {apple} and {pear}.

**Step 1**. Determine the support for itemsets. Keep the itemsets that meet your minimum support threshold, and remove itemsets that do not.

**Step 2**. Using the itemsets you have kept from Step 1, generate all the possible itemset configurations.

**Step 3**. Repeat Steps 1 & 2 until there are no more new itemsets.





#### Finding item rules with high confidence or lift

We have seen how the apriori algorithm can be used to identify itemsets with high support. The same principle can also be used to identify item associations with high confidence or lift. Finding rules with high confidence or lift is less computationally taxing once high-support itemsets have been identified, because confidence and lift values are calculated using support values.

Take for example the task of finding high-confidence rules. If the rule `{beer, chips -> apple}` has low confidence, all other rules with the same constituent items and with apple on the right hand side would have low confidence too. Specifically, the rules`{beer -> apple, chips}` `{chips -> apple, beer}` would have low confidence as well. 

As before, lower level candidate item rules can be pruned using the apriori algorithm, so that fewer candidate rules need to be examined.

 

#### Limitations

- **Computationally Expensive**. Even though the apriori algorithm reduces the number of candidate itemsets to consider, this number could still be huge when store inventories are large or when the support threshold is low. However, an alternative solution would be to reduce the number of comparisons by using advanced data structures, such as hash tables, to sort candidate itemsets more efficiently.

- **Spurious Associations**. Analysis of large inventories would involve more itemset configurations, and the support threshold might have to be lowered to detect certain associations. However, lowering the support threshold might also increase the number of spurious associations detected. To ensure that identified associations are generalizable, they could first be distilled from a training dataset, before having their support and confidence assessed in a separate test dataset.









