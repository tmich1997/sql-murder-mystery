<p align="center">
    <img src="https://media.discordapp.net/attachments/1008571020480876554/1203916482661056582/timble20_a_neon_lit_crime_alley_5c626489-1a3b-4f62-ac77-dd794ce2304e.png?ex=65d2d599&is=65c06099&hm=573cad76c6b03f0f28f5a626e355705fc60d215c149586c21eac6b7cada87ba2&=&format=webp&quality=lossless&width=593&height=593">
</p>

<div align="center">
    <h1>Welcome to SQL City</h1>
    <h2>Where Queries Unveil the Mysteries</h2>
</div>

There's been a murder in Savannah! Sorry, couldn't help myself with the Office reference. Let's start again. 

There's been a murder in SQL City and it is up to you to solve it. All you have is your hours of watching Sherlock Holmes and SQL querying skills.

I was introduced to SQL last year and one of the things that really got me to use SQL was solving a murder mystery. Even though I did it last year, my laptop was wiped and I lost the SQL murder mystery database. As an anlayst using SQL is the bare minimum and I wanted to start of my SQL advancement journey in 2024 with solving a murder mystery.

## To Get Started
- **The Murder Mystery Database:** This is an SQLite database that contains all the data you will need to work with. The database and other material that you need can be found on this [GitHub Repo](https://github.com/NUKnightLab/sql-mysteries/tree/master?tab=readme-ov-file).
- **An SQLite Environment:** You can use DBeaver, which is an open source database tool that supports different flavour of SQL like SQLite. I personally ended up using SQLite Studio, as it natively supports SQLite and for this exercise thats all I need.

## Who is the Murderer?
The database has a total of 9-tables:
- `crime_scene_report`
- `drivers_license`
- `facebook_event_checkin`
- `get_fit_now_check_in`
- `get_fit_now_member`
- `income`
- `interview`
- `person`
- `solution`

You may or may not be using all 9-tables to find the solution. The `solution` table is purely for verification only to ensure that you got the correct answer.

### What happened?

#### Step 1: The Initial Inquiry
The initial info given is simply that: there has been a murder, it took place on the 15th January 2018 and it happened in SQL City. Therefore, I queried the `crime_scene_report` table. This is because it has fields that correspond directly with the initial info, and the query looks like this:
````sql
select *
from crime_scene_report
where 
    type = "murder" and
    date = "20180115" and 
    city = "SQL City";
````
Breaking it down to plain english, I am querying the `crime_scene_report` table **WHERE** the `type` of crime is `murder`, the `date` is 15th January 2018 formatted as `20180115` and the `city` is `SQL City`.
The result of this query is:
> [!Tip]
> Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".

From this we have clues to keep going, which are:
- 2 witnesses
- Witness 1, lives at the last house on "Northwestern Dr"
- Witness 2, has a name, Annabel, and lives somewhere on "Franklin Ave"

#### Step 2: Witnesses
With our two witnesses, we don't have a lot of details except for a name and an address. Therefore, to find more details I queried the `person` table for both witnesses, and the query looks like this:
````sql
-- Witness #1
select *
from person
where address_street_name like "Northwestern Dr"
order by address_number;
````
````sql
-- Witness #2
select *
from person
where address_street_name like "Franklin%" and name like "Annabel%";
````
The query for **Witness #1** in plain english means, querying the `person` table **WHERE** the `address_street_name` is **LIKE** `Northwestern Dr`. Finally, it was ordered by, **ORDER BY** based on the `address_number` (smallest to largest), this was beacuse we got a clue saying this person lived on the last house on the address. When we look at the last house number, we get a name:
> [!Tip]
> Morty Schapiro,
> id: 14887,
> license_id: 118009

The query for **Witness #2** in plain english means, querying the `person` table **WHERE** the `address_street_name` is **LIKE** `Franklin%` (the, %, indicates anything that comes after Franklin) **AND** her `name` was `Annabel%`. When we look at the result we get a match for:
> [!Tip]
> Annabel Miller,
> id: 16371,
> license_id: 490173

Along with the two names, we also got their `id` and `license_id` which will become useful in the later sections.

#### Step 3: Witness Testimony
We have more clues regarding the witnesses. We have a table `interview` which we can query to find out more. The query looks like this:
````sql
select *
from interview
where person_id = 14887 or person_id = 16371;
````
The query in plain english means, querying the `interview` table **WHERE** the `person_id` (`person_id` in the `interview` table is the FK which can be linked to `id` in the `person` table) corresponds to the `id` of the two individuals. The **OR** word is being used as I want both results to come back.

The testimony for **Witness #1** is:
> [!Tip]
> I heard a gunshot and then saw a man run out. 
> He had a "Get Fit Now Gym" bag. 
> The membership number on the bag started with "48Z". 
> Only gold members have those bags. 
> The man got into a car with a plate that included "H42W".

The testimony for **Witness #2** is:
> [!Tip]
> I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

From the testimony it is clear that, there are more clues related to **Witness #1**:
- Had a "Get Fit Now Gym"
- Details on the membership number
- Deatils on the gym
- Deatils about the car as well

It is possible to take the testimony of **Wtiness #2** and get to the solution, however, I took clues from **Wtiness #1**.

#### Step 4: Let's Get Fit
A pivotal clue that we got was realted to the **Get Fit Now Gym**. Therefore, I queried the `get_fit_now_member` table, and it looked liket this:
````sql
select * 
from get_fit_now_member
where membership_status = "gold" and id like "%48Z%";
````
In plain english the query means, in the table `get_fit_now_member` I want to filter to `gold` for `membership_status` and `id` to be `%48Z%` (the % on either side means that I am not sure what is before or after it, and it will give anything that contains those characters). The result from this was, I was able to narrow the suspect list to two people which are:
> [!Tip]
> Joe Germuska, person_id: 28819

> [!Tip]
> Jeremy Bowers, person_id: 67318

