# Worktree Config & Workflow Order Design

## Goal

Dwie zmiany w konfiguracji superpowers:
1. Zmiana domyślnego katalogu worktrees z `.worktrees/` na `worktrees/` (widoczny zamiast ukrytego)
2. Zmiana kolejności faz w `commands/feature-development.md`: worktree PIERWSZY, potem brainstorming

## Approach

### Zmiana 1: Katalog worktrees

**Obecny stan w `skills/using-git-worktrees/SKILL.md`:**
- Preferuje `.worktrees/` (ukryty)
- `worktrees/` jako alternatywa
- Pyta użytkownika jeśli żaden nie istnieje

**Nowy stan:**
- Zawsze używaj `worktrees/` (widoczny)
- Nie pytaj użytkownika - domyślnie `worktrees/`
- Zachowaj opcję globalną w CLAUDE.md dla zaawansowanych użytkowników

**Reasoning:** Użytkownik preferuje widoczny katalog - łatwiejszy do znalezienia i zarządzania.

### Zmiana 2: Kolejność faz w workflow

**Obecna kolejność w `commands/feature-development.md`:**
```
Phase 1: Design (brainstorming) → docs w głównym repo
Phase 2: Setup (worktree)
Phase 3: Implementation
Phase 4: Completion
```

**Nowa kolejność:**
```
Phase 1: Setup (worktree) → izolacja NAJPIERW
Phase 2: Design (brainstorming) → docs wewnątrz worktree
Phase 3: Implementation
Phase 4: Completion
```

**Benefits:**
- Wszystkie artefakty (dokumenty + kod) w jednym izolowanym worktree
- Przy merge/PR wszystko idzie razem
- Główne repo pozostaje czyste

## Architecture

### Pliki do modyfikacji:

1. **`skills/using-git-worktrees/SKILL.md`** - zmiana logiki wyboru katalogu
2. **`commands/feature-development.md`** - zmiana kolejności faz (NIE tworzenie nowego skill'a!)

### .gitignore

Już zawiera `worktrees/` - nie wymaga zmian.

## Implementation Notes

### W `skills/using-git-worktrees/SKILL.md`:
- Usunąć sprawdzanie `.worktrees` - tylko `worktrees/`
- Zmienić "Ask User" na domyślne `worktrees/`
- Zaktualizować przykłady i Quick Reference

### W `commands/feature-development.md`:
- Zamienić Phase 1 (Design) i Phase 2 (Setup) miejscami
- Zaktualizować Workflow Overview diagram
- Zaktualizować Quick Reference table
- Zaktualizować "Starting the Workflow" section

## Testing Strategy

1. Przeczytać zmodyfikowane pliki i sprawdzić spójność
2. Sprawdzić że workflow ma poprawną kolejność faz

## Open Questions

Brak - wymagania są jasne.

---

## Implementation Plan

**Tech Stack:** Markdown

### Task 1: Update using-git-worktrees skill

**Files:**
- Modify: `skills/using-git-worktrees/SKILL.md`

**Steps:**
1. Zmienić sekcję "Check Existing Directories" - tylko `worktrees/`
2. Zmienić sekcję "Ask User" na "Default to worktrees/"
3. Zaktualizować Safety Verification - tylko `worktrees/`
4. Zaktualizować case statement w Creation Steps
5. Zaktualizować Quick Reference table
6. Zaktualizować Example Workflow
7. Commit: `git commit -m "feat: use worktrees/ as default directory"`

### Task 2: Update feature-development command

**Files:**
- Modify: `commands/feature-development.md`

**Steps:**
1. Zamienić Workflow Overview - Setup first, Design second
2. Zamienić sekcje Phase 1 i Phase 2 miejscami (treść)
3. Zaktualizować Quick Reference table
4. Zaktualizować "Starting the Workflow" section
5. Zaktualizować Red Flags table
6. Commit: `git commit -m "feat: worktree-first workflow order"`

### Task 3: Final verification

**Steps:**
1. Przeczytać oba pliki i sprawdzić spójność
2. Sprawdzić że `.gitignore` zawiera `worktrees/`
