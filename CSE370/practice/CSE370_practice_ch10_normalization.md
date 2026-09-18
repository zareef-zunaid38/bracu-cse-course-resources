# CSE370 Chapter 10 Normalization Practice Sheet — sidecar (read this, not the .docx)

CSE370 normalization practice problems and official solutions covering decomposition of relational schemas into 1NF, 2NF, and 3NF by identifying partial and transitive functional dependencies.
Docx last modified: 27-06-2026.

## Content

### Instructions & Assumptions
- If the dependency of any attribute is not explicitly mentioned, assume it is dependent on the whole primary key.
- Unless indicated as composite or multivalued, consider all attributes to be atomic/simple.

---

### Question 1: Car Sales
**Relational Schema:**
$$\text{Car\_Sales}(\underline{\text{CarID}}, \underline{\text{SalesmanID}}, \text{CarModel}, \text{CarYear}, \text{CarPrice}, \text{CarCompany}, \text{SMName}, \text{Discount}, \text{CommissionPercentage}, \text{DateSold})$$
*Primary Key:* `(CarID, SalesmanID)`

**Functional Dependencies:**
- FD1: $\text{CarID} \to \text{CarModel}, \text{CarYear}, \text{CarPrice}, \text{CarCompany}$
- FD2: $\text{SalesmanID} \to \text{SMName}, \text{CommissionPercentage}$
- FD3: $\text{DateSold} \to \text{Discount}$

**Tasks:**
1. Explain if schema is in 1NF; if not, normalize to 1NF.
2. Explain if schema is in 2NF; if not, normalize to 2NF.
3. Explain if schema is in 3NF; if not, normalize to 3NF.
*(Must show complete current schema after each step).*

---

### Question 2: Books (ISBN Key)
**Relational Schema:**
$$\text{Books}(\underline{\text{ISBN}}, \text{AuthorName}, \text{BookTitle}, \text{Publisher}, \text{Price}, \text{Year}, \text{AuthorAffiliation}, \text{Type}, \text{Category})$$
*Primary Key:* `ISBN`

**Functional Dependencies:**
- $\text{AuthorName} \to \text{AuthorAffiliation}$
- $\text{Type} \to \text{Category}$

**Tasks:**
1. Identify and explain current normal form.
2. Step-by-step normalize to 3NF showing complete schema after each step.

---

### Question 3: Books (Composite Key)
**Relational Schema:**
$$\text{Books}(\underline{\text{AuthorName}}, \underline{\text{BookTitle}}, \text{Publisher}, \text{Price}, \text{Year}, \text{AuthorAffiliation}, \text{Type}, \text{Category})$$
*Primary Key:* `(AuthorName, BookTitle)`

**Functional Dependencies:**
- $\text{AuthorName} \to \text{AuthorAffiliation}$
- $\text{BookTitle} \to \text{Publisher}, \text{Price}, \text{Year}, \text{Type}, \text{Category}$
- $\text{Type} \to \text{Category}$

**Tasks:**
1. Identify and explain current normal form.
2. Step-by-step normalize to 3NF showing complete schema after each step.

---

### Question 4: Student Project
**Relational Schema:**
$$\text{Student\_Project}(\underline{\text{StudentID}}, \underline{\text{ProjectID}}, \text{SName}, \text{PName}, \text{CourseCode}, \text{CTitle}, \text{Semester}, \text{Score}, \text{CGPA})$$
*Primary Key:* `(StudentID, ProjectID)`

**Functional Dependencies:**
- FD1: $\text{StudentID} \to \text{SName}, \text{CGPA}$
- FD2: $\text{ProjectID} \to \text{PName}, \text{CourseCode}, \text{CTitle}, \text{Semester}$
- FD3: $\text{CourseCode} \to \text{CTitle}$

**Tasks:**
1. Explain if schema is in 1NF; if not, normalize to 1NF.
2. Explain if schema is in 2NF; if not, normalize to 2NF.
3. Explain if schema is in 3NF; if not, normalize to 3NF.

---

### Official Solutions

#### Answer 1
- **1NF Analysis:** In 1NF because all attributes are atomic (no multivalued/composite attributes or nested relations).
- **2NF Analysis & Normalization:** Not in 2NF due to partial dependencies FD1 ($\text{CarID} \to \dots$) and FD2 ($\text{SalesmanID} \to \dots$) depending on proper subsets of the composite primary key.
  - $\text{Cars}(\underline{\text{CarID}}, \text{CarModel}, \text{CarYear}, \text{CarPrice}, \text{CarCompany})$
  - $\text{Salesmen}(\underline{\text{SalesmanID}}, \text{SMName}, \text{CommissionPercentage})$
  - $\text{Car\_Sales}(\underline{\text{CarID}}, \underline{\text{SalesmanID}}, \text{Discount}, \text{DateSold})$
- **3NF Analysis & Normalization:** Not in 3NF because $\text{Car\_Sales}$ has a transitive dependency via FD3 ($\text{DateSold} \to \text{Discount}$).
  - $\text{Cars}(\underline{\text{CarID}}, \text{CarModel}, \text{CarYear}, \text{CarPrice}, \text{CarCompany})$
  - $\text{Salesmen}(\underline{\text{SalesmanID}}, \text{SMName}, \text{CommissionPercentage})$
  - $\text{Car\_Sales}(\underline{\text{CarID}}, \underline{\text{SalesmanID}}, \text{DateSold})$
  - $\text{Discount\_Info}(\underline{\text{DateSold}}, \text{Discount})$

#### Answer 2
- **Current Normal Form:** In 2NF. 1NF holds (atomic values), and no partial dependencies exist since the primary key `ISBN` is single-attribute (non-composite).
- **3NF Normalization:** Not in 3NF due to transitive dependencies ($\text{AuthorName} \to \text{AuthorAffiliation}$, $\text{Type} \to \text{Category}$).
  - $\text{Books}(\underline{\text{ISBN}}, \text{AuthorName}, \text{BookTitle}, \text{Publisher}, \text{Price}, \text{Year}, \text{Type})$
  - $\text{Author}(\underline{\text{AuthorName}}, \text{AuthorAffiliation})$
  - $\text{BookType}(\underline{\text{Type}}, \text{Category})$

#### Answer 3
- **Current Normal Form:** In 1NF. Not in 2NF due to partial dependencies on composite key `(AuthorName, BookTitle)` from $\text{AuthorName} \to \text{AuthorAffiliation}$ and $\text{BookTitle} \to \text{Publisher}, \text{Price}, \text{Year}, \text{Type}, \text{Category}$.
- **2NF Normalization:**
  - $\text{Author}(\underline{\text{AuthorName}}, \text{AuthorAffiliation})$
  - $\text{Book1}(\underline{\text{BookTitle}}, \text{Publisher}, \text{Price}, \text{Year}, \text{Type}, \text{Category})$
  - $\text{Book2}(\underline{\text{AuthorName}}, \underline{\text{BookTitle}})$
- **3NF Normalization:** Not in 3NF due to transitive dependency $\text{Type} \to \text{Category}$ in `Book1`.
  - $\text{Author}(\underline{\text{AuthorName}}, \text{AuthorAffiliation})$
  - $\text{Book1}(\underline{\text{BookTitle}}, \text{Publisher}, \text{Price}, \text{Year}, \text{Type})$
  - $\text{BookType}(\underline{\text{Type}}, \text{Category})$
  - $\text{Book2}(\underline{\text{AuthorName}}, \underline{\text{BookTitle}})$

#### Answer 4
- **1NF Analysis:** In 1NF (atomic attributes).
- **2NF Analysis & Normalization:** Not in 2NF due to partial dependencies FD1 ($\text{StudentID} \to \text{SName}, \text{CGPA}$) and FD2 ($\text{ProjectID} \to \text{PName}, \text{CourseCode}, \text{CTitle}, \text{Semester}$).
  - $\text{Students}(\underline{\text{StudentID}}, \text{SName}, \text{CGPA})$
  - $\text{Projects}(\underline{\text{ProjectID}}, \text{PName}, \text{CourseCode}, \text{CTitle}, \text{Semester})$
  - $\text{Student\_Project}(\underline{\text{StudentID}}, \underline{\text{ProjectID}}, \text{Score})$
- **3NF Analysis & Normalization:** Not in 3NF due to transitive dependency FD3 ($\text{CourseCode} \to \text{CTitle}$) inside `Projects`.
  - $\text{Students}(\underline{\text{StudentID}}, \text{SName}, \text{CGPA})$
  - $\text{Projects}(\underline{\text{ProjectID}}, \text{PName}, \text{CourseCode}, \text{Semester})$
  - $\text{Courses}(\underline{\text{CourseCode}}, \text{CTitle})$
  - $\text{Student\_Project}(\underline{\text{StudentID}}, \underline{\text{ProjectID}}, \text{Score})$
