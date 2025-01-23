# Part Database Interface in Java
Please cd into the src directory within the unzipped Assgn1 directory. Once there, run javac PartInterface.java. Then, run java PartInterface.java. An unedited copy of part.tbl has already been placed within the src directory. If you would like to replace it, please remove the file currently in the src directory and place the replacement file inside the src directory. There are no libraries necessary to install.

# Searching for Parts
This interface allows users to search for parts by entering up to nine attributes separated by the pipe (|) delimiter. The entered attributes do not need to match the order in which they appear in the file, but they must be spelled correctly and fully typed. The search is case-insensitive. For example, entering "2" will display all parts with a size of 2 as well as the part with a partkey of 2. In this instance, any part containing the attribute "2," regardless of its specific category, will be included in the results.
