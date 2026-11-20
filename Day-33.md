# Day 33: Resolve Git Merge Conflicts

## The Problem
Max has new story changes in the `story-blog` repository on the storage server (`/home/max/story-blog`). When he tries to push them to `origin`, Git rejects the push because the server has commits he doesn't have locally. Sarah's commits are on the server, and Max's commits are on his machine, so the histories have diverged.

## Why a Rebase Happens Without Two Branches
A rebase doesn't need two branch names. It needs two sets of commits:

1. **Max's local commits:** his new changes, not yet on the server.
2. **Sarah's remote commits:** on `origin/master`, which Max doesn't have yet.

`origin/master` is a local copy of the server's `master` branch. Git treats it as the other side of the divergence.

## What a Rebase Does
1. Downloads Sarah's new commits.
2. Sets Max's commits aside.
3. Moves `master` up to match `origin/master`.
4. Replays Max's commits on top, one at a time.

If Max and Sarah changed the same lines, Git stops and asks someone to resolve the conflict.

## Steps to Fix (run as `max` in `/home/max/story-blog`)

```bash
git status                          # check the current state
git fetch origin                    # download Sarah's commits
git log --oneline --graph --all     # see how the histories diverged
git pull origin master --rebase      # replay Max's commits on top
