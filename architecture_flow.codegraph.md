## Exploration: Explain how a new game session is created, persisted, restored, and shown on the game screen. Include the main classes/functions and the file paths.

Found 170 symbols across 43 files.

### Blast radius — what depends on these (update/verify before editing)

- `_NewGameScreenState` (lib/ui/screens/new_game_screen.dart:17) — 1 caller in `lib/ui/screens/new_game_screen.dart`; ⚠️ no covering tests found
- `ChallengeGameScreen` (lib/ui/screens/challenge_game_screen.dart:10) — 3 callers in `lib/ui/screens/challenge_pack_screen.dart`, `lib/ui/screens/challenge_game_screen.dart`; tests: `test/ui/screens/challenge_pack_screen_test.dart`
- `_ChallengeGameScreenState` (lib/ui/screens/challenge_game_screen.dart:19) — 1 caller in `lib/ui/screens/challenge_game_screen.dart`; ⚠️ no covering tests found
- `NewGameScreen` (lib/ui/screens/new_game_screen.dart:11) — 3 callers in `lib/ui/screens/home_screen.dart`, `lib/ui/screens/stats_screen.dart`, `lib/ui/screens/new_game_screen.dart`; ⚠️ no covering tests found
- `GameScreen` (lib/ui/screens/game_screen.dart:23) — 5 callers in `lib/ui/screens/home_screen.dart`, `lib/ui/screens/new_game_screen.dart`, `lib/ui/screens/play_by_code_dialog.dart`, `lib/ui/screens/game_screen.dart`; tests: `test/ui/screens/game_header_test.dart`

### Source Code

> The code below is the **verbatim, current on-disk source** of these files — re-read from disk on this call and line-numbered, byte-for-byte identical to what the Read tool returns. It is NOT a summary, outline, or stale cache. Treat each block as a Read you have already performed: do not Read a file shown here.

#### lib/ui/screens/game_screen.dart — watch(calls), instantiates(instantiates), of(calls), select(calls), build(method), +36 more

```dart
20	import 'watch_ad_dialog.dart';
21	
22	/// Main play screen. Drives a single [Game] through to completion.
23	class GameScreen extends ConsumerStatefulWidget {
24	  const GameScreen({super.key});
25	
26	  @override
27	  ConsumerState<GameScreen> createState() => _GameScreenState();
28	}
29	
30	class _GameScreenState extends ConsumerState<GameScreen>
31	    with WidgetsBindingObserver {
32	  /// Hint reveal stage:
33	  ///   0 = no hint pending
34	  ///   1 = region highlight
35	  ///   2 = technique label shown
36	  ///   3 = cell about to be revealed (next tap consumes)
37	  int _hintStage = 0;
38	  Deduction? _pendingHint;
39	
40	  late final ConfettiController _confetti = ConfettiController(
41	    duration: const Duration(milliseconds: 1800),
42	  );
43	
44	  // Cached so dispose() can call deactivate() without going through ref,
45	  // which throws a StateError when the widget is already unmounted.
46	  late final GameController _gameController;
47	
48	  @override
49	  void initState() {
50	    super.initState();
51	    _gameController = ref.read(gameProvider.notifier);
52	    // Observe app lifecycle so the timer pauses when the app is backgrounded
53	    // *while the board is on screen* — and never runs on the home screen, since
54	    // this observer only exists while the GameScreen is mounted.
55	    WidgetsBinding.instance.addObserver(this);
56	    // Every entry into the game screen counts as a play. Enforce the daily
57	    // ad gate before the board becomes interactive, then start the timer.
58	    WidgetsBinding.instance.addPostFrameCallback(
59	      (_) => _enforceGateThenActivate(),
60	    );
61	  }
62	
63	  /// Pause the puzzle timer while the app is backgrounded so off-screen wall
64	  /// time doesn't inflate the player's solve time; resume on return. Scoped to
65	  /// the game screen — the home screen has no such observer.
66	  @override
67	  void didChangeAppLifecycleState(AppLifecycleState s) {
68	    switch (s) {
69	      case AppLifecycleState.paused:
70	      case AppLifecycleState.inactive:
71	      case AppLifecycleState.hidden:
72	        _gameController.deactivate();
73	      case AppLifecycleState.resumed:
74	        _gameController.activate();
75	      case AppLifecycleState.detached:
76	        break;
77	    }
78	  }
79	
80	  /// If the player is out of free plays for the day, present the rewarded-ad
81	  /// gate. Watching grants bonus plays and lets the game proceed; backing out
82	  /// returns to the home screen without consuming a play.
83	  Future<void> _enforceGateThenActivate() async {
84	    final gate = ref.read(adGateProvider.notifier);
85	    if (gate.needsAd) {
86	      final earned = await WatchAdDialog.show(
87	        context,
88	        fallbackTier: ref.read(adFallbackTierProvider),
89	      );
90	      if (!mounted) return;
91	      if (!earned) {
92	        Navigator.of(context).maybePop();
93	        return;
94	      }
95	    }
96	    gate.registerPlay();
97	    _gameController.activate();
98	  }
99	
100	  @override
101	  void dispose() {
102	    WidgetsBinding.instance.removeObserver(this);
103	    _gameController.deactivate();
104	    _confetti.dispose();
105	    super.dispose();
106	  }
107	
108	  void _onHint() {
109	    final g = ref.read(gameProvider);
110	    if (g == null || g.completed) return;
111	    // If the player edits the board between hint stages, recompute. The solver
112	    // returns a conclusive result: a forced move for a legal state, or a
113	    // contradiction for one carrying a mistake (overt or silent).
114	    final result = ref.read(nextHintProvider);
115	    if (result == null) return;
116	
117	    switch (result.status) {
118	      case HintStatus.contradiction:
119	        _showHintMessage(
120	          'There are mistakes on the board — fix them before using a hint.',
121	        );
122	        return;
123	      case HintStatus.ambiguous:
124	        // Only reachable on a non-unique (hand-built) board; nothing is forced.
125	        _showHintMessage('No deduction available from this state.');
126	        return;
127	      case HintStatus.solved:
128	        return; // board already complete — completion handling takes over
129	      case HintStatus.ok:
130	        break;
131	    }
132	    final fresh = result.deduction!;
133	
134	    // Click 1 — new hint target: show witness cells and count it.
135	    if (_pendingHint == null ||
136	        _pendingHint!.r != fresh.r ||
137	        _pendingHint!.c != fresh.c ||
138	        _pendingHint!.value != fresh.value) {
139	      _pendingHint = fresh;
140	      ref.read(gameProvider.notifier).recordHint();
141	      setState(() => _hintStage = 1);
142	      return;
143	    }
144	    if (_hintStage < 3) {
145	      setState(() => _hintStage += 1);
146	      if (_hintStage == 2) {
147	        // Click 2 — show reason text: count it.
148	        ref.read(gameProvider.notifier).recordHint();
149	      }
150	      if (_hintStage == 3) {
151	        // Click 3 — reveal: answer already paid for; do NOT count again.
152	        ref.read(gameProvider.notifier).revealHint(fresh);
153	        setState(() {
154	          _hintStage = 0;
155	          _pendingHint = null;
156	        });
157	      }
158	    }
159	  }
160	
161	  void _showHintMessage(String message) {
162	    ScaffoldMessenger.of(
163	      context,
164	    ).showSnackBar(SnackBar(content: Text(message)));
165	  }
166	
167	  void _onTap(int r, int c) {
168	    if (_hintStage != 0) {
169	      setState(() {
170	        _hintStage = 0;
171	        _pendingHint = null;
172	      });
173	    }
174	    ref.read(gameProvider.notifier).tap(r, c);
175	  }
176	
177	  @override
178	  Widget build(BuildContext context) {
179	    ref.listen(achievementsProvider, (previous, next) {
180	      final before = previous?.map((a) => a.id).toSet() ?? const <String>{};
181	      final fresh = next.where((a) => !before.contains(a.id)).toList();
182	      if (fresh.isEmpty || !mounted) return;
183	      ref.read(freshAchievementUnlocksProvider.notifier).addAll(fresh);
184	      final name = achievementDefinitions
185	          .firstWhere((d) => d.id == fresh.last.id)
186	          .name;
187	      ScaffoldMessenger.of(
188	        context,
189	      ).showSnackBar(SnackBar(content: Text('Achievement unlocked: $name')));
190	    });
191	    ref.listen<Game?>(gameProvider, (previous, next) {
192	      if (next == null || !next.completed || previous?.completed == true) {
193	        return;
194	      }
195	      WidgetsBinding.instance.addPostFrameCallback((_) {
196	        Future.delayed(const Duration(milliseconds: 330), () {
197	          if (!context.mounted) return;
198	          _confetti.play();
199	          _showSolvedDialog(context, ref, next);
200	        });
201	      });
202	    });
203	    final t = context.tokens;
204	    final hasGame = ref.watch(gameProvider.select((g) => g != null));
205	
206	    if (!hasGame) {
207	      return Scaffold(
208	        backgroundColor: t.background,
209	        body: Center(child: Text('No active game.', style: t.body)),
210	      );
211	    }
212	
213	    return Scaffold(
214	      backgroundColor: t.background,
215	      body: Stack(
216	        children: [
217	          SafeArea(
218	            child: Column(
219	              children: [
220	                const _Header(),
221	                const SizedBox(height: 8),
222	                Expanded(
223	                  child: _GameBoardArea(
224	                    onTap: _onTap,
225	                    hintStage: _hintStage,
226	                    pendingHint: _pendingHint,
227	                  ),
228	                ),
229	                const SizedBox(height: 8),
230	                _Controls(onHint: _onHint, hintStage: _hintStage),
231	                const SizedBox(height: 12),
232	              ],
233	            ),
234	          ),
235	          // Confetti emitter centered top, blasts downward.
236	          Align(
237	            alignment: Alignment.topCenter,
238	            child: ConfettiWidget(
239	              confettiController: _confetti,
240	              blastDirection: math.pi / 2,
241	              maxBlastForce: 24,
242	              minBlastForce: 12,
243	              emissionFrequency: 0.04,
244	              numberOfParticles: 18,
245	              gravity: 0.25,
246	              shouldLoop: false,
247	              colors: [t.sun, t.moon, t.accent, t.success],
248	            ),
249	          ),
250	        ],
251	      ),
252	    );
253	  }
254	}
255	
256	class _Header extends ConsumerWidget {
257	  const _Header();
258	
259	  @override
260	  Widget build(BuildContext context, WidgetRef ref) {
261	    final t = context.tokens;
262	    final meta = ref.watch(
263	      gameProvider.select(
264	        (g) => g == null
265	            ? null
266	            : (
267	                size: g.size,
268	                difficulty: g.puzzle.difficulty.code,
269	                hintsUsed: g.hintsUsed,
270	                mistakes: g.mistakes,
271	                code: g.puzzle.code,
272	                completed: g.completed,
273	              ),
274	      ),
275	    );
276	    final elapsed = _fmt(
277	      ref.watch(gameProvider.select((g) => g?.elapsedMs ?? 0)),
278	    );
279	    if (meta == null) {
280	      return const SizedBox.shrink();
281	    }
282	    return Padding(
283	      padding: EdgeInsets.fromLTRB(t.screenPadding, 8, t.screenPadding, 0),
284	      child: Column(
285	        crossAxisAlignment: CrossAxisAlignment.stretch,
286	        children: [
287	          // Row 1: nav buttons on the left, puzzle meta on the right.
288	          // Keeping the 44 px icon targets on the far left and the difficulty
289	          // + code on the far right makes intentional use of the full width
290	          // while guaranteeing neither side squeezes the other.
291	          Row(
292	            crossAxisAlignment: CrossAxisAlignment.center,
293	            children: [
294	              IconButton(
295	                tooltip: 'Back',
296	                icon: const Icon(Icons.arrow_back_rounded),
297	                color: t.textPrimary,
298	                padding: EdgeInsets.zero,
299	                constraints: const BoxConstraints(minWidth: 44, minHeight: 44),
300	                onPressed: () => Navigator.of(context).maybePop(),
301	              ),
302	              const SizedBox(width: 4),
303	              IconButton(
304	                tooltip: 'How to Play',
305	                icon: const Icon(Icons.help_outline_rounded),
306	                color: t.textPrimary,
307	                padding: EdgeInsets.zero,
308	                constraints: const BoxConstraints(minWidth: 44, minHeight: 44),
309	                onPressed: () async {
310	                  final controller = ref.read(gameProvider.notifier);
311	                  controller.deactivate();
312	                  await Navigator.of(context).push(
313	                    MaterialPageRoute(builder: (_) => const HowToPlayScreen()),
314	                  );
315	                  if (context.mounted && !meta.completed) {
316	                    controller.activate();
317	                  }
318	                },
319	              ),
320	              const Spacer(),
321	              // Difficulty label + puzzle code, right-aligned.
322	              // Wrap handles text-scale overflow gracefully without extra rows.
323	              Flexible(
324	                child: Wrap(
325	                  spacing: 6,
326	                  runSpacing: 4,
327	                  alignment: WrapAlignment.end,
328	                  crossAxisAlignment: WrapCrossAlignment.center,
329	                  children: [
330	                    Text(
331	                      '${meta.size}×${meta.size} · ${_diffName(meta.difficulty)}',
332	                      style: t.label,
333	                      textAlign: TextAlign.end,
334	                    ),
335	                    PuzzleCodeChip(code: meta.code),
336	                  ],
337	                ),
338	              ),
339	            ],
340	          ),
341	          const SizedBox(height: 8),
342	          // Row 2: stat chips spread evenly across the full width.
343	          // IntrinsicHeight + Row(crossAxisAlignment.stretch) makes all three
344	          // chips the same height regardless of value length.
345	          IntrinsicHeight(
346	            child: Row(
347	              crossAxisAlignment: CrossAxisAlignment.stretch,
348	              children: [
349	                Expanded(
350	                  child: StatChip(
351	                    label: 'time',
352	                    value: elapsed,
353	                    icon: Icons.timer_outlined,
354	                  ),
355	                ),
356	                const SizedBox(width: 8),
357	                Expanded(
358	                  child: StatChip(
359	                    label: 'hints',
360	                    value: '${meta.hintsUsed}',
361	                    icon: Icons.lightbulb_outline,
362	                  ),
363	                ),
364	                const SizedBox(width: 8),
365	                Expanded(
366	                  child: StatChip(
367	                    label: 'mistakes',
368	                    value: '${meta.mistakes}',
369	                    icon: Icons.error_outline,
370	                  ),
371	                ),
372	              ],
373	            ),
374	          ),
375	        ],
376	      ),
377	    );
378	  }
379	}
380	
381	class _GameBoardArea extends ConsumerWidget {
382	  final CellTap onTap;
383	  final int hintStage;
384	  final Deduction? pendingHint;
385	
386	  const _GameBoardArea({
387	    required this.onTap,
388	    required this.hintStage,
389	    required this.pendingHint,
390	  });
391	
392	  @override
393	  Widget build(BuildContext context, WidgetRef ref) {
394	    final t = context.tokens;
395	    final game = ref.watch(
396	      gameProvider.select(
397	        (g) => g == null
398	            ? null
399	            : (puzzle: g.puzzle, board: g.board, completed: g.completed),
400	      ),
401	    );
402	    if (game == null) return const SizedBox.shrink();
403	
404	    final highlights = <(int, int)>{};
405	    String? hintReason;
406	    if (pendingHint != null) {
407	      if (hintStage >= 1) {
408	        highlights.addAll(pendingHint!.witnessCells);
409	      }
410	      if (hintStage >= 2) hintReason = pendingHint!.reason;
411	    }
412	    final errors = ref.watch(violationCellsProvider);
413	
414	    return Padding(
415	      padding: EdgeInsets.symmetric(horizontal: t.screenPadding),
416	      child: Stack(
417	        children: [
418	          BoardWidget(
419	            puzzle: game.puzzle,
420	            board: game.board,
421	            onTap: onTap,
422	            highlightCells: highlights,
423	            errorCells: errors,
424	            locked: game.completed,
425	          ),
426	          Positioned(
427	            left: 0,
428	            right: 0,
429	            bottom: 8,
430	            child: AnimatedSwitcher(
431	              duration: const Duration(milliseconds: 200),
432	              transitionBuilder: (child, anim) => FadeTransition(
433	                opacity: anim,
434	                child: SlideTransition(
435	                  position: Tween<Offset>(
436	                    begin: const Offset(0, 0.2),
437	                    end: Offset.zero,
438	                  ).animate(anim),
439	                  child: child,
440	                ),
441	              ),
442	              child: hintReason == null
443	                  ? const SizedBox.shrink(key: ValueKey('none'))
444	                  : _HintBanner(key: ValueKey(hintReason), text: hintReason),
445	            ),
446	          ),
447	        ],
448	      ),
449	    );
450	  }
451	}
452	
453	class _Controls extends ConsumerWidget {
454	  final VoidCallback? onHint;
455	  final int hintStage;
456	  const _Controls({required this.onHint, required this.hintStage});
457	
458	  @override
459	  Widget build(BuildContext context, WidgetRef ref) {
460	    final game = ref.watch(
461	      gameProvider.select(
462	        (g) => g == null
463	            ? null
464	            : (historyEmpty: g.history.isEmpty, completed: g.completed),
465	      ),
466	    );
467	    final t = context.tokens;
468	    if (game == null) return const SizedBox.shrink();
469	    final hintLabel = switch (hintStage) {
470	      0 => 'Hint',
471	      1 => 'Where',
472	      2 => 'Why',
473	      _ => 'Reveal',
474	    };
475	    return Padding(
476	      padding: EdgeInsets.symmetric(horizontal: t.screenPadding),
477	      child: Wrap(
478	        alignment: WrapAlignment.center,
479	        spacing: 8,
480	        runSpacing: 8,
481	        children: [
482	          ControlButton(
483	            icon: Icons.undo_rounded,
484	            label: 'Undo',
485	            onPressed: game.historyEmpty || game.completed
486	                ? null
487	                : () => ref.read(gameProvider.notifier).undo(),
488	          ),
489	          ControlButton(
490	            icon: Icons.refresh_rounded,
491	            label: 'Restart',
492	            onPressed: game.historyEmpty || game.completed
493	                ? null
494	                : () => ref.read(gameProvider.notifier).restart(),
495	          ),
496	          ControlButton(
497	            icon: Icons.lightbulb_outline,
498	            label: hintLabel,
499	            primary: hintStage > 0,
500	            onPressed: game.completed ? null : onHint,
501	            badge: hintStage > 0 ? hintStage : null,
502	          ),
503	        ],
504	      ),
505	    );
506	  }
507	}
508	
509	class _HintBanner extends StatelessWidget {

... (output truncated to budget; the source above is complete and verbatim — treat it as already Read. For any area not covered, run another codegraph_explore with the specific names — do NOT Read these files.)
