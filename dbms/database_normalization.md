# Database Normalization

Normalized tables are

- Easier to understand
- Easier to enhance and extend
- Protected from:
  - insertion anomalies
  - update anomalies
  - deletion anomalies

## 1. Normal Forms

### 1.1. 1NF

Violations

1. Using row order to convey information violates 1NF
2. Mixing data types within the same column
3. Having a table without a primary key
4. Storing a repeating group of data items on a single row

Solution - 1NF

- Devote a separate column for it (1)
- Data type for 1 column (2)
- Assign a PK (3)
- Build a separate table and list 1 item in 1 row (4)

### 1.2 2NF

Violation - Logical inconsistencies

    1. insertion anomaly - Unable to add a row because some attribute which is part of PK is empty
    2. update anomaly - when not all rows are not updated when it should have been
    3. deletion anomaly - when row is deleted through attribute

Solution - 2NF: Each non-key attribute must depend on the entire PK. If not, make a new additional table for the attribute and check again.

### 1.3. 3NF

Solution - 3NF: Every non-key attribute in a table should depend on the key, the whole key and nothing but the key.

### 1.4. BCNF

Solution - Boyce-Codd NF: Every attribute in a table should depend on the key, the whole key and nothing but the key.

### 1.5. 4NF

Solution - 4NF: Multivalued dependencies in a table must be multivalued dependencies on the key.

### 1.6. 5NF

Solution - 5NF: The table (which must be in 4NF) cannot be describable as the logical result of joinng some other table together.

## 2. Examples

### 2.1. 1NF

### 2.2. 2NF

Player_Inventory

| <ins>Player_ID</ins> | <ins>Item_Type</ins> | Item_Quantity | Player_Rating |
| -------------------- | -------------------- | ------------- | ------------- |
| jdog21               | amulets              | 2             | Intermediate  |
| jdog21               | rings                | 4             | Intermediate  |
| gila19               | copper coins         | 18            | Beginner      |
| trev73               | shields              | 3             | Advanced      |
| trev73               | arrows               | 5             | Advanced      |
| trev73               | copper coins         | 30            | Advanced      |
| trev73               | rings                | 7             | Advanced      |

    {Player_ID, Item_Type} -> {Item_Quantity} ✔️
    {Player_ID} -> {Player_Rating} ❌

After 2NF Normalization

Player_Inventory

| <ins>Player_ID</ins> | <ins>Item_Type</ins> | Item_Quantity |
| -------------------- | -------------------- | ------------- |
| jdog21               | amulets              | 2             |
| jdog21               | rings                | 4             |
| gila19               | copper coins         | 18            |
| trev73               | shields              | 3             |
| trev73               | arrows               | 5             |
| trev73               | copper coins         | 30            |
| trev73               | rings                | 7             |

    {Player_ID, Item_Type} -> {Item_Quantity} ✔️

Player

| <ins>Player_ID</ins> | Player_Rating |
| -------------------- | ------------- |
| jdog21               | Intermediate  |
| gila19               | Beginner      |
| trev73               | Advanced      |
| tina42               | Beginner      |

    {Player_ID} -> {Player_Rating} ✔️

### 2.3. 3NF

Consider

    Beginner -> 1, 2, 3
    Intermediate -> 4, 5, 6
    Advanced -> 7, 8, 9

Player

| <ins>Player_ID</ins> | Player_Rating | Player_Skill_Level |
| -------------------- | ------------- | ------------------ |
| jdog21               | Intermediate  | 4                  |
| gila19               | Beginner      | 1                  |
| trev73               | Advanced      | 9                  |
| tina42               | Beginner      | 2                  |

    gila19 now becomes 4

Player

| <ins>Player_ID</ins> | Player_Rating | Player_Skill_Level |
| -------------------- | ------------- | ------------------ |
| jdog21               | Intermediate  | 4                  |
| gila19               | Beginner      | 4                  |
| trev73               | Advanced      | 9                  |
| tina42               | Beginner      | 2                  |

    Now becomes inconsistent since 4 is not beginner

    Why?
    {Player_ID} -> {Player_Skill_Level} ✔️
    {Player_ID} -> {Player_Skill_Level} -> {Player Rating} ❌
    Because of transitive dependency

Player

| <ins>Player_ID</ins> | Player_Skill_Level |
| -------------------- | ------------------ |
| jdog21               | 4                  |
| gila19               | 1                  |
| trev73               | 9                  |
| tina42               | 2                  |

    {Player_ID} -> {Player_Skill_Level} ✔️

Player_Skill_Levels

| <ins>Player_Skill_Level</ins> | Player_Rating |
| ----------------------------- | ------------- |
| 1                             | Beginner      |
| 2                             | Beginner      |
| 3                             | Beginner      |
| 4                             | Intermediate  |
| 5                             | Intermediate  |
| 6                             | Intermediate  |
| 7                             | Advanced      |
| 8                             | Advanced      |
| 9                             | Advanced      |

    {Player_Skill_Level} -> {Player Rating} ✔️

### 2.4. BCNF

### 2.5. 4NF

    Added Prairie Green only for Bungalow but not for Schoolhouse in

| <ins>Model</ins> | <ins>Color</ins> | <ins>Style</ins> |
| ---------------- | ---------------- | ---------------- |
| Tweety           | Yellow           | Bungalow         |
| Tweety           | Yellow           | Duplex           |
| Tweety           | Blue             | Bungalow         |
| Tweety           | Blue             | Duplex           |
| Metro            | Brown            | High-Rise        |
| Metro            | Brown            | Modular          |
| Metro            | Grey             | HIgh-Rise        |
| Metro            | Grey             | Modular          |
| Prairie          | Brown            | Bungalow         |
| Prairie          | Brown            | Schoolhouse      |
| Prairie          | Beige            | Bungalow         |
| Prairie          | Beige            | Schoolhouse      |
| _Prairie_        | _Green_          | _Bungalow_       |

    Schoolhouse missing

    {Model} -> {Color}
    {Model} -> {Style}

After 4NF normalization

Model_Colors_Available

| <ins>Model</ins> | Color   |
| ---------------- | ------- |
| Tweety           | Yellow  |
| Tweety           | Blue    |
| Metro            | Brown   |
| Metro            | Grey    |
| Prairie          | Brown   |
| Prairie          | Beige   |
| _Prairie_        | _Green_ |

    {Model} -> {Color} ✔️

Model_Styles_Available

| <ins>Model</ins> | Style     |
| ---------------- | --------- |
| Tweety           | Bungalow  |
| Tweety           | Duplex    |
| Metro            | High-Rise |
| Metro            | Modular   |
| Prairie          | Bungalow  |
| Prairie          | Bungalow  |

    {Model} -> {Style} ✔️

### 2.6. 5NF

Before:

    Brand: Frosty's
    Flavor offered: Vanilla, CHocolate, Strawberry, Mint Chocolate Chip

    Brand: Alpine
    Flavor offered: Vanilla, Rum Raisin

    Brand: Ice Queen
    Flavors offered: Vanilla, Strawberry, Mint Chocolate Chip

    Jason likes vanilla and chocolate flavors and brands Frosty's and Alpine

    Suzy likes rum raisin, mint chocolate chip and strawberry flavors and brands Alpine and Ice Queen

    Now Suzy also likes brand Frosty's

Preferred_Ice_Cream_Products_By_Person

| <ins>Person</ins> | <ins>Brand</ins> | <ins>Flavor</ins>   |
| ----------------- | ---------------- | ------------------- |
| Jason             | Frosty's         | Vanilla             |
| Jason             | Frosty's         | Chocolate           |
| Jason             | Alpine           | Vanilla             |
| Suzy              | Alpine           | Rum Raisin          |
| Suzy              | Ice Queen        | Mint CHocolate Chip |
| Suzy              | Ice Queen        | Strawberry          |
| _Suzy_            | _Frosty's_       | _Strawberry_        |

After 5NF Normalization

Tables:

Available_Flavors_By_Brand

| <ins>Brand</ins> | <ins>Flavor</ins>   |
| ---------------- | ------------------- |
| Frosty's         | Vanilla             |
| Frosty's         | Strawberry          |
| Frosty's         | Mint Chocolate Chip |
| Alpine           | Vanilla             |
| Alpine           | Rum Raisin          |
| Ice Queen        | Vanilla             |
| Ice Queen        | Strawberry          |
| Ice Queen        | Mint Chocolate Chip |

Preferred_Flavors_By_Person

| <ins>Person</ins> | <ins>Flavor</ins>   |
| ----------------- | ------------------- |
| Jason             | Vanilla             |
| Jason             | Chocolate           |
| Suzy              | Rum Raisin          |
| Suzy              | Mint Chocolate CHip |
| Suzy              | Strawberry          |

Preferred_Brands_By_Person

| <ins>Person</ins> | <ins>Brand</ins> |
| ----------------- | ---------------- |
| Jason             | Frosty's         |
| Jason             | Alpine           |
| Suzy              | Alpine           |
| Suzy              | Ice Queen        |

Query:

```sql
SELECT
    pbrand.Person,
    bf.Brand,
    bf.Flavor
FROM
    Preferred_Brands_By_Persob pBrand
INNER JOIN
    Preferred_Flavors_By_Person pflavor
ON
    pbrand.Person = pflavor.Person
INNER JOIN
    Available_Flavors_By_Brand bf
ON
    pbrand.Brand = bf.Brand
    AND pflavor.Flavor = bf.Flavor
```

## See More

- [Video Tutorial](https://youtu.be/GFQaEYEc8_8?si=FnN7I-CplJyWqFOn)
