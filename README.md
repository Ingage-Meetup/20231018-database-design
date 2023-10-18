# DB Design Challenge
Your software development company just won a contract with the Cincinnati Public Schools to build an application to track students progress through their high school career. The specs below are the requirements that were in the request for proposal (RFP). While we know the requirements will be clarified and changed throughout the development process we want to make sure we understand the problem domain and would like to sketch out the database design (Tables, Relationships, etc.) to ensure we can capture the data necessary to fulfill the initial requirements, and to help drive further discussions with the client.
 
As you review the requirements try to determine if a requirement will be implemented directly in database tables or handled in a different part of the system (API, UI, Authentication/Authorization, or some other component of the system). Once you have determined what DB entities (i.e. Tables) need to be stored determine the relationships between the tables and the cardinality (one-to-one, one-to-many, many-to-many), and create at least a rough Entity Relationship Diagram. Add columns that are necessary to the fulfill the requirements, and feel free to add additional columns that would make a better User Experience for at least one of the primary users of the system.
 
Time permitting create the tables in the provided Postgres DB using your own assigned database (team1, team2, etc.). Refer to the sample scripts for details on creating tables, inserting test data, and queries for any of the requirements / reports. Also feel free to use the PG Admin UI provided, IntelliJ, SQuirrel SQL, or any other SQL Client that you might have installed to design the tables.
 
## Requirements
The Cincinnati Public Schools want to allow high school students to manage all aspects of their school curriculum through a student portal. The goal is that the portal application will replace the existing system, School Time®, within 2 years.
 
The School Time® system is outdated, slow, contains security vulnerabilities, and the maintenance cost has been steadily increasing. They have also been slow to deliver much needed functionality to run a modern school system. The software company that developed the application was also recently purchased by a much larger competitor. We suspect that the software may be sunset in the next couple of years as a way to upsell us to their holistic School 365 SaaS offering at 5 times our current annual maintenance cost, which would wreak havoc on the current budget.
 
Cincinnati Public Schools realizes that the current functionality available in the School Time® system can not be developed before the next school year. They would like a basic version of the portal to be used by one pilot school for the next year. The scope of the work is initially focused around students, the students family, and the teachers.
 
### Registrations
We want to allow the students to build their school schedule based on a list of classes that are being offered each semester. They need to signup for at least 5 classes, but not more than 7 classes per semester. The same class may be offered multiple times per semester. Students should also be able to see who is teaching the class they are registering for. Some classes are only offered one once a year, and so may not be available every semester. Class sizes are limited by the rooms on which they are assigned, and enrollment in a course is on a first come first served basis.
 
Prior to registration opening a school administrator will add any new classes that are being offered for the first time. Classes should have a class name and a brief description. The administrators will also key in the data assigning the teachers to the classes and set the size of the class.
 
Students should not be able to take the same class again unless they have previously failed the class.
 
### Grades
The portal should make grades available to view for both the student and the family of the student. The email address of the teach should also be available to facilitate discussions should the need arise.
 
Teachers should be able to enter the current overall grade of the student at the end of each week. For the initial pilot this will remain a manual process for the teachers to calculate and input the current overall grade. Teachers should only have access to enter the grades for the students in the classes they teach. We would like to improve this in future phases of the project to be able to capture individual grades for assignments, quizzes, tests, etc. along with the relative weighting.
 
Students and their families must not have access to change grades. Back office staff may be called on to help with inputting grades, so should have the ability to enter grades for all students.
 
In addition to viewing the current semesters grades we want to be able to send out report card reports at the end of the semester. The report cards will be emailed to the students and their legal guardians.
 
For each class it will have the following information
* Class Name
* Teacher's Name & Email
* Letter Grade
* Numeric Grade
 
It will also have the GPA for the current semester, the overall GPA for their high school career, the number of classes completed with passing grades, and the number of classes needed to graduate. Students currently need 48 classes total to receive their diploma, or an average of 6 classes per semester.
 
For those that are in their senior year we need to be able to produce a transcript report that will contain information similar to the report card, but for all 4 years for a single student. For official transcript for college admissions purposes will be handled with a back office process initially, where admins run the report, print it out, and mail it to the colleges. This will be initiated by a request from a guidance counselor, a parent, legal guardian, or the student if over 18 years old by sending an email to the office admin. The transcript report should contain all classes that have been completed to date, ordered from earliest to latest. It should have the class name, letter grade, numeric grade, as well as overall GPA.
 
### Authentication and Authorization
The school system current uses Microsoft Azure Active Directory (AAD) to login information for students, teacher, back office staff. AAD groups also identify what kind of user they are (student, teach, principal, office admin, IT staff, etc.). They would like to use the same credentials for logging into the portal. Parents and Guardians do not and can not have accounts in the main Azure Active Directory, but they need to provide them access to the portal. We think that Azure Active Directory Business to Consumer (AAD B2C) can be used to store their credentials. The portal would have to configure the relationships between the parents and all their children.
 
### Non-Functional Requirements
We want to track the person and time that each record in the system was created, and also the last person to change the record and the time of the change. We don't need an entire audit trail of each and every change at this time.
 
### Additional Consideration
A stretch goal is being able to capture classes that have pre-requisites, and prevent students from signing up for class when they have not fulfilled the pre-requisites. We have extracted the students data from School Time®, and will backfill the data into the new system.