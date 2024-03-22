# uuid-splitter
Utility to generate UUID ranges for SQL statements

# What is it? 
A script to generate UUID ranges for SQL scripts that need to update / delete many rows and would cause unacceptable locking issues.

 parameters: -n number of batches (will round up to the next power of 2)
                OR
             -t total number of expected rows
             -b batch size
                OPTIONAL
             -u UUID column name
             -s filename with SQL to prepend
             -f filename with SQL to prepend

splitter can either accept a number of batches, or it can calculate the number of batches given the number of rows and a batch size. In either case, it will round up to the next power of 2 if needed. So, for example, -N 1000 will actually create 1024 batches. -T 500000 -B 500 would also request 1000 batches, which would round up to 1024. -N 1024 will not round up and will create 1024 batches.

# Why?
Some of us use Apache Spark for this exact thing, but Spark simply and blindly appends the final where clause to the end of the statement and if you have a complex query or a CTE, it may not be possible for the optimiser to push the where clause far enough into the query to help performance.

This is very blunt, but does at least give you the necessary flexibility.
