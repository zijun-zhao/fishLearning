
### This file will record all problems I have encountered during studying the course W4111 in 2020 Spring.
* Prof. Donald F. Ferguson's page for this course [GitHub home page](https://donald-f-ferguson.github.io/IntroToDatabases/)
* Prof. Donald F. Ferguson's [Repository/Project](https://github.com/donald-f-ferguson/IntroToDatabases)

### Table of Contents

1. [Lecture1&2-24Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture1&2_Intro&Overview.md)
2. [Lecture3-31Jan](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture3.md)
3. [Lecture4-7Feb](#my-second-title)
4. [Lecture5-14Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lecture5_ERModel_SQL.md)
5. [Lecture6-21Feb](https://github.com/zijun-zhao/fishLearning/blob/master/COMS4111/Lec6_RelationalAlgebra.md)

## 14 Feb 2020
1. Lecture 5
	* "" treat like a string
	* '' treat like a string
	* ` ` it is not a string, it is table or database with weird thing in it. Will not decompose it.
 2. In postman, use GET www.cnn.com is like select the web. Browser knows how to handle those information.
 
 3. All databases have a set of common conceopts. They differ in exactly how they sercice and the detials underneath them. But in the top level, all databases have the same basic behavior including the web, the basic bbehaviours:
  * Object, row, tuple, sets of them that are related.
 
 
 
 
 
 
 
 
## 20 Feb 2020
----------
1. Prof. Ferguson often follows the following sequence because he found entering all of the details on a physical model in a tool to be too tedious and just creating DDL is easier.
	* Get agreement on the conceptual model.
	* Get agreement on the logical model.
	* Use DDL and database tools like MySQL Workbench to define the schema.
	* Use the reverse engineering tool to produce the explanatory diagram.
	
## 21 Feb 2020
----------
1. Data cleanup for column with type int.
	* According to Prof. Ferguson, the reason why we cannot use select * all from table_a where column_a = '' to findout the null row is that, if **The column is of type INT. MySQL tries to cast "" to an INT, which is 0.**
	* **If there were a "" in the CSV file you tried to import in an INT column, the import would usually fail. When you imported the columns were all set to integers and there were no "".**

2. When using group by towards columnB, and show other column, like column A, the result will only select the first one of columnA's row name, for example
	```sql
	%sql select id, sceneStart, count(*) from w4111gotsolutionclean.scenes group by sceneStart limit 10
	```
	here the id will be the first id beloning to each sceneStart

3. Why we create foreign keys:
*  According to Prof. Ferguson, 
	> side effect of foreign keys is that the database creates indexes that speed up queries. People tend to add information to databases over time. Foreign keys prevent some integrity errors on update.

4. When setting foreign key, as long as the reference is UNIQUE you should be good.
5. We need to create index before creating foreign key
	> MySQL requires indexes on foreign keys and referenced keys so that foreign key checks can be fast and not require a table scan. In the **referencing table**, there must be an index where the foreign key columns are listed as the first columns in the same order. 
	
6. a foreign key must reference either the primary key or a unique key of the parent table.

## 22 Feb 2020
----------
1. A candidate key should be unique and not null. A primary key is just a randomly chosen candidate key

2. text cannot be a primary key, varchar can.

3. 
4. natural join： two tables have same columns, stip rows has same column value.
	* For example, 
	```
	select * from scenes_characters 
	join episodes using(seasonNum, episodeNum)
	```
	* Using is another way of saying 
	```
	select * from scenes_characters
	join episodes 
	on episodes.espisodeNum = scenes_characters.espisodeNum AND
	episodes.seasonNum = scenes_characters.seasonNum
	```
	* Using "on" can even do crazy operation, the following code do:
		* Have two rows, if they have the same episodeNumber, but if the episode's seasonNum bigger than scenes_characters, stippen them together.
		```
		select * from scenes_characters
		join episodes 
		on episodes.espisodeNum = scenes_characters.espisodeNum AND
		episodes.seasonNum > scenes_characters.seasonNum
		```
4. Equally join：
	* If all the conditions are equal, and the boolean operation is and, then it is called equal join. Equally join is like a natural join, only the column name are not the same
	```
	select * from scenes_characters
	join episodes 
	on episodes.espisodeNum = scenes_characters.espisodeNum AND
	episodes.seasonNum >= scenes_characters.seasonNum
	```
	
5. Dangeours natural join: The most dangeours is that two tables have same column name but refers to different thing

6. On in join condition is similar to where. But afer on you can use where.

	* AN example of natural join
	```
	select * from scenes_characters
	join episodes 
	on episodes.espisodeNum = scenes_characters.espisodeNum AND
	episodes.seasonNum = scenes_characters.seasonNum
	where sceneNum = 1
	```
		* Where: which one in the result that I want
7. Outer join:
	* right left(1:06:20)
	* If I want to make sure all values in the left table 

	* AN example: even if the characterName in the scens did not match anything in scene, still keep te record.
	```
	select * from characters 
	left join scenes_characters using(characterName)
	on characters.characterName = event.name;
	```
	
8. Do you have the danger when having an accurate data
	* In principle, some database engine will rematerilize the (14:6)
	* Relational database is good at reading
	* view is a table that is computed. So if you update the view, you can write through the view. But there(1:49)
	* Originally, you could not update thourhg views. Prof. Ferguson do not suggest to do that, although we can do it.
	
9. So we learn that a candidate key can have null, but can a foreign key has null?


10. How to Join two tables by multiple columns in SQL?
	* Use inner join
11. Every derived table must have its own alia
	> Every derived table (AKA sub-query) must indeed have an alias. I.e. *each query in brackets must be given an alias* (***AS whatever***), which can the be used to refer to it in the rest of the outer query.
	```
	SELECT ID FROM (
		SELECT ID, msisdn FROM (
			SELECT * FROM TT2
			) AS T
		) AS T
	```
	* For example, the code below is not correct
	```sql
	%%sql
	select * from
	(
	    select location, characterName, count(*) as count from W4111GotSolutionclean.scenes_characters as a
	    inner join W4111GotSolutionclean.scenes as b
	    ON a.seasonNum = b.seasonNum and
	    a.episodeNum=b.episodeNum and 
	    a.sceneNo=b.sceneNo
	    group by location, characterName
	    order by count desc
	) 
	limit 6
	```
	* We should use
	```
	%%sql
	select * from
	(
	    select location, characterName, count(*) as count from W4111GotSolutionclean.scenes_characters as a
	    inner join W4111GotSolutionclean.scenes as b
	    ON a.seasonNum = b.seasonNum and
	    a.episodeNum=b.episodeNum and 
	    a.sceneNo=b.sceneNo
	    group by location, characterName
	    order by count desc
	) as t where t.count>=50
	```




12. Homework: Plan to connect LOCATIONS with SCENES using LOCATION's PK, but how to do that?

13. The query below displays scenes and lengths if you represent the sceneStart and sceneEnd as type time.
```
SELECT *,
	timediff(sceneEnd, sceneStart) as scene_duration
    	from W4111GoTSolutionClean.scenes;
```

14. Test whether we can convert a column to data
	```
	select 
	*,
	cast(episodeAirDate as Date) as testd
	from database.episodes
	```
	* If it works, we can transfer the type to data
	```
	ALTER TABLE `w4111gotsolutionclean`.`episodes` 
	CHANGE COLUMN `episodeAirDate` `episodeAirDate` DATE NOT NULL ;
	```
	* We can use the property of type date
	```
	select *, year(episodeAirDate) from episodes;
	```
	* We can use the property of type date
	```
	select *, dayofweek(episodeAirDate) from episodes;
	```
15. SeasonNum, episodeNum, sceneNo from scenes, we can set PK to be a combination of these three.
	* Go to table episodes, make an index, called ep_to_s, choosing two columns seasonNum and episodeNum
	```
	ALTER TABLE `w4111gotsolutionclean`.`episodes` 
	ADD UNIQUE INDEX `ep_to_s` (`seasonNum` ASC, `episodeNum` ASC) VISIBLE;
	```
	* Go to scenes, add foreign keys s_to_episodes
	```
	ALTER TABLE `w4111gotsolutionclean`.`scenes` 
	ADD CONSTRAINT `s_to_episodes`
	  FOREIGN KEY (`episodeNum` , `seasonNum`)
	  REFERENCES `w4111gotsolutionclean`.`episodes` (`episodeNum` , `seasonNum`)
	  ON DELETE NO ACTION
	  ON UPDATE NO ACTION;
	```
## 23 Feb 2020
----------
1. Error "" occurs during HW1
	* When adding a foreign key in table B(scenes) refering to table A(episodes), knowing that the foreign has multiple columns
		* First create a combination of unique key in A
		```
		%sql ALTER TABLE `w4111gotsolutionclean`.`episodes` ADD UNIQUE `unique_index`(`episodeNum` , `seasonNum`);
		```
		* Go to b, add the corresponding foreign key
		```
		%%sql
		ALTER TABLE `w4111gotsolutionclean`.`scenes` 
		ADD CONSTRAINT `s_to_episodes`
		  FOREIGN KEY (`episodeNum` , `seasonNum`)
		  REFERENCES `w4111gotsolutionclean`.`episodes` (`episodeNum` , `seasonNum`)
		  ON DELETE NO ACTION
		  On UPDATE NO ACTION;	
		```
2. How to show ALL NAMES OF view in MYSQL?
	```
	W4111GotSolutionclean
	```
3. How to show column name of view in mysql?
	```
	%sql SHOW COLUMNS FROM W4111GotSolutionclean.actors_episodes;
	```
4. How to join two sql tables when the common column has different names but information is the same in both tables
	```
	
5. Show all the databases
	```
	SHOW SCHEMAS;
	```

6. Find rows which have nan value in **Pandas**
	```
	df[df.isnull().any(axis=1)]
	```
7. Drop columns in pandas
```
df.drop(['character_name_1', 'character_name_2'], axis=1,inplace = True)
```

8. if i have the value for x, the corresponding y must unique. But it will be okay if x is null. We cannot chave a email that does not exist, but student may not have email or we may not know
