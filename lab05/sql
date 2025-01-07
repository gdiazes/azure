-- Create Person table          
CREATE TABLE person        
(          
personid INT IDENTITY PRIMARY KEY,          
firstname NVARCHAR(128) NOT NULL,          
middelinitial NVARCHAR(10),        
lastname NVARCHAR(128) NOT NULL,            
dateofbirth DATE NOT NULL          
)          
-- Create Student table        
CREATE TABLE student            
(          
studentid INT IDENTITY PRIMARY KEY,        
personid INT REFERENCES person (personid),          
email NVARCHAR(256)        
)          
-- Create Course table          
CREATE TABLE course        
(          
courseid INT IDENTITY PRIMARY KEY,          
NAME NVARCHAR(50) NOT NULL,        
teacher NVARCHAR(256) NOT NULL          
)          
-- Create Credit table          
CREATE TABLE credit        
(          
studentid INT REFERENCES student (studentid),          
courseid INT REFERENCES course (courseid),          
grade DECIMAL(5, 2) CHECK (grade <= 100.00),            
attempt TINYINT,            
CONSTRAINT [UQ_studentgrades] UNIQUE CLUSTERED ( studentid, courseid, grade, attempt )          
)
