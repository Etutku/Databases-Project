## Project for Databases | Fourth Semester - AGH UST
   * [Full Project Repository](https://gitlab.kis.agh.edu.pl/databases-2-2023/db2-edibe-kacper)
   * Prepared by Edibe Tutku GAYDA
   * ID: 414151
 
| No. | Table of Contents                                                                   |
| --- | ----------------------------------------------------------------------- |
| 1   | [**Project Description and Tasks**](https://gitlab.kis.agh.edu.pl/databases-2-2023/db2-edibe-kacper/-/blob/main/README.md)  
| 2   | [**Instruction**](https://gitlab.kis.agh.edu.pl/databases-2-2023/db2-edibe-kacper/-/blob/main/README.md)   |
| 4   | [**Project Base**](https://gitlab.kis.agh.edu.pl/databases-2-2023/db2-edibe-kacper/-/blob/main/ProjectBase.md)   |
| 5   | [**Project General Repository**](https://gitlab.kis.agh.edu.pl/databases-2-2023/db2-edibe-kacper/-/blob/main/ProjectBase.md)   |

# Prerequisites
 **1.** Installation of [PostgreSQL](https://www.postgresql.org/download/)

 **2.** "taxonomy_iw.csv.gz" file available.

 **3.** Compile tasks with using [Project Base](https://gitlab.kis.agh.edu.pl/databases-2-2023/db2-edibe-kacper/-/blob/main/ProjectBase.md)

# Setup
 **1.** Create a new PostgreSQL database.

 **2.** Import the CSV data into the database.
The goal of this project is to develop a command line utility that leverages SQL to perform various operations on a hierarchical data structure. The utility will primarily focus on manipulating nodes within the structure and achieving the following objectives:

- [X] **1.** Find all children of a given node,
- [X] **2.** Count all children of a given node,
- [X] **3.** Find all grand children of a given node,
- [X] **4.** Find all parents of a given node,
- [X] **5.** Count all parents of a given node,
- [X] **6.** Find all grand parents of a given node,
- [X] **7.** Counts how many uniquely named nodes there are,
- [X] **8.** Find a root node, one which is not a subcategory of any other node,
- [X] **9.** Find nodes with the most children, there could be more the one,
- [X] **10.** Find nodes with the least children, there could be more the one,
- [X] **11.** Rename a given node.

