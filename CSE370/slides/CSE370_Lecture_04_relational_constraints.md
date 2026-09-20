# CSE370 Lecture 4: Relational Data Model & Database Constraints

BRAC University CSE370: Database Systems slide deck, 15 slides. Clean digital slides, fully
legible — transcribed with confidence.

## Content

**Slide 1 (title):** Lecture 4: Relational Data Model & Database Constraints.

**Slide 2 — Relational Model Concepts:** The relational model is proposed by Dr. E.F. Codd
of IBM Research in 1970 ("A Relational Model for Large Shared Data Banks"), earning him the
ACM Turing Award. Based on set theory. Data organized in tables (formally: relations), with
columns (attributes) and rows (tuples) representing real-world facts/entities/relationships.
Example: a `Student` relation with attributes ID, Name, Phone, Email, CGPA and 3 sample
tuples (Tanzim/09304423/2.5, Sanjana/21093784/3.99, Anik/22000123/3.87).

**Slide 3 — Relation Schema & State:** The table's description is the relation schema,
denoted R(A1, A2, ..., An) — R = relation name, A1...An = attributes. Each attribute has a
domain (its set of possible values). The relation state is the actual data stored at a given
moment. Example schema: `Student(ID, Name, Phone, Email, CGPA)`. Tuples are unordered
(reordering rows doesn't change the state), but attributes are ordered (reordering columns
changes the schema).

**Slide 4 — "Key" Concepts:** **Superkey** of R: an attribute/set of attributes with a
unique value per tuple. **Key**: a *minimal* superkey — every attribute in it is necessary
for uniqueness (removing one breaks uniqueness); if removing an attribute still leaves it
unique, it's a superkey but not a key. Example: in `Student`, {ID, Name} or {ID, Email} are
superkeys but not keys (ID alone suffices). In `Grades`, K3={SID, Ccode} is both a superkey
and a key.

**Slide 5 — "Key" Concepts (cont.):** A relation may have several **candidate keys** (e.g.
{ID} or {Email} in Student) — one is chosen as the **Primary Key**, used to uniquely
identify/reference/search tuples (standard practice: pick the smallest-valued one, e.g.
{ID}). Each relation has exactly one primary key, which can be composite (e.g. {SID, Ccode}
in Grades). **Foreign Keys** reference the primary key of a different relation — e.g. {SID}
in Grades references {ID} in Student.

**Slide 6 — Relational Integrity Constraints:** Conditions that must hold on all valid
relation states, upholding structural integrity/reliability/validity. Four constraints:
Domain Constraint, Key Constraint, Entity Integrity Constraint, Referential Integrity
Constraint.

**Slide 7 — Domain Constraint:** Every value in a tuple must be from its attribute's domain
(or null if allowed). Domain = data type (integer, String, etc.) plus optional
format/range. Example violations: Age = -5 (must be a positive integer < 100); CCode =
"370CSE" (must be letters-then-digits format) — both invalidate the tuple.

**Slide 8 — Key Constraint:** Every value for a key must be unique — no two tuples in R can
share a key value. Example violations: duplicate ID 21093784 appearing twice in Student;
duplicate (SID, Ccode) pair (09304423, CSE110) appearing twice in Grades.

**Slide 9 — Entity Integrity Constraint:** Primary key attributes of a relation cannot have
null values in any tuple (if the PK is composite, none of its attributes can be null).
Example violations: null ID in Student; null Ccode in Grades (part of the composite PK
{SID, Ccode}). Note: non-PK attributes can also be constrained to disallow nulls, at the
designer's discretion.

**Slide 10 — Referential Integrity Constraint:** A foreign key column's value must be either
(1) an existing value in the referenced table's primary key column, or (2) null (only
allowed if the FK isn't itself part of the referencing table's primary key). Example: Student
has FK `DeptID` referencing Department(ID) — value 'BBS' violates the constraint (doesn't
exist in Department); Grades' FK `SID` referencing Student(ID) cannot be null since SID is
also part of Grades' own primary key {SID, Ccode}.

**Slide 11 — Enforcing Relational Integrity Constraints:** Constraints must be enforced
during the three data manipulation operations: **Insert** (new row), **Delete** (existing
row removed), **Update** (a value modified).

**Slide 12 — Data Insertion:** An INSERT may violate any of the four constraints. Domain:
enforce via declared data type plus CHECK constraints/NOT NULL/regex/application-side
validation. Key: enforce via PRIMARY KEY declaration + UNIQUE keyword (other candidate
keys). Entity Integrity: enforce via declaring the primary key. Referential Integrity:
enforce via FOREIGN KEY ... REFERENCES with proper reference declarations. Example SQL shown
(annotated/highlighted):
```sql
CREATE TABLE Student (
  ID INT,
  Name VARCHAR(100) NOT NULL,
  EmailAdd VARCHAR(100) UNIQUE,
  CGPA DECIMAL(3,2),
  Dept CHAR(3),
  PRIMARY KEY (ID),
  FOREIGN KEY (Dept) REFERENCES Department(DeptID),
  CHECK (CGPA >= 0.00 AND CGPA <= 4.00)
);
```

**Slide 13 — Data Deletion:** A DELETE can only violate Referential Integrity — if a deleted
row's PK value is referenced elsewhere (e.g. deleting a Student referenced in Grades). Enforced
via declaring FOREIGN KEY plus an `ON DELETE` clause with one of three options: **Restrict**
(disallow the deletion), **Cascade** (delete all referencing rows too), **Set Null** (null
out the referencing FK — only valid if that FK isn't part of the referencing table's own PK).
Example SQL:
```sql
CREATE TABLE Grades (
  ID INT,
  Ccode CHAR(6),
  Grade VARCHAR(2),
  PRIMARY KEY (ID, Ccode),
  FOREIGN KEY (ID) REFERENCES Student(ID) ON DELETE Cascade,
  FOREIGN KEY (Ccode) REFERENCES Course(Ccode) ON DELETE Restrict
);
```

**Slide 14 — Data Modification:** An UPDATE's constraint risk depends on which attribute is
modified. Domain Constraint: same enforcement as INSERT. Key/Entity Integrity: risk when
updating PK/candidate-key values, enforced by PK/UNIQUE declarations. Referential Integrity:
risk if updating a referenced PK or a referencing FK; enforced via FOREIGN KEY + `ON UPDATE`
clause with the same three options (Restrict/Cascade/Set Null). Example SQL:
```sql
CREATE TABLE Grades (
  ID INT,
  Ccode CHAR(6),
  Grade VARCHAR(2),
  PRIMARY KEY (ID, Ccode),
  FOREIGN KEY (ID) REFERENCES Student(ID) ON UPDATE Cascade,
  FOREIGN KEY (Ccode) REFERENCES Course(Ccode) ON DELETE Cascade ON UPDATE Cascade
);
```

**Slide 15 (closing/teaser):** "What next? Lecture 5: Relational Schema Mapping" — shows an
EER diagram (User/Account/Post/Report entities with relationships) mapping down into a
relational schema (Project, Department, Project_Members, Department_Members, Person, Entity,
Organization tables with PK/FK annotations) — loading screen graphic.
