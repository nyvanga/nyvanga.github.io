# ISO 8601 file names

Source: [ISO 8601 - Wikipedia](https://en.wikipedia.org/wiki/ISO_8601).

## File names with dates

The ISO standard uses dashes: `[YYYY]-[MM]-[DD]` but I find dots more readable in file names. And I choose to use a dash when representing a range.

Single date: `[YYYY].[MM].[DD]`

Date range: `[YYYY].[MM].[DD]-[DD]`

Date range crossing months: `[YYYY].[MM].[DD]-[MM].[DD]`

Single month: `[YYYY].[MM]`

Month range: `[YYYY].[MM]-[MM]`

Single year: `[YYYY]`

Example:

```
2024.08-09 August through September
2024.08.14-17 August 14th to 17th
2024.08.16-19 August 16th to 19th
2024.08.17-09.19 August 17th to September 19th
2024.08.17-19 August 17th to 19th
2024.08.17 September 17th
2024.08.19 September 19th
2024.08 August
2024.09.17-09.19 September 17th to September 19th
2024.09 September
2024 year 2024
```

Obviously when mixing date and month ranges, the letters in the rest of the file name influences the sorting.

So keeping to full dates only, or months only, or years only, tends to work best.

Also some sorting treat dashes and dots differently:

```
2024.08.14-17 August 14th to 17th
2024.08.16-19 August 16th to 19th
2024.08.17-09.19 August 17th to September 19th
2024.08.17-19 August 17th to 19th
2024.08.17 September 17th
2024.08.19 September 19th
2024.08-09 August through September
2024.08 August
2024.09.17-09.19 September 17th to September 19th
2024.09 September
2024 year 2024
```

## File names with date and time

The ISO standard uses colons: `[hh]:[mm]:[ss]` but to be kind to OS'es like windows, that is not a good idea.

Mostly only makes sense with the full timestamp: `[YYYY].[MM].[DD] [hh].[mm].[ss]`

I haven't found a use case for ranges of seconds, minutes or hours in file names, but it could be achieved with same approach as dates above.
