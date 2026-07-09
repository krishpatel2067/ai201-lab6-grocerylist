# Code Review Notes

Fill this in as you work through the milestones. Each section mirrors the structure of a real GitHub pull request review.

---

## PR #1 — Bulk Purchase (`pr1_bulk_purchase.py`)

### Summary
*What does this PR do? (1–2 sentences in your own words)*

> This PR creates a new `POST /lists/<list_id>/purchase-all` endpoint along with the corresponding service function to mark items as purchased in bulk.

### Issues

For each issue you find, note: where it is (file + function), what's wrong, and why it matters in production.

**Issue 1**
- Location: `pr1_bulk_purchase.py::purchase_all_items`
- What's wrong: The query fetches _all_ items, not just those that aren't purchased. 
- Why it matters: Fetching all item causes `purchased_at` and `purchased_by` to be overwritten in the code after. Also, unnecessary work performed getting and iterating over items that are already purchased.
- Suggested fix: Add a `filter_by` condition to only return non-purchased items.

**Issue 2**
- Location: `pr1_bulk_purchase.py::purchase_all_items`
- What's wrong: No validation for whether `User` of given `user_id` exists.
- Why it matters: An invalid `user_id` silently gets stored for `purchased_by`, compromising data integrity.
- Suggested fix: Add a validation clause at the beginning of the service function to see whether a `User` with the given `user_id` exists. If not, raise a descriptive error.

**Issue 3** *(if found)*
- Location: `pr1_bulk_purchase.py::purchase_all_items`
- What's wrong: The total number of items is returned instead of the number of newly purchased items.
- Why it matters: Users would be misled if they see the total count instead of the newly purchased count.
- Suggested fix: Addressing Issue 1 already fixes this, but just to be sure, use a counter variable that increments only if the current item starts as not purchased and successfully gets marked as purchased.

### Questions for the Author
*Things you're uncertain about — design choices that could be intentional or bugs depending on intent.*

> Was your original intent to return the count of items that were not purchased before and successfully marked as purchased, or the count of items that were marked as purchased (looked at by the loop) regardless of whether it was purchased before or not?

### Verdict
- [ ] Approve — ship it
- [x] Request Changes — needs fixes before merging
- [ ] Comment — needs discussion before a verdict

**Rationale** *(1–2 sentences)*:

> The code works on the happy path but fails in certain edge cases, namely when the given `user_id` doesn't exist and when at least one item in the list is already purchased. It needs to be revised to match current design choices and user expectations.

---

## PR #2 — List Stats (`pr2_list_stats.py`)

### Summary
*What does this PR do? (1–2 sentences in your own words)*

>

### Issues

**Issue 1**
- Location:
- What's wrong:
- Why it matters:
- Suggested fix:

**Issue 2**
- Location:
- What's wrong:
- Why it matters:
- Suggested fix:

**Issue 3** *(if found)*
- Location:
- What's wrong:
- Why it matters:
- Suggested fix:

### Questions for the Author
*A good code review often surfaces design questions, not just bugs. What would you want to clarify before approving?*

>

### Verdict
- [ ] Approve — ship it
- [ ] Request Changes — needs fixes before merging
- [ ] Comment — needs discussion before a verdict

**Rationale** *(1–2 sentences)*:

>

---

## Reflection

*Answer after completing both reviews.*

**1.** Which issue was hardest to spot, and why?

>

**2.** Which issues do you think an LLM reviewer (like Claude reviewing its own code) would most likely miss? Why?

>

**3.** One thing you'd add to a code review checklist for AI-generated backend code:

>
