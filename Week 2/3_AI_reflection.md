I asked an AI to write clean code for me:

AI Line: `df = df.dropna()`**
Verdict: "REJECT".
Why? This deletes the row with the missing price. But that row also has a date, an item, and a city. Deleting it loses real information. I kept the row and just left the price empty.

AI Line: `df['price'] = df['price'].replace(9999, df['price'].median())`**
Verdict: "MODIFY".
Why? This line replaces the 9999 with the median price (about 166). But that's making up fake data. Instead, I changed it to replace 9999 with `NaN` (missing). That flags the error without inventing a fake number.

AI Line: `df['item'] = df['item'].str.lower()`**
Verdict: "MODIFY".
Why? This would make everything lowercase (e.g "staple a"). That fixes the casing problem but looks ugly in a report. I changed it to Title Case (".str.title()") so it stays readable.

AI Line: `df['price'] = df['price'].astype(float)`
Verdict: "REJECT" as written.
Why? This line crashes because of the "1,250" string. I had to remove the comma first before converting to a number.

Reflection:

What did the AI do differently from me?
The AI wanted a perfectly clean table. It dropped the missing row and filled the outlier with a fake median. I wanted to keep the data honest. I kept the missing row and left the outlier as missing too. That way, the table shows what is actually wrong instead of hiding it.

Which was better?
My approach was seemingly better. The assignment said: "A clean-looking table with undocumented changes is worse than a messy honest one." The AI's code made the table look clean, but it hid the fact that data was missing or wrong.

What would I have missed without checking?
I almost deleted the 9999 row entirely. But then I realized that would mess up the count—every other item appears 24 times, but that item would only appear 23 times. By flagging it as missing instead of deleting it, I kept the row count right and still fixed the math.
