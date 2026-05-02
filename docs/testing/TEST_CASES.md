# urleval Test Cases

## Phase 2: Hello World / Input Parsing

### TC-001: Basic invocation, no flags
- **Input:** Site = "recipe sharing platform for home cooks"; Candidates = recipebox.com, homechef.io, cookwith.me
- **Expected:** Echoes inputs, notes scoring not yet implemented
- **Status:** [ ] Pass / [ ] Fail

### TC-002: Invocation with --update flag
- **Input:** Same as TC-001, plus `--update`
- **Expected:** Echoes inputs with `--update` noted in flags
- **Status:** [ ] Pass / [ ] Fail

### TC-003: No inputs provided
- **Input:** `/urleval` with nothing else
- **Expected:** Claude asks for site description first, then asks for candidate URLs
- **Status:** [ ] Pass / [ ] Fail

### TC-004: Inline mode
- **Input:** `/urleval --site "recipe sharing platform" recipebox.com homechef.io`
- **Expected:** Proceeds directly, echoes parsed inputs without asking questions
- **Status:** [ ] Pass / [ ] Fail
