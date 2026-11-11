# Initial attempt (written BEFORE using any AI tool)
Problem 1: Inconsistent Item Casing
Code that revealed it: `df['item'].value_counts()`
Evidence: 'Oil B' appears 23 times, but 'oil b' appears once (row 41).
Planned Decision: "Correct". Standardize all text in the `item` column to Title Case.
Reason: Inconsistent casing will break groupby operations and distort aggregations. This is a safe, deterministic fix.

Problem 2: Numeric Comma in Price Column
Code that revealed it: `df['price'].dtype` returned `object` instead of `float`.
Evidence: The value "1,250" appears in row 18. `pd.to_numeric()` will fail on this string.
Planned Decision: "Correct". Remove commas and convert the column to float.
Reason: The column must be numeric for any statistical analysis. The comma is a formatting artifact, not a data value.

Problem 3: Extreme Outlier in Price
Code that revealed it: `df['price'].describe()` and `df[df['price'] > 1000]`
Evidence:** Sweetener D, City 1 on 2026-01-22 has a price of 9999. The median price for this item is ~160.
Planned Decision: "Flag". Replace 9999 with `NaN` (missing value) but do not delete the row.
Reason: 9999 is likely a data entry error (e.g placeholder or typo). However, deleting the entire row removes valid date/city/item context. Flagging it preserves the row's existence for count integrity while preventing it from skewing averages.

Problem 4: Missing Values
Code that revealed it: `df.isnull().sum()`
Evidence: Pulse C, City 2 on 2026-01-01 has an empty price.
Planned Decision: "Retain". Leave as `NaN`.
Reason: Missing data is a reality of the dataset. Imputing it (e.g., filling with the mean) would fabricate data and alter the distribution. Leaving it as `NaN` is honest.

Problem 5: Duplicate Rows
Code that revealed it: `df.duplicated().sum()`
Evidence: Row 10 (Staple A, City 1, 2026-01-08, 99.5) and the final row (Row 97) are identical.
Planned Decision: "Exclude". Drop exact duplicates.
Reason: Duplicates inflate observation counts and bias averages. One copy is sufficient.

Problem 6: Constant Column (Unit)
Code that revealed it: `df['unit'].nunique()`
Evidence: All rows contain "PKR per kg (illustrative)".
Planned Decision: "Retain".
Reason: While it has no variance, it is accurate metadata for the dataset. Dropping it might lose context for future users.
