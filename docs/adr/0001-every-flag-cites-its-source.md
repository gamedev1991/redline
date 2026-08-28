# 1. Every flag cites its source

## Decision

Every risk flag Redline produces must include the exact sentence, verbatim
from the uploaded document, that it is based on. A flag whose source
sentence cannot be shown is a bug, not a formatting gap — it does not ship,
regardless of how accurate the underlying judgment might be.

## Alternatives

- Let the model describe each risk in its own words, with no quoted text
  tying it back to the document. Faster to build, reads more naturally, but
  gives the reader nothing to check the claim against.
- Cite a paragraph or section number instead of a sentence. Cheaper to
  locate, but forces the reader to re-read a whole block to find what
  actually triggered the flag.
- Show the citation only on request (e.g. behind a "source" toggle) rather
  than always. Reduces visual noise, but makes unverifiable output the
  default instead of the exception.

## Why

A reader can take the quoted sentence and find it, unedited, in the document
they uploaded. That means every flag is checkable without trusting the
model's judgment or Redline's — the reader verifies the match themselves,
in seconds, rather than taking a paraphrase on faith. This is what makes the
severity ranking and the drafted counter-offers usable for something as
consequential as a contract: the product's claims are falsifiable, not just
plausible-sounding.

## Consequences

- The model must be prompted and validated to return exact substrings of the
  parsed text, not summaries or paraphrases — a flag fails validation if its
  cited sentence isn't found verbatim in the document.
- Every flag needs an automated check, not just eyeballing, since a broken
  citation is a shipped bug by definition.
- This constrains how the document is chunked and parsed: the pipeline must
  preserve exact sentence boundaries and text, since a citation can only be
  verified against text that was extracted faithfully.
- It rules out any risk category the model can only justify by inference or
  by combining scattered context — if no single sentence carries the claim,
  the flag cannot be made.
