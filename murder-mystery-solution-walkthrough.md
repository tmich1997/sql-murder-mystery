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

#### Step 2: Witness Testimony
With our two witnesses, we don't have a lot of details except for a name and an address. Therefore, to find more details I queried the `person` table for both witnesses, and the query looks like this:
````sql
select *
from person
where address_street_name like "Northwestern Dr"
order by address_number;
````
This was the query for witness 1. Breaking down the query into plain english means, querying the `person` table **WHERE** the `address_street_name` is **LIKE** `Northwestern Dr`. Finally, it was ordered by, **ORDER BY** based on the `address_number` (smallest to largest), this was beacuse we got a clue saying this person lived on the last house on the address. When we look at the last house number, we get a name, **Morthy Schapiro**



