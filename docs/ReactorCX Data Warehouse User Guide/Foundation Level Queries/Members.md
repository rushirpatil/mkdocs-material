# Members Data

### Introduction

members_data refers to the segment of a program dedicated to
understanding and engaging with members_data who actively participate in the
program. It focuses on analyzing member behavior, preferences, and
interactions to tailor personalized experiences that enhance loyalty. By
analyzing member data and preferences, the program can deliver
personalized offers such as discounts, promotions, or bonus points on
products or services that align with individual interests and purchase
history.

### Comprehensive Insights of members_data

#### **employee_ids**

A emp_id is a unique identifier associated with a member and it is
used to identify a member during a transaction. A member could have
multiple emp_ids associated with him/her. A emp_id could be a
members_datahip card number, phone number, email, Facebook ID, twitter
handler, etc.

**LIST OF TABLES:** The table employee_ids is employed to retrieve
the relevant fields

-   employee_ids

Loyalty information can be retrieved from the database in a few
different ways, as listed below.

* *How to retrieve member loyalty information based on emp_id ?*

```
SELECT emp_id,
cust_id,
emp_id_name,
status,
is_primary
FROM schema_name.employee_ids
WHERE delete_flag <> 'Y' -- To get only non-deleted records from employee_ids
AND emp_id = '6898566527c0484f1ff1c' -- To filter based on emp_id
;
```

**<u>Sample Data:</u>**

| **emp_id**            | **cust_id**            | **emp_id_name** | **status** | **is_primary** |
|-----------------------|------------------------|-----------------|------------|----------------|
| 6898566527c0484f1ff1c | 94568566ba0744e7d65603 | CardID          | Active     | false          |


* *How to retrieve emp_ids associated with a member based on
    cust_id value?*

```
SELECT cust_id,
emp_id,
emp_id_name,
status,
is_primary
FROM schema_name.employee_ids
WHERE delete_flag <> 'Y' -- To get only non-deleted records from employee_ids
AND cust_id = '94568566ba0744e7d65603' -- To filter based on cust_id
;
```

**<u>Sample Data:</u>**

| **cust_id**            | **emp_id**    | **emp_id_name** | **status**  | **is_primary** |
|------------------------|---------------|-----------------|-------------|----------------|
| 94568566ba0744e7d65603 | hpp1758799    | CardID          | Lost_Stolen | false          |
| 94568566ba0744e7d65603 | kft3720300098 | CardID          | Active      | true           |
| 94568566ba0744e7d65603 | 815htr46904   | AlternateID     | Active      | true           |


* *How to retrieve total number of cards for each emp_id_name?*

```
SELECT emp_id_name,
COUNT(*) AS total_cards
FROM schema_name.employee_ids
WHERE delete_flag <> 'Y' -- To get only non-deleted records from employee_ids
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **emp_id_name** | **total_cards** |
|-----------------|-----------------|
| AlternateID     | 5,651           |
| CardID          | 33,879          |


* *How to retrieve total number of cards for each card_status?*

```
SELECT status AS card_status,
COUNT(*) AS total_cards
FROM schema_name.employee_ids
WHERE delete_flag <> 'Y' -- To get only non-deleted records from emp_ids
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **card_status** | **total_cards** |
|-----------------|-----------------|
| Active          | 39,420          |
| Locked          | 110             |


#### **members_data**

The existing consumers who register for the programs of client are identified as members_data of Organization.

**LIST OF TABLES:** The below mentioned fields can be retrieved by performing joins across the indicated tables.

-  members_data

-  employee_ids

members_data details data can be retrieved in a few different ways, as listed
below.

* *How to retrieve members_data based on emp_id?*

```
SELECT members_data.id AS cust_id,
employee_ids.emp_id,
members_data.member_status,
members_data.enrollment_date
FROM schema_name.members_data
INNER JOIN schema_name.employee_ids
ON members_data.id = employee_ids.cust_id
WHERE members_data.delete_flag <> 'Y'-- To get only non-deleted records from members_data
AND emp_id = 'a2g30k20098' -- To filter based on emp_id
;
```

**<u>Sample Data:</u>**

| **cust_id**         | **emp_id**  | **member_status** | **enrollment_date**    |
|---------------------|-------------|-------------------|------------------------|
| 8768a9965fhf45d4051 | a2g30k20098 | Active            | 2004-12-17 05:00:00+00 |


* *How to retrieve members_data info based on cust_id?*

```
SELECT id AS cust_id,
member_status,
enrollment_date
FROM schema_name.members_data
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members_data
AND id = '90701a99fshj7245d4051'-- To filter based on id
;
```

**<u>Sample Data:</u>**

| **cust_id**              | **member_status** | **enrollment_date**    |
|--------------------------|-------------------|------------------------|
| 9560c0df1a9sjcu7245d4051 | Active            | 2004-12-17 05:00:00+00 |

* *How to retrieve the list of members_data enrolled on a certain enrollment_date?*

```
SELECT id AS cust_id,
member_status,
enrollment_date,
enrollment_source
FROM schema_name.members_data
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members_data
AND enrollment_date ::DATE BETWEEN '2023-07-31' AND '2023-09-08' -- To filter by start and end dates of the enrollment_date
;
```

**<u>Sample Data:</u>**

| **cust_id**              | **member_status** | **enrollment_date**    | **enrollment_source** |
|--------------------------|-------------------|------------------------|-----------------------|
| 907698368hafca147f6c86ee | PreEnrolled       | 2023-07-31 00:00:00+00 | HPNS Lookup           |
| 457698368hafca147f6c86ee | Active            | 2023-07-31 00:00:00+00 | HPNS Lookup           |
| 6782698368hafca147f6c86e | Active            | 2023-07-31 00:00:00+00 | UCD                   |


*  *How to retrieve total number of members_data enrolled in each month based on enrollment_date in a given year ?*

```
SELECT TO_CHAR(enrollment_date::DATE,'mon-yyyy') AS enrollment_date,
COUNT(*) AS total_members_data
FROM schema_name.members_data
WHERE DATE_PART(year,enrollment_date::DATE) BETWEEN '2022' AND '2023' -- To filter based on enrollment_date
AND delete_flag <> 'Y' -- To get only non-deleted records from members_data
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **enrollment_date** | **total_members_data** |
|---------------------|------------------------|
| Jan-22              | 1,866                  |
| Jan-23              | 3,733                  |
| Feb-22              | 4,881                  |
| Feb-23              | 2,759                  |

* *How to retrieve total number of members_data enrolled via different enrollment_source?*

```
SELECT enrollment_source,
COUNT(*) AS total_members_data
FROM schema_name.members_data
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members_data
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **enrollment_source** | **total_members_data** |
|-----------------------|------------------------|
| HPNS Conversion       | 23,958                 |
| HPNS Lookup           | 20,256                 |
| UCD                   | 25,527                 |
| Unknown               | 21,402                 |

* *How to retrieve total number of members_data enrolled using different enrollment_channel’s ?*

```
SELECT enrollment_channel,
COUNT(*) AS total_members_data
FROM schema_name.members_data
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members_data
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **enrollment_channel** | **total_members_data** |
|------------------------|------------------------|
| WCON                   | 24,532                 |
| WWEB                   | 12,978                 |
| ORET                   | 14,222                 |

* *How to retrieve total number of members_data for each member_status?*

```
SELECT member_status,
COUNT(*) AS total_members_data
FROM schema_name.members_data
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members_data
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **member_status** | **total_members_data** |
|-------------------|------------------------|
| PreEnrolled       | 18,409                 |
| Terminated        | 113                    |
| Active            | 21,008                 |

* *How to retrieve number of members_data deleted ?*

```
SELECT delete_flagAS delete_status,
COUNT(*) AS total_members_data
FROM schema_name.members_data
WHERE delete_flag= 'Y' -- To get only deleted records from members_data
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **delete_status** | **total_members_data** |
|-------------------|------------------------|
| Y                 | 1,785,679              |


#### **member_wallets**
wallets are virtual containers or buckets that contain the points earned
by the member. A member can have multiple wallets, each wallet can contain
points earned for different transactions.

**LIST OF TABLES:** The table member_wallets is employed to retrieve the relevant fields

- member_wallets

- employee_ids

wallet and their balance data can be retrieved from the database in a few
different ways, as listed below.

* *How to retrieve all the wallets of a member using emp_id?*

```
SELECT employee_ids.emp_id,
member_wallets.wallet_policy_id,
member_wallets.wallet_name,
member_wallets.wallet_balance,
member_wallets.available_balance
FROM schema_name.member_wallets
INNER JOIN schema_name.employee_ids
ON member_wallets.cust_id = employee_ids.cust_id
WHERE emp_id IN ('89771089785fh2') -- To filter based on emp_id
AND member_wallets.delete_flag <> 'Y' -- To get only non-deleted records from member_wallets
;
```

**<u>Sample Data:</u>**

| **emp_id**     | **wallet_policy_id**     | **wallet_name** | **wallet_balance** | **available_balance** |
|----------------|--------------------------|-----------------|--------------------|-----------------------|
| 89771089785fh2 | 6jeg5c2459ca0027fa78d9   | Points          | 2232               | 2232                  |
| 89771089785fh2 | 96019d5c245rda0027fa78da | Body Armor Club | 0                  | 0                     |
| 89771089785fh2 | 6jg19d5c245rha0027fa78dd | Beverage Club   | 4                  | 4                     |


* *How to retrieve all the wallet details of a member based on cust_id?*

```
SELECT cust_id,
wallet_policy_id,
wallet_name,
wallet_balance,
available_balance
FROM schema_name.member_wallets
WHERE cust_id IN ('84576358ccaaffar501a8ab') -- To filter based on cust_id
AND delete_flag <> 'Y' -- To get only non-deleted records from member_wallets
;
```

**<u>Sample Data:</u>**

| **cust_id**             | **wallet_policy_id**   | **wallet_name** | **wallet_balance** | **available_balance** |
|-------------------------|------------------------|-----------------|--------------------|-----------------------|
| 84576358ccaaffar501a8ab | 0079d5c2459cafa78d8uh  | Frito Lay Club  | 0                  | 0                     |
| 84576358ccaaffar501a8ab | 00419d5c2459ca0027fa7  | Points          | 2,232              | 2,232                 |
| 84576358ccaaffar501a8ab | 47919d5c2459ca0027fa78 | Beverage Club   | 4                  | 4                     |


* *How to retrieve the total balance available on each wallet?*

```
SELECT wallet_name,
wallet_policy_id,
SUM(wallet_balance) wallet_balance,
SUM(available_balance) available_balance
FROM schema_name.member_wallets
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_wallets
GROUP BY 1,
2;
```

**<u>Sample Data:</u>**

| **wallet_name**    | **wallet_policy_id**    | **wallet_balance** | **available_balance** |
|--------------------|-------------------------|--------------------|-----------------------|
| US Alto Club 33273 | 6068e13c20c6ker62ef5c5  | 1923               | 1923                  |
| US CarWash Card    | 6282bf77b68e8c0bh47f1ca | 22537              | 22537                 |


* *How to retrieve the total accrued points on each wallet?*

```
SELECT wallet_name,
wallet_policy_id,
SUM(accrued_pts) AS total_accrued_pts
FROM schema_name.member_wallets
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_wallets
GROUP BY 1, 2;
```

**<u>Sample Data:</u>**

| **wallet_name**     | **wallet_policy_id** | **total_accrued_pts** |
|---------------------|----------------------|-----------------------|
| US Pizza Punch Card | 9087913c20c6002683u  | 144132                |
| CA Coke Punch       | 8969bd532a5c65adra6  | 0                     |


* *How to retrieve the total redeemed points on each wallet?*

```
SELECT wallet_name,
wallet_policy_id,
SUM(redeemed_pts) AS total_redeemed_pts
FROM schema_name.member_wallets
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_wallets
GROUP BY 1,
2;
```

**<u>Sample Data:</u>**

| **wallet_name**            | **wallet_policy_id**   | **total_redeemed_pts** |
|----------------------------|------------------------|------------------------|
| US Celsius 11th Free Punch | 9685gf81106c7c5c0024d1 | 111,370                |
| US Pizza 8th Free Punch    | y86h9e376a16b0027a7e8  | 10,594,255             |

* *How to retrieve the total expired points on each wallet?*

```
SELECT wallet_name,
wallet_policy_id,
SUM(expired_points) AS total_expired_points
FROM schema_name.member_wallets
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member wallets
GROUP BY 1, 2;
```

**<u>Sample Data:</u>**

| **wallet_name**         | **wallet_policy_id** | **total_expired_points** |
|-------------------------|----------------------|--------------------------|
| US Coke 11th Free Punch | 693g1106c7c000026890 | 48,562                   |
| CA 7th Coffee Punch     | 628308b5d170200281e1 | 476,247                  |

* *How to retrieve the number of members_data whose wallet balance is not zero?*

```
SELECT wallet_name,
COUNT(DISTINCT cust_id) AS members_data_count
FROM schema_name.member_wallets
WHERE available_balance  <> 0 -- To filter based on available balance
AND delete_flag <> 'Y' -- To get only non-deleted records from member_wallets
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **wallet_name**   | **members_data_count** |
|-------------------|------------------------|
| Monster Java Club | 490,974                |
| Stokers Club      | 27,984                 |
| Tourney Club      | 39,609                 |


#### **member_levels**

member_levels is an attribute associated with a member to define an order between levels by ranking them in order of achievement and categorizing/grouping them based on the achievements of the member.

A member_level that is assigned to a member based on their engagement with the program.

**LIST OF TABLES:** The below mentioned fields can be retrieved by performing joins across the indicated tables.

- member_levels

- employee_ids

member_levels data can be retrieved from the database in a few different ways, as listed below.

_* How to retrieve the member_level details of a member using their emp_id?_

```
SELECT employee_ids.emp_id,
member_levels.member_level_policy_level_id,
member_levels.prev_level_name,
member_levels.level_number,
member_levels.level_name,
member_levels.achieved_on_utc_ts,
member_levels.requal_on_utc_ts
FROM schema_name.member_levels
INNER JOIN schema_name.employee_ids
ON member_levels.cust_id = employee_ids.cust_id
WHERE employee_ids.emp_id IN ('7te28001') -- To filter based on emp_id
AND member_levels.delete_flag <> 'Y' -- To get only non-deleted records from member_levels
;
```

**<u>Sample Data:</u>**

| **emp_id** | **member_level_policy_level_id** | **prev_level_name** | **level_number** | **level_name** | **achieved_on_utc_ts** | **requal_on_utc_ts**       |
|------------|--------------------------|---------------------|------------------|----------------|------------------------|----------------------------|
| 7te28001   | 89066d7328034c0aklreb0f  | Sapphire            | 2                | Pearl          | 2023-01-01 12:08:33+00 | 2025-02-01 07:59:59.999+00 |


_* How to retrieve member member_levels based on cust_id?_

```
SELECT cust_id,
member_level_policy_level_id,
prev_level_name,
level_number,
level_name,
achieved_on_utc_ts,
requal_on_utc_ts
FROM schema_name.member_levels
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_levels
AND cust_id = '907c0df1a996fsg244aw1' -- To filter based on cust_id
;
```

**<u>Sample Data:</u>**

| **cust_id**           | **member_level_policy_level_id** | **prev_level_name** | **level_number** | **level_name** | **achieved_on_utc_ts** | **requal_on_utc_ts**   |
|-----------------------|--------------------------|---------------------|------------------|----------------|------------------------|------------------------|
| 907c0df1a996fsg244aw1 | jkd5b245rt0027fa78d7     |                     | 1                | Welcome        | 2019-01-01 00:00:00+00 | 3000-01-01 00:00:00+00 |

_* How to retrieve total no of member_levels for each level_name?_
```
SELECT level_name,
COUNT(*) AS total_members_data
FROM schema_name.member_levels
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_levels
GROUP BY 1;
```

**<u>Sample Data:</u>**

| level_name   | total_members_data |
|--------------|--------------------|
| Sapphire     | 59,711,897         |
| NOIR - No EC | 2,026              |
| Gold         | 238,532            |

_* How to retrieve total no of member_levels achieved based on achieved_on_utc_ts?_

```
SELECT TO_CHAR(achieved_on_utc_ts::DATE,'mon-yyyy') AS
achieved_on_utc_ts,
COUNT(*) AS total_member_levels_achieved
FROM schema_name.member_levels
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_levels
AND DATE_PART(year,achieved_on_utc_ts :: DATE) BETWEEN '2022' AND '2023' -- To filter based on achieved_on_utc_ts
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **achieved_on_utc_ts** | **total_member_levels_achieved** |
|------------------------|----------------------------------|
| Apr-2022               | 303,484                          |
| Apr-2023               | 296,561                          |
| Aug-2022               | 216,669                          |
| Aug-2023               | 275,699                          |

#### **member_level_histories**

The member_level_histories tracks each member's member_level changes over time.
It includes the assigned member_levels, and dates of assignment. This detailed
record allows for monitoring the progression and status of members_data
within the program, ensuring an accurate historical account of
member_level adjustments.

**LIST OF TABLES:** The aforementioned fields can be retrieved by
performing joins across the indicated tables.

- member_level_histories

- employee_ids

member_level history information can be retrieved from the database in a few
different ways, as listed below.

_* How to retrieve the member_levels assigned to a member based on their emp_id?_

```
SELECT employee_ids.emp_id,
member_level_histories.prev_member_level_level,
member_level_histories.current_member_level_level,
member_level_histories.assign_date
FROM schema_name.member_level_histories mt
INNER JOIN schema_name.employee_ids mli
ON member_level_histories.cust_id = employee_ids.cust_id
WHERE member_level_histories.delete_flag <> 'Y' -- To get only non-deleted records from member_level_histories
AND employee_ids.emp_id IN ('aj28001') -- To filter based on emp_id
;
```

**<u>Sample Data:</u>** 

| **emp_id** | **prev_member_level_level** | **current_member_level_level** | **assign_date**            |
|------------|-----------------------------|--------------------------------|----------------------------|
| aj28001    | Sapphire                    | Sapphire                       | 2016-08-23 13:09:34.593+00 |
| aj28001    | Sapphire                    | Pearl                          | 2023-01-01 12:08:33+00     |

_* How to retrieve the member_level change histories for a member using their cust_id?_

```
SELECT cust_id,
prev_member_level_level,
current_member_level_level,
assign_date
FROM schema_name.member_level_histories
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_level_histories
AND cust_id IN ('79766041drh1a04cdd92a') -- To filter based on cust_id
;
```

**<u>Sample Data:</u>**

| **cust_id**           | **prev_member_level_level** | **current_member_level_level** | **assign_date**            |
|-----------------------|-----------------------------|--------------------------------|----------------------------|
| 79766041drh1a04cdd92a | Sapphire                    | Sapphire                       | 2016-08-23 13:09:34.593+00 |
| 79766041drh1a04cdd92a | Sapphire                    | Pearl                          | 2023-01-01 12:08:33+00     |

_* How to retrieve the member_levels assigned to a member based on assign_date?_

```
SELECT assign_date,
cust_id,
prev_member_level_level,
current_member_level_level
FROM schema_name.member_level_histories
WHERE assign_date::DATE = '2022-08-01' -- To filter based on assign_date
AND delete_flag <> 'Y' -- To get only non-deleted records from member_level_histories
;
```

 **<u>Sample Data:</u>** 

| **assign_date**            | **cust_id**          | **prev_member_level_level** | **current_member_level_level** |
|----------------------------|----------------------|-----------------------------|--------------------------------|
| 2022-08-01 07:00:00+00     | 6588847961325210fc30 | Sapphire                    | Pearl                          |
| 2022-08-01 00:19:04.062+00 | 8962a5d9d03a422f239b | Sapphire                    | Pearl                          |
| 2022-08-01 22:12:31.623+00 | 67843c8c45a043avhf43 | Platinum                    | Platinum - No EC               |
