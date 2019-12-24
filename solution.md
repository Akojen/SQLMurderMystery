## Getting the witness reports

* `select * from crime_scene_report where type = "murder" and (date between 20180101 and 20180130) and city = "SQL City"`

| date     | type   | description                                                                                                                                                                               | city     |
|----------|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| 20180115 | murder | Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave". | SQL City |

* `select * from person where address_street_name LIKE '%Franklin%' and name LIKE '%Annabel%'`

| id    | name           | license_id | address_number | address_street_name | ssn       |
|-------|----------------|------------|----------------|---------------------|-----------|
| 16371 | Annabel Miller | 490173     | 103            | Franklin Ave        | 318771143 |

* `select * from person where address_street_name like'%Northwestern%' order by address_number desc limit 1`

| id    | name           | license_id | address_number | address_street_name | ssn       |
|-------|----------------|------------|----------------|---------------------|-----------|
| 14887 | Morty Schapiro | 118009     | 4919           | Northwestern Dr     | 111564949 |

* `select * from person where address_street_name like'%Northwestern%' order by address_number desc limit 1`

| person_id | transcript                                                                                                                                                                                                                      |
|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 14887     | I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W". |
| 16371     | I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.                                                                                                           |

## Following the clues

* `select p.name, dl.* from drivers_license as dl join person as p on dl.id = p.license_id where plate_number LIKE '%H42W%'`

| name                | id     | age | height | eye_color | hair_color | gender | plate_number | car_make  | car_model |
|---------------------|--------|-----|--------|-----------|------------|--------|--------------|-----------|-----------|
|  Tushar Chandra     | 183779 | 21  | 65     | blue      | blonde     | female | H42W0X       | Toyota    | Prius     |
|  **Jeremy Bowers**  | 423327 | 30  | 70     | brown     | brown      | male   | 0H42W2       | Chevrolet | Spark LS  |
|  Maxine Whitely     | 664760 | 21  | 71     | black     | black      | male   | 4H42WR       | Nissan    | Altima    |

* `select * from get_fit_now_check_in as cin inner join get_fit_now_member as mem on cin.membership_id = mem.id where check_in_date = 20180109 and membership_id LIKE '48Z%' and mem.membership_status = 'gold'`

| membership_id | check_in_date | check_in_time | check_out_time | id    | person_id | name              | membership_start_date | membership_status |
|---------------|---------------|---------------|----------------|-------|-----------|-------------------|-----------------------|-------------------|
| 48Z7A         | 20180109      | 1600          | 1730           | 48Z7A | 28819     | Joe Germuska      | 20160305              | gold              |
| 48Z55         | 20180109      | 1530          | 1700           | 48Z55 | 67318     | **Jeremy Bowers** | 20160101              | gold              |


## Both clues point to Jeremy Bowers

> Congrats, you found the murderer! But wait, there's more... If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villian behind this crime. If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries. Use this same INSERT statement to check your answer

## Querying the interview with Jeremy Bowers

* `select * from interview where person_id = 67318`

| person_id | transcript                                                                                                                                                                                                                                       |
|-----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 67318     | I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017. |

## Following the clues once more

`select distinct p.name from drivers_license as dl join person as p on dl.id = p.license_id join facebook_event_checkin as fec on p.id = fec.person_id where dl.car_make = 'Tesla' and dl.car_model = 'Model S' and dl.hair_color = 'red' and dl.gender = 'female'`

| name             |
|------------------|
| Miranda Priestly |

> Congrats, you found the brains behind the murder! Everyone in SQL City hails you as the greatest SQL detective of all time. Time to break out the champagne!

🍾
