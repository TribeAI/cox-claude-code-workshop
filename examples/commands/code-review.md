---
description: Reviews code changes
argument-hint: input_file | review_file
---

## Context

Parse $ARGUMENTS to get the following variables:
[input_file]: The file to review
[report_file]: The file to save the review to



## Task
Review the code for quality. Follow these steps:
Analyze [input_file] for speed optimizations
Identify any obvious security concerns in [input_file]
Run lint to validate code

## Output
 - Print the code review results in `docs/code_reviews/[review_file]` with the following format:
	- Summary: A summary of the overall review
	- Security: Any critical security flaws, emphasized
	- Optimizations: Identify areas for optimizations, ranked by ROI
	- Lint: Paste the full lint output 
