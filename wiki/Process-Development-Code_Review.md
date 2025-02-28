# Process-Development-Code Review

There are three main steps in the code review process: first review,
second review, and comment remediation.

**FIRST REVIEW**

The original developer(s) will review the code changes before commit.
The following items should be reviewed:

- Implementation of business requirements
- Code cleanliness (i.e., formatting, comments, warning remediation,
  etc.)
- Quality unit tests
- Code logic (i.e., search for errors in logic)

**SECOND REVIEW**

The code reviewer will review the code changes after commit. The
following items should be reviewed:

- Items included in the first review
- Overall best practices - modularity, reusability, design patterns,
  library usage, etc.
- High-risk code (e.g., security-related code, etc.)

**COMMENT REMEDIATION**

- If the commit is associated with a bug, the second reviewer will place
  their comments in bugzilla, and reopen the bug.
- If the commit is not associated with a bug, the second reviewer will
  send their comments to the developer via email.
- The developer will address the code review comments, and
  recommit/reclose the bug upon completion.
