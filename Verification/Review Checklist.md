# Review Checklist — game.cpp (Static Code Verification)

| Category | Result (ISSUE/OK) | Defect |
|----------|-------------------|--------|
| 1. Initialization | ISSUE | Line 27: `monsters[]` pointers are never initialized. Line 77: `double sum;` is uninitialized and then used with `+=`. Line 103: `int health;` is uninitialized and used without input validation. |
| 2. Pointers (null/delete) | ISSUE | Lines 54–56 and 71–72: `findMonster()` result is used without a null check (it can return `nullptr`). Line 85 and 86: `m->name` is used after `delete m` (use after free). Line 85 and 37: the deleted pointer stays in `monsters[]`, so the destructor deletes it again (double delete). Line 109: `delete name` is used for memory allocated with `new[]` (should be `delete[]`). Line 121: `bonus` is allocated but never deleted (memory leak). |
| 3. Arrays / buffers | ISSUE | Line 41: no check `monsterCount < MAX_MONSTERS`, and `main` spawns 6 monsters (line 119) into an array of 5. Line 65: `monsters[monsterCount]` is read out of bounds. Lines 102 and 105: `cin >> name` into `char[10]` has no length limit (buffer overflow). |
| 4. Loops | ISSUE | Line 64: `i <= monsterCount` is an off-by-one error, so the loop reads one element past the last valid monster. Loops at lines 36, 46 and 78 are OK. |
| 5. Arithmetic | ISSUE | Lines 55–56: `unsigned int dmg` is mixed with a signed `int`, so `m->health - dmg < 0` is never true. Line 80: division by zero when `monsterCount == 0`, and the `int` return type truncates the average. Line 91: `attack * armorPercent / 100` is integer division, which truncates the result. |
| 6. Control flow | ISSUE | Line 47: `=` is used instead of `==` in the `if` condition (assignment instead of comparison). Lines 94–99: `playerRank()` has no `return` when `score <= 100` (undefined behaviour). |
| 7. Function call | ISSUE | Lines 70–73: `getMonsterLabel()` returns a reference to the local variable `label` (dangling reference). Line 123: `attackMonster(99, 20)` is called with an id that does not exist, which leads to a null dereference. |
| 8. Classes | ISSUE | Line 133: `Game copy = game;` makes a shallow copy of raw pointers, so both objects delete the same monsters (double free). No copy constructor or assignment operator is defined (Rule of Three). Lines 17–21: the constructor assigns in the body instead of using a member initializer list. |
