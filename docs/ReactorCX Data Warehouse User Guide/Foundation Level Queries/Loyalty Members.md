# Loyalty Members

### Introduction

Loyalty Members refers to the segment of a loyalty program dedicated to
understanding and engaging with members who actively participate in the
program. It focuses on analyzing member behavior, preferences, and
interactions to tailor personalized experiences that enhance loyalty. By
analyzing member data and preferences, the program can deliver
personalized offers such as discounts, promotions, or bonus points on
products or services that align with individual interests and purchase
history.

### Comprehensive Insights of Loyalty Members

#### **member_loyalty_ids**

A loyalty_id is a unique identifier associated with a member and it is
used to identify a member during a transaction. A member could have
multiple loyalty_ids associated with him/her. A loyalty_id could be a
membership card number, phone number, email, Facebook ID, twitter
handler, etc.

**LIST OF TABLES:** The table member_loyalty_ids is employed to retrieve
the relevant fields

-   member_loyalty_ids

Loyalty information can be retrieved from the database in a few
different ways, as listed below.

* *How to retrieve member loyalty information based on loyalty_id ?*

```
SELECT loyalty_id,
member_id,
loyalty_id_name,
status,
is_primary
FROM rcx_demo_db.member_loyalty_ids
WHERE delete_flag <> 'Y' -- To get only non-deleted records from
member_loyalty_ids
AND loyalty_id = '55199feed25b57bcb2f27c0484f1ff1c' -- To filter based
on loyalty_id
;
```

**<u>Sample Data:</u>**

| **loyalty_id**                   | **member_id**                    | **loyalty_id_name** | **status** | **is_primary** |
|----------------------------------|----------------------------------|---------------------|------------|----------------|
| 55199feed25b57bcb2f27c0484f1ff1c | 0bc3d8c77fffba0744e9eac007d65603 | CardID              | Active     | false          |


* *How to retrieve loyalty_ids associated with a member based on
    member_id value?*

```
SELECT member_id,
loyalty_id,
loyalty_id_name,
status,
is_primary
FROM rcx_demo_db.member_loyalty_ids
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_loyalty_ids
AND member_id = '6433c0df1a9965f7245d4051' -- To filter based on
member_id
;
```

**<u>Sample Data:</u>**

| **member_id**            | **loyalty_id**  | **loyalty_id_name** | **status**  | **is_primary** |
|--------------------------|-----------------|---------------------|-------------|----------------|
| 6433c0df1a9965f7245d4051 | 773339021758799 | CardID              | Lost_Stolen | false          |
| 6433c0df1a9965f7245d4051 | 773372030020098 | CardID              | Active      | true           |
| 6433c0df1a9965f7245d4051 | 8152746904      | AlternateID         | Active      | true           |


* *How to retrieve total number of cards for each loyalty_id_name?*

```
SELECT loyalty_id_name,
COUNT(*) AS total_cards
FROM rcx_demo_db.member_loyalty_ids
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_loyalty_ids
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **loyalty_id_name** | **total_cards** |
|---------------------|-----------------|
| AlternateID         | 5,651           |
| CardID              | 33,879          |


* *How to retrieve total number of cards for each card_status?*

```
SELECT status AS card_status,
COUNT(*) AS total_cards
FROM rcx_demo_db.member_loyalty_ids
WHERE delete_flag <> 'Y' -- To get only non-deleted records from loyaltyids
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **card_status** | **total_cards** |
|-----------------|-----------------|
| Active          | 39,420          |
| Locked          | 110             |


#### **members**

The existing consumers who register for the loyalty programs of client
are identified as members of RCX.

**LIST OF TABLES:** The below mentioned fields can be retrieved by
performing joins across the indicated tables.

-  members

-  member_loyalty_ids

Members details data can be retrieved in a few different ways, as listed
below.

* *How to retrieve members based on loyalty_id?*

```
SELECT members.id AS member_id,
member_loyalty_ids.loyalty_id,
members.member_status,
members.enrollment_date
FROM rcx_demo_db.members
INNER JOIN rcx_demo_db.member_loyalty_ids
ON members.id = member_loyalty_ids.member_id
WHERE members.delete_flag <> 'Y'-- To get only non-deleted records from members
AND loyalty_id = '773372030020098' -- To filter based on loyalty_id
;
```

**<u>Sample Data:</u>**

| **member_id**            | **loyalty_id**  | **member_status** | **enrollment_date**    |
|--------------------------|-----------------|-------------------|------------------------|
| 6433c0df1a9965f7245d4051 | 773372030020098 | Active            | 2004-12-17 05:00:00+00 |


* *How to retrieve members info based on member_id?*

```
SELECT id AS member_id,
member_status,
enrollment_date
FROM rcx_demo_db.members
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members
AND id = '6433c0df1a9965f7245d4051'-- To filter based on id
;
```

**<u>Sample Data:</u>**

| **member_id**            | **member_status** | **enrollment_date**    |
|--------------------------|-------------------|------------------------|
| 6433c0df1a9965f7245d4051 | Active            | 2004-12-17 05:00:00+00 |

* *How to retrieve the list of members enrolled on a certain enrollment_date?*

```
SELECT id AS member_id,
member_status,
enrollment_date,
enrollment_source
FROM rcx_demo_db.members
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members
AND enrollment_date::DATE BETWEEN '2023-07-31' AND '2023-09-08' -- To filter by start and end dates of the enrollment_date
;
```

**<u>Sample Data:</u>**

| **member_id**                    | **member_status** | **enrollment_date**    | **enrollment_source** |
|----------------------------------|-------------------|------------------------|-----------------------|
| 78c1b29e80a98368daa2ca147f6c86ee | PreEnrolled       | 2023-07-31 00:00:00+00 | HPNS Lookup           |
| 94f9af926ca8574cd83264672d471c0f | Active            | 2023-07-31 00:00:00+00 | HPNS Lookup           |
| 2d60d8e746b9c12c547a713bc58a6509 | Active            | 2023-07-31 00:00:00+00 | UCD                   |


*  *How to retrieve total number of members enrolled in each month based on enrollment_date in a given year ?*

```
SELECT TO_CHAR(enrollment_date::DATE,'mon-yyyy') AS enrollment_date,
COUNT(*) AS total_members
FROM rcx_demo_db.members
WHERE DATE_PART(year,enrollment_date::DATE) BETWEEN '2022' AND '2023' -- To filter based on enrollment_date
AND delete_flag <> 'Y' -- To get only non-deleted records from members
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **enrollment_date** | **total_members** |
|---------------------|-------------------|
| Jan-22              | 1,866             |
| Jan-23              | 3,733             |
| Feb-22              | 4,881             |
| Feb-23              | 2,759             |

* *How to retrieve total number of Members enrolled via different enrollment_source?*

```
SELECT enrollment_source,
COUNT(*) AS total_members
FROM rcx_demo_db.members
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **enrollment_source** | **total_members** |
|-----------------------|-------------------|
| HPNS Conversion       | 23,958            |
| HPNS Lookup           | 20,256            |
| UCD                   | 25,527            |
| Unknown               | 21,402            |

* *How to retrieve total number of Members enrolled using different enrollment_channel’s ?*

```
SELECT enrollment_channel,
COUNT(*) AS total_members
FROM rcx_demo_db.members
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **enrollment_channel** | **total_members** |
|------------------------|-------------------|
| WCON                   | 24,532            |
| WWEB                   | 12,978            |
| ORET                   | 14,222            |

* *How to retrieve total number of members for each member_status?*

```
SELECT member_status,
COUNT(*) AS total_members
FROM rcx_demo_db.members
WHERE delete_flag <> 'Y' -- To get only non-deleted records from members
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **member_status** | **total_members** |
|-------------------|-------------------|
| PreEnrolled       | 18,409            |
| Terminated        | 113               |
| Active            | 21,008            |

* *How to retrieve number of members deleted ?*

```
SELECT delete_flagAS delete_status,
COUNT(*) AS total_members
FROM rcx_sei_prod_db.members
WHERE delete_flag= 'Y' -- To get only deleted records from members
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **delete_status** | **total_members** |
|-------------------|-------------------|
| Y                 | 1785679           |


#### **member_purses**
Purses are virtual containers or buckets that contain the points earned
by the member. A member can have multiple purses, each purse can contain
points earned for different transactions.

**LIST OF TABLES:** The table member_purses is employed to retrieve the
relevant fields

- member_purses

- member_loyalty_ids

Purse and their balance data can be retrieved from the database in a few
different ways, as listed below.

* *How to retrieve all the purses of a member using loyalty_id?*

```
SELECT member_loyalty_ids.loyalty_id,
member_purses.purse_policy_id,
member_purses.purse_name,
member_purses.purse_balance,
member_purses.available_balance
FROM rcx_demo_db.member_purses
INNER JOIN rcx_demo_db.member_loyalty_ids
ON member_purses.member_id = member_loyalty_ids.member_id
WHERE loyalty_id IN ('773371089785182') -- To filter based on loyalty_id
AND member_purses.delete_flag <> 'Y' -- To get only non-deleted records
from member_purses
;
```

**<u>Sample Data:</u>**

| **loyalty_id**  | **purse_policy_id**      | **purse_name**       | **purse_balance** | **available_balance** |
|-----------------|--------------------------|----------------------|-------------------|-----------------------|
| 773371089785182 | 64019d5c2459ca0027fa78d9 | SPWY Points          | 2232              | 2232                  |
| 773371089785182 | 64019d5c2459ca0027fa78da | SPWY Body Armor Club | 0                 | 0                     |
| 773371089785182 | 64019d5c2459ca0027fa78dd | SPWY Beverage Club   | 4                 | 4                     |


* *How to retrieve all the purse details of a member based on member_id?*

```
SELECT member_id,
purse_policy_id,
purse_name,
purse_balance,
available_balance
FROM rcx_demo_db.member_purses
WHERE member_id IN ('6467358ccaaf25501b15a8ab') -- To filter based on member_id
AND delete_flag <> 'Y' -- To get only non-deleted records from member_purses
;
```

**<u>Sample Data:</u>**

| **member_id**            | **purse_policy_id**      | **purse_name**      | **purse_balance** | **available_balance** |
|--------------------------|--------------------------|---------------------|-------------------|-----------------------|
| 6467358ccaaf25501b15a8ab | 64019d5c2459ca0027fa78d8 | SPWY Frito Lay Club | 0                 | 0                     |
| 6467358ccaaf25501b15a8ab | 64019d5c2459ca0027fa78d9 | SPWY Points         | 2232              | 2232                  |
| 6467358ccaaf25501b15a8ab | 64019d5c2459ca0027fa78dd | SPWY Beverage Club  | 4                 | 4                     |


* *How to retrieve the total balance available on each purse?*

```
SELECT purse_name,
purse_policy_id,
SUM(purse_balance) purse_balance,
SUM(available_balance) available_balance
FROM rcx_demo_db.member_purses
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_purses
GROUP BY 1,
2;
```

**<u>Sample Data:</u>**

| **purse_name**                 | **purse_policy_id**      | **purse_balance** | **available_balance** |
|--------------------------------|--------------------------|-------------------|-----------------------|
| US 2022P7 Alto Pods Club 33273 | 630ec8e13c20c600262ef5c5 | 1923              | 1923                  |
| US 2022P6 CarWash Punch Card   | 62f5bf77b68e8c002847f1ca | 22537             | 22537                 |


* *How to retrieve the total accrued points on each purse?*

```
SELECT purse_name,
purse_policy_id,
SUM(accrued_pts) AS total_accrued_pts
FROM rcx_demo_db.member_purses
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_purses
GROUP BY 1,
2;
```

**<u>Sample Data:</u>**

| **purse_name**                  | **purse_policy_id**      | **total_accrued_pts** |
|---------------------------------|--------------------------|-----------------------|
| US 2022P6 Pizza Club Punch Card | 630ec8e13c20c600262ef5c3 | 144132                |
| CA 2019P2 Coke Punch            | 5d80bd532a5c65010a678ce2 | 0                     |


* *How to retrieve the total redeemed points on each purse?*

```
SELECT purse_name,
purse_policy_id,
SUM(redeemed_pts) AS total_redeemed_pts
FROM rcx_demo_db.member_purses
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_purses
GROUP BY 1,
2;
```

**<u>Sample Data:</u>**

| **purse_name**                    | **purse_policy_id**      | **total_redeemed_pts** |
|-----------------------------------|--------------------------|------------------------|
| US 2023P3 Celsius 11th Free Punch | 642f81106c7c5c00268904d1 | 111370                 |
| US 2024P1 Pizza 8th Free Punch    | 658ce9e376a16b0027a7e185 | 10594255               |

* *How to retrieve the total expired points on each purse?*

```
SELECT purse_name,
purse_policy_id,
SUM(expired_points) AS total_expired_points
FROM rcx_demo_db.member_purses
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member purses
GROUP BY 1,
2;
```

**<u>Sample Data:</u>**

| **purse_name**                 | **purse_policy_id**      | **total_expired_points** |
|--------------------------------|--------------------------|--------------------------|
| US 2023P3 Coke 11th Free Punch | 642f81106c7c5c00268904d2 | 48562                    |
| CA 2022P3 7th Coffee Punch     | 628308b5d172b200281e1d70 | 476247                   |

* *How to retrieve the number of members whose purse balance is not zero?*

```
SELECT purse_name,
COUNT(DISTINCT member_id) AS members_count
FROM rcx_demo_db.member_purses
WHERE available_balance  <> 0 -- To filter based on available balance
AND delete_flag <> 'Y' -- To get only non-deleted records from member_purses
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **purse_name**         | **members_count** |
|------------------------|-------------------|
| SPWY Monster Java Club | 490,974           |
| SPWY Stokers Club      | 27,984            |
| SPWY Tourney Club      | 39,609            |


#### **member_tiers**

Tiers is an attribute associated with a member to define an order
between levels by ranking them in order of achievement and
categorizing/grouping them based on the achievements of the member.

A tier that is assigned to a member based on their engagement with the
loyalty program.

**LIST OF TABLES:** The below mentioned fields can be retrieved by
performing joins across the indicated tables.

- member_tiers

- member_loyalty_ids

Tiers data can be retrieved from the database in a few different ways,
as listed below.

_* How to retrieve the tier details of a member using their loyalty_id?_

```
SELECT member_loyalty_ids.loyalty_id,
member_tiers.tier_policy_level_id,
member_tiers.prev_level_name,
member_tiers.level_number,
member_tiers.level_name,
member_tiers.achieved_on_utc_ts,
member_tiers.requal_on_utc_ts
FROM rcx_demo_db.member_tiers
INNER JOIN rcx_demo_db.member_loyalty_ids
ON member_tiers.member_id = member_loyalty_ids.member_id
WHERE member_loyalty_ids.loyalty_id IN ('70628001') -- To filter based on loyalty_id
AND member_tiers.delete_flag <> 'Y' -- To get only non-deleted records from member_tiers
;
```

**<u>Sample Data:</u>**

| **loyalty_id** | **tier_policy_level_id** | **prev_level_name** | **level_number** | **level_name** | **achieved_on_utc_ts** | **requal_on_utc_ts**       |
|----------------|--------------------------|---------------------|------------------|----------------|------------------------|----------------------------|
| 70628001       | 626a6d7328034c0263eb900f | Sapphire            | 2                | Pearl          | 2023-01-01 12:08:33+00 | 2025-02-01 07:59:59.999+00 |


_* How to retrieve member tiers based on member_id?_

```
SELECT member_id,
tier_policy_level_id,
prev_level_name,
level_number,
level_name,
achieved_on_utc_ts,
requal_on_utc_ts
FROM rcx_demo_db.member_tiers
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_tiers
AND member_id = '6433c0df1a9965f7245d4051' -- To filter based on member_id
;
```

**<u>Sample Data:</u>**

| **member_id**            | **tier_policy_level_id** | **prev_level_name** | **level_number** | **level_name** | **achieved_on_utc_ts** | **requal_on_utc_ts**   |
|--------------------------|--------------------------|---------------------|------------------|----------------|------------------------|------------------------|
| 6433c0df1a9965f7245d4051 | 64019d5b2459ca0027fa78d7 |                     | 1                | Welcome        | 2019-01-01 00:00:00+00 | 3000-01-01 00:00:00+00 |

_* How to retrieve total no of tiers for each level_name?_
```
SELECT level_name,
COUNT(*) AS total_members
FROM rcx_demo_db.member_tiers
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_tiers
GROUP BY 1;
```

**<u>Sample Data:</u>**

| level_name   | total_members |
|--------------|---------------|
| Sapphire     | 59,711,897    |
| NOIR - No EC | 2,026         |
| Gold         | 238,532       |

_* How to retrieve total no of Tiers achieved based on achieved_on_utc_ts?_

```
SELECT TO_CHAR(achieved_on_utc_ts::DATE,'mon-yyyy') AS
achieved_on_utc_ts,
COUNT(*) AS total_tiers_achieved
FROM rcx_demo_db.member_tiers
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_tiers
AND DATE_PART(year,achieved_on_utc_ts :: DATE) BETWEEN '2022' AND '2023' -- To filter based on achieved_on_utc_ts
GROUP BY 1;
```

**<u>Sample Data:</u>**

| **achieved_on_utc_ts** | **total_tiers_achieved** |
|------------------------|--------------------------|
| Apr-2022               | 303,484                  |
| Apr-2023               | 296,561                  |
| Aug-2022               | 216,669                  |
| Aug-2023               | 275,699                  |

#### **member_tier_histories**

The member_tier_histories tracks each member's tier changes over time.
It includes the assigned tiers, and dates of assignment. This detailed
record allows for monitoring the progression and status of members
within the loyalty program, ensuring an accurate historical account of
tier adjustments.

**LIST OF TABLES:** The aforementioned fields can be retrieved by
performing joins across the indicated tables.

- member_tier_histories

- member_loyalty_ids

Tier history information can be retrieved from the database in a few
different ways, as listed below.

_* How to retrieve the tiers assigned to a member based on their loyalty_id?_

```
SELECT member_loyalty_ids.loyalty_id,
member_tier_histories.prev_tier_level,
member_tier_histories.current_tier_level,
member_tier_histories.assign_date
FROM rcx_demo_db.member_tier_histories mt
INNER JOIN rcx_demo_db.member_loyalty_ids mli
ON member_tier_histories.member_id = member_loyalty_ids.member_id
WHERE member_tier_histories.delete_flag <> 'Y' -- To get only non-deleted records from member_tier_histories
AND member_loyalty_ids.loyalty_id IN ('70628001') -- To filter based on loyalty_id
;
```

**<u>Sample Data:</u>** 

| **loyalty_id** | **PREV_TIER_LEVEL** | **CURRENT_TIER_LEVEL** | **ASSIGN_DATE**            |
|----------------|---------------------|------------------------|----------------------------|
| 70628001       | Sapphire            | Sapphire               | 2016-08-23 13:09:34.593+00 |
| 70628001       | Sapphire            | Pearl                  | 2023-01-01 12:08:33+00     |

_* How to retrieve the tier change histories for a member using their member_id?_

```
SELECT member_id,
prev_tier_level,
current_tier_level,
assign_date
FROM rcx_demo_db.member_tier_histories
WHERE delete_flag <> 'Y' -- To get only non-deleted records from member_tier_histories
AND member_id IN ('631810418e16f61a04cdd92a') -- To filter based on member_id
;
```

**<u>Sample Data:</u>**

| **member_id**            | **prev_tier_level** | **current_tier_level** | **assign_date**            |
|--------------------------|---------------------|------------------------|----------------------------|
| 631810418e16f61a04cdd92a | Sapphire            | Sapphire               | 2016-08-23 13:09:34.593+00 |
| 631810418e16f61a04cdd92a | Sapphire            | Pearl                  | 2023-01-01 12:08:33+00     |

_* How to retrieve the tiers assigned to a member based on assign_date?_

```
SELECT assign_date,
member_id,
prev_tier_level,
current_tier_level
FROM rcx_demo_db.member_tier_histories
WHERE assign_date::DATE = '2022-08-01' -- To filter based on assign_date
AND delete_flag <> 'Y' -- To get only non-deleted records from member_tier_histories
;
```

 **<u>Sample Data:</u>** 

| **assign_date**            | **member_id**            | **prev_tier_level** | **current_tier_level** |
|----------------------------|--------------------------|---------------------|------------------------|
| 2022-08-01 07:00:00+00     | 62f454884796efc25210fc30 | Sapphire            | Pearl                  |
| 2022-08-01 00:19:04.062+00 | 62f42a5d9d01683a422f239b | Sapphire            | Pearl                  |
| 2022-08-01 22:12:31.623+00 | 62f443c8c45a043a80ffb943 | Platinum            | Platinum - No EC       |
