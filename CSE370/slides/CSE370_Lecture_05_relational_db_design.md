---
source: CSE370_Lecture_05_relational_db_design.pdf (the original, included in this repo)
fidelity: transcription (this file's own note says it is transcribed with confidence); page markers not recorded
verified: sampled slides 6, 17 compared with the PDF images on 2026-09-20: matched
source-mtime: 2026-06-27T15:58:36
generated: earlier (2026-09); header added 2026-09-20
---
# CSE370 Lecture 5: Relational Database Design (Mapping ER/EER to Relational Schema)

BRAC University CSE370: Database Systems slide deck, 20 slides. Clean digital slides, fully
legible — transcribed with confidence.

## Content

**Slide 1 (title):** Lecture 5: Relational Database Design — Mapping ER/EER to Relational
Schema.

**Slide 2 — ER to Relational Schema, overview (7 steps):** Step 1: Regular Entity Types.
Step 2: Weak Entity Types. Step 3: Binary N:M Relationship Types. Step 4: Binary 1:N
Relationship Types. Step 5: Binary 1:1 Relationship Types. Step 6: Multivalued attributes.
Step 7: N-ary Relationship Types. (Uses the standard Employee/Department/Project/Dependent
ER diagram as the running example throughout.)

**Slide 3 — Step 1: Mapping Regular Entity Types:** Create a separate table per regular
entity type (tables must be named). Simple attributes become columns; simple components of
a composite attribute (e.g. Address → City/House/Road/Zip) become separate columns.
Multivalued attributes (e.g. Phone) are deferred to Step 6. Derived attributes (e.g. Age)
are not shown in the schema at all. Only one candidate key is chosen as primary key
(underlined) — e.g. Student(ID, Name, Email, DOB, City, House, Road, Zip), with ID
underlined and Email left as a plain candidate key.

**Slide 4 — Step 2: Mapping Weak Entity Types:** Separate table per weak entity, with its own
simple attributes/components (multivalued deferred to later). The weak entity must have an
identifying relationship with its owner — the owner's PK is added as an FK (shown with
arrows). The combined PK of the weak entity's table = owner's FK + the weak entity's own
partial key (both underlined). Example: Course(Ccode, Title, Credits); Section(**Ccode**,
**Section_no**, RoomNo, Semester) — Ccode is FK from Course, and Ccode+Section_no together
form Section's composite PK.

**Slide 5 — Step 3: Mapping Binary N:M Relationship Types:** Create a separate table for the
relationship itself (the "cross-referencing approach"). The PKs of both connected entities
become FKs in this new table, and together (underlined) form its combined PK. Any
relationship attributes (e.g. grade) are added to this same table. Example:
Enrolls_in(**StudentID**, **Ccode**, grade), cross-referencing Student and Course.

**Slide 6 — Step 4: Mapping Binary 1:N Relationship Types:** Cross-referencing works but is
space-inefficient here — instead use the **foreign key approach**: take the PK of the entity
on the "1" side (e.g. Program) and add it as an FK directly into the entity table on the "N"
side (e.g. Student) — no separate relationship table needed. All non-multivalued relationship
attributes are also added to the "N"-side table. Example: Program(PID, Name, totalCredits);
Student(ID, Name, CGPA, **PID**, completedCredit) — PID is FK from Program.

**Slide 7 — Step 5: Mapping Binary 1:1 Relationship Types:** Three possible approaches:
**cross-referencing** (same as Step 3); **merged relation** (merge both entity tables into
one, PK = combination of both keys — inefficient, can waste storage); **foreign key approach**
(most common — take either side's PK and add as FK to the other table, with relationship
attributes going in the same table as the FK). Best practice: put the FK on the entity with
**total participation** in the relationship, to avoid unnecessary nulls. Example:
Faculty(ID, Name); Department(DID, Name, Location, **FacID**, startDate) — FacID is FK from
Faculty, placed here since every Department must have a head.

**Slide 8 — Mapping Recursive Relationships:** Same rules as ordinary binary relationships,
based on cardinality: N:M → cross-referencing (separate table with the entity's own PK
listed twice under different role names, e.g. prereq_Ccode/dependent_Ccode both referencing
Course); 1:N → foreign key approach (the entity's own PK added as a second, differently-named
FK column in its own table, e.g. Course(Ccode, Title, Credits, prereq_Ccode)); 1:1 →
cross-referencing or foreign key approach, same as Step 5.

**Slide 9 — Step 6: Mapping Multivalued Attributes:** Separate table per multivalued
attribute (including multivalued-composite ones). Add the owning entity's PK as FK. For a
purely multivalued attribute (e.g. Phone): add one more column for the value itself; PK+value
column together (both underlined) form the combined PK — e.g. Student_Phone(**ID**,
**Phone**). For a multivalued-*composite* attribute (e.g. PreviousDegree: Year/DName/
Institute): add only the simple sub-components as separate columns, with FK + any indicated
partial-key components underlined as the combined PK (if no partial key is shown, underline
all components) — e.g. PreviousDegree(**ID**, **Year**, DName, Institute).

**Slide 10 — Step 7: Mapping N-ary (Ternary+) Relationship Types:** Separate table per N-ary
relationship. PKs of *all* connected entities become FKs in this table, together forming its
combined PK (all underlined); relationship attributes added too. Example: Enrolls_in
(**StudentID**, **Ccode**, **SemName**, grade), cross-referencing Student, Course, and
Semester (a true 3-way/ternary relationship, distinct from the earlier 1:N example that
happened to share the same relationship name).

**Slides 11-12 — ER to Relational Schema in Practice (full worked example, both panels
combine into one schema):** The full Employee/Department/Project/Dependent university-style
ER diagram (same as Lecture 2's applied example) is mapped end to end into 6 tables:
`EMPLOYEE(Fname, Minit, Lname, **Ssn**, Bdate, Address, Sex, Salary, Super_ssn, Dno)` —
Super_ssn is a recursive FK (from the Supervision relationship) and Dno is an FK to
Department (from Works_for);
`DEPARTMENT(Dname, **Dnumber**, Mgr_ssn, Mgr_start_date)` — Mgr_ssn/Mgr_start_date are FK +
attribute from the 1:1 Manages relationship;
`DEPT_LOCATIONS(**Dnumber**, **Dlocation**)` — multivalued Locations attribute mapped per
Step 6;
`PROJECT(Pname, **Pnumber**, Plocation, Dnum)` — Dnum is FK to Department (from Controls,
1:N);
`WORKS_ON(**Essn**, **Pno**, Hours)` — cross-referencing table for the M:N Works_on
relationship between Employee and Project;
`DEPENDENT(**Essn**, **Dependent_name**, Sex, Bdate, Relationship)` — weak entity table, Essn
FK from Employee (identifying relationship) + Dependent_name as partial key.
(Note: Number_of_employees, a derived attribute on Department, is correctly omitted from the
schema.)

**Slide 13 — EER to Relational Schema — Step 8 overview (Specialization/Generalization):**
Four sub-options for mapping a superclass/subclass hierarchy: **8A** — multiple tables
(superclass + subclass tables). **8B** — multiple tables (subclasses only, no superclass
table). **8C** — single table with one "type" discriminator attribute. **8D** — single table
with multiple boolean "flag" attributes. Running example throughout: Person → (overlapping,
partial) Employee/Alumnus/Student; Employee → (disjoint, total) Staff/Faculty/
Student_Assistant; Student_Assistant → (disjoint, total) Research_Assistant/
Teaching_Assistant; Student → (disjoint, total) Graduate_Student/Undergraduate_Student.

**Slide 14 — Option 8A (separate tables for superclass + each subclass):** Superclass table
built as in Step 1. Its PK becomes both an FK *and* the PK in each subclass table. Local
subclass attributes go into their own tables only. Example: Student(**ID**, Name, CGPA);
Undergrad(**ID**); PostGrad(**ID**, NID, major) — both subclass tables reuse ID as PK/FK back
to Student.

**Slide 15 — Option 8B (tables only for subclasses, no superclass table):** Only valid when
the specialization is **total** (partial would silently lose superclass members that belong
to no subclass). All superclass attributes are duplicated into every subclass table, with one
superclass key attribute serving as each subclass table's own PK. If the superclass has a
multivalued attribute, its Step-6 table gets duplicated once per subclass too. Example:
Undergrad(**ID**, Name, CGPA); PostGrad(**ID**, Name, CGPA, NID, major).

**Slide 16 — Option 8C (single table, one discriminator column):** Only valid when the
specialization is **disjoint** (overlapping would need more than one flag to capture
multi-membership). One combined table holds superclass + all subclass attributes, one
superclass key as PK, plus one extra "type" column (e.g. StudentType) as a discriminator
naming which subclass each row belongs to. Example: Student(**ID**, Name, CGPA, NID, major,
StudentType).

**Slide 17 — Option 8D (single table, multiple flag columns):** Valid for any
disjoint/overlapping × total/partial combination, but wasteful for disjoint cases (8C is
more efficient there — one flag suffices when only one subclass can ever apply, vs. N
separate boolean columns here). One combined table as in 8C, but with N boolean flag columns
(e.g. UGFlag, PGFlag — one per subclass) instead of a single type column. Example:
Student(**ID**, Name, CGPA, NID, major, UGFlag, PGFlag).

**Slides 18-19 — EER to Relational Schema in Practice (full worked example, Option 8A style,
both panels combine into one schema):** The Person/Employee/Alumnus/Student hierarchy mapped
using foreign-key-as-PK subclass tables: `PERSON(**Ssn**, Name, Birth_date, Sex, Address)`;
`EMPLOYEE(**Ssn**, Salary, Employee_type, Position, Rank, Percent_time, Ra_flag, Ta_flag,
Project, Course)` — note this specific worked table actually applies Option 8D/hybrid
treatment *within* the Employee branch (a discriminator "Employee_type" column plus
Ra_flag/Ta_flag booleans for the nested Student_Assistant sub-split, rather than fully
separate tables per sub-subclass), all Ssn-keyed back to Person; `ALUMNUS(**Ssn**)` +
`ALUMNUS_DEGREES(**Ssn**, **Year**, Degree, Major)` — Alumnus's multivalued Degrees attribute
mapped per Step 6; `STUDENT(**Ssn**, Major_dept, Grad_flag, Undergrad_flag, Degree_program,
Class, Student_assist_flag)` — Option 8D style for the Student branch. All four tables share
Ssn as both PK and the FK chain back to Person.

**Slide 20 (closing/teaser):** "What next? Lecture 6: Normalization" — graphic showing one
table splitting into two normalized tables — loading screen graphic.
