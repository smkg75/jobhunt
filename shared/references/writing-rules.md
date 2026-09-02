# Writing rules

Every resume bullet and every cover letter this plugin writes, in any language, obeys the list below.

- Twenty words per sentence at most.
- One idea per sentence.
- Ordinary punctuation: commas and full stops carry the sentence, em dashes and dash chains stay out.
- Plain words. "Leveraging", "track record", "aligns perfectly" and the rest of the assistant register
  stay out, jargon with them.
- Each sentence opens on a different word from the one before it. Same between paragraphs.
- A sentence carrying a figure stays on one role.
- A finished job is told in the past tense, and its end is stated as a dated fact.
- An ambiguous fact is left out rather than smoothed over.
- French text carries its full accents.
- Every figure, client and date comes from `DATA_DIR/profile.md` or from the resume.

## Check after writing

Read the finished text line by line against the list, one line at a time, before the file is saved.
A line that breaks a rule is rewritten and re-read. The text is done when every line has passed every
rule.
