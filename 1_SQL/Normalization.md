> **Database normalization** is the process of structuring a [relational database](https://en.wikipedia.org/wiki/Relational_database)[*[clarification needed](https://en.wikipedia.org/wiki/Wikipedia:Please_clarify)*] in accordance with a series of so-called [normal forms](https://en.wikipedia.org/wiki/Database_normalization#Normal_forms) in order to reduce [data redundancy](https://en.wikipedia.org/wiki/Data_redundancy) and improve [data integrity](https://en.wikipedia.org/wiki/Data_integrity). It was first proposed by [Edgar F. Codd](https://en.wikipedia.org/wiki/Edgar_F._Codd) as part of his [relational model](https://en.wikipedia.org/wiki/Relational_model).

The objective is to isolate data so that additions, deletions  and modifications for a field can be made in just one table and propagated through the rest of the database using defined relationships. 



Functional Dependencies (FDs)

A *functional dependency* (FD) is a relationship between two attributes, typically between the PK and other non-key attributes within a table. 

For any relation R, attribute Y is functionally dependent on attribute X (usually the PK), if for every valid instance of X, that value of X uniquely determines the value of Y. This relationship is indicated by the representation below :

**X —–> Y**

The left side of the above FD diagram is called the *determinant*, and the right side is the *dependent*. Here are a few examples.

In the first example, below, SIN determines Name, Address and Birthdate. Given SIN, we can determine any of the other attributes within the table.

**SIN  ———-> Name, Address, Birthdate**

For the second example, SIN and Course determine the date completed (DateCompleted). This must also work for a composite PK.

**SIN, Course ———>   DateCompleted**

The third example indicates that ISBN determines Title.

**ISBN ———–> Title**







## Normalization

1. Identify functional dependencies

Given LHS(left-hand side), could we definitely determine RHS?

Trivial FD/Nontrivial FD

1NF: A relation in which there is no nesting or repeating groups in tables
1NF -> 2NF: 1NF + remove partial dependencies
2NF -> 3NF: 2NF + remove transitive dependencies

2NF can suffer update anomalies

Boyce-Codd Normal Form

Decomposition:
Lossless decomposition does not preserve FDs

Theorem 1
It is __NOT__ always possible to decompose R into relations R1,…, Rn so that:

- The decomposition is lossless
- Each R1,…, Rn is in **BCNF**
- FDs are preserved


Theorem 2
It is __always__ possible to decompose any R into relations R1,…, Rn so that:

- The decomposition is lossless
- Each R1,…, Rn is in **3NF**
- FDs are preserved



