## Introduction
- Operands: Table
- Operators:
	- choose only the rows (tuples) you want  
	- choose only the columns (attributes) you want
	- combine tables  
	- and a few other things..

## Select: choose rows
>[! notation] Notation: Select
>${\Large\sigma}_C(R)$, where
>- $R$ is the relation/table,
>- Condition $c$ is a boolean expression.

- It can use comparison operators and boolean operators
- The operands are either constants or attributes of R.
- The result is a relation:
	- with the same schema as the operand  
	- but with only the tuples that satisfy the condition.
>[! example]
>To select all British actors, we can use ${\Large\sigma}_{\text{nationality}=\text{``British''}}(\texttt{Artists})$. To select all movies from the 1970s, we can use ${\Large\sigma}_{\text{year}>1969\text{ and }\text{year}<1980}(\texttt{Movies})$. Instead of using the word "$\text{and}$," we can also use the and operator in logic: $\land$

## Project: choose columns
>[!notation] Notation: Project
>${\Large\pi}_L(R)$, where
>- $R$ is a relation/table,
>- $L$ is a subset of the attributes of $R$.

- The result is a relation
	- with all the tuples from $R$  
	- but with only the attributes in $L$, and in that order.
- The project operator removes duplicates: 
	- This is due to that we are using the set notation, and so we cannot have duplicate tuples in a table. 
>[!example]
>${\Large\pi}_{\text{director}}(\texttt{Movies})$ will result $\{\text{Kubrick}, \text{Altman}, \text{Polanski}, \text{Lucas}\}$ although the original table has $7$ director names. 

>[!example] Example: Combine select and project
>***Q: Write an RA expression to find the names of all directors of movies from the 1970s.***
>A: ${\Large\pi}_{\text{director}}{\Large\sigma}_{\text{year}>1969\land\text{year}<1980}(\texttt{Movies})$
>- Note that, as we want to find the names of directors, so we will to the project operator as the last step.
>- Hence, we will do select first, and then project. 

## Combining Tables
### Cartesian Product: Combine Tables
>[!notation] Notation: Cartesian Production
>$R_1\times R_2$, where
>- $R_1$ and $R_2$ are tables.

- The result is a relation with
	- every combination of a tuple from $R1$ concatenated to a tuple from $R_2$
	- Its schema is every attribute from $R_1$ followed by every attribute of $R_2$, in order.
- The number of tuples in $R_1\times R_2$ is given by $|R_1|\times|R_2|$, where $|R|$ is the cardinality of $R$.
- If an attribute occurs in both relations, it occurs twice in the result, prefixed by relation name.
>[!example]
>In the result of $\texttt{Movie}\times\texttt{Roles}$, we will see $\text{Movies.mID}$ and $\text{Roles.mID}$.

- However, Cartesian product can be inconvenient
	- It can introduce **nonsense** tuples.
	- However, we can get rid of them with select operators. 
- Due to this inconvenience, we need ***join*** operators.

### Natural Join
>[!notation] Notation: Natural Join
>$R\bowtie S$, where
>- $R$ and $S$ are tables
>- $\bowtie$ is the natural join operator.

- The result is formed by 
	- taking the Cartesian product,
	- select to ensure equality on attributes that are in both relations (determine by name),
	- projecting to remove duplicate attributes. 
>[!important] Properties of Natural Join
>- Commutativity (although attribute orders may vary): $$R\bowtie S=S\bowtie R$$
>- Associativity: $$R\bowtie(S\bowtie T)=(R\bowtie S)\bowtie T$$
>- When writing $n$-ary joins, brackets are irrelevant. We can just write: $$R_1\bowtie R_2\bowtie\cdots\bowtie R_n.$$

- Special cases of Natural Join
	- If there is not tuple match, natural join will result in the empty set $\emptyset$.
	- Natural join is looking for **exact match**.
	- If there is no attributes in common, the result will be the **Cartesian product** of the two relations.
- Natural join can **over-match**
	- Natural join bases the matching on attribute names.
	- What if two attributes have the same name, but we don’t want them to have to match?
- Natural join can **under-match**
	- - What if two attributes don’t have the same name and we do want them to match?

### Theta Join
>[!definition] Definition: Theta Join ${\Large\bowtie}_{\text{condition}}$
>- Rationale: it is easier to check conditions after making the Cartesian production: ${\Large\sigma}_{\text{condition}}(R\times S)$
>- Syntax: $R\ {\Large\bowtie}_{\text{condition}}\ S$
>- The result is the same as Cartesian product (not natural join!) followed by select: $$R\ {\Large\bowtie}_{\text{condition}}\ S={\Large\sigma}_{\text{condition}}(R\times S)$$

### Outer Join
- Outer Join keeps all the tuples and adds `null` values whenever no value exists
- Three variants:
	- **Left Outer Join**: only do this "null padding" for left operand: ⟕
	- **Right Outer Join**: only do this "null padding" for right operand: ⟖
	- **Full Outer Join**: pad other operands: ⟗

### Cardinality of Some Operators
- Given $R_1(X_1)$ and $R_2(X_2)$, the cardinality (number of tuples) of 
	- Cartesian product: $|R_1\times R_2|=|R_1|\times|R_2|$
	- Natural join: $0\leq|R_1\Join R_2|\leq|R_1|\times|R_2|$
		- Notice that if the attributes used in a natural join (i.e., the attributes in $X_1\cap X_2$)  contain a key of $R_2$, then $$0\leq|R_1\Join R_2|\leq|R_1|$$
	- Same applies for left/right outer joins, if and only if the "outer" part is pointint to the table with the non-key attribute. 

## Composing Larger Expressions
### Assignment and Rename Operator
>[!notation] Notation: Assignment
>$R\coloneqq\text{Expression}$ or $R(A_1,\dots,A_n)\coloneqq\text{Expression}$
>- The later notation lets us name all the attributes of the new relation
>- Sometimes, we don't want the name they would get from Expression
>- $R$ must be a temporary variable, **not one of the relations in the schema**. That is, we are not updating the content of a relation.

>[!example] Using Assignment
>$$\begin{aligned}\text{Temp }1&\coloneqq Q\times R\\\text{Temp }2&\coloneqq{\Large\sigma}_{a=99}(\text{Temp }1)\times S\\\text{Answer}(\text{part},\text{price})&\coloneqq{\Large\pi}_{b,c}(\text{Temp }2)\end{aligned}$$
>- This is all for **readability**.
>- Assignment helps us break a problem down.
>- It also allows us to change the names of relations (and attributes).

>[!notation] Notation: Rename
>${\Large\rho}_{R_1}R_2$ or ${\Large\rho}_{R_1(A_1,\dots,A_n)}(R_2)$
>- The later notation lets us rename all the attributes as well as the relation.

- Equivalent Notations: $$R_1(A_1,\dots,A_n)\coloneqq R_2\equiv R_1\coloneqq{\Large\rho}_{R_1(A_1,\dots,A_n)}(R_2)$$
- $\rho$ is useful if we want to rename within an expression.

### Set Operations
- Because relations are sets, we can use set **intersection** ($\cap$), **union** ($\cup$), and **difference** ($-$). But only if the operands are relations over the *same* attributes (in number, name, and order). ***Schema needs to be identical***.

## Precedence of Operators
- Expressions can be composed recursively
- Parentheses $()$ and precedence rules define the order of evaluation
- Precedence, from highest to lowest, is: 
	- $\Large{\sigma,\ \pi, \rho}$
	- $\Large{\times,\ \Join}$
	- $\Large{\cap}$
	- $\Large{\cup,\ -}$

## Specific Types of Query
### Max
- The function `Max` is not directly supported in relational algebra. 
- Idea:
	- Pair tuples and find those that are *not* the max
	- Then, subtract from all to find the maxes. 
- Procedure: $S$ to be the table and we want to find $\max(\text{GPA})$
	- Create the pair of tuples: ${\Large\rho}_{S_1}(S)\times{\Large\rho}_{S_2}(S)$
	- Filter those GPA that is smaller than the other one in each pair: we will left will everything "smaller;" that is, $\max{\text{GPA}}$ will not be present. $$S_3(\text{GPA})\coloneqq{\Large\pi}_{S_1.\text{GPA}}{\Large\sigma}_{(S_1.\text{GPA}<S_2.\text{GPA})}({\Large\rho}_{S_1}(S)\times{\Large\rho}_{S_2}(S))$$
	- Then, doing the subtraction, we will get the maximum: $$\max(\text{GPA})={\Large\pi}_\text{GPA}(S)-{\Large\pi}_\text{GPA}(S_3)$$
### $k$ or more
- To write a query about something like "students with two or more classes with grade > 80" we:
	- Make all combos of $k$ different tuples that satisfy the condition. 
>[!example]
> $$S\coloneqq{\Large\rho_{R_1}}(R)\times{\Large\rho_{R_2}}(R)$$ $$R_3={\Large\sigma}_{(R_1.\text{sID}=R_2.\text{sID}\land R_1.\text{Class}\neq R_2.\text{Class})}(S)$$ $${\Large\pi}_{\text{sID}}{\Large\sigma}_{(R_1.\text{Grade}>80\land R_2.\text{Grade}>80)}(R_3)$$

### Exactly $k$
- “$k$ or more” – “$(k+1)$ or more”

## Integrity Constraints via RA
- In a schema description, we can express integrity constraints using a relational algebra expression
- We use the notation
	- $R=\emptyset$, or
	- $R\subseteq S$
>[!example]
>- Courses at the 400-level cannot count for breadth. $${\Large\sigma}_(400\leq\text{cNum}\leq500)\land\text{breadth}(\text{Course})=\emptyset$$
>- In terms when CS 490 is offered, CS 454 must be offered. $$\text{CS490Terms}(\text{term})\coloneqq{\Large\pi}_\text{term}({\Large\sigma}_{(\text{dept}=\text{'CS'}\land\text{cNum}=490)}(\text{Offering}))$$$$\text{CS454Terms}(\text{term})\coloneqq{\Large\pi}_\text{term}({\Large\sigma}_{(\text{dept}=\text{'CS'}\land\text{cNum}=454)}(\text{Offering}))$$ $$\text{CS490Terms}-\text{CS454Terms}=\emptyset$$

