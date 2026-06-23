## Exploration: Suppose we change the Game model to track a new per-cell annotation state. What code would likely need updates, what tests should be run or added, and what are the risky dependencies?

Found 186 symbols across 46 files.

### Blast radius — what depends on these (update/verify before editing)

- `NewGameScreen` (lib/ui/screens/new_game_screen.dart:11) — 3 callers in `lib/ui/screens/home_screen.dart`, `lib/ui/screens/stats_screen.dart`, `lib/ui/screens/new_game_screen.dart`; ⚠️ no covering tests found
- `_NewGameScreenState` (lib/ui/screens/new_game_screen.dart:17) — 1 caller in `lib/ui/screens/new_game_screen.dart`; ⚠️ no covering tests found
- `_ChallengeGameScreenState` (lib/ui/screens/challenge_game_screen.dart:19) — 1 caller in `lib/ui/screens/challenge_game_screen.dart`; ⚠️ no covering tests found
- `_GameScreenState` (lib/ui/screens/game_screen.dart:30) — 1 caller in `lib/ui/screens/game_screen.dart`; ⚠️ no covering tests found
- `addAll` (lib/ui/state/providers.dart:453) — 6 callers in `lib/domain/challenge/challenge_puzzle.dart`, `lib/domain/solver/techniques.dart`, `lib/ui/screens/game_screen.dart`; ⚠️ no covering tests found

### Source Code

> The code below is the **verbatim, current on-disk source** of these files — re-read from disk on this call and line-numbered, byte-for-byte identical to what the Read tool returns. It is NOT a summary, outline, or stale cache. Treat each block as a Read you have already performed: do not Read a file shown here.

#### lib/domain/difficulty.dart — Difficulty(enum), code(method)

```dart
1	/// Difficulty bands — determined by the *cumulative* solving effort a puzzle
2	/// requires: the sum of every applied technique's weight (see
3	/// [Technique.weight] and [SolveResult.score]).
4	///
5	/// Because that sum grows with the number of cells, the band cut-points are
6	/// expressed relative to board area (n²) so a label means the same "effort
7	/// density" at every size. See [fromScore].
8	enum Difficulty {
9	  easy,
10	  medium,
11	  hard,
12	  expert;
13	
14	  /// Upper cut-points for easy / medium / hard, as effort-per-cell (score ÷ n²).
15	  /// At or above the last value is [expert]. Calibrated from the generator's
16	  /// achievable score range across sizes 6–12.
17	  static const List<double> _bandsPerCell = [0.9, 1.35, 1.9];
18	
19	  /// Classify a puzzle of the given [size] from its cumulative solve [score].
20	  static Difficulty fromScore(int size, int score) {
21	    final perCell = score / (size * size);
22	    if (perCell < _bandsPerCell[0]) return Difficulty.easy;
23	    if (perCell < _bandsPerCell[1]) return Difficulty.medium;
24	    if (perCell < _bandsPerCell[2]) return Difficulty.hard;
25	    return Difficulty.expert;
26	  }
27	
28	  /// Short single-letter code for puzzle codes (`S6E-…`).
29	  String get code => switch (this) {
30	        Difficulty.easy => 'E',
31	        Difficulty.medium => 'M',
32	        Difficulty.hard => 'H',
33	        Difficulty.expert => 'X',
34	      };
35	
36	  static Difficulty fromCode(String s) => switch (s) {
37	        'E' => Difficulty.easy,
38	        'M' => Difficulty.medium,
39	        'H' => Difficulty.hard,
40	        'X' => Difficulty.expert,
41	        _ => throw ArgumentError('not a difficulty code: $s'),
42	      };
43	}
```

#### lib/ui/screens/challenge_game_screen.dart — puzzle(references), build(calls), of(calls), Cell(references), ChallengePuzzle(references), +25 more

```dart
7	import '../theme/sundown_theme.dart';
8	import '../widgets/challenge_board.dart';
9	
10	class ChallengeGameScreen extends ConsumerStatefulWidget {
11	  final ChallengePuzzle puzzle;
12	  const ChallengeGameScreen({super.key, required this.puzzle});
13	
14	  @override
15	  ConsumerState<ChallengeGameScreen> createState() =>
16	      _ChallengeGameScreenState();
17	}
18	
19	class _ChallengeGameScreenState extends ConsumerState<ChallengeGameScreen> {
20	  late List<Cell> _cells;
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

#### lib/ui/screens/new_game_screen.dart — Difficulty(references), watch(calls), _diffName(calls), copyWith(calls), set(calls), +18 more

```dart
8	import 'game_screen.dart';
9	import 'premium_upsell_dialog.dart';
10	
11	class NewGameScreen extends ConsumerStatefulWidget {
12	  const NewGameScreen({super.key});
13	  @override
14	  ConsumerState<NewGameScreen> createState() => _NewGameScreenState();
15	}
16	
17	class _NewGameScreenState extends ConsumerState<NewGameScreen> {
18	  bool _busy = false;
19	
20	  @override
21	  Widget build(BuildContext context) {
22	    final t = context.tokens;
23	    final size = ref.watch(preferredSizeProvider);
24	    final diff = ref.watch(preferredDifficultyProvider);
25	    final premium = ref.watch(premiumProvider);
26	    return Scaffold(
27	      backgroundColor: t.background,
28	      appBar: AppBar(
29	        backgroundColor: t.background,
30	        foregroundColor: t.textPrimary,
31	        title: Text('New Puzzle', style: t.title),
32	        elevation: 0,
33	      ),
34	      body: SafeArea(
35	        child: Padding(
36	          padding: EdgeInsets.all(t.screenPadding),
37	          child: Column(
38	            crossAxisAlignment: CrossAxisAlignment.stretch,
39	            children: [
40	              Text('Grid size', style: t.label),
41	              const SizedBox(height: 4),
42	              Text(
43	                'Smaller grids finish faster; larger ones add more of a stretch.',
44	                style: t.label.copyWith(color: t.textMuted),
45	              ),
46	              const SizedBox(height: 8),
47	              Wrap(
48	                spacing: 8,
49	                children: [
50	                  for (final n in [6, 8, 10, 12])
51	                    _Choice<int>(
52	                      value: n,
53	                      selected: size,
54	                      label: '$n × $n',
55	                      onTap: (v) =>
56	                          ref.read(preferredSizeProvider.notifier).set(v),
57	                    ),
58	                ],
59	              ),
60	              const SizedBox(height: 24),
61	              Text('Difficulty', style: t.label),
62	              const SizedBox(height: 4),
63	              Text(
64	                'Expert is a Premium feature.',
65	                style: t.label.copyWith(color: t.textMuted),
66	              ),
67	              const SizedBox(height: 8),
68	              Row(
69	                children: [
70	                  for (var i = 0; i < Difficulty.values.length; i++) ...[
71	                    if (i > 0) const SizedBox(width: 8),
72	                    Expanded(
73	                      child: () {
74	                        final d = Difficulty.values[i];
75	                        final locked = d == Difficulty.expert && !premium;
76	                        return _Choice<Difficulty>(
77	                          value: d,
78	                          selected: diff,
79	                          label: _diffName(d),
80	                          fill: true,
81	                          locked: locked,
82	                          semanticsLabel: locked
83	                              ? '${_diffName(d)}, Premium feature'
84	                              : _diffName(d),
85	                          onTap: (v) {
86	                            if (locked) {
87	                              _promptExpertPremium();
88	                            } else {
89	                              ref
90	                                  .read(preferredDifficultyProvider.notifier)
91	                                  .set(v);
92	                            }
93	                          },
94	                        );
95	                      }(),
96	                    ),
97	                  ],
98	                ],
99	              ),
100	              const Spacer(),
101	              if (_busy)
102	                Padding(
103	                  padding: const EdgeInsets.all(20),
104	                  child: Column(
105	                    children: [
106	                      CircularProgressIndicator(color: t.accent),
107	                      const SizedBox(height: 12),
108	                      Text('Generating puzzle…', style: t.body),
109	                      Text(
110	                        '${size == 12
111	                            ? "12×12"
112	                            : size == 10
113	                            ? "10×10"
114	                            : "this"} can take a moment',
115	                        style: t.label,
116	                      ),
117	                    ],
118	                  ),
119	                ),
120	              Center(
121	                child: ControlButton(
122	                  icon: Icons.play_arrow_rounded,
123	                  label: 'Start',
124	                  primary: true,
125	                  onPressed: _busy ? null : () => _start(size, diff),
126	                ),
127	              ),
128	            ],
129	          ),
130	        ),
131	      ),
132	    );
133	  }
134	
135	  Future<void> _start(int size, Difficulty d) async {
136	    setState(() => _busy = true);
137	    try {
138	      // Run in microtask so spinner paints first; generator is sync but can
139	      // take several hundred ms at 10×10/12×12.
140	      await Future<void>.delayed(const Duration(milliseconds: 16));
141	      await ref.read(gameProvider.notifier).start(size, d);
142	      await ref.read(storageProvider).setPreferredSize(size);
143	      await ref.read(storageProvider).setPreferredDifficulty(d.code);
144	      if (!mounted) return;
145	      Navigator.of(
146	        context,
147	      ).pushReplacement(MaterialPageRoute(builder: (_) => const GameScreen()));
148	    } catch (e) {
149	      if (!mounted) return;
150	      setState(() => _busy = false);
151	      ScaffoldMessenger.of(
152	        context,
153	      ).showSnackBar(SnackBar(content: Text('Could not generate: $e')));
154	    }
155	  }
156	
157	  /// Tapping a locked Expert chip opens the Premium upsell so the player can
158	  /// buy Unlimited Play (or redeem a code) without leaving the New Puzzle flow.
159	  Future<void> _promptExpertPremium() {
160	    return PremiumUpsellDialog.show(
161	      context,
162	      title: 'Expert is Premium',
163	      message:
164	          'Unlock Expert puzzles, all premium themes, custom symbols, and '
165	          'unlimited play with a one-time purchase.',
166	    );
167	  }
168	}
169	
170	class _Choice<T> extends StatelessWidget {
```


... (output truncated to budget; the source above is complete and verbatim — treat it as already Read. For any area not covered, run another codegraph_explore with the specific names — do NOT Read these files.)
