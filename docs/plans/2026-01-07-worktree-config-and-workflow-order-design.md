# Worktree Configuration & Workflow Order Design

## Goal

Dwie zmiany w skill'ach superpowers:
1. Zmiana domyślnego katalogu worktrees z `.worktrees` na `worktrees/` (bez kropki)
2. Zmiana kolejności faz w feature-development workflow: git-worktree → brainstorming → implementation (dokumenty tworzone wewnątrz worktree)

## Approach

### Zmiana 1: Katalog worktrees

**Obecny stan:**
- Skill `using-git-worktrees` preferuje `.worktrees/` (ukryty)
- `.gitignore` zawiera tylko `.worktrees/`

**Nowy stan:**
- Skill `using-git-worktrees` powinien zawsze używać `worktrees/` (bez kropki, widoczny)
- `.gitignore` musi zawierać `worktrees/`

**Reasoning:** Użytkownik preferuje jawny katalog `worktrees/` zamiast ukrytego `.worktrees/`. To sprawa preferencji - widoczny katalog jest łatwiejszy do znalezienia.

### Zmiana 2: Kolejność faz workflow

**Obecny workflow:**
```
Phase 1: brainstorming      → design + plan w docs/plans/
Phase 2: using-git-worktrees → worktree
Phase 3: executing-plans    → implementation
Phase 4: finishing-branch   → merge/PR
```

**Problem:** Dokumenty tworzone są w głównym repozytorium, nie w worktree. To oznacza, że:
- Dokumenty zostają w głównym katalogu po zakończeniu pracy
- Nie są izolowane z resztą zmian
- Trzeba je ręcznie przenosić lub commitować osobno

**Nowy workflow:**
```
Phase 1: using-git-worktrees → worktree (PIERWSZE)
Phase 2: brainstorming      → design + plan w worktree/docs/plans/
Phase 3: executing-plans    → implementation
Phase 4: finishing-branch   → merge/PR
```

**Benefits:**
- Wszystkie artefakty (dokumenty + kod) są w jednym izolowanym worktree
- Przy merge/PR wszystko idzie razem
- Czystsza separacja pracy

## Architecture

### Pliki do modyfikacji:

1. **`skills/using-git-worktrees/SKILL.md`**
   - Zmiana priorytetu: `worktrees/` jako domyślny
   - Usunięcie preferencji dla `.worktrees/`
   - Update przykładów

2. **`.gitignore`**
   - Zamiana `.worktrees/` na `worktrees/`

3. **Feature-development workflow** (inline w poleceniu użytkownika)
   - Ten workflow jest przekazywany jako argument, nie jest plikiem
   - Użytkownik będzie musiał zaktualizować go ręcznie lub stworzyć plik skill'a

## Implementation Notes

### Key files to modify:
- `skills/using-git-worktrees/SKILL.md` - główna logika wyboru katalogu
- `.gitignore` - dodanie `worktrees/`

### Patterns to follow:
- Zachować strukturę skill'a
- Zachować safety verification (git check-ignore)
- Zachować symlink logic dla .env i .venv

## Testing Strategy

1. Sprawdzić, że `worktrees/` jest ignorowany przez git
2. Sprawdzić, że skill tworzy worktree w `worktrees/`
3. Sprawdzić, że workflow documentation jest jasna

## Open Questions

1. Czy usunąć `.worktrees/` z `.gitignore` całkowicie, czy zostawić dla kompatybilności wstecznej?
   - **Rekomendacja:** Usunąć - tylko `worktrees/` powinien być używany

2. Feature-development workflow jest przekazywany inline - czy chcesz utworzyć osobny plik skill'a `skills/feature-development/SKILL.md`?
   - **Rekomendacja:** Tak, to pozwoli na łatwą edycję i wersjonowanie

---

## Implementation Plan

**Tech Stack:** Markdown, Bash

### Task 1: Update .gitignore

**Files:**
- Modify: `.gitignore`

**Steps:**
1. Zamienić `.worktrees/` na `worktrees/`
2. Commit: `git commit -m "chore: use worktrees/ instead of .worktrees/"`

### Task 2: Update using-git-worktrees skill

**Files:**
- Modify: `skills/using-git-worktrees/SKILL.md`

**Steps:**
1. Zmienić sekcję "Check Existing Directories" - usunąć preferencję dla `.worktrees`, używać tylko `worktrees/`
2. Zmienić sekcję "Ask User" - usunąć opcję `.worktrees/`, domyślnie `worktrees/`
3. Zmienić Quick Reference table
4. Zmienić Example Workflow
5. Commit: `git commit -m "feat: use worktrees/ as default directory"`

### Task 3: Create feature-development skill file

**Files:**
- Create: `skills/feature-development/SKILL.md`

**Steps:**
1. Utworzyć plik z workflow ze zmienioną kolejnością faz
2. Phase 1: using-git-worktrees (setup izolacji)
3. Phase 2: brainstorming (design + plan w worktree)
4. Phase 3: executing-plans (implementacja)
5. Phase 4: finishing-branch (zakończenie)
6. Commit: `git commit -m "feat: add feature-development skill with worktree-first workflow"`

### Task 4: Final verification

**Steps:**
1. Przeczytać wszystkie zmienione pliki i sprawdzić spójność
2. Upewnić się, że `worktrees/` jest w `.gitignore`
3. Sprawdzić, że skill'e mają poprawne opisy
