---
name: drm-vmwgfx-multi-file-bugfix-series
description: Workflow command scaffold for drm-vmwgfx-multi-file-bugfix-series in linux.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /drm-vmwgfx-multi-file-bugfix-series

Use this workflow when working on **drm-vmwgfx-multi-file-bugfix-series** in `linux`.

## Goal

Applying a coordinated series of bugfixes and improvements to the drm/vmwgfx driver, often touching multiple related files in a short time window, each commit addressing a specific issue.

## Common Files

- `drivers/gpu/drm/vmwgfx/vmwgfx_resource.c`
- `drivers/gpu/drm/vmwgfx/vmwgfx_execbuf.c`
- `drivers/gpu/drm/vmwgfx/vmwgfx_page_dirty.c`
- `drivers/gpu/drm/vmwgfx/vmwgfx_fence.c`
- `drivers/gpu/drm/vmwgfx/ttm_object.c`
- `drivers/gpu/drm/vmwgfx/vmwgfx_vkms.c`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Identify a set of related issues or improvements in drm/vmwgfx.
- For each issue, edit the relevant file (e.g., validation, blit, execbuf, etc.) to address the problem.
- Write a detailed commit message for each fix, referencing the specific bug and file.
- Commit each change separately, often as a patch series.

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.