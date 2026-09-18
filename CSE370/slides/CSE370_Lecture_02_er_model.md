# CSE370 Lecture 2: Data Modeling Using ER Model — sidecar (read this, not the .pdf)

BRAC University CSE370: Database Systems slide deck, 17 slides. Clean digital slides, fully
legible — transcribed with confidence.

## Content

**Slide 1 (title):** Lecture 2: Data Modeling Using ER Model. Generic Chen-notation ER
diagram shown (entities A/B/C, relationships D/E, attributes F/G/H/I/J/K/L, with G/I/K
underlined as key attributes).

**Slide 2 — Introduction:** ER models provide a visual framework for mapping data
relationships. Components: Entity, Attributes, Relationships. Note: this course uses **Chen
notation** (vs. alternatives like crow's foot or UML class diagrams).

**Slide 3 — The Mini World:** The mini-world is the part of the real world represented in a
database (e.g., a train reservation system, or a university). The mini-world itself is not
shown on the ER diagram.

**Slide 4 — Entities and Entity Types:** Entities are specific objects in the mini-world
(e.g. student Sakib Chowdhury, the CSE department, course CSE370). Entities sharing the same
properties/role are grouped into an **Entity Type** (e.g. 5000 students → Student entity
type). Shown as a **rectangle**.

**Slide 5 — Attributes (1):** Attributes describe an entity (e.g. Student: id, name, cgpa,
email). Three types: **Simple** (single atomic value, e.g. ID, cgpa — shown as an oval);
**Multivalued** (multiple values possible, e.g. car color, student email — shown as a
**double oval**); **Composite** (composed of sub-components, e.g. Address = Apt#+House#+
Street+City+State+ZipCode+Country, or Name = First+Middle+Last — sub-ovals connect to the
parent oval). An attribute can be composite-multivalued (e.g. a student's previous degrees,
each itself composite: Institute/Dname/Year).

**Slide 6 — Attributes (2):** **Key Attribute**: unique per entity (e.g. Student ID/email,
Course code) — shown **underlined**; an entity can have multiple key attributes; a key
attribute cannot be multivalued. **Derived Attribute**: calculated from other stored data,
not itself stored (e.g. "age" derived from birthdate, "total bill" from unit price ×
quantity) — shown as a **dotted oval**.

**Slide 7 — Weak Entity:** An entity type with no unique/key attribute is a **weak entity
type** (shown as a **double rectangle**) — e.g. Section (in a university mini-world), which
can't be uniquely identified alone. A weak entity may have a **partial key** — an attribute
that's part of (but not by itself) a unique identifier — shown with a **dotted underline**
(e.g. Section_no isn't unique alone, since many courses share section numbers, but
contributes to a full key).

**Slide 8 — Relationships:** A relationship associates two or more distinct entities with
specific meaning (e.g. Employee John Smith works on ProjectX; Student Ahnaf enrolls in
CSE370). Shown as a **diamond** connected to its entity types. Relationship types may have
their own attributes (e.g. "grade" belongs to the enrolls_in relationship, not Student or
Course alone, since it only exists once a specific enrollment happens). The same two entity
types can have multiple distinct relationship types between them (e.g. Student–Course via
both "enrolls_in" and "ST_of").

**Slide 9 — Degree of Relationships:** The degree = number of participating entity types.
Degree 2 = **Binary Relationship** (e.g. ST_of between Student and Course). Degree 3 =
**Ternary Relationship** (e.g. enrolls_in among Student, Course, and Semester). Degree n =
**n-ary relationship**.

**Slide 10 — Recursive Relationship:** When two entities of the *same* entity type
participate in a relationship (e.g. CSE221 is a Prerequisite_of CSE370 — both are Course
entities). The two participants take **distinct roles**, which must be labeled on the
relationship's edges in the diagram (e.g. "Dependent"/"Prereq" for courses; "old_version"/
"new_version" for a Project's extension_of relationship to itself).

**Slide 11 — Identifying Relationship:** A weak entity must participate in an **Identifying
Relationship** with its owner/identifying entity type (e.g. Section is identified via its
"has" relationship to Course) — shown as a **double diamond**. A weak entity's full
identifier = its partial key + the identifying entity's key. A weak entity can also have
ordinary non-identifying relationships with other entities (e.g. Section–Student via
"enrolled," a normal single-diamond relationship).

**Slide 12 — Relationship Constraints (1) — Cardinality Ratio:** Specifies *maximum*
participation, shown as numbers/letters (1, N, M) on relationship edges. **One-to-one
(1:1)**: e.g. 1 Faculty coordinates only 1 Course, and vice versa. **One-to-many/
many-to-one (1:N/N:1)**: e.g. 1 Course has many Sections, but each Section belongs to only 1
Course. **Many-to-many (M:N)**: e.g. many Students enroll in a Section, and a Section has
many Students. Full example diagram given: Course –prereq_of(N)–Course (self, N:M);
Course–ST_of(1:N)–Student; Course–Coordinates(1:1)–Faculty; Course–has(1:N)–Section;
Section–enrolled(N:M)–Student.

**Slide 13 — Relationship Constraints (2) — Participation Constraint (a.k.a. Existence
Dependency):** Specifies *minimum* participation. **Zero/optional**: shown as a **single
line** (e.g. it's optional for a Student to be an ST of a Course, and a Course need not have
an ST). **One-or-more/mandatory**: shown as a **double line** (e.g. every Course must have a
coordinating Faculty, though not every Faculty coordinates a course). Note: if not stated in
the requirements, assume based on real-world logic.

**Slide 14 — From Data Requirements to ER Diagram:** Three-step process: (1) Collect Data
Requirements → (2) Analyze Data and Identify ER components → (3) Create the ER Model.

**Slide 15 — Applied example — problem statement ("XYZ" company employee/project system):**
Company divided into Departments (unique name, unique number, a managing employee with a
tracked start date, possibly multiple locations). A department controls multiple Projects
(unique name, unique number, single location). Employees: SSN, name (composite:
first/middle/last), address, salary, sex, birthdate; each works for exactly one department
but may work on several projects; hours/week per project tracked; each employee has a direct
supervisor tracked. Employees may have dependents (name, sex, birthdate, relationship to
employee — no separate SSN/ID for dependents). Department's employee count is a **derived**
value (countable from relationship instances), not stored directly.

**Slide 16 — Applied example — solution ER diagram:** EMPLOYEE (Ssn key, Fname/Minit/Lname
composite Name, Bdate, Address, Sex, Salary) —N:1 WORKS_FOR (mandatory both sides)— DEPARTMENT
(Name key, Number key, multivalued Locations); DEPARTMENT —1:1 MANAGES (with Start_date
attribute)— EMPLOYEE; DEPARTMENT —1:N CONTROLS— PROJECT (Name key, Number key, Location);
EMPLOYEE —M:N WORKS_ON (Hours attribute)— PROJECT; EMPLOYEE —1:N SUPERVISION (recursive,
roles Supervisor/Supervisee)— EMPLOYEE; EMPLOYEE —1:N DEPENDENTS_OF (identifying
relationship)— weak entity DEPENDENT (partial key Name [dashed underline], Sex, Birth_date,
Relationship). DEPARTMENT also has a derived attribute Number_of_employees (dotted oval).

**Slide 17 (closing/teaser):** "What next? Lecture 3: Extending the ER Model Using EER" —
shows a generalization/specialization diagram (Vehicle superclass with Color/Type/Engine
attributes, disjoint 'D' circle, subclasses Scooter/Car/Truck) — loading screen graphic.
