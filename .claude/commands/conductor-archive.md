---
description: Archive completed tracks
---

# Conductor Archive

Archive completed tracks to clean up the project.

## 1. Find Completed Tracks
List all `[x]` tracks from `tracks.md`.

## 2. Select Tracks
- Archive all completed
- Or select specific ones
- Or cancel

## 3. Archive Process
For each selected track:
1. Create `conductor/archive/` if needed
2. **Teardown worktree (if exists):**
   - Read `conductor/tracks/<track_id>/metadata.json` for `worktree_path` and `git_branch`
   - If `worktree_path` exists on disk:
     ```bash
     bd worktree remove .worktrees/<track_id>
     ```
     - This atomically removes the git worktree AND the `.beads` redirect file.
     - **If `bd` command fails:** → Follow Beads Error Handler Protocol (references/beads-error-handler.md)
       - Degraded fallback: `git worktree remove .worktrees/<track_id> --force`
   - Delete the track branch: `git branch -d track/<track_id>`
     - If `-d` fails (unmerged changes): warn user and ask: "A) Force delete (`-D`)  B) Keep branch"
3. **Check for stale parallel state**: If `parallel_state.json` exists, warn and clean up
4. **Extract learnings before archiving** (see step 3a)
5. Move track folder to archive
6. Remove from `tracks.md`
7. Add archive comment

### 3a. Extract Learnings Before Archiving

**PROTOCOL: Preserve learnings from completed tracks (Ralph-style knowledge capture).**

1. **Read Track Learnings:** Load `conductor/tracks/<track_id>/learnings.md`

2. **Identify Patterns to Elevate:**
   - Find patterns not yet in `conductor/patterns.md`
   - Find gotchas with general applicability
   
3. **Present Extraction Summary:**
   > "## Learnings from `<track_id>`"
   > 
   > **Patterns to preserve:**
   > | Pattern | Elevate to project? |
   > |---------|---------------------|
   > | "Use X for Y" | ☐ |
   > | "Don't forget Z" | ☐ |
   > 
   > "Select patterns to add to `conductor/patterns.md` (Enter numbers, 'all', or 'skip'):"

4. **Update Project Patterns:**
   - Append selected patterns to `conductor/patterns.md`:
     ```markdown
     - <pattern> (from: <track_id>, archived <date>)
     ```

5. **Preserve Learnings in Archive:**
   - Keep `learnings.md` in the archived track folder
   - Future tracks can reference via `/conductor-newtrack`

## 4. Commit
Commit the archive operation.

---

## 5. BEADS COMPACTION

**PROTOCOL: Compact Beads history for archived tracks.**

1. **Availability Check:**
   - Run the standard Beads availability check (see `references/beads-error-handler.md`)
   - If `BEADS_AVAILABLE=false`: skip this section silently

2. **Clear Stale Parallel Assignees:**
   - For each archived track, clear any leftover parallel worker assignees:
     ```bash
     bd update <task_id> --assignee "" --json
     ```
   - This prevents orphaned worker assignments from parallel execution

3. **Compact Archived Track Epics:**
   - For each archived track with `beads_epic` in metadata:
     - Run `bd admin compact --auto <epic_id>`
     - Run `bd rules compact --auto`
   - If any `bd` command fails: Follow Beads Error Handler Protocol (see `references/beads-error-handler.md`)
   - Announce: "Compacted Beads history for archived tracks."

4. **Offer Project-Wide Compaction:**
   > "Would you like to compact all completed Beads tasks?"
   > A) Yes - Compact all completed tasks project-wide
   > B) No - Only compact archived tracks
   
   If A: 
     - Run `bd admin compact --auto --all`
     - Run `bd rules compact --auto`
