# CSE370 Lecture 3: Enhanced Entity Relationships — sidecar (read this, not the .pdf)

BRAC University CSE370: Database Systems slide deck, 13 slides. Clean digital slides, fully
legible — transcribed with confidence (diagrams described structurally rather than
pixel-traced, since the notation is standard EER Chen-style).

## Content

**Slide 1 (title):** Lecture 3: Enhanced Entity Relationships. Example diagram: entity A
with attributes E and G (G underlined = key), connected via a relationship circle to
entities B (attributes L, J) and C.

**Slide 2 — Enhanced Entity Relationship Modeling:** EER extends the traditional ER model to
represent more complex scenarios with nuanced relationships/constraints. Includes all basic
ER concepts, models data more completely/accurately, and adds: subclass/superclass,
inheritance, specialization/generalization, categories.

**Slide 3 — Subclasses & Superclasses (1):** An entity type can have meaningful
sub-groupings; EER diagrams extend ER diagrams to represent these. Example: STUDENTS in a
'University' mini-world divide into Undergrad/Graduate (by degree level); FACULTY divide by
designation: Lecturer, Senior Lecturer, Assistant Prof, Associate Prof, Prof. STUDENT and
FACULTY are superclasses; Undergrad/Graduate and Lecturer/etc. are their subclasses.

**Slide 4 — Subclasses & Superclasses (2):** Subclasses can have further subclasses forming
a hierarchy/tree (e.g. Graduate → Masters, PhD). A subclass may be shared by multiple
superclasses forming a lattice (e.g. StudentTutor is a subclass of both Student and
Employee — diagram shows Student and Employee both connecting to Student Tutor). This is
called an IS-A relationship (e.g. undergrad student IS-A student). If an entity is a member
of any subclass it MUST belong to the superclass, but a superclass member may belong to
several, or none, of the subclasses.

**Slide 5 — Inheritance & Local Attributes/Relationships:** A subclass entity inherits all
attributes and relationships of its superclass — ID, name, cgpa and the enrolls_in
relationship (to Course, attributes grade; Course has attribute Code) are inherited by all
Undergrad/Graduate students; Masters/PhD inherit from both Student and Graduate. Subclasses
can also have local/specific attributes (e.g. clubs on Undergrad, researchArea on PhD) and
local relationships (e.g. TA_of, connecting PhD to Course, cardinality N on one side / 1 on
the other via TA_of — diagram shows PhD —TA_of(N)— Course(1)).

**Slide 6 — Specialization:** Specialization is the process of defining a set of subclasses
of a superclass, based on distinguishing characteristics. Example: {Undergrad, Graduate} is
a specialization of Student (by academic degree); Student has another specialization by job:
{StudentTutor, Research Assistant}. A superclass can have several specializations at once —
Faculty has three: by contract type (Contractual, Full-time), by designation (Lecturer,
Assistant Prof, Associate Prof, Prof — 4 subclasses), and by department-head status
(DeptHead — 1 subclass).

**Slide 7 — Generalization:** Generalization is the opposite of specialization — defining a
superclass from a set of subclasses. Several entity types with common attributes can be
generalized into a superclass; the originals become its subclasses. It's not possible to
tell from a given EER whether specialization or generalization was used to derive it (e.g.
Car/Truck → Vehicle could go either way). Example: TheoryRoom (attributes boardType, RoomNo,
capacity) and LabRoom (attributes equipmentList, RoomNo, capacity, type, boardType) share
common attributes, generalized into superclass Classroom (capacity, RoomNo, boardType);
common attributes/relationships move to the superclass, non-common ones (equipmentList,
type) stay on the Lab/Theory subclasses.

**Slide 8 — Constraints on Specialization/Generalization (1) — Disjointness:** Indicates the
max number of subclasses an entity can belong to. **Disjoint** ('d' in the diagram): an
entity can be a member of max 1 subclass — e.g. Vehicle is either Car, Bus, or Truck; Student
is either Undergrad or Graduate but not both. **Overlapping** ('o' in the diagram): an entity
can be a member of more than one subclass at once — e.g. a Student can be both StudentTutor
and Research Assistant; a Vehicle can be both Rental and ForSale.

**Slide 9 — Constraints on Specialization/Generalization (2) — Completeness:** Indicates the
min number of subclasses an entity must belong to. **Total** (double line in diagram): an
entity must belong to at least 1 subclass — e.g. every Student must be either Undergrad or
Graduate. **Partial** (single line): an entity may not belong to any subclass — e.g. a
Student may or may not be a StudentTutor/Research Assistant. Combining disjointness and
completeness gives 4 possible types: disjoint-total, disjoint-partial, overlapping-total,
overlapping-partial.

**Slide 10 — Applying EER Concepts in Practice (1) — problem statement:** Given data
requirements for a University DBMS, construct an EER diagram. Requirements: 3 person types —
Employee, Alumnus, Student (overlapping — a person can be more than one type at once); every
person has unique SSN, Name, Sex, Address, Birth date; Employees have Salary, Students have
Major; Alumnus has multiple Degrees (composed of year, institute, major). Employees must
belong to exactly 1 type (disjoint): Staff (Position), Faculty (Rank), or Student Assistant
(Percent_Time) — Student Assistant also inherits from Student. Students are exactly one of
(disjoint): undergraduate (Class attribute) or graduate (Degree_program attribute). Student
Assistants are exactly one of (disjoint): Research Assistant (project) or Teaching Assistant
(course).

**Slide 11 — Applying EER Concepts in Practice (2) — solution diagram:** PERSON (Ssn key,
Name, Sex, Address, Birth_date) —overlapping(o)— EMPLOYEE (Salary), ALUMNUS (Degrees:
composite/multivalued with Year, Degree, Major), STUDENT (Major_dept). EMPLOYEE
—disjoint(d)— STAFF (Position), FACULTY (Rank), STUDENT_ASSISTANT (Percent_time, and also
connects up to STUDENT as a second superclass). STUDENT —disjoint(d)— GRADUATE_STUDENT
(Degree_program), UNDERGRADUATE_STUDENT (Class). STUDENT_ASSISTANT —disjoint(d)—
RESEARCH_ASSISTANT (Project), TEACHING_ASSISTANT (Course).

**Slide 12 — Applying EER Concepts in Practice (3) — open-ended exercise:** Design an EER
diagram for a School Management System tracking employees, students, clubs, courses, grades,
etc. Constraints the design must satisfy: at least one disjoint-total specialization/
generalization; at least five regular/strong entities (excluding subclasses); a recursive
relationship; at least one 1:1 relationship; at least one multivalued-composite attribute.
Marked as open-ended — "different people should have different solutions. Try it yourself."

**Slide 13 (closing/teaser):** "What next? Lecture 4: Relational Database Constraints" —
loading screen graphic.
