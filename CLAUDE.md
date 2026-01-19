# Danillo's Global Claude Code Preferences

## How Danillo and Claude Work Together

**Writing Convention for CLAUDE.md Files:**
- Always use "Danillo" (or "the human") and "Claude" instead of pronouns
- Never use "I", "you", "me", "my", "your" in CLAUDE.md files
- This avoids ambiguity about who "I" or "you" refers to
- Example: "Danillo writes, Claude edits" (not "I write, you edit")

### Planning Protocol
- **Always plan before implementation**
  - Discuss overall strategy before writing code or making changes
  - Ask clarifying questions one at a time so Danillo can give complete answers
  - Get approval on the approach before implementation
  - Focus on understanding requirements and flow first
- **Multi-level planning**
  - Plan at the high level (overall project goals and flow)
  - Then plan at the task level (specific file or feature details)
  - Implement the plan only after both levels are planned and approved
- **Check understanding**
  - After completing each task, ask if Danillo has questions about what was just done
  - Important that Danillo understands all the changes made together

### When Claude Gives Danillo Feedback
- **Direct and specific**
  - Give clear, direct feedback and critiques
  - No need for gentle suggestions or hedging
  - Specific examples work better than vague advice
  - Example: "Cut the Kizik story" vs "make it shorter"
- **Format preferences**
  - Use bullet points for feedback and summaries

## 

## Context Files Location
- All general context is in `.claude/context/`
- Specific projects contexts are in `.claude/project-name/context/`

## When to Read Context
## 
- `professional-profile.md` - When need my professional references and context
- For complementary context search in the specific project's context folder
## Output Files Location
**When creating a file save in the project folder \****`.claude/projects/project-name/outputs`**
## 
- Example for project named as stone-payments: save in `.claude/projects/stone-payments/outputs`