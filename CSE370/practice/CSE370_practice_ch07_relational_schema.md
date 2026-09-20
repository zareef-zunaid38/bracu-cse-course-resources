---
source: CSE370_practice_ch07_relational_schema.pdf (the original, included in this repo)
fidelity: text pages = full text-layer extraction; figure pages = described (data tables and charts transcribed, photos/diagrams summarised) — open the PDF page for detail
figure-pages: 4, 6, 7, 8
source-mtime: 2026-06-27T16:04:14
generated: 2026-09-20
---
# CSE370_practice_ch07_relational_schema.pdf

<!-- p.1 -->
Practice Sheet
Chapter 7 (Relational Schema from ER/EER)
*Solutions are at the end of the document. SOLVE IT YOURSELF first.
Question 1:
Construct a Relational Schema from the following ER diagram:
Question 2:
Construct a Relational Schema from the following ER diagram:

<!-- p.2 -->
Question 3:
Construct a Relational Schema from the following ER diagram:
Question 4:
Construct a Relational Schema from the following ER diagram:

<!-- p.3 -->
Question 5:
Construct a Relational Schema from the following EER diagram. For each of the subclasses/superclasses
use any suitable option out of the 4 options.
Question 6:
There are four options for mapping subclasses and superclasses:
8A: separate tables for each subclass and superclass
8B: tables for only subclasses
8C: 1 table with 1 type attribute
8D: 1 table with many type/flag attributes
State which of the above options are not applicable for the following diagrams and explain why. If all 4
options are applicable then simply state “all options are applicable”.
A.

<!-- p.4 -->
B.
C.
D.
> **[Figure, PDF p.4]** Three EER diagram options. All share Student (attributes ID [underlined key], dept, Name, cgpa). B: disjoint specialization ('d') into Undergrad and Graduate (Graduate has attribute major). C: overlapping specialization ('O') into Teaching Assistant (attribute course) and Research Assistant. D: same as C but with a double line (total participation) into the specialization.

<!-- p.5 -->
** Note some questions may have more than one solution.
SOLUTIONS:
Answer 1:
Answer 2:

<!-- p.6 -->
Answer 3:
> **[Figure, PDF p.6]** Answer 3: relational-schema diagram with foreign-key arrows. Books(ISBN, Title, Year, Price, Publisher_Name, Warehouse_City, Warehouse_SerialNo); Books_Author(ISBN, Author); Publisher(Name, Email, Address, Website, Phone); Shopping_Cart(Cart_ID); Added_To(ISBN, Cart_ID, Quantity, Date); Customer(User_ID, Name, Phone, Email, Street, Country, Zip, City, Cart_ID); Warehouse(City, SerialNo, Manager, Address, Phone). Which attributes are underlined keys and where each arrow points: see PDF page.

<!-- p.7 -->
Answer 4:
> **[Figure, PDF p.7]** Answer 4 (handwritten schema, numbered 3): Pet(TagNo, description, price, Name, type, cost, Nid, PDate, Ptime, Phone, SDate, Stime); Vet(Phone, fees, address, name); Visits(TagNo, Phone, Date, reason, time); Visit_medicine(TagNo, Phone, med_name, dose); Supplier(Phone, zip, city, street, country, Name); Customer(Nid, address, name, contact, email, rNid, points). Handwritten; key underlines and FK arrows: see PDF page.

<!-- p.8 -->
Answer 5:
> **[Figure, PDF p.8]** Answer 5 (handwritten schema, numbered 4): BUILDINGS(BNumber, Address, Mgr_Name, Mgr_Phone); ROOMS(BNumber, Floor No, Serial No, Type, Capacity); EXAM of(BNumber, Floor No, Serial No, CCode, Etype, Date, Time); COURSES(CCode, Name, Credits, Department). Keys and FK arrows: see PDF page.

<!-- p.9 -->
Answer 6:
A. All options are applicable
B. Option 8B is not applicable. The specialization/generalization is partial and 8B is not applicable
for partial. Because in 8B we only have tables for subclasses, so if a superclass entity does not
belong to any of the subclasses the data for that entity will be lost.
C. Option 8B and 8C not applicable. 8B reason same as above. 8C not applicable for overlapping as
only 1 type attribute is not sufficient to store complete information if an entity belongs to multiple
subclasses.
D. 8C not applicable. Reason same as above.
