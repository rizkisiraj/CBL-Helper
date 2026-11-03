# raw/ — immutable source documents

Drop source material here to be compiled into the wiki:
PDFs, papers, clipped web articles (.md/.html), notes (.txt), datasets (.csv), images.

Then run **/project:cbl:ingest** to summarize each into `sources/`, extract `concepts/`,
and wire backlinks. This folder is read-only to the LLM — the wiki can always be
recompiled from it. (Tip: the Obsidian Web Clipper extension is a handy way to save
articles as .md here.)
