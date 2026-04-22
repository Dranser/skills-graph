# Pipeline Architecture

## Purpose

Use this reference when architecture itself should take the form of a data-oriented processing pipeline.

## Core Rules

- treat architecture and optimization as one problem
- define explicit stages around explicit data products
- make recompute boundaries visible
- retain derived products only when reuse is cheaper than rerun

## Decision Table

| Situation | Preferred action | Why |
| --- | --- | --- |
| One broad update loop transforms many kinds of data | Split into named stages with explicit outputs | Optimization needs visible products and boundaries. |
| Derived products are read repeatedly downstream | Retain them between runs | Reuse may be cheaper than recompute. |
| Local source change affects one region only | Limit recompute to affected stage outputs | Broad reruns waste architecture-level gains. |

## Anti-Pattern Matrix

| Anti-pattern | Symptom | Likely correction |
| --- | --- | --- |
| Monolithic update pass | Profiling and recompute reasoning are opaque | Split by data products and dependencies. |
| Pipeline as diagram only | Runtime behavior still hides ownership | Make stages real runtime architecture, not documentation. |
| Optimization as late patch | Design never carries recompute boundaries cleanly | Start from a pipeline-first structure. |

## Review Playbook

1. Name the source inputs.
2. Name each derived product.
3. Map which stage owns each product.
4. Mark when that product becomes dirty.
5. Verify broad recompute is not the default answer.
