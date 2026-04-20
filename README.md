# Code Review Graph — Comprehensive Project Report

**Project Name:** Code Review Graph — AI Token Optimization for Code Review  
**Developer:** Tirth Patel (original) — integrated and used by Krish Choudhary  
**Organization:** Ernst & Young — AI Incubator Division  
**Date:** April 2026  
**Status:** Active (Open Source, PyPI published)  
**Source Location:** `/Users/krishchoudhary/GITHUB/SSH/code-review-graph/`  
**PyPI:** `pip install code-review-graph`

---

## 1. Executive Summary

Code Review Graph solves the **token waste problem** in AI-assisted code review. AI tools re-read entire codebases for every task — for a 2,900-file project, this means 100K+ tokens per query. Code Review Graph builds a structural graph of the codebase using Tree-sitter AST parsing, tracks changes incrementally, and serves only the **blast radius** (affected files, functions, tests) to AI assistants via 28 MCP tools. Benchmarked across 6 repos, it achieves an **average 8.2x token reduction** with 100% recall.

---

## 2. How It Works

```
Repository Files → Tree-sitter Parser → AST → Graph nodes + edges → SQLite
                                                                      │
Changed files → Blast Radius Query → Minimal affected set → MCP tools
                                                              │
AI Assistant receives only: changed code + callers + dependents + tests
Instead of: entire codebase (8.2x fewer tokens)
```

### 2.1 Graph Structure
- **Nodes:** Functions, classes, methods, imports, modules
- **Edges:** Calls (function A calls function B), Inheritance, Imports, Test coverage
- **Storage:** SQLite with indexes for fast traversal

### 2.2 Supported Languages (23)
Python, TypeScript/TSX, JavaScript, Vue, Svelte, Go, Rust, Java, Scala, C#, Ruby, Kotlin, Swift, PHP, Solidity, C/C++, Dart, R, Perl, Lua, Zig, PowerShell, Julia + Jupyter notebooks

---

## 3. Benchmarks

| Repository | Files | Naive Tokens | Graph Tokens | **Reduction** |
|-----------|:-----:|:------------:|:------------:|:-------------:|
| express | 141 | 693 | 983 | 0.7x |
| fastapi | 1,122 | 4,944 | 614 | **8.1x** |
| flask | 2,108 | 44,751 | 4,252 | **9.1x** |
| gin (Go) | 547 | 21,972 | 1,153 | **16.4x** |
| httpx | 891 | 12,044 | 1,728 | **6.9x** |
| nextjs | 2,900+ | 9,882 | 1,249 | **8.0x** |
| **Average** | | | | **8.2x** |

**Impact Accuracy:** 100% recall (never misses impacted files), 0.54 F1 (conservative)

---

## 4. 28 MCP Tools

| Category | Tools |
|----------|-------|
| Build | `build_or_update_graph_tool` |
| Query | `get_impact_radius_tool`, `get_review_context_tool`, `detect_changes_tool` |
| Architecture | `get_hub_nodes_tool`, `get_bridge_nodes_tool`, `get_knowledge_gaps_tool` |
| Navigation | `get_callers_tool`, `get_callees_tool`, `get_dependencies_tool` |
| Refactoring | `refactor_tool` (rename preview, dead code detection) |
| Documentation | `generate_wiki_tool` (markdown wiki from code communities) |
| Cross-repo | `cross_repo_search_tool` |

---

## 5. Usage in EY Internship

This tool was used to:
1. **Optimize AI token spend** when reviewing the multi-project codebase
2. **Understand blast radius** of changes across interconnected projects
3. **Auto-generate architecture docs** for reporting
4. **Detect dead code** and structural weaknesses

### Installation
```bash
pip install code-review-graph
code-review-graph install  # Configures MCP for Claude/Cursor/VS Code
```

---

*Report prepared for the EY AI Incubator Internship — April 2026*
