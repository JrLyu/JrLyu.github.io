## DB Systems to Rescue
- DB are designed to solve the problem **Physical Data Dependence**: so, they are physical data independence
- "The three-schema architecture" separates user programs from the physical DB

## Data Model
> [!definition]
> Every DBMS is based on some **data model**, a notation for describing data, including
> - The structure of the data
> - Constraints on the content of the data
> - Operations on the data
- Examples: 
	- Network & hierarchical data model -- of historical interest
	- Relational data model
	- Semistructured data model

## Relations: Schema, Instance, and Keys
> [!definition]
> A **domain** is a set of values. 
- Suppose $D_1,D_2,\dots,D_n$ are domains.
	- The **Cartesian (cross) product** $D_1\times D_2\times\cdots\times D_n$ is the set of all tuples $\langle d_1,d_2,\dots,d_n\rangle$ such that $d_1\in D_1, d_2\in D_2,\dots,d_n\in D_n$.
	- Mathematical relation on $D_1,\dots,D_n$ is a subset of the Cartesian product. 
- Our database **tables** are relations, too.

>[!definition]
>**Schema**: definition of the structure of the relation.
>- Notation: $$\text{Teams}(\text{Name},\text{HomeField},\text{Coach})$$

>[!definition]
> **Instance**: particular data in the relation.
- Instance change constantly; schemas rarely.
- <font color='green'><i>Example: Adding a column (attribute) changes the schema as well as the instance</i></font>
- When designing the DB, we should avoid changing schema very frequently because changing schema is expensive. 

>[!definition]
>In a DB table:
>- **relation** = table itself
>- **attribute** = column (optionally, we can specify that attributes have domains)
>- **tuple** = row
>- **arity** = number of attributes (columns)
>- **cardinality** = umber of tuples (rows)

- A relation is a **set** of tuples, which means:
	- there can be *no duplicate* tuples
	- *order* of the tuples *does not* matter

>[!definition]
>**Database schema**: a set of relation schemas --> *no actual data*
>**Database instance**: a set of relation instances

>[!definition]
>**Superkeys**: a set of one or more attributes whose *combined* values are unique. That is, no two tuples can have the same values on all of these attributes. 
- By default, tuples cannot be duplicated. So, the entire tuple is viewed as a superkey. But, we can declare other superkeys. So, every relation have a superkey.
- There can be multiple superkeys for a relation. 

>[!definition]
>**Key**: a minimal superkey.
>- That is, you *may not* remove an attribute from a key, and still have a set of attributes whose combined values are *unique.*

>[!example] Example
>A relation called `Course`, which attributes: department codes, course number, and course name. One tuple might be $$\text{Course}\langle\text{``cs''},\text{``377''},\text{``Introduction to Databases''}\rangle$$
>The superkey for this relation would be the combinations of `cs` and `377`. The entire tuple can also be viewed as a superkey. However, $\langle\text{``cs''},\text{``377''}\rangle$ is the key.

>[!example] Example
>Consider the following relation: $$\text{Studnet}\langle\text{student\#},\text{netID},\text{surname},\text{firstname},\text{gpa}\rangle.$$
>This relation has multiple key: `student#` or `netID` can either serve as a key. 
- We underline attributes in the schema to indicate that they form a key.
	- For example, $$\text{Team}\langle\underline{\text{Name},\text{HomeField}},\text{Coach}\rangle$$
- Aside: Called “superkey” because it is a superset of some key. (Not necessarily a proper superset.)
- If a set of attributes is a key for a relation
	- It does not mean merely that "there are no duplicates" in particular instance of the relation
	- It means that in principle there ***cannot*** be any
- Often we have to invent an artificial new attribute to ensure all tuples will be unique.
- A key is a kind of **integrity constraint**.

## Reference and Keys
- Relations often ***refer*** to each other.
>[!definition]
>If the referring attribute refers to a *key* attribute in another table, it is called a **foreign key**.
>- For example, in table `Roles`, the attribute `aID` refers to the key `aID` in the `Artists` table, we write $\text{Roles}[\text{aID}]\subseteq\text{Artists}[\text{aID}]$.
>- More generally, we use the following notation: $R_1[X]\subseteq R_2[Y]$.
- This gives us a way to refer to a single tuple in *that* relation.
- A foreign key may need to have several attributes.
- The $R[A]$ notation:
	- $R$ is the relation, and $A$ is a list of attributes in $R$.
	- $R[A]$ is the set of all tuples from $R$, but with only the attributes in list $A$.

>[!definition]
>These $R_1[X]\subseteq R_2[Y]$ relationships are called **referential integrity constraints** or inclusion dependencies. 
>- Not all referential integrity constraints are foreign key constraints. 
>- $R_1[X]\subseteq R_2[Y]$ is a **foreign key constraint** $\iff$ $Y$ is a key for relation $R_2$.

- Integrity constraints is part of the process: designing a schema
	- Mapping from the real world to a relational schema is surprisingly challenging and interesting.
	- Two important goals:
		- Represent the data well.
		- Avoid redundancy.