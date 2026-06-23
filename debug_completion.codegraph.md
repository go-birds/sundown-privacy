## Exploration: Investigate why a completed puzzle might not transition to the completed state or show completion UI. Trace the likely state flow and identify the most relevant files/functions to inspect or change.

Found 191 symbols across 43 files.

### Blast radius — what depends on these (update/verify before editing)

- `complete` (lib/ui/state/challenge_providers.dart:24) — 3 callers in `lib/ads/rewarded_ad_service.dart`, `lib/ads/ump_consent_service.dart`, `lib/ui/screens/challenge_game_screen.dart`; ⚠️ no covering tests found
- `_completeIfDone` (lib/ui/state/providers.dart:797) — 2 callers in `lib/ui/state/providers.dart`; ⚠️ no covering tests found
- `base_complete_ok` (experiments/deductive-mechanics/verify.py:53) — 1 caller in `experiments/deductive-mechanics/verify.py`; ⚠️ no covering tests found
- `ChallengePuzzle` (lib/domain/challenge/challenge_puzzle.dart:23) — 9 callers in `lib/domain/challenge/challenge_puzzle.dart`, `lib/ui/screens/challenge_game_screen.dart`; tests: `test/ui/screens/challenge_pack_screen_test.dart`, `test/domain/challenge_pack_test.dart`
- `InvalidPuzzleCodeException` (lib/domain/code/puzzle_code.dart:120) — 2 callers in `lib/domain/code/puzzle_code.dart`; ⚠️ no covering tests found

### Source Code

> The code below is the **verbatim, current on-disk source** of these files — re-read from disk on this call and line-numbered, byte-for-byte identical to what the Read tool returns. It is NOT a summary, outline, or stale cache. Treat each block as a Read you have already performed: do not Read a file shown here.

#### lib/ui/screens/challenge_game_screen.dart — puzzle(references), of(calls), build(calls), Cell(references), ChallengePuzzle(references), +20 more

```dart
21	  final List<List<Cell>> _history = [];
22	  bool _completed = false;
23	
24	  ChallengePuzzle get puzzle => widget.puzzle;
25	
26	  @override
27	  void initState() {
28	    super.initState();
29	    _cells = puzzle.initialCells();
30	  }
31	
32	  @override
33	  Widget build(BuildContext context) {
34	    final t = context.tokens;
35	    final errors = ChallengeRules.violationCells(puzzle, _cells);
36	    return Scaffold(
37	      backgroundColor: t.background,
38	      appBar: AppBar(
39	        title: Text('${puzzle.number}. ${puzzle.title}'),
40	        backgroundColor: t.background,
41	      ),
42	      body: SafeArea(
43	        child: Padding(
44	          padding: EdgeInsets.fromLTRB(
45	            t.screenPadding,
46	            6,
47	            t.screenPadding,
48	            t.screenPadding,
49	          ),
50	          child: Column(
51	            children: [
52	              _MechanicHeader(puzzle: puzzle),
53	              const SizedBox(height: 8),
54	              Expanded(
55	                child: ChallengeBoard(
56	                  puzzle: puzzle,
57	                  cells: _cells,
58	                  errorCells: errors,
59	                  onTap: _tap,
60	                  locked: _completed,
61	                ),
62	              ),
63	              const SizedBox(height: 8),
64	              Row(
65	                children: [
66	                  Expanded(
67	                    child: OutlinedButton.icon(
68	                      onPressed: _history.isEmpty || _completed ? null : _undo,
69	                      icon: const Icon(Icons.undo_rounded),
70	                      label: const Text('Undo'),
71	                    ),
72	                  ),
73	                  const SizedBox(width: 10),
74	                  Expanded(
75	                    child: OutlinedButton.icon(
76	                      onPressed: _completed ? null : _confirmRestart,
77	                      icon: const Icon(Icons.refresh_rounded),
78	                      label: const Text('Restart'),
79	                    ),
80	                  ),
81	                ],
82	              ),
83	            ],
84	          ),
85	        ),
86	      ),
87	    );
88	  }
89	
90	  void _tap(int row, int column) {
91	    final index = puzzle.index(row, column);
92	    _history.add(List<Cell>.from(_cells));
93	    setState(() {
94	      _cells[index] = switch (_cells[index]) {
95	        Cell.empty => Cell.sun,
96	        Cell.sun => Cell.moon,
97	        Cell.moon => Cell.empty,
98	      };
99	    });
100	    if (ChallengeRules.isSolved(puzzle, _cells)) {
101	      _finish();
102	    }
103	  }
104	
105	  void _undo() {
106	    setState(() => _cells = _history.removeLast());
107	  }
108	
109	  Future<void> _confirmRestart() async {
110	    final restart = await showDialog<bool>(
111	      context: context,
112	      builder: (context) => AlertDialog(
113	        title: const Text('Restart challenge?'),
114	        content: const Text('Your progress on this puzzle will be cleared.'),
115	        actions: [
116	          TextButton(
117	            onPressed: () => Navigator.of(context).pop(false),
118	            child: const Text('Cancel'),
119	          ),
120	          FilledButton(
121	            onPressed: () => Navigator.of(context).pop(true),
122	            child: const Text('Restart'),
123	          ),
124	        ],
125	      ),
126	    );
127	    if (restart != true) return;
128	    setState(() {
129	      _cells = puzzle.initialCells();
130	      _history.clear();
131	    });
132	  }
133	
134	  Future<void> _finish() async {
135	    setState(() => _completed = true);
136	    await ref.read(challengeProgressProvider.notifier).complete(puzzle.id);
137	    if (!mounted) return;
138	    await showDialog<void>(
139	      context: context,
140	      barrierDismissible: false,
141	      builder: (context) => AlertDialog(
142	        icon: Icon(Icons.auto_awesome_rounded, color: context.tokens.success),
143	        title: const Text('Challenge complete'),
144	        content: Text('${puzzle.title} is solved.'),
145	        actions: [
146	          FilledButton(
147	            onPressed: () => Navigator.of(context).pop(),
148	            child: const Text('Back to pack'),
149	          ),
150	        ],
151	      ),
152	    );
153	    if (mounted) Navigator.of(context).pop();
154	  }
155	}
156	
157	class _MechanicHeader extends StatelessWidget {
158	  final ChallengePuzzle puzzle;
159	  const _MechanicHeader({required this.puzzle});
160	
161	  @override
162	  Widget build(BuildContext context) {
163	    final t = context.tokens;
164	    return Container(
165	      width: double.infinity,
166	      padding: const EdgeInsets.symmetric(horizontal: 14, vertical: 10),
167	      decoration: BoxDecoration(
168	        color: t.surface,
169	        borderRadius: BorderRadius.circular(t.cardRadius),
170	      ),
171	      child: Column(
172	        crossAxisAlignment: CrossAxisAlignment.start,
173	        children: [
174	          Row(
175	            children: [
176	              Text('${puzzle.rows}×${puzzle.columns}', style: t.mono),
177	              const SizedBox(width: 10),
178	              Expanded(child: Text(puzzle.subtitle, style: t.label)),
179	            ],
180	          ),
181	          const SizedBox(height: 5),
182	          Text(_instruction(puzzle), style: t.body),
183	        ],
184	      ),
185	    );
186	  }
187	
188	  String _instruction(ChallengePuzzle puzzle) {
189	    final parts = <String>[];
190	    if (puzzle.cages.isNotEmpty) {
191	      parts.add('numbers inside outlined regions count Suns in that region');
192	    }
193	    if (puzzle.rowBands.isNotEmpty || puzzle.columnBands.isNotEmpty) {
194	      parts.add(
195	        'numbers outside the grid count Sun/Moon runs in that row or column',
196	      );
197	    }
198	    if (puzzle.horizonFold) {
199	      parts.add('cells across the horizon are opposites');
200	    }
201	    return '${parts.join('; ')}.';
202	  }
203	}
204	
```

#### lib/ads/rewarded_ad_service.dart — show(method), RewardedAdService(class), preload(method), RewardedAdOutcome(enum)

```dart
1	import 'dart:async';
2	
3	import 'package:flutter/foundation.dart';
4	import 'package:google_mobile_ads/google_mobile_ads.dart';
5	
6	import 'ad_privacy_service.dart';
7	import 'ad_config.dart';
8	import 'ump_consent_service.dart';
9	
10	enum RewardedAdOutcome {
11	  earned,
12	  dismissed,
13	  adUnavailableOnline,
14	  offline,
15	  timeout,
16	  showFailed,
17	}
18	
19	/// Loads and shows rewarded video ads for the "watch to keep playing" gate.
20	///
21	/// Keeps one ad preloaded so the gate can present instantly, and reloads after
22	/// each show. If no ad is available (no fill, network error, or unsupported
23	/// platform) the service returns [RewardedAdOutcome.adUnavailableOnline] so the
24	/// caller can fall back to the premium gate or offline messaging.
25	class RewardedAdService {
26	  RewardedAdService._();
27	  static final RewardedAdService instance = RewardedAdService._();
28	
29	  @visibleForTesting
30	  static Future<RewardedAdOutcome> Function()? debugShowOverride;
31	
32	  RewardedAd? _ad;
33	  bool _loading = false;
34	
35	  /// Preload a rewarded ad if one isn't already loaded/loading. Safe to call
36	  /// repeatedly (e.g. on app start and after each show).
37	  void preload() {
38	    if (!AdConfig.supported ||
39	        !UmpConsentService.instance.canRequestAds ||
40	        _ad != null ||
41	        _loading) {
42	      return;
43	    }
44	    _loading = true;
45	    RewardedAd.load(
46	      adUnitId: AdConfig.rewardedUnitId,
47	      request: AdPrivacyService.instance.adRequest,
48	      rewardedAdLoadCallback: RewardedAdLoadCallback(
49	        onAdLoaded: (ad) {
50	          _ad = ad;
51	          _loading = false;
52	        },
53	        onAdFailedToLoad: (err) {
54	          _ad = null;
55	          _loading = false;
56	          debugPrint('RewardedAd failed to load: $err');
57	        },
58	      ),
59	    );
60	  }
61	
62	  /// Present the rewarded ad and return a typed outcome for the caller.
63	  Future<RewardedAdOutcome> show({
64	    Duration timeout = const Duration(seconds: 15),
65	  }) async {
66	    final override = debugShowOverride;
67	    if (override != null) return override();
68	    if (!AdConfig.supported || !UmpConsentService.instance.canRequestAds) {
69	      return RewardedAdOutcome.adUnavailableOnline;
70	    }
71	    final ad = _ad;
72	    if (ad == null) {
73	      // Nothing to show — kick off a load for next time and let the play
74	      // fall back rather than trapping the user.
75	      preload();
76	      return RewardedAdOutcome.adUnavailableOnline;
77	    }
78	    _ad = null; // a rewarded ad is single-use
79	
80	    final completer = Completer<bool>();
81	    var earned = false;
82	    ad.fullScreenContentCallback = FullScreenContentCallback(
83	      onAdDismissedFullScreenContent: (ad) {
84	        ad.dispose();
85	        preload();
86	        if (!completer.isCompleted) completer.complete(earned);
87	      },
88	      onAdFailedToShowFullScreenContent: (ad, err) {
89	        ad.dispose();
90	        preload();
91	        debugPrint('RewardedAd failed to show: $err');
92	        if (!completer.isCompleted) completer.completeError(err);
93	      },
94	    );
95	
96	    try {
97	      ad.show(onUserEarnedReward: (_, _) => earned = true);
98	      await completer.future.timeout(timeout);
99	      return earned ? RewardedAdOutcome.earned : RewardedAdOutcome.dismissed;
100	    } on TimeoutException {
101	      ad.dispose();
102	      preload();
103	      return RewardedAdOutcome.timeout;
104	    } catch (err) {
105	      debugPrint('RewardedAd failed to show: $err');
106	      return RewardedAdOutcome.showFailed;
107	    }
108	  }
109	}
```

#### experiments/deductive-mechanics/verify.py — base_complete_ok(function), backtrack(function), solve(function), col_ok_so_far(function), row_matches_givens(function), +1 more

```python
1	#!/usr/bin/env python3
2	"""Uniqueness verifier / ground-truth solver for experimental Sundown mechanics.
3	
4	The base game (Sundown / Tango-like):
5	  - N x N grid, each cell 'S' (sun) or 'M' (moon).
6	  - Each row and column has exactly N/2 of each symbol.
7	  - No three of the same symbol consecutive (horizontally or vertically).
8	  - '=' edges: joined cells equal; 'x' edges: joined cells opposite.
9	  - No two rows identical; no two columns identical.
10	
11	This module enumerates EVERY full grid satisfying the base rules plus any
12	extra mechanic constraints, so we can confirm a puzzle has exactly one
13	solution (i.e. is fair / deduction-solvable in principle).
14	
15	Enumeration is row-by-row over precomputed legal row patterns, with column
16	pruning -- fast for N=4 and N=6.
17	"""
18	from itertools import product
19	from functools import lru_cache
20	
21	
22	def legal_rows(n):
23	    """All length-n S/M strings with n/2 of each and no 3 consecutive same."""
24	    half = n // 2
25	    out = []
26	    for bits in product('SM', repeat=n):
27	        if bits.count('S') != half:
28	            continue
29	        ok = True
30	        for i in range(n - 2):
31	            if bits[i] == bits[i + 1] == bits[i + 2]:
32	                ok = False
33	                break
34	        if ok:
35	            out.append(''.join(bits))
36	    return out
37	
38	
39	def col_ok_so_far(grid, n):
40	    """Partial-column check: no 3 vertical same, no column over half."""
41	    half = n // 2
42	    rows = len(grid)
43	    for c in range(n):
44	        col = [grid[r][c] for r in range(rows)]
45	        if col.count('S') > half or col.count('M') > half:
46	            return False
47	        for r in range(rows - 2):
48	            if col[r] == col[r + 1] == col[r + 2]:
49	                return False
50	    return True
51	
52	
53	def base_complete_ok(grid, n):
54	    """Final checks that can only be done on a full grid."""
55	    half = n // 2
56	    # columns balanced
57	    for c in range(n):
58	        col = ''.join(grid[r][c] for r in range(n))
59	        if col.count('S') != half:
60	            return False
61	    # no duplicate columns
62	    cols = [''.join(grid[r][c] for r in range(n)) for c in range(n)]
63	    if len(set(cols)) != n:
64	        return False
65	    # no duplicate rows
66	    if len(set(grid)) != n:
67	        return False
68	    return True
69	
70	
71	def solve(n, givens=None, edges=None, extra=None, limit=5):
72	    """Return up to `limit` full solutions.
73	
74	    givens: dict (r,c) -> 'S'/'M'      fixed cells
75	    edges:  list of (r1,c1,r2,c2,kind) kind in {'=','x'}  (works at any range)
76	    extra:  predicate(grid)->bool      mechanic-specific full-grid constraint
77	    """
78	    givens = givens or {}
79	    edges = edges or []
80	    rows_by_len = legal_rows(n)
81	    sols = []
82	
83	    def row_matches_givens(r, pattern):
84	        for c in range(n):
85	            if (r, c) in givens and givens[(r, c)] != pattern[c]:
86	                return False
87	        return True
88	
89	    candidates = [[p for p in rows_by_len if row_matches_givens(r, p)]
90	                  for r in range(n)]
91	
92	    grid = []
93	
94	    def backtrack(r):
95	        if len(sols) >= limit:
96	            return
97	        if r == n:
98	            if not base_complete_ok(tuple(grid), n):
99	                return
100	            full = tuple(grid)
101	            for (r1, c1, r2, c2, kind) in edges:
102	                a, b = full[r1][c1], full[r2][c2]
103	                if kind == '=' and a != b:
104	                    return
105	                if kind == 'x' and a == b:
106	                    return
107	            if extra is not None and not extra(full):
108	                return
109	            sols.append(full)
110	            return
111	        for pat in candidates[r]:
112	            grid.append(pat)
113	            if col_ok_so_far(grid, n):
114	                backtrack(r + 1)
115	            grid.pop()
116	
117	    backtrack(0)
118	    return sols
119	
120	
121	def show(grid):
122	    return '\n'.join(' '.join(row) for row in grid)
123	
124	
125	# ---------- mechanic constraint builders ----------
126	
127	def runs(line):
128	    """Number of maximal same-symbol runs in a line string."""
129	    return 1 + sum(1 for i in range(1, len(line)) if line[i] != line[i - 1])
130	
131	
132	def band_extra(row_clues, col_clues):
133	    """row_clues/col_clues: dict index->expected run count (partial ok)."""
134	    def pred(grid):
135	        n = len(grid)
136	        for r, want in row_clues.items():
137	            if runs(grid[r]) != want:
138	                return False
139	        for c, want in col_clues.items():
140	            col = ''.join(grid[r][c] for r in range(n))
141	            if runs(col) != want:
142	                return False
143	        return True
144	    return pred
145	
146	
147	def antisym_extra():
148	    """180-degree rotational ANTI-symmetry: (r,c) opposite of (N-1-r,N-1-c)."""
149	    def pred(grid):
150	        n = len(grid)
151	        for r in range(n):
152	            for c in range(n):
153	                if grid[r][c] == grid[n - 1 - r][n - 1 - c]:
154	                    return False
155	        return True
156	    return pred
157	
158	
159	def cage_extra(cages):
160	    """cages: list of (cells, sun_count) ; cells = list of (r,c)."""
161	    def pred(grid):
162	        for cells, want in cages:
163	            s = sum(1 for (r, c) in cells if grid[r][c] == 'S')
164	            if s != want:
165	                return False
166	        return True
167	    return pred
168	
169	
170	if __name__ == '__main__':
171	    # smoke: a plain 4x4 base puzzle space size
172	    print('legal 4-rows:', len(legal_rows(4)))
173	    print('legal 6-rows:', len(legal_rows(6)))
```


... (output truncated to budget; the source above is complete and verbatim — treat it as already Read. For any area not covered, run another codegraph_explore with the specific names — do NOT Read these files.)
