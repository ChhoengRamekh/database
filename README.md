

//Select the Province that not in the other table
SELECT stu.province from tblstudent stu WHERE stu.province 
not in(SELECT pro.pro_en_name FROM tblprovinces pro);

//select the province not start with letter P
use test;
SELECT * FROM tblstudent 
WHERE province NOT LIKE 'P%';



//select the province in tblprovince that not in the tbl student
use test;
select pro_id, pro_en_name, pro_kh_name 
from tblprovinces pro where pro_en_name 
not in(select province from tblstudent);



//select the student who come from Phnom Penh
SELECT * FROM tblstudent 
WHERE province LIKE 'Phnom Penh';


//select the province which is the same in table tblprovinces
use test;
SELECT studentid, firstname, lastname, subject, province 
FROM tblstudent WHERE province in
(SELECT pro_en_name FROM tblprovinces WHERE pro_kh_name='???????');


//select the student who has teacher incharge lastname Vongkol
SELECT firstname, lastname, subject, province FROM tblstudent 
WHERE teacherinchargeid 
in (SELECT teacherid FROM tblteacher WHERE lastname="Vongkol");



//inner join or in// 

//select the student province which has in tblprovince
select firstname, province from tblstudent 
where province 
in (select pro_en_name from tblprovinces);


//select the student province which has in tblprovince
select studentid, firstname, lastname, province from tblstudent
where province in(select pro_en_name from tblprovinces);

//select the student province which has in tblprovince
select studentid, firstname, lastname, province from tblstudent
inner join tblprovinces on province = pro_en_name;

//select the student who has the teacher incharge 
select stu.studentid, stu.firstname, stu.lastname from tblstudent stu
where stu.teacherinchargeid in(select teacherid from tblteacher);


select stu.studentid, stu.firstname, stu.lastname from tblstudent stu
inner join tblteacher on teacherid = teacherinchargeid;

select stu.studentid, stu.firstname, stu.lastname, tea.lastname 
from tblstudent stu, tblteacher tea where stu.teacherinchargeid = tea.teacherid;


select stu.studentid, stu.firstname, stu.lastname, tea.lastname as "TeacherLastname"
from tblstudent stu inner join tblteacher tea where stu.teacherinchargeid = tea.teacherid;



//select the student and teacher who come from the same provicne
select stu.firstname, stu.lastname, tea.firstname , tea.lastname, 
from tblstudent stu inner join tblteacher tea on tea.province = stu.province; 



//subquery
//select the student who the parent are police
select * from tblstudent where parentinchargeid in (select parentid from tblparent where job = 'Police');



//select the student that has lowest GTA 
select concat(firstname,' ',lastname) as "Full Name",
sex as "Gender",
province as "Come From",
phone as "contact",
gpa as "Great Point Average"
from tblstudent
where gpa = (select min(gpa) from tblstudent);




//select the student who not have the teacher incharge id have email hengvongkol and not the parent id PT004
select * from tblstudent
where teacherinchargeid !=(
select teacherid from tblteacher 
where email="hengvongkol@gmail.com")
and province=(
select province from tblparent where parentid='PT0004');



//select the film that leng is greater than average and order by title a-z
use sakila;
select film_id, title, length from film 
where length > (select avg(length) from film )
Order by title asc;




//select province of student that not the come from the teacher that gender Male and gpa more than 2 and less than or equal 5
use test;
select * from tblstudent
where province 
in(select province from tblteacher where sex = "M")
and gpa not in (select gpa from tblstudent where gpa>2 and gpa <= 5);



//select the student student and has teacher incharge who come from Phnom Penh
select * from tblstudent stu
where exists (select * from tblteacher tea
where stu.teacherinchargeid = tea.teacherid and province ="Phnom Penh");



//list teacher who don't have studentincharge
select * from tblteacher tea
where not exists (select * from tblstudent stu
where tea.teacherid = stu.teacherinchargeid);


//select the student who has lowest gpa 
use test;
select concat(firstname,' ', lastname) as "Full Name", 
sex as "Gender", 
province as "Come From",
phone as "Concact" from tblstudent
where gpa = (select MIN(gpa) from tblstudent);


//select student who has teacherincharge
use test;
select stu.studentid as "Student ID",
concat(stu.firstname,' ',stu.lastname) as "Student Name",
stu.sex as "Gender",
stu.subject as "Registered Course",
concat(tea.firstname,' ',tea.lastname) as "Instructor",
stu.email as "Contact"
from tblstudent stu
inner join tblteacher tea on
tea.teacherid = stu.teacherinchargeid and tea.province="Phnom Penh";



use test;
select stu.studentid as "Student ID",
concat(stu.firstname,' ',stu.lastname) as "Student Name",
stu.sex as "Gender",
stu.subject as "Registered Course",
concat(tea.firstname,' ',tea.lastname) as "Instructor",
stu.email as "Contact"
from tblstudent stu, tblteacher tea
where stu.teacherinchargeid = tea.teacherid and tea.province="Phnom Penh";


//select film that has category and has category Animation
use sakila;
select film_category.film_id,
f.description, category.name from film f
inner join film_category on
f.film_id = film_category.film_id
inner join category on category.category_id = film_category.category_id 
where category.name = "Animation";


*****My Solution
use sakila;
select f.film_id as "Film ID",
f.description as "Description"
from film f
inner join film_category fc 
on f.film_id=fc.film_id
inner join category c 
on fc.category_id=c.category_id
where c.name = "Animation";


use sakila;
select f.film_id as "Film ID",
f.description as "Description"
from film f, film_category fc, category c
where f.film_id = fc.film_id and c.category_id=fc.category_id
and c.name="Animation";


****count 
use sakila;
select count(f.film_id)
from film f, film_category fc, category c
where f.film_id = fc.film_id and c.category_id=fc.category_id
and c.name="Animation";













//Create View

use test;
create view studentGPA as
select concat(stu.firstname,' ',stu.lastname) as "Student Name",
stu.subject as "Subject",
stu.gpa as "GPA"
from tblstudent stu;


use test;
select * from studentGPA;




//[view] student who parent as teacher

create view StuJobTeacher as
select stu.studentid,
concat(stu.firstname,' ',stu.lastname) as "Student Name",
stu.subject as "Subject",
stu.gpa as "GPA",
concat(pa.firstname,' ',pa.lastname) as "Parent"
from tblstudent stu inner join tblparent pa
on stu.parentinchargeid = pa.parentid and pa.job = "Teacher";


create view StuJobTeacherGPALessThanOrEqual2 as
select stu.studentid,
concat(stu.firstname,' ',stu.lastname) as "Student Name",
stu.subject as "Subject",
stu.gpa as "GPA",
concat(pa.firstname,' ',pa.lastname) as "Parent"
from tblstudent stu inner join tblparent pa
on stu.parentinchargeid = pa.parentid and pa.job = "Teacher" and stu.GPA <=2;


select concat(stu.firstname,' ',stu.lastname) as "Student Name",
stu.sex as "Gender",
stu.gpa as "GPA",
stu.phone as "Contact"
from tblstudent stu where gpa = (select min(gpa) from tblstudent);


//Using procedure

se test;
DELIMITER //
	CREATE PROCEDURE GetAllStudent()
		BEGIN
			SELECT * FROM tblstudent;
		END //

call GetAllStudent();


//Get student by universtity
use test;
DELIMITER //
create procedure getStudentByUniversity(IN university varchar(40))
	BEGIN
		select * from tblstudent stu where stu.university = university;
	END //
	
call getStudentByUniversity('PNC');





//get student id and get the gpa of that student
DELIMITER //
	create procedure getStudentGPAByID(IN ID varchar(10),OUT gpa int(11))
		BEGIN
			select stu.GPA FROM tblstudent stu where stu.studentid = ID INTO gpa;
		END //


call getStudentGPAByID('ST0007',@gpa);
select @gpa;



//find the related name

use test;
DELIMITER //
	create procedure getStudentLastName(IN lastname varchar(30))
		BEGIN
			select stu.lastname FROM tblstudent stu where stu.lastname like concat('%',lastname,'%');
		END //

call getStudentLastName('Ou');

//find age knowing the born year
DELIMITER //
	create procedure findYourAge(IN bornYear INT)	
		BEGIN
			SELECT YEAR(CURDATE()) - bornYear as YourAge;
		END //
		
call findYourAge(1989);

//trigger
DELIMITER //
	create trigger before_province_update
	BEFORE UPDATE ON tblprovinces
	FOR EACH ROW
		BEGIN
			INSERT INTO tblprovince_backup (provinceid,action,update_at)
			VALUES(OLD.pro_id,'update',CURDATE());
		END //


>>use test;
UPDATE tblprovinces set pro.status = 2 where pro_id =5;



use test;
DELIMITER //
	create trigger after_province_delete
	AFTER DELETE ON tblprovinces
	FOR EACH ROW
		BEGIN
			DELETE FROM tblprovince_delete where pro_id=id; 
		END //


DELETE FROM tblprovinces where pro_id=21;




//

use test;
DELIMITER //
	create trigger after_province_insert
	AFTER INSERT ON tblprovinces
	FOR EACH ROW
		BEGIN
			INSERT INTO tblprovinces_insert(provinceid,command,province_name_eng,province_name_kh,updated_at)
			VALUES(NEW.pro_id,'insert',NEW.pro_en_name,NEW.pro_kh_name,CURDATE());
		END   
		
		
		
		insert into tblprovinces (pro_en_name, pro_kh_name, status);
VALUES ("Phnom Penh", "ភ្នំពេញ", 1);























