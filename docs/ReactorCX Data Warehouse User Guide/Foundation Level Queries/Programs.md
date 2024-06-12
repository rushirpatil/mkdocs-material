## Introduction

Programs are designed to retain customers by inspiring continued
purchases from your brand, rather than competitors. These initiatives
acknowledge and incentivize ongoing interactions with the brand, with
increased engagement translating into greater rewards over time.

Through a program, businesses can extend points or perks to
their clientele, who in turn redeem these rewards for discounts,
complimentary items, special incentives, or exclusive privileges. The
objective is to foster repeat transactions and cultivate a sense of
trust and loyalty between customers and brands.

## Comprehensive Insights of Programs

####  **location_overrides**

location_overrides provide targeted rewards or deals that are
constrained to specific geographic areas, utilizing location-specific
data to customize offerings for customers in those regions

**LIST OF TABLES:** The table for location_overrides is employed to
fetch the corresponding fields mentioned above.

- location_overrides

* location_overrides Details data can be retrieved in a few different ways, as listed below.

* _How to retrieve the location_overrides details?_

```
SELECT location_id,
target_id,
target_type,
optin_flag
FROM schema_name.location_overrides
WHERE delete_flag <> 'Y' --To get only non-deleted records from location_overrides
;
```

| **location_id**          | **target_id**            | **target_type** | **optin_flag** |
|--------------------------|--------------------------|-----------------|----------------|
| 5d039086eb256687e4ddc5fa | 5df8b81a04f3a60172bfbfc6 | Rewardwallet    | true           |
| 5d039087eb256687e4ddcb6f | 5df8b81a04f3a60172bfbfc6 | Rewardwallet    | false          |

#### **programs**

Stores basic information about the program like program_name,
Description.

We have the ability to customize the program according to the client's
specifications. Within the program, we can manage specific
functionalities such as loyalty points, offers, rewards, and more as
needed.

For a program to be functional there are three components: - Rules,
Policies, and Flows. These in turn are based on reference data.

**LIST OF TABLES:** The table for programs is employed to fetch
the corresponding fields mentioned above.

- programs

programs Details data can be retrieved in a few different ways,
as listed below.

* *How to retrieve the programs details based on the program_name?*

```
SELECT program_name,
program_description,
is_multinational
FROM schema_name.programs
WHERE program_name = 'Rewards' -- To filter based on program_name
AND delete_flag <> 'Y' --To get only non-deleted records from programs
;
```

**<u>Sample Data:</u>**

| **program_name** | **program_description** | **is_multinational** |
|------------------|-------------------------|----------------------|
| Rewards          | Rewards                 | false                |

#### **wallet_policies**

wallet_policies are configurations to define the different categories of
virtual currencies that are tracked for a member. Without a purse there
can be no rewarding of points. There is no limitation to the quantity of
purses created in a program. However, per user experience there is only
one purse visible; the division of purses only exists in the backend of
the loyalty system.

**LIST OF TABLES:** The table for wallet_policies is employed to fetch
the corresponding fields mentioned above.

- wallet_policies

wallet_policies data can be retrieved from the database in a few
different ways, as listed below

* *How to retrieve the wallet_policies details based on wallet_name?*

```
SELECT wallet_name,
wallet_description,
effective_from,
expiration_dt,
pt_multiplier,
wallet_group_name,
period_start_date,
period_end_date
FROM schema_name.wallet_policies
WHERE wallet_name = 'level Credits' -- To filter based on wallet_name
AND delete_flag <> 'Y' -- To get only non-deleted records from wallet_policies
;
```

**<u>Sample Data:</u>**

| **wallet_name** | **wallet_description** | **effective_from**     | **expiration_dt**          | **pt_multiplier** | **wallet_group_name** | **period_start_date**  | **period_end_date**        |
|-----------------|------------------------|------------------------|----------------------------|-------------------|-----------------------|------------------------|----------------------------|
| level Credits    | level Credits           | 2000-01-01 00:00:00+00 | 3000-01-01 23:59:59.999+00 | 10000.000         | level Credits          | 2023-01-01 08:00:00+00 | 2024-01-01 07:59:59.999+00 |


* *How to retrieve active wallet_policies details?*

```
SELECT wallet_name,
wallet_description,
effective_from,
expiration_dt,
pt_multiplier,
wallet_group_name,
period_start_date,
period_end_date
FROM schema_name.wallet_policies
WHERE ((wallet_group_name IS NULL AND expiration_dt::DATE\>=
SYSDATE::DATE) OR (wallet_group_name IS NOT NULL AND
period_end_date::DATE\>= SYSDATE::DATE))
AND delete_flag <> 'Y' -- To get only non-deleted records from wallet_policies
;
```

**<u>Sample Data:</u>**

| **wallet_name**   | **wallet_description** | **effective_from**     | **expiration_dt**          | **pt_multiplier** | **wallet_group_name** | **period_start_date**  | **period_end_date**        |
|-------------------|------------------------|------------------------|----------------------------|-------------------|-----------------------|------------------------|----------------------------|
| Deferred Debits   | Deferred Debits        | 2000-01-01 00:00:00+00 | 3000-01-01 23:59:59.999+00 | 10000.000         |                       |                        |                            |
| level Credits 2024 | level Credits 2024      | 2000-01-01 00:00:00+00 | 3000-01-01 23:59:59.999+00 | 10000.000         | level Credits          | 2024-01-01 08:00:00+00 | 2025-01-01 07:59:59.999+00 |


#### **level_policies**

level_policies are the configurations to define the classification of
members based on milestones that have been awarded. They provide
guidelines for how a level shall operate. Often these guidelines dictate
how many levels are available in the program, how many levels exist
within a level, how many points required for movement between levels, etc.

**LIST OF TABLES:** The table for level_policies is employed to fetch the
corresponding fields mentioned above.

- level_policies

level_policies data can be retrieved from the database in a few different
ways, as listed below:

* *How to retrieve the level_policies details?*

```
SELECT loyalty_program_id,
level_name,
primary_flag
FROM schema_name.level_policies
WHERE delete_flag <> 'Y' -- To get only non-deleted records from level_policies
;
```

**<u>Sample Data:</u>**

| **loyalty_program_id**   | **level_name** | **primary_flag** |
|--------------------------|----------------|------------------|
| 626a6d6128034c0263eb83dd | Base           | true             |

#### **wallet_levels**

The wallet_levels table is responsible for storing and managing
information related to the various levels of membership levels for
members.

**LIST OF TABLES:** The table for wallet_levels is employed to
fetch the corresponding fields mentioned above.

- wallet_levels

- level_policies

* *How to retrieve the wallet_levels details based on level Name?*

```
SELECT TP.level_name,
level_name,
is_default,
level_number,
level_threshold
FROM schema_name.wallet_levels TPL
INNER JOIN schema_name.level_policies TP ON TPL.level_wallet_id = TP.id
WHERE TP.level_name = 'Base' -- To filter based on level name
AND TP.delete_flag <> 'Y' -- To get only non-deleted records from level_policies
;
```

**<u>Sample Data:</u>**

| **level_name** | **level_name** | **is_default** | **level_number** | **level_threshold** |
|----------------|----------------|----------------|------------------|---------------------|
| Base           | Pearl          | false          | 2                | 20,000              |
| Base           | Gold           | false          | 3                | 75,000              |

* *How to retrieve the wallet_levels details based on level Level Name?*

```
SELECT TP.level_name,
level_name,
is_default,
level_number,
level_threshold
FROM schema_name.wallet_levels TPL
INNER JOIN schema_name.level_policies TP ON TPL.level_wallet_id = TP.id
WHERE TPL.level_name = 'Gold' -- To filter based on level name
AND TP.delete_flag <> 'Y' -- To get only non-deleted records from level_policies
;
```

**<u>Sample Data:</u>**

| **level_name** | **level_name** | **is_default** | **level_number** | **level_threshold** |
|---------------|----------------|----------------|------------------|---------------------|
| Base          | Gold           | false          | 3                | 75,000              |

* *How to retrieve the wallet_levels details based on the level_threshold?*

```
SELECT TP.level_name,
level_name,
is_default,
level_number,
level_threshold
FROM schema_name.wallet_levels TPL
INNER JOIN schema_name.level_policies TP ON TPL.level_wallet_id = TP.id
WHERE level_threshold BETWEEN '20000' AND '200000' -- To filter based on level threshold
AND TP.delete_flag <> 'Y' -- To get only non-deleted records from level_policies
;
```

**<u>Sample Data:</u>**

| **level_name** | **level_name** | **is_default** | **level_number** | **level_threshold** |
|----------------|----------------|----------------|------------------|---------------------|
| Base           | Pearl          | false          | 2                | 20,000              |
| Base           | Gold           | false          | 3                | 75,000              |
