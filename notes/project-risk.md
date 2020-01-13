## Questions

1. Why do projects get delayed and what can we do about?
2. How should we best deal with uncertainty of open tasks?
3. How do we make sure we are building the thing?
4. How can we reduce technical risk?

## Notes

**Risks and Rabbit Holes (Shape Up - Part 1: Shaping - Chapter 5)** (https://basecamp.com/shapeup/1.4-chapter-05):

- Context: Shape Up book by Basecamp, on language and technique for shipping the right thing (for product development). Assuming: Fixed time, a six week bet / risk appetite, and that this thinking is largely done in "shaping up" stage.
- We want project deliverable uncertainity to be thin-tailed around expected time, i.e. 95% within a week of fixed time. If there are unknowns, it is easy (very easy) for a project to drag out 3x on the right tail.
- Avoiding rabbit holes: Need to look for technical unknowns, design unknowns, and interdependencies. Example questions to ask: have we built this type of thing before? have we vetted it with a technical expert? can we cut scope further? are there any unknowns in terms of dependencies? are there any hard decisions we should make upfront?

Further reading:

- Decide When to Stop https://basecamp.com/shapeup/3.5-chapter-13
- Set boundaries https://basecamp.com/shapeup/1.2-chapter-03
- The longer it has taken the longer it will take https://www.johndcook.com/blog/2015/12/21/power-law-projects/
- Book (forgot name) on large scale projects shipping in time
- People shipping things quickly https://patrickcollison.com/fast

**Decide When to stop (Shape Up - Part 3: Building - Chapter 13)** (https://basecamp.com/shapeup/3.5-chapter-13)

- Compare to baseline (what current users get), not some perfect ideal. The use of fixed time forces trade-offs in what we care about.
- Scope hammering: forcefully question if things need to be done. Scope grows like grass. Separate must-have vs nice to have (marked with ~). QA at edges and by default these are discovered tasks -> nice to have. (Same for code review)
- When to extend project? Circuit breaker: by default projects get cancelled. In some cases, can extend by two weeks. Only if all work is downhill (only execution left, no unknowns). If there's uphill work to do, this is too risky and shaping step failed.
