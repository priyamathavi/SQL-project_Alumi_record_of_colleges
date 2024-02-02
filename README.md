# SQL-project_Alumi_record_of_colleges
-- 1 question--
use alumni;
-- 2&3question--
select *from college_a_hs;
select*from college_a_se;
select*from college_a_sj;
select* from college_b_hs;
select *from college_b_se;
select *from college_b_sj;

-- 6 th question--
CREATE  OR REPLACE VIEW  college_a_hs_v1
AS
(select* from college_a_hs where Degree is not null and HSDegree is not null and EntranceExam is not null and Institute is not null and Location is not null );
-- 7 TH QUESTION--
CREATE or replace VIEW college_a_se_v
as
(select* from college_a_se where Degree is not null and  Organization is not null and location is not null);
-- -- 8 th question--
CREATE VIEW  college_a_sj_v1 
AS
(select * from college_a_sj where RollNO is not null and LastUpdate is not null and degree is not null and Organization is not null and designation is not null and location is not null);
-- 9th question--
CREATE VIEW college_b_hs_v
AS
(select* from college_b_hs where Branch is not null and Degree is not null and  HSDegree is not null and EntranceExam is not null );
-- 10 th question--
CREATE VIEW  college_b_sj_v
AS
(select* from college_b_sj where Branch is not null and Degree is not null and Organization is not null and Designation is not null);
-- 11 th question--
CREATE VIEW college_b_se_v
AS
(select* from college_b_se where Branch is not null and Organization is not null and Location is not null);

-- 12 th question --
delimiter $$
CREATE  PROCEDURE names_hs1()

BEGIN
select lower(Name),lower(FatherName),LOWER(MotherName) from college_a_hs_v1;

END $$
delimiter ;

call names_hs1();
delimiter $$
CREATE  PROCEDURE names_se1()

BEGIN
select lower(Name),lower(FatherName),LOWER(MotherName) from college_a_se_v;

END $$
delimiter ;

call names_se1();
delimiter $$
CREATE  PROCEDURE names_sj1()

BEGIN
select lower(Name),lower(FatherName),LOWER(MotherName) from college_a_sj_v1;

END $$
delimiter ;

call names_sj1();
delimiter $$
CREATE  PROCEDURE names_hs2()

BEGIN
select lower(Name),lower(FatherName),LOWER(MotherName) from college_b_hs_v;

END $$
delimiter ;

call names_hs2();
delimiter $$
CREATE  PROCEDURE names_se2()

BEGIN
select lower(Name),lower(FatherName),LOWER(MotherName) from college_b_se_v;

END $$
delimiter ;

call names_se2();
delimiter $$
CREATE  PROCEDURE names_sj2()

BEGIN
select lower(Name),lower(FatherName),LOWER(MotherName) from college_b_sj_v;

END $$
delimiter ;

call names_sj2();
-- 14 question --
use alumni;
select * from college_a_hs ;
select * from college_a_se ;
select * from college_a_sj;
DELIMITER $$
CREATE PROCEDURE get_name_collegeA(
inout Name1 text(160000)
)
BEGIN
declare finished INT DEFAULT 0;
declare namelist varchar(50)default'';

declare namedetail cursor for select Name from college_a_hs union select Name from college_a_se union select name from  college_a_sj;
declare CONTINUE HANDLER FOR NOT FOUND SET FINISHED=1;
open namedetail;
label:loop
fetch namedetail into namelist;
if FINISHED=1 then
leave label;
END IF;
set Name1 =concat(namelist,',',Name1);

end loop label;
close  namedetail;
select namelist;
end $$
DELIMITER ;
DROP PROCEDURE get_name_collegeA;
set @Name1='';

call get_name_collegeA(@Name1);
select @Name1;
-- 15question--
use alumni;
select * from college_a_hs ;
select * from college_a_se ;
select * from college_a_sj;
DELIMITER $$
CREATE PROCEDURE get_name_collegeB(
inout Name1 text(160000)
)
BEGIN
declare finished INT DEFAULT 0;
declare namelist varchar(50)default'';

declare namedetail cursor for select Name from college_b_hs union select Name from college_b_se union select name from  college_b_sj;
declare CONTINUE HANDLER FOR NOT FOUND SET FINISHED=1;
open namedetail;
label:loop
fetch namedetail into namelist;
if FINISHED=1 then
leave label;
END IF;
set Name1 =concat(namelist,',',Name1);

end loop label;
close  namedetail;
select namelist;
end $$
DELIMITER ;
DROP PROCEDURE get_name_collegeB;
set @Name1='';

call get_name_collegeB(@Name1);
select @Name1;

-- 16 question--

select'higher studies' as career_choice,(select count(*) from college_a_hs)
/((select count(*) from college_a_hs)
+(select count(*) from college_a_se)
+(select count(*) from college_a_sj))*100 collegeA_percentage,
(select count(*) from college_b_hs)
/((select count(*) from college_b_se)
+(select count(*) from college_b_sj)
+(select count(*) from college_b_hs))*100 college_b_percentage
union
select'self employed' as career_choice,(select count(*) from college_a_se)
/((select count(*) from college_a_hs)
+(select count(*) from college_a_se)
+(select count(*) from college_a_sj))*100 collegeA_percentage,
(select count(*)from college_b_se)
/((select count(*) from college_b_se)
+(select count(*) from college_b_hs)
+(select count(*) from college_b_sj))*100 college_b_percentage
union
select'sevice job' as career_choice,(select count(*)from college_a_sj)
/((select count(*) from college_a_hs)
+(select count(*) from college_a_se)
+(select count(*) from college_a_sj))*100 college_A_percentage,
(select count(*) from college_b_sj)
/((select count(*) from college_b_hs)
+(select count(*) from college_b_se)
+(select count(*) from college_b_sj))*100 college_b_percentage

