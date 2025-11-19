# Telcolearn-day-9
# Telecom Log Analysis Pipeline

This repository contains a shell-based pipeline for analyzing telecom logs to identify call drop events, extract key metrics, and summarize network performance. It uses standard UNIX tools (grep, awk, sed, sort, uniq, head) for efficient and reproducible parsing.

## Pipeline Overview

Requirements:
- A standard Linux/Unix environment (or WSL/Cygwin)
- Familiarity with: grep, awk, sed, sort, uniq, head
- A telecom log file for analysis (e.g., network.log)

## Step 1: Filter Log for a Specific Event

Use grep to isolate lines in the log that correspond to the event you are investigating:

grep -w "CALL_DROP" segregat.csv

## Step 2: Extract a Field with awk (e.g., CELL ID)

Pipe the filtered results to awk to extract the specific field of interest, such as the Cell ID. This assumes the field is delimited (space-separated, comma-separated).

## Step 3: Normalize Field with sed

Use sed (or awk's string functions) to clean up the extracted field. This is crucial for accurate counting. Normalization often involves removing labels, specific strings, or surrounding whitespace.

## Step 4: Sort and Count with sort + uniq -c

To summarize the data, pipe the normalized list to sort to group identical entries together, and then use uniq -c to count the occurrences of each unique entry.

Example:

grep -oE '[0-9]{15}' segregat.csv | sort -nr

## Step 5: Add a Header and Limit Results (Optional)

For a tidy output, use sort -nr to order the counts from highest to lowest, and head to display only the top results. echo can add a header line for clarity.

## Step 6: Extract IMSIs That Experienced Call Drops

To identify affected IMSIs, modify the awk command to extract the IMSI field along with the Call Drop event:

awk -F',' -f salary.awk segregat.csv

## Step 7: Filter Call Drops with RSRP Worse than -105 dBm

This requires a conditional check within awk to filter lines based on the numerical RSRP value. This is useful for identifying poor signal conditions:

awk -F',' '$5 < -105 { print $1, $2, $3, $4 }' segregat.csv
