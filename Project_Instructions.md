# Group Project Instructions — Apache Spark (ABD)

> Merged from two sources from the professor (RR):
> - **[Brief]** the project brief presented at the end of class #7 ("Takeaways" slides)
> - **[Email]** the follow-up email with submission and presentation details
>
> ⚠️ Where the two sources differ, see the **Flags** section at the end.

## Objective
- Create a structured, incremental data analysis process that replicates a real-world situation. *[Brief]*

## Requirements
- Create a small **Data Lakehouse** with an analysis theme and **at least one** data source. *[Brief]*
  - Examples: weather temperature history, water levels in dams, population characteristics, economic or financial indicators, city transport data, etc.
- Use **Apache Spark** on **Databricks Free Edition** or **Google Colab**. *[Brief]*
  - *[Group]* We use the **Databricks Free Edition** (student account, serverless compute). Everything in the notebook must run there.
- Submit the work as a **Notebook in HTML format** with a header (see *Submission* for what the header must contain). *[Brief + Email]*

## Process (end-to-end)
*[Brief]*
1. Read data from a source (ideally more than once).
2. Create a data layer with the read history, ready for reprocessing.
3. Normalize the data.
4. Store the normalized, detailed data in a new layer modeled for the chosen analysis theme.
5. Prepare the data for optimized analysis.
6. Create a new data layer with the data optimized for analysis.
7. Perform the analyses: **query, dashboard, ML model**.

## Conclusion
- Summarize the conclusions about the data analysis, the process and the technology used. *[Brief]*

## Submission
*[Email]*
- **Deadline:** **November 16th, 23:59**.
- **Where:** Moodle → ABD page → class #10 → **"Group Project Submissions"** section.
- **Format:** a Notebook in **HTML** format, **named after the project**.
- **Header:** the very beginning of the notebook must clearly state:
  - the **project title**;
  - the **names and student numbers** of all contributing group members.
- ⚠️ **Students whose identification is missing from the header will not be graded.**

## Presentation
*[Email]*
- **When:** class #10, **November 17th and 19th**.
- **Duration:** **15 minutes** per group. Be precise and manage your time well.
- **Sign-up:** book a time slot on Moodle → ABD page → class #10 → **"Presentations Schedule"** section.
- **All students who contributed to the project must actively take part in the presentation.**
- Questions: email the professor.

### Group decisions on the presentation
*[Group]*
- **The notebook is the presentation support.** We present on top of the notebook and explain it cell by cell (Landing → Bronze → Silver → Gold → Analysis → Conclusions), not with a separate slide deck.
- So the notebook must read well out loud: clear markdown before each section, comments that explain the *why*, results visible in the cells, and a run order that follows the 15-minute story.

---

## ⚠️ Flags (differences between the sources)
There are no direct contradictions. These are differences in wording or detail:

1. **Header contents.** The Brief says "group name and elements". The Email says "**project title** + names + **student numbers**", and a missing ID means no grade. → **Follow the Email** (stricter). Put the project title and all 4 names with student numbers in the header (20250313, 20251110, 20251957, 20251705).
2. **File name.** Only the Email says the HTML must be "**named after the project**". The notebook is currently `Lakehouse_Petroleo_PSI.ipynb`. → Make sure the exported HTML file name matches the project title.
3. **Every member presents.** This rule appears only in the Email. Plan the 15 minutes so that all 4 members speak.
4. **Year.** Neither source states the year. The dates are assumed to be 2026 (consistent with the project calendar).
