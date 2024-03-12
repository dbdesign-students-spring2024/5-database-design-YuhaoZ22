# Data Normalization and Entity-Relationship Diagramming
## Original Data Set Table

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |

## What Makes This Data Set not Compliant With 4NF
The dataset presented exhibits a failure to adhere to Fourth
Normal Form (4NF) guidelines due to the presence of non-trivial multi-valued dependencies, which are apparent when one attribute or a set of attributes can determine multiple independent attributes. For instance, considering a single student could be enrolled in courses that require different sets of readings (relevant_reading) for different assignments, this condition suggests a multi-valued dependency between `student_id` and `relevant_reading` that does not depend on another attribute to be determined. Such a situation illustrates a multi-valued dependency that is not directly related to a candidate key, indicating a 4NF violation.


To bring the dataset in line with 4NF, the objective is to dismantle any table harboring two or more independent sets of multi-valued facts relative to an entity and identify potential candidate keys. This approach entails segregating data into more focused tables that eliminate these multi-valued dependencies, thus enhancing the dataset's structure and integrity.

## Tables Containing the 4NF-Compliant Version of the Data Set

### 1 Sections

| section_id | course_id | professor_id | classroom |
| :--------- | :-------- | :----------- | :-------- |
| 101        | 1         | 1            | WWH 101   | 
| 102        | 2         | 2            | 60FA 314  |
| 203        | 2         | 3            | WWH 201   |
| ...        | ...       | ...          | ...       |

Primary Key: `section_id` Foriegn Key: `course_id`, `professor_id`

### 2 Courses

| course_id | course_name |
| :-------- | :---------- |
| 1         | Intro to Econometrics  |
| 2         | Data Structure  |
| 3         | Intro to CS |
| ...       | ...         |

Primary Key: `course_id` 

### 3 Professors

| professor_id | professor_name | professor_email    |
| :----------- | :------------- | :----------------- |
| 1            | Melvin         | l.melvin@foo.edu   |
| 2            | Logston        | e.logston@foo.edu  |
| 3            | Nevarez        | i.nevarez@foo.edu  |
| ...          | ...            | ...                |

Primary Key: `professor_id`

### 4 Assignments

| assignment_id | section_id | assignment_topic                    | due_date | 
| :------------ | :-------- | :---------------------------------- | :------- |
| 1             | 101         | Data normalization                  | 23.02.21 |
| 2             | 203         | Single table queries                | 18.11.21 |
| 4             | 102         | Spreadsheet aggregate functionns    | 05.05.21 |
| ...           | ...       | ...                                 | ...      |

Primary Key: `assignment_id` Foreign Key: `course_id`

### 5 Reading 

| reading_id | assignment_id | relevant_reading     |
| :--------- | :------------ | :------------------- | 
| 1          | 1             | Deumlich Chapter 3   |
| 3          | 3             | Dümmlers Chapter 11  |
| 4          | 2             | Zehnder Page 87      |
| ...        | ...           | ...                  |

Primary Key: `reading_id` Foreign Key: `assignment_id`



### 6 Students
| student_id | student_first_name | student_last_name |
| :--------- | :----------------- | :---------------- |
| 1          |  Morgan            | Johnson           |
| 2          |  Alex              | Smith             |
| 4          |  Taylor            | Williams          |
| 7          |  Jordan            | Brown             |
| ...        | ...                | ...               |

Primary Key: `student_id`

## ER diagram
![Example Image](/images/ER.svg)

## Description of Changes
The `Sections` table now associates courses with professors and classrooms, with a section ID as a primary key.
The `Courses` table lists courses independently.
The `Professors` table lists professor details independently.
The `Assignments` table lists assignments, now associated with courses via course_id.
The `Reading` table associates readings with assignments.
The `Students` table lists student information independently.

By doing so, we eliminated multi-valued dependencies, ensuring that each table only contains independent attributes related to its key, thereby achieving 4NF compliance. Each table now represents one piece of information about an entity, avoiding the intertwining of unrelated data, which is the essence of 4NF.