---
name: make-ads
description: Research the winning ads, write a brief and make a batch of Meta ads with Dovey. Run with how many ads and what they are for, for example "make-ads 5 for the summer sale".
disable-model-invocation: true
argument-hint: "[how many ads] [what they are for]"
---

{{generated}}

# Make ads with Dovey

The person's words after the command say how many ads and what they are for. With no number, ask once; on the Free plan suggest 3.

1. **Check.** @get_me. No Dovey tools or no sign-in: follow the `setup` skill first. No brand: set up the brand first (the `setup` skill). Note the plan and this week's usage.
2. **Research.** @research_ads with the category (what they named, else what the brand sells) and the competitors they named. Show the top ads as a table with their pictures and why they win.
3. **Brief.** @plan_creatives with `n`, their focus, and `source_ad_ids` from the research. Show the brief as a table. Wait for the person's go.
4. **Make.** @make_plan. Poll @get_plan until every row has landed. Show every ad with its primary text and headline, and the canvas link.
5. **Judge.** Check each ad against the checklist in the `dovey` skill. Say which you would change and offer @refine_design or @rewrite_copy.

Never publish. Publishing to Meta is the person's own press on the card @meta_publish_prepare returns.
