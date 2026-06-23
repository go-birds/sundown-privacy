# Prompt
Investigate why a completed puzzle might not transition to the completed state or show completion UI. Trace the likely state flow and identify the most relevant files/functions to inspect or change.


# rg -n 'complete|completed|isComplete|win|finish|solved|transition'
test/domain/rules_test.dart:118:    test('accepts a known-valid completed board', () {
test/domain/achievements_test.dart:47:  solvedAt: DateTime(2026, 1, 1),
lib/domain/challenge/challenge_puzzle.dart:155:    final solvedCells = [
lib/domain/challenge/challenge_puzzle.dart:160:    if (!ChallengeRules.isSolved(this, solvedCells)) {
test/domain/solver_test.dart:180:    test('completes a near-solved board', () {
test/domain/solver_test.dart:192:      expect(r.solved, isTrue);
test/domain/solver_test.dart:193:      expect(r.finalBoard.isComplete, isTrue);
test/domain/solver_test.dart:211:      expect(r.solved, isFalse);
test/domain/solver_test.dart:238:    test('a solved board reports solved', () {
test/domain/solver_test.dart:240:      expect(Solver.hint(p.solution!).status, HintStatus.solved);
test/domain/solver_test.dart:271:            if (board.isComplete) continue; // covered by the solved-board case
test/domain/solver_test.dart:292:      while (!board.isComplete) {
test/domain/solver_test.dart:309:      // this immediately breaks a rule, the board can no longer be completed,
lib/domain/achievement.dart:249:  if ((summary.solvedByDifficulty[Difficulty.easy] ?? 0) >= 10) {
lib/domain/achievement.dart:252:  if ((summary.solvedByDifficulty[Difficulty.medium] ?? 0) >= 10) {
lib/domain/achievement.dart:255:  if ((summary.solvedByDifficulty[Difficulty.hard] ?? 0) >= 10) {
lib/domain/achievement.dart:258:  if ((summary.solvedByDifficulty[Difficulty.expert] ?? 0) >= 5) {
lib/domain/achievement.dart:291:  final solvedSizes = records.map((r) => r.size).toSet();
lib/domain/achievement.dart:292:  if ({6, 8, 10, 12}.every(solvedSizes.contains)) {
test/domain/generator_test.dart:16:        expect(r.solved, isTrue, reason: 'seed=$seed should be solvable');
test/domain/generator_test.dart:42:        expect(Solver.solve(p.clues).solved, isTrue);
test/domain/generator_test.dart:65:        expect(Solver.solve(puzzle.clues).solved, isTrue);
lib/domain/generator.dart:84:    if (!classification.solved) {
lib/domain/generator.dart:148:    // No-three at (r,c) — check the placement's row and column windows.
lib/domain/generator.dart:156:    // Uniqueness: a row finishes when we place its last cell (c == n-1);
lib/domain/generator.dart:157:    // a column finishes when we place its last cell (r == n-1, given the
lib/domain/generator.dart:158:    // row-major fill order). Reject if the completed row/col duplicates an
lib/domain/generator.dart:222:      if (Solver.solve(candidate, maxTier: cap).solved) {
lib/domain/generator.dart:239:      if (!result.solved) continue;
lib/domain/rules.dart:5:/// Rule-checking utilities for partial and complete boards.
lib/domain/rules.dart:17:      board.isComplete && firstViolation(board) == null;
lib/domain/game/game.dart:11:/// Riverpod controller can treat each transition as a single value change.
lib/domain/game/game.dart:34:  /// into the game on transitions.
lib/domain/game/game.dart:39:  final bool completed;
lib/domain/game/game.dart:54:    required this.completed,
lib/domain/game/game.dart:66:    completed: false,
lib/domain/game/game.dart:79:    if (completed) return this;
lib/domain/game/game.dart:92:    if (completed) return this;
lib/domain/game/game.dart:103:    return copyWith(board: newBoard, history: newHistory, completed: done);
lib/domain/game/game.dart:108:  /// Undoes the most recent move. No-op if history is empty or game completed.
lib/domain/game/game.dart:110:    if (completed || history.isEmpty) return this;
lib/domain/game/game.dart:133:      completed ? this : copyWith(elapsedMs: newElapsedMs);
lib/domain/game/game.dart:142:    bool? completed,
lib/domain/game/game.dart:152:    completed: completed ?? this.completed,
lib/domain/game/game_codec.dart:18:    'completed': g.completed,
lib/domain/game/game_codec.dart:41:      completed: j['completed'] as bool,
lib/domain/game/stats.dart:3:/// One completed-puzzle record. Stored in the stats history list.
lib/domain/game/stats.dart:11:  final DateTime solvedAt;
lib/domain/game/stats.dart:20:    required this.solvedAt,
lib/domain/game/stats.dart:30:        'solvedAt': solvedAt.toIso8601String(),
lib/domain/game/stats.dart:40:        solvedAt: DateTime.parse(j['solvedAt'] as String),
lib/domain/game/stats.dart:48:  final Map<Difficulty, int> solvedByDifficulty;
lib/domain/game/stats.dart:58:    required this.solvedByDifficulty,
lib/domain/game/stats.dart:85:      solvedByDifficulty: {for (final d in Difficulty.values) d: byDiff[d]!.length},
lib/domain/game/stats.dart:96:      final d = r.solvedAt.toUtc();
test/ui/screens/tutorial_screen_test.dart:48:      // the navigation transition.
test/ui/screens/tutorial_screen_test.dart:124:      // HomeScreen should be showing (no auto-launch).
lib/domain/solver/techniques.dart:304:// arrangement equals an already-completed parallel line, the other is forced.
lib/domain/solver/techniques.dart:319:  // Collect completed lines.
lib/domain/solver/techniques.dart:320:  final completed = <int, List<Cell>>{};
lib/domain/solver/techniques.dart:326:      completed[i] = line;
lib/domain/solver/techniques.dart:345:    for (final j in completed.keys) {
lib/domain/solver/techniques.dart:346:      final other = completed[j]!;
test/ui/screens/game_header_test.dart:6://        resumes it on return (unless the puzzle is already completed).
test/ui/screens/game_header_test.dart:130:      completed: false,
test/ui/screens/game_header_test.dart:186:        completed: false,
test/ui/screens/game_header_test.dart:215:    'and reactivates on return (puzzle not completed)',
test/ui/screens/game_header_test.dart:224:        completed: false,
test/ui/screens/game_header_test.dart:270:      // The timer must resume because the puzzle was not completed.
test/ui/screens/game_header_test.dart:276:            'with an incomplete puzzle',
test/ui/screens/game_header_test.dart:283:    'when the puzzle is already completed',
test/ui/screens/game_header_test.dart:285:      final completedGame = Game(
test/ui/screens/game_header_test.dart:292:        completed: true, // <-- puzzle done
test/ui/screens/game_header_test.dart:297:      final controller = _FakeGameController(completedGame);
test/ui/screens/game_header_test.dart:320:      // Because game.completed is true, activate() must NOT be called.
test/ui/screens/game_header_test.dart:324:        reason: 'activate() must NOT be called when the puzzle is completed',
test/ui/screens/game_header_test.dart:341:        completed: false,
test/ui/screens/game_header_test.dart:382:        completed: false,
test/ui/screens/game_header_test.dart:411:        completed: false,
test/ui/screens/game_header_test.dart:440:        completed: false,
test/ui/screens/game_header_test.dart:468:        completed: false,
lib/domain/solver/solver.dart:9:  final bool solved;
lib/domain/solver/solver.dart:16:    required this.solved,
lib/domain/solver/solver.dart:30:  /// trivially-complete board (empty trace).
lib/domain/solver/solver.dart:41:  /// The board cannot be completed — it holds a mistake, whether an outright
lib/domain/solver/solver.dart:46:  solved,
lib/domain/solver/solver.dart:63:  static const HintResult solved = HintResult._(HintStatus.solved);
lib/domain/solver/solver.dart:69:  /// new deduction. Stops when solved, contradicted, or stuck.
lib/domain/solver/solver.dart:82:          solved: false,
lib/domain/solver/solver.dart:89:      if (cur.isComplete) {
lib/domain/solver/solver.dart:91:          solved: true,
lib/domain/solver/solver.dart:101:          solved: false,
lib/domain/solver/solver.dart:129:  ///   2. Probe the first empty cell with a complete (but deductively-pruned)
lib/domain/solver/solver.dart:130:  ///      completability search. If *neither* value can complete the board, the
lib/domain/solver/solver.dart:137:  ///      completeness-forced value at the first empty cell. For a uniquely-
lib/domain/solver/solver.dart:143:    if (empty == null) return HintResult.solved;
lib/domain/solver/solver.dart:149:    // Neither completes → a placed cell silently contradicts the solution.
lib/domain/solver/solver.dart:151:    // Both complete → more than one solution. A generated puzzle is unique, so
lib/domain/solver/solver.dart:160:    // give the completeness-forced cell with an honest explanation.
lib/domain/solver/solver.dart:185:  /// True if [start] can be completed to a fully valid board. Uses the
lib/domain/solver/solver.dart:192:    if (r.solved) return true;
lib/domain/solver/solver.dart:201:  /// True if [state] can be solved by deduction alone — equivalently, has a
lib/domain/solver/solver.dart:205:    return r.solved;
lib/domain/solver/technique.dart:33:  /// Uniqueness: two rows (or cols), one complete and one with just two empty
lib/domain/solver/technique.dart:35:  /// that does NOT replicate the complete row.
lib/domain/solver/technique.dart:64:  /// minimizer's `maxTier` cap) — not for classifying a finished puzzle.
test/ui/stats_screen_test.dart:48:        solvedAt: DateTime.now().subtract(const Duration(hours: 2)),
lib/domain/board/puzzle.dart:6:/// A complete puzzle definition.
lib/domain/board/puzzle.dart:10:/// - [solution]: the unique completed solution. Optional — generators produce it,
lib/domain/board/puzzle.dart:11:///   but a [Puzzle] parsed from a code does not require it until solved.
lib/domain/board/board.dart:79:  bool get isComplete => cells.every((c) => c.isFilled);
lib/storage/storage_service.dart:38:  // Whether the first-run tutorial has been shown (completed or skipped).
lib/storage/storage_service.dart:227:  /// True once the first-run tutorial has been completed or skipped.
lib/ads/rewarded_ad_service.dart:80:    final completer = Completer<bool>();
lib/ads/rewarded_ad_service.dart:86:        if (!completer.isCompleted) completer.complete(earned);
lib/ads/rewarded_ad_service.dart:92:        if (!completer.isCompleted) completer.completeError(err);
lib/ads/rewarded_ad_service.dart:98:      await completer.future.timeout(timeout);
lib/ads/iap_service.dart:54:  /// can react (the purchase completes asynchronously on [purchaseStream], long
lib/ads/iap_service.dart:75:      // Listen for purchase updates (e.g., completed purchases from the platform).
lib/ads/iap_service.dart:133:  /// Handle a completed purchase: verify it and unlock the feature.
lib/ads/iap_service.dart:187:          await InAppPurchase.instance.completePurchase(purchase);
lib/ui/widgets/cell_widget.dart:97:                transitionBuilder: (child, animation) => ScaleTransition(
lib/ui/state/challenge_providers.dart:24:  Future<void> complete(String puzzleId) async {
lib/ads/ump_consent_service.dart:85:    final completer = Completer<bool>();
lib/ads/ump_consent_service.dart:92:        if (!completer.isCompleted) completer.complete(true);
lib/ads/ump_consent_service.dart:96:        if (!completer.isCompleted) completer.complete(false);
lib/ads/ump_consent_service.dart:99:    return completer.future;
lib/ui/state/providers.dart:423:    final List<SolvedRecord> solved = records ?? ref.read(statsProvider);
lib/ui/state/providers.dart:424:    final ids = evaluateAchievementIds(solved, event: event);
lib/ui/state/providers.dart:602:    if (g != null && !g.completed) {
lib/ui/state/providers.dart:616:    if (g != null && !g.completed) {
lib/ui/state/providers.dart:620:      // for the next microtask so the widget tree has finished tearing down.
lib/ui/state/providers.dart:676:    if (cur == null || cur.completed) return;
lib/ui/state/providers.dart:681:        if (next.completed) {
lib/ui/state/providers.dart:705:    if (g == null || g.completed) {
lib/ui/state/providers.dart:797:  Future<void> _completeIfDone(Game prev, Game next) async {
lib/ui/state/providers.dart:798:    if (next.completed && !prev.completed) {
lib/ui/state/providers.dart:811:              solvedAt: DateTime.now(),
lib/ui/state/providers.dart:831:    if (g.completed) return;
lib/ui/state/providers.dart:839:      if (cur == null || cur.completed) {
lib/ui/state/providers.dart:862:    unawaited(_completeIfDone(ticked, next));
lib/ui/state/providers.dart:884:  if (g == null || g.completed) return null;
lib/ui/state/providers.dart:903:/// tap for a 1-second commit window) are filtered out so the red flash
lib/ui/state/providers.dart:952:  // No-three windows.
lib/ui/screens/challenge_pack_screen.dart:17:    final completed = ref.watch(challengeProgressProvider);
lib/ui/screens/challenge_pack_screen.dart:39:            _PackHeader(completed: completed.length, total: puzzles.length),
lib/ui/screens/challenge_pack_screen.dart:44:                completed: completed.contains(puzzle.id),
lib/ui/screens/challenge_pack_screen.dart:71:                'a completed row or column.',
lib/ui/screens/challenge_pack_screen.dart:102:  final int completed;
lib/ui/screens/challenge_pack_screen.dart:104:  const _PackHeader({required this.completed, required this.total});
lib/ui/screens/challenge_pack_screen.dart:124:                Text('$completed / $total complete', style: t.title),
lib/ui/screens/challenge_pack_screen.dart:140:  final bool completed;
lib/ui/screens/challenge_pack_screen.dart:141:  const _PuzzleCard({required this.puzzle, required this.completed});
lib/ui/screens/challenge_pack_screen.dart:151:      semanticHint: completed ? 'completed' : puzzle.subtitle,
lib/ui/screens/challenge_pack_screen.dart:152:      borderColor: completed ? t.success : t.gridLine,
lib/ui/screens/challenge_pack_screen.dart:153:      borderWidth: completed ? 1.5 : 0.5,
lib/ui/screens/challenge_pack_screen.dart:161:              color: completed ? t.success : t.surfaceElevated,
lib/ui/screens/challenge_pack_screen.dart:164:            child: completed
lib/ui/screens/home_screen.dart:95:            if (inProgress != null && !inProgress.completed) _ContinueCard(),
lib/ui/screens/home_screen.dart:96:            if (inProgress != null && !inProgress.completed)
lib/ui/screens/home_screen.dart:334:                    semanticHint: '${stats.totalSolved} solved',
lib/ui/screens/home_screen.dart:350:                          '${stats.totalSolved} solved',
lib/ui/screens/home_screen.dart:455:/// play path. Only shown to non-premium players who have solved at least one
lib/ui/screens/home_screen.dart:464:    final solved = ref.watch(statsSummaryProvider).totalSolved;
lib/ui/screens/home_screen.dart:467:    if (premium || dismissed || solved < 1) return const SizedBox.shrink();
lib/ui/screens/stats_screen.dart:34:                final solved = _Stat(
lib/ui/screens/stats_screen.dart:37:                  semanticValue: '${summary.totalSolved} puzzles solved',
lib/ui/screens/stats_screen.dart:46:                    children: [solved, const SizedBox(height: 8), streak],
lib/ui/screens/stats_screen.dart:51:                    Expanded(child: solved),
lib/ui/screens/stats_screen.dart:64:                solved: summary.solvedByDifficulty[d] ?? 0,
lib/ui/screens/stats_screen.dart:125:  final int solved;
lib/ui/screens/stats_screen.dart:130:    required this.solved,
lib/ui/screens/stats_screen.dart:140:      '$solved solved',
lib/ui/screens/stats_screen.dart:170:                Expanded(child: Text('$solved solved', style: t.label)),
lib/ui/screens/stats_screen.dart:230:          '${_fmt(record.elapsedMs)}, solved ${_relative(record.solvedAt)}',
lib/ui/screens/stats_screen.dart:268:              _relative(record.solvedAt),
lib/ui/screens/watch_ad_dialog.dart:25:  /// Returns `true` when the reward path completed and play should continue.
lib/ui/screens/how_to_play_screen.dart:89:                ('Hard — requires reasoning about all valid ways to complete a row or column.',
lib/ui/screens/settings_screen.dart:273:      // The purchase completes asynchronously on the purchase stream, which
lib/ui/screens/settings_screen.dart:787:    // Cover: never let either source axis shrink inside the crop window.
lib/ui/screens/settings_screen.dart:794:      // First layout: cover-fit and centre the source under the window.
lib/ui/screens/settings_screen.dart:807:      // but re-clamp against the new bounds so nothing drifts off-window.
lib/ui/screens/challenge_game_screen.dart:22:  bool _completed = false;
lib/ui/screens/challenge_game_screen.dart:60:                  locked: _completed,
lib/ui/screens/challenge_game_screen.dart:68:                      onPressed: _history.isEmpty || _completed ? null : _undo,
lib/ui/screens/challenge_game_screen.dart:76:                      onPressed: _completed ? null : _confirmRestart,
lib/ui/screens/challenge_game_screen.dart:101:      _finish();
lib/ui/screens/challenge_game_screen.dart:134:  Future<void> _finish() async {
lib/ui/screens/challenge_game_screen.dart:135:    setState(() => _completed = true);
lib/ui/screens/challenge_game_screen.dart:136:    await ref.read(challengeProgressProvider.notifier).complete(puzzle.id);
lib/ui/screens/challenge_game_screen.dart:143:        title: const Text('Challenge complete'),
lib/ui/screens/challenge_game_screen.dart:144:        content: Text('${puzzle.title} is solved.'),
lib/ui/screens/premium_upsell_dialog.dart:13:/// promo code, mirroring the Settings premium card. Buying completes async on
lib/ui/screens/premium_upsell_dialog.dart:93:      // Purchase completes async on the purchase stream and flips
lib/ui/screens/game_screen.dart:110:    if (g == null || g.completed) return;
lib/ui/screens/game_screen.dart:127:      case HintStatus.solved:
lib/ui/screens/game_screen.dart:128:        return; // board already complete — completion handling takes over
lib/ui/screens/game_screen.dart:192:      if (next == null || !next.completed || previous?.completed == true) {
lib/ui/screens/game_screen.dart:272:                completed: g.completed,
lib/ui/screens/game_screen.dart:315:                  if (context.mounted && !meta.completed) {
lib/ui/screens/game_screen.dart:399:            : (puzzle: g.puzzle, board: g.board, completed: g.completed),
lib/ui/screens/game_screen.dart:424:            locked: game.completed,
lib/ui/screens/game_screen.dart:432:              transitionBuilder: (child, anim) => FadeTransition(
lib/ui/screens/game_screen.dart:464:            : (historyEmpty: g.history.isEmpty, completed: g.completed),
lib/ui/screens/game_screen.dart:485:            onPressed: game.historyEmpty || game.completed
lib/ui/screens/game_screen.dart:492:            onPressed: game.historyEmpty || game.completed
lib/ui/screens/game_screen.dart:500:            onPressed: game.completed ? null : onHint,
lib/ui/screens/game_screen.dart:575:                        // Close the dialog to view the finished board (it's
lib/ui/screens/game_screen.dart:576:                        // still behind, fully solved). Re-openable? No - but
lib/ui/screens/new_game_screen.dart:43:                'Smaller grids finish faster; larger ones add more of a stretch.',


# rg -n 'GameScreen|GameController|providers|BoardWidget|CellWidget|dialog'
test/widget_smoke_test.dart:8:import 'package:sundown/ui/state/providers.dart';
test/ui/widgets/board_widget_semantics_test.dart:10:import 'package:sundown/ui/state/providers.dart';
test/ui/widgets/board_widget_semantics_test.dart:23:      widget is CellWidget && widget.row == row && widget.column == column,
test/ui/widgets/board_widget_semantics_test.dart:45:              child: BoardWidget(
test/ui/widgets/banner_ad_widget_test.dart:12:import 'package:sundown/ui/state/providers.dart';
test/ui/screens/settings_screen_test.dart:6:import 'package:sundown/ui/screens/premium_upsell_dialog.dart';
test/ui/screens/settings_screen_test.dart:9:import 'package:sundown/ui/state/providers.dart';
test/ui/screens/settings_screen_test.dart:159:      // redeem-only dialog.
test/ui/screens/challenge_pack_screen_test.dart:9:import 'package:sundown/ui/state/providers.dart';
test/ui/screens/challenge_pack_screen_test.dart:62:          child: const MaterialApp(home: ChallengeGameScreen(puzzle: puzzle)),
lib/main.dart:16:import 'ui/state/providers.dart';
lib/main.dart:110:  // Game-timer pause/resume on app background is handled by GameScreen itself
lib/main.dart:130:  /// Bridge our tokens to Material — used by AppBar, dialogs, snackbars, etc.
lib/main.dart:150:      dialogTheme: DialogThemeData(
test/ui/screens/tutorial_screen_test.dart:9:import 'package:sundown/ui/state/providers.dart';
test/ui/screens/watch_ad_dialog_test.dart:11:import 'package:sundown/ui/screens/watch_ad_dialog.dart';
test/ui/screens/watch_ad_dialog_test.dart:12:import 'package:sundown/ui/state/providers.dart';
test/ui/stats_screen_test.dart:10:import 'package:sundown/ui/state/providers.dart';
test/ui/state/providers_test.dart:7:import 'package:sundown/ui/state/providers.dart';
test/ui/screens/game_header_test.dart:1:// Tests for the _Header widget rendered inside GameScreen.
test/ui/screens/game_header_test.dart:23:import 'package:sundown/ui/state/providers.dart';
test/ui/screens/game_header_test.dart:43:/// A [GameController] subclass that immediately returns a preset [Game] from
test/ui/screens/game_header_test.dart:46:class _FakeGameController extends GameController {
test/ui/screens/game_header_test.dart:52:  _FakeGameController(this._preset);
test/ui/screens/game_header_test.dart:73:/// Pumps [GameScreen] inside a [ProviderScope] that injects the given fake
test/ui/screens/game_header_test.dart:76:Future<_FakeGameController> _pumpGameScreen(
test/ui/screens/game_header_test.dart:78:  required _FakeGameController controller,
test/ui/screens/game_header_test.dart:100:          child: const MaterialApp(home: GameScreen()),
test/ui/screens/game_header_test.dart:135:    await _pumpGameScreen(
test/ui/screens/game_header_test.dart:137:      controller: _FakeGameController(longGame),
test/ui/screens/game_header_test.dart:191:      await _pumpGameScreen(
test/ui/screens/game_header_test.dart:193:        controller: _FakeGameController(scaledGame),
test/ui/screens/game_header_test.dart:229:      final controller = _FakeGameController(activeGame);
test/ui/screens/game_header_test.dart:230:      await _pumpGameScreen(tester, controller: controller);
test/ui/screens/game_header_test.dart:297:      final controller = _FakeGameController(completedGame);
test/ui/screens/game_header_test.dart:298:      await _pumpGameScreen(tester, controller: controller);
test/ui/screens/game_header_test.dart:346:      await _pumpGameScreen(
test/ui/screens/game_header_test.dart:348:        controller: _FakeGameController(game),
test/ui/screens/game_header_test.dart:387:      await _pumpGameScreen(
test/ui/screens/game_header_test.dart:389:        controller: _FakeGameController(game),
test/ui/screens/game_header_test.dart:416:      await _pumpGameScreen(
test/ui/screens/game_header_test.dart:418:        controller: _FakeGameController(game),
test/ui/screens/game_header_test.dart:445:      await _pumpGameScreen(
test/ui/screens/game_header_test.dart:447:        controller: _FakeGameController(game),
test/ui/screens/game_header_test.dart:473:      await _pumpGameScreen(
test/ui/screens/game_header_test.dart:475:        controller: _FakeGameController(game),
test/ads/ad_gate_test.dart:7:import 'package:sundown/ui/state/providers.dart';
lib/ads/ad_gate.dart:6:import '../ui/state/providers.dart';
lib/ads/ump_consent_service.dart:31:  /// consent dialog can be exercised from a non-EEA location (incl. emulators).
lib/ui/widgets/banner_ad_widget.dart:11:import '../state/providers.dart';
lib/ui/widgets/cell_widget.dart:8:import '../state/providers.dart';
lib/ui/widgets/cell_widget.dart:19:class CellWidget extends ConsumerWidget {
lib/ui/widgets/cell_widget.dart:29:  const CellWidget({
lib/ui/widgets/board_widget.dart:10:import '../state/providers.dart';
lib/ui/widgets/board_widget.dart:18:class BoardWidget extends StatelessWidget {
lib/ui/widgets/board_widget.dart:28:  const BoardWidget({
lib/ui/widgets/board_widget.dart:112:      child: CellWidget(
lib/ui/state/providers.dart:37:/// Single async-init point. App `runApp` awaits this so all later providers
lib/ui/state/providers.dart:545:class GameController extends Notifier<Game?> {
lib/ui/state/providers.dart:700:  /// charge one mistake. Re-emit state so providers refresh.
lib/ui/state/providers.dart:866:final gameProvider = NotifierProvider<GameController, Game?>(
lib/ui/state/providers.dart:867:  GameController.new,
lib/ui/state/challenge_providers.dart:7:import 'providers.dart';
lib/ui/screens/tutorial_screen.dart:5:import '../state/providers.dart';
lib/ui/screens/challenge_game_screen.dart:6:import '../state/challenge_providers.dart';
lib/ui/screens/challenge_game_screen.dart:10:class ChallengeGameScreen extends ConsumerStatefulWidget {
lib/ui/screens/challenge_game_screen.dart:12:  const ChallengeGameScreen({super.key, required this.puzzle});
lib/ui/screens/challenge_game_screen.dart:15:  ConsumerState<ChallengeGameScreen> createState() =>
lib/ui/screens/challenge_game_screen.dart:16:      _ChallengeGameScreenState();
lib/ui/screens/challenge_game_screen.dart:19:class _ChallengeGameScreenState extends ConsumerState<ChallengeGameScreen> {
lib/ui/screens/challenge_pack_screen.dart:5:import '../state/challenge_providers.dart';
lib/ui/screens/challenge_pack_screen.dart:148:        MaterialPageRoute(builder: (_) => ChallengeGameScreen(puzzle: puzzle)),
lib/ui/widgets/challenge_board.dart:94:                      child: CellWidget(
lib/ui/screens/home_screen.dart:5:import '../state/providers.dart';
lib/ui/screens/home_screen.dart:15:import 'play_by_code_dialog.dart';
lib/ui/screens/home_screen.dart:101:              ).push(MaterialPageRoute(builder: (_) => const NewGameScreen())),
lib/ui/screens/home_screen.dart:408:      ).push(MaterialPageRoute(builder: (_) => const GameScreen())),
lib/ui/screens/stats_screen.dart:6:import '../state/providers.dart';
lib/ui/screens/stats_screen.dart:75:                    MaterialPageRoute(builder: (_) => const NewGameScreen()),
lib/ui/screens/game_screen.dart:13:import '../state/providers.dart';
lib/ui/screens/game_screen.dart:20:import 'watch_ad_dialog.dart';
lib/ui/screens/game_screen.dart:23:class GameScreen extends ConsumerStatefulWidget {
lib/ui/screens/game_screen.dart:24:  const GameScreen({super.key});
lib/ui/screens/game_screen.dart:27:  ConsumerState<GameScreen> createState() => _GameScreenState();
lib/ui/screens/game_screen.dart:30:class _GameScreenState extends ConsumerState<GameScreen>
lib/ui/screens/game_screen.dart:46:  late final GameController _gameController;
lib/ui/screens/game_screen.dart:54:    // this observer only exists while the GameScreen is mounted.
lib/ui/screens/game_screen.dart:418:          BoardWidget(
lib/ui/screens/game_screen.dart:542:    builder: (dialogCtx) => SafeArea(
lib/ui/screens/game_screen.dart:575:                        // Close the dialog to view the finished board (it's
lib/ui/screens/game_screen.dart:579:                          onPressed: () => Navigator.of(dialogCtx).pop(),
lib/ui/screens/game_screen.dart:608:                                    Navigator.of(dialogCtx).pop();
lib/ui/screens/achievements_screen.dart:5:import '../state/providers.dart';
lib/ui/screens/new_game_screen.dart:5:import '../state/providers.dart';
lib/ui/screens/new_game_screen.dart:9:import 'premium_upsell_dialog.dart';
lib/ui/screens/new_game_screen.dart:11:class NewGameScreen extends ConsumerStatefulWidget {
lib/ui/screens/new_game_screen.dart:12:  const NewGameScreen({super.key});
lib/ui/screens/new_game_screen.dart:14:  ConsumerState<NewGameScreen> createState() => _NewGameScreenState();
lib/ui/screens/new_game_screen.dart:17:class _NewGameScreenState extends ConsumerState<NewGameScreen> {
lib/ui/screens/new_game_screen.dart:147:      ).pushReplacement(MaterialPageRoute(builder: (_) => const GameScreen()));
lib/ui/screens/watch_ad_dialog.dart:12:import '../../ui/state/providers.dart';
lib/ui/screens/premium_upsell_dialog.dart:5:import '../state/providers.dart';
lib/ui/screens/premium_upsell_dialog.dart:7:import 'redeem_code_dialog.dart';
lib/ui/screens/premium_upsell_dialog.dart:17:  /// Headline shown at the top of the dialog. Defaults to a generic premium
lib/ui/screens/settings_screen.dart:12:import '../state/providers.dart';
lib/ui/screens/settings_screen.dart:16:import 'premium_upsell_dialog.dart';
lib/ui/screens/settings_screen.dart:17:import 'redeem_code_dialog.dart';
lib/ui/screens/play_by_code_dialog.dart:5:import '../state/providers.dart';
lib/ui/screens/play_by_code_dialog.dart:138:      ).push(MaterialPageRoute(builder: (_) => const GameScreen()));
lib/ui/screens/redeem_code_dialog.dart:5:import '../state/providers.dart';


# Read lib/domain/game/game.dart
```dart
import '../board/board.dart';
import '../board/cell.dart';
import '../board/puzzle.dart';
import '../rules.dart';

/// One in-flight game session: the puzzle, the player's current state, undo
/// history, hint usage, mistake count, and timing.
///
/// All fields are immutable; mutations produce a new [Game] via [copyWith]
/// or one of the convenience methods. Keeping it immutable means the
/// Riverpod controller can treat each transition as a single value change.
class Game {
  final Puzzle puzzle;

  /// Current board state — same constraints as puzzle.clues, with player
  /// placements overlaid on top of the original clue cells. Clue cells are
  /// never modified.
  final Board board;

  /// Stack of moves the player has made (most-recent last). Used for undo.
  final List<Move> history;

  /// Number of hints the player has requested. Incremented once when witness
  /// cells are first shown (stage 1) and once more when the reason text is
  /// shown (stage 2). The final reveal (stage 3) does not add to this count.
  final int hintsUsed;

  /// Number of times the player placed a cell that immediately produced a
  /// rule violation (not counting moves later undone).
  final int mistakes;

  /// Wall-clock time spent on this puzzle, in milliseconds. Tracked outside
  /// the model by the controller (resumed/paused appropriately) and snapped
  /// into the game on transitions.
  final int elapsedMs;

  /// True once the player has filled the board correctly. Frozen at that
  /// point — further taps are no-ops.
  final bool completed;

  /// UTC timestamp when the player first opened this puzzle.
  final DateTime startedAt;

  /// True once the player has used undo in this puzzle.
  final bool undoUsed;

  const Game({
    required this.puzzle,
    required this.board,
    required this.history,
    required this.hintsUsed,
    required this.mistakes,
    required this.elapsedMs,
    required this.completed,
    required this.startedAt,
    required this.undoUsed,
  });

  factory Game.fresh(Puzzle p, {DateTime? now}) => Game(
    puzzle: p,
    board: p.clues,
    history: const [],
    hintsUsed: 0,
    mistakes: 0,
    elapsedMs: 0,
    completed: false,
    startedAt: now ?? DateTime.now(),
    undoUsed: false,
  );

  int get size => puzzle.size;

  /// True if `(r,c)` was given as a clue and cannot be edited.
  bool isClue(int r, int c) => puzzle.clues.at(r, c).isFilled;

  /// Returns the next [Game] after the player taps cell `(r,c)`. Cycle order:
  /// empty → sun → moon → empty. Tapping a clue cell is a no-op.
  Game tap(int r, int c) {
    if (completed) return this;
    if (isClue(r, c)) return this;
    final cur = board.at(r, c);
    final next = switch (cur) {
      Cell.empty => Cell.sun,
      Cell.sun => Cell.moon,
      Cell.moon => Cell.empty,
    };
    return _applyMove(r, c, cur, next);
  }

  /// Sets `(r,c)` directly to [value] (used by hint reveals).
  Game place(int r, int c, Cell value) {
    if (completed) return this;
    if (isClue(r, c)) return this;
    final cur = board.at(r, c);
    if (cur == value) return this;
    return _applyMove(r, c, cur, value);
  }

  Game _applyMove(int r, int c, Cell prev, Cell next) {
    final newBoard = board.set(r, c, next);
    final newHistory = [...history, Move(r: r, c: c, previous: prev)];
    final done = Rules.isSolved(newBoard);
    return copyWith(board: newBoard, history: newHistory, completed: done);
  }

  Game incrementMistakes() => copyWith(mistakes: mistakes + 1);

  /// Undoes the most recent move. No-op if history is empty or game completed.
  Game undo() {
    if (completed || history.isEmpty) return this;
    final last = history.last;
    return copyWith(
      board: board.set(last.r, last.c, last.previous),
      history: history.sublist(0, history.length - 1),
      undoUsed: true,
    );
  }

  /// Resets all player placements back to the original clues. Resets
  /// timer/mistakes/hints too.
  Game restart() => Game.fresh(puzzle, now: startedAt);

  /// Increments the hint counter without revealing a cell.
  /// Called when the player asks to see witness cells (stage 1) or
  /// the reason text (stage 2) — before the answer is given.
  Game recordHint() => copyWith(hintsUsed: hintsUsed + 1);

  /// Applies a [Cell] reveal at `(r,c)`. Counter is NOT bumped here;
  /// it was already bumped at stages 1 and 2.
  Game revealHint(int r, int c, Cell value) => place(r, c, value);

  Game tick(int newElapsedMs) =>
      completed ? this : copyWith(elapsedMs: newElapsedMs);

  Game copyWith({
    Puzzle? puzzle,
    Board? board,
    List<Move>? history,
    int? hintsUsed,
    int? mistakes,
    int? elapsedMs,
    bool? completed,
    DateTime? startedAt,
    bool? undoUsed,
  }) => Game(
    puzzle: puzzle ?? this.puzzle,
    board: board ?? this.board,
    history: history ?? this.history,
    hintsUsed: hintsUsed ?? this.hintsUsed,
    mistakes: mistakes ?? this.mistakes,
    elapsedMs: elapsedMs ?? this.elapsedMs,
    completed: completed ?? this.completed,
    startedAt: startedAt ?? this.startedAt,
    undoUsed: undoUsed ?? this.undoUsed,
  );
}

/// One step in [Game.history]. `previous` is what the cell was BEFORE the
/// player's tap — what restoring it to undoes the move.
class Move {
  final int r;
  final int c;
  final Cell previous;
  const Move({required this.r, required this.c, required this.previous});
}

```

# Read lib/domain/rules.dart
```dart
import 'board/board.dart';
import 'board/cell.dart';
import 'board/edge.dart';

/// Rule-checking utilities for partial and complete boards.
///
/// "Partial" validity = no current placement violates a rule (but the board may
/// still have empties). Used by the solver to detect contradictions.
/// "Complete" validity = all rules satisfied and no empties remain.
class Rules {
  /// Returns true if every filled cell on [board] is consistent with the rules.
  /// Empty cells are ignored except for "count cannot already exceed N/2".
  static bool isPartiallyValid(Board board) => firstViolation(board) == null;

  /// Returns true if the board is fully filled and every rule is satisfied.
  static bool isSolved(Board board) =>
      board.isComplete && firstViolation(board) == null;

  /// Returns the first violation found, or null if none.
  /// Useful both for "is this valid" and for diagnostic display.
  static Violation? firstViolation(Board board) {
    final n = board.size;
    final half = n ~/ 2;

    // Counts per row and column.
    for (var i = 0; i < n; i++) {
      final row = board.row(i);
      final col = board.col(i);
      final rs = _count(row, Cell.sun);
      final rm = _count(row, Cell.moon);
      final cs = _count(col, Cell.sun);
      final cm = _count(col, Cell.moon);
      if (rs > half) return Violation.tooMany(Axis.row, i, Cell.sun);
      if (rm > half) return Violation.tooMany(Axis.row, i, Cell.moon);
      if (cs > half) return Violation.tooMany(Axis.col, i, Cell.sun);
      if (cm > half) return Violation.tooMany(Axis.col, i, Cell.moon);
    }

    // No three consecutive same — rows.
    for (var r = 0; r < n; r++) {
      for (var c = 0; c <= n - 3; c++) {
        final a = board.at(r, c), b = board.at(r, c + 1), d = board.at(r, c + 2);
        if (a.isFilled && a == b && b == d) {
          return Violation.threeInARow(Axis.row, r, c, a);
        }
      }
    }
    // No three consecutive same — cols.
    for (var c = 0; c < n; c++) {
      for (var r = 0; r <= n - 3; r++) {
        final a = board.at(r, c), b = board.at(r + 1, c), d = board.at(r + 2, c);
        if (a.isFilled && a == b && b == d) {
          return Violation.threeInARow(Axis.col, c, r, a);
        }
      }
    }

    // No two fully-filled rows may be identical, and the same for cols.
    // Partial rows are not checked — they could still resolve to non-
    // duplicates as the player fills them in.
    for (var i = 0; i < n; i++) {
      final rowI = board.row(i);
      if (!rowI.every((c) => c.isFilled)) continue;
      for (var j = i + 1; j < n; j++) {
        final rowJ = board.row(j);
        if (!rowJ.every((c) => c.isFilled)) continue;
        var same = true;
        for (var k = 0; k < n; k++) {
          if (rowI[k] != rowJ[k]) { same = false; break; }
        }
        if (same) return Violation.duplicate(Axis.row, i, j);
      }
    }
    for (var i = 0; i < n; i++) {
      final colI = board.col(i);
      if (!colI.every((c) => c.isFilled)) continue;
      for (var j = i + 1; j < n; j++) {
        final colJ = board.col(j);
        if (!colJ.every((c) => c.isFilled)) continue;
        var same = true;
        for (var k = 0; k < n; k++) {
          if (colI[k] != colJ[k]) { same = false; break; }
        }
        if (same) return Violation.duplicate(Axis.col, i, j);
      }
    }

    // Edge constraints.
    for (final entry in board.constraints.edges.entries) {
      final pos = entry.key;
      final kind = entry.value;
      final (or, oc) = pos.otherCell;
      if (or >= n || oc >= n) {
        return Violation.edgeOutOfBounds(pos);
      }
      final a = board.at(pos.r, pos.c);
      final b = board.at(or, oc);
      if (!kind.satisfiedBy(a, b)) {
        return Violation.edge(pos, kind);
      }
    }

    return null;
  }

  static int _count(List<Cell> cells, Cell target) =>
      cells.where((c) => c == target).length;
}

enum Axis { row, col }

enum ViolationKind { tooMany, threeInARow, duplicate, edge, edgeOutOfBounds }

class Violation {
  final ViolationKind kind;
  final Axis? axis;
  final int? index;
  final int? offset;
  final int? otherIndex;
  final Cell? cell;
  final EdgePos? edge;
  final EdgeKind? edgeKind;

  const Violation._({
    required this.kind,
    this.axis,
    this.index,
    this.offset,
    this.otherIndex,
    this.cell,
    this.edge,
    this.edgeKind,
  });

  factory Violation.tooMany(Axis axis, int idx, Cell cell) =>
      Violation._(kind: ViolationKind.tooMany, axis: axis, index: idx, cell: cell);

  factory Violation.threeInARow(Axis axis, int idx, int offset, Cell cell) =>
      Violation._(
        kind: ViolationKind.threeInARow,
        axis: axis,
        index: idx,
        offset: offset,
        cell: cell,
      );

  factory Violation.duplicate(Axis axis, int a, int b) =>
      Violation._(
        kind: ViolationKind.duplicate,
        axis: axis,
        index: a,
        otherIndex: b,
      );

  factory Violation.edge(EdgePos pos, EdgeKind kind) =>
      Violation._(kind: ViolationKind.edge, edge: pos, edgeKind: kind);

  factory Violation.edgeOutOfBounds(EdgePos pos) =>
      Violation._(kind: ViolationKind.edgeOutOfBounds, edge: pos);

  @override
  String toString() => switch (kind) {
        ViolationKind.tooMany =>
          'too many ${cell!.name} in ${axis!.name} $index',
        ViolationKind.threeInARow =>
          'three ${cell!.name} in ${axis!.name} $index at offset $offset',
        ViolationKind.duplicate =>
          'duplicate ${axis!.name}s $index and $otherIndex',
        ViolationKind.edge =>
          'edge constraint ${edgeKind!.glyph} violated at $edge',
        ViolationKind.edgeOutOfBounds => 'edge $edge out of bounds',
      };
}

```

# Read lib/ui/state/providers.dart
```dart
import 'dart:async';
import 'dart:convert';
import 'dart:io';

import 'package:flutter/foundation.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:image_picker/image_picker.dart';

import 'package:in_app_purchase/in_app_purchase.dart';
import 'package:google_mobile_ads/google_mobile_ads.dart';
import 'package:path_provider/path_provider.dart';

import '../../ads/ad_config.dart';
import '../../ads/ad_event_queue.dart';
import '../../ads/ad_fallback_tier.dart';
import '../../ads/ad_privacy_service.dart';
import '../../ads/iap_service.dart';
import '../../ads/network_status_service.dart';
import '../../domain/achievement.dart';
import '../../domain/board/board.dart';
import '../../domain/board/cell.dart';
import '../../domain/difficulty.dart';
import '../../domain/game/game.dart';
import '../../domain/game/game_codec.dart';
import '../../domain/game/stats.dart';
import '../../domain/board/puzzle.dart';
import '../../domain/generator.dart';
import '../../domain/premium/custom_symbol_images.dart';
import '../../domain/premium/premium_service.dart';
import '../../domain/solver/solver.dart';
import '../../domain/solver/technique.dart';
import '../../storage/storage_service.dart';
import 'custom_symbol_image_normalizer.dart';
import '../theme/sundown_theme.dart';
import '../theme/themes.dart';

/// Single async-init point. App `runApp` awaits this so all later providers
/// can read it synchronously.
final storageProvider = Provider<StorageService>(
  (_) => throw UnimplementedError('overridden in main()'),
);

// Top-level helpers used by `compute()`. Must NOT close over state.
Puzzle _makeInIsolate((int, Difficulty) args) =>
    Generator.make(args.$1, args.$2);

Puzzle _fromCodeInIsolate(String code) => Generator.fromCode(code);

// Reconstructs a saved Game from its raw JSON string in an isolate so that
// Generator.fromCode() (which can take several seconds for expert 12×12) never
// runs on the main thread.
Game? _gameFromRawJsonInIsolate(String raw) {
  try {
    return GameCodec.fromJson(jsonDecode(raw) as Map<String, dynamic>);
  } catch (_) {
    return null;
  }
}

// ─── premium ────────────────────────────────────────────────────────────

final premiumServiceProvider = Provider<PremiumService>((ref) {
  final storage = ref.watch(storageProvider);
  return PremiumService(storage);
});

/// Single premium entitlement. True if the user has either purchased "Unlimited
/// Play" or redeemed a promo code. Drives both the rewarded-ad gate removal and
/// premium-theme unlocking. Synchronous so UI gates read it without a loading
/// state; reacts live to an in-session purchase via [IapService.entitlementNotifier].
class PremiumController extends Notifier<bool> {
  @override
  bool build() {
    final iap = ref.read(iapServiceProvider);
    void onPurchase() => state = _compute();
    iap.entitlementNotifier.addListener(onPurchase);
    ref.onDispose(() => iap.entitlementNotifier.removeListener(onPurchase));
    return _compute();
  }

  bool _compute() =>
      ref.read(storageProvider).unlimitedPlay ||
      ref.read(premiumServiceProvider).isPremiumSync();

  /// Recompute after a promo-code redemption or a restore-purchases call.
  void refresh() => state = _compute();
}

final premiumProvider = NotifierProvider<PremiumController, bool>(
  PremiumController.new,
);

/// Whether developer testing mode is currently switched *on*. When on, the
/// challenge pack is reachable from the home screen and the banner slot shows a
/// developer marker instead of ads. Unlocked by redeeming the GREENBEAN code;
/// toggled on/off from Settings thereafter.
class DeveloperTestingModeController extends Notifier<bool> {
  @override
  bool build() => ref.read(storageProvider).developerTestingMode;

  void refresh() => state = ref.read(storageProvider).developerTestingMode;

  /// Toggle developer mode on or off and persist the choice.
  Future<void> setEnabled(bool value) async {
    await ref.read(storageProvider).setDeveloperTestingMode(value);
    state = value;
  }
}

final developerTestingModeProvider =
    NotifierProvider<DeveloperTestingModeController, bool>(
      DeveloperTestingModeController.new,
    );

/// Whether the GREENBEAN developer password has ever been entered. Gates the
/// visibility of the developer-mode toggle in Settings. Recomputed after a
/// promo-code redemption via [refresh].
class DeveloperModeUnlockedController extends Notifier<bool> {
  @override
  bool build() => ref.read(storageProvider).developerModeUnlocked;

  void refresh() => state = ref.read(storageProvider).developerModeUnlocked;
}

final developerModeUnlockedProvider =
    NotifierProvider<DeveloperModeUnlockedController, bool>(
      DeveloperModeUnlockedController.new,
    );

/// Premium expiry date for display (null if not premium).
final premiumExpiryProvider = FutureProvider<DateTime?>((ref) async {
  final service = ref.watch(premiumServiceProvider);
  return service.getPremiumExpiryDate();
});

// ─── ads ─────────────────────────────────────────────────────────────────

final networkStatusServiceProvider = Provider<NetworkStatusService>(
  (_) => NetworkStatusService(),
);

final networkStatusProvider = StreamProvider<NetworkStatus>((ref) {
  return ref.watch(networkStatusServiceProvider).watch();
});

final adPlatformSupportedProvider = Provider<bool>((_) => AdConfig.supported);

class AdsToggleController extends Notifier<bool> {
  @override
  bool build() {
    final storage = ref.read(storageProvider);
    return storage.getShowAds() ?? true; // Default: on
  }

  void setShowAds(bool show) {
    state = show;
    unawaited(ref.read(storageProvider).setShowAds(show));
  }
}

final adsToggleProvider = NotifierProvider<AdsToggleController, bool>(
  AdsToggleController.new,
);

final adFallbackTierProvider = Provider<AdFallbackTier>((ref) {
  final premium = ref.watch(premiumProvider);
  final networkStatus = ref
      .watch(networkStatusProvider)
      .when(
        data: (value) => value,
        loading: () => NetworkStatus.online,
        error: (error, stackTrace) => NetworkStatus.offline,
      );
  return routeAdFallbackTier(
    premiumState: premium,
    hasInternet: networkStatus == NetworkStatus.online,
    liveAdAvailable:
        ref.watch(adPlatformSupportedProvider) && ref.watch(adsToggleProvider),
  );
});

class AdEventQueueController extends Notifier<List<AdEventRecord>> {
  @override
  List<AdEventRecord> build() => ref.read(storageProvider).loadAdEventQueue();

  Future<void> append(AdEventRecord event) async {
    final next = [...state, event];
    state = next;
    await ref.read(storageProvider).appendAdEvent(event);
  }

  Future<void> clearAcknowledged(Iterable<String> acknowledgedIds) async {
    final ids = acknowledgedIds.toSet();
    if (ids.isEmpty) return;
    final next = state.where((event) => !ids.contains(event.id)).toList();
    state = next;
    await ref.read(storageProvider).clearAcknowledgedAdEvents(ids);
  }

  Future<void> clearAll() async {
    state = const [];
    await ref.read(storageProvider).clearAdEventQueue();
  }
}

final adEventQueueProvider =
    NotifierProvider<AdEventQueueController, List<AdEventRecord>>(
      AdEventQueueController.new,
    );

/// Banner loader seam. The default implementation uses Google Mobile Ads, but
/// tests can override it with a fake that never touches platform channels.
final bannerAdLoaderProvider =
    Provider<BannerAd? Function(BannerAdListener listener)>((_) {
      return (listener) {
        final ad = BannerAd(
          adUnitId: AdConfig.bannerUnitId,
          size: AdSize.banner,
          request: AdPrivacyService.instance.adRequest,
          listener: listener,
        );
        ad.load();
        return ad;
      };
    });

/// In-app purchase service for the "Unlimited Play" premium upgrade.
final iapServiceProvider = Provider<IapService>((_) => IapService());

/// The "Unlimited Play" store product (null if the store has no such product
/// configured yet, or the platform doesn't support IAP). Drives the price label
/// and enabled/disabled state of the purchase button.
final unlimitedPlayProductProvider = FutureProvider<ProductDetails?>((
  ref,
) async {
  return ref.read(iapServiceProvider).getUnlimitedPlayProduct();
});

typedef PremiumPurchaseLauncher = Future<bool> Function();

final premiumPurchaseLauncherProvider = Provider<PremiumPurchaseLauncher>((
  ref,
) {
  return () async {
    final product = await ref.read(unlimitedPlayProductProvider.future);
    if (product == null) return false;
    await ref.read(iapServiceProvider).buyUnlimitedPlay(product);
    return true;
  };
});

// ─── theme ──────────────────────────────────────────────────────────────

class ThemeController extends Notifier<SundownTheme> {
  @override
  SundownTheme build() {
    final id = ref.read(storageProvider).themeId;
    return id == null ? SundownThemes.dusk : SundownThemes.byId(id);
  }

  void select(SundownTheme t) {
    state = t;
    unawaited(ref.read(storageProvider).setThemeId(t.id));
  }
}

final themeProvider = NotifierProvider<ThemeController, SundownTheme>(
  ThemeController.new,
);

// ─── custom symbols ────────────────────────────────────────────────────

enum CustomSymbolSlot { sun, moon }

class CustomSymbolImageException implements Exception {
  final String message;
  const CustomSymbolImageException(this.message);

  @override
  String toString() => message;
}

class CustomSymbolImagesController extends AsyncNotifier<CustomSymbolImages> {
  final ImagePicker _picker = ImagePicker();

  @override
  Future<CustomSymbolImages> build() async {
    final storage = ref.read(storageProvider);
    return CustomSymbolImages(
      sunPath: await _existingPathOrNull(storage.customSunImagePath),
      moonPath: await _existingPathOrNull(storage.customMoonImagePath),
    );
  }

  Future<XFile?> pickSourceImage() {
    return _picker.pickImage(source: ImageSource.gallery);
  }

  Future<void> save(
    CustomSymbolSlot slot,
    String sourcePath, {
    CustomSymbolImageCrop? crop,
  }) async {
    try {
      final normalizedBytes = await normalizeCustomSymbolImageFile(
        sourcePath,
        crop: crop,
      );
      final support = await getApplicationSupportDirectory();
      final symbolDir = Directory('${support.path}/custom-symbols');
      await symbolDir.create(recursive: true);
      final fileName = customSymbolFileName(slot);
      final localFile = File('${symbolDir.path}/$fileName');
      await localFile.writeAsBytes(normalizedBytes, flush: true);

      final storage = ref.read(storageProvider);
      final current = state.value ?? const CustomSymbolImages();
      if (slot == CustomSymbolSlot.sun) {
        await _deleteIfDifferent(current.sunPath, localFile.path);
        await storage.setCustomSunImagePath(localFile.path);
        state = AsyncData(
          CustomSymbolImages(
            sunPath: localFile.path,
            moonPath: current.moonPath,
          ),
        );
      } else {
        await _deleteIfDifferent(current.moonPath, localFile.path);
        await storage.setCustomMoonImagePath(localFile.path);
        state = AsyncData(
          CustomSymbolImages(
            sunPath: current.sunPath,
            moonPath: localFile.path,
          ),
        );
      }
      return;
    } on FormatException catch (_) {
      throw const CustomSymbolImageException('Choose a supported photo.');
    } catch (_) {
      throw const CustomSymbolImageException('Image could not be saved.');
    }
  }

  Future<void> pick(CustomSymbolSlot slot) async {
    final image = await pickSourceImage();
    if (image == null) return;
    await save(slot, image.path);
  }

  Future<void> clear(CustomSymbolSlot slot) async {
    final storage = ref.read(storageProvider);
    final current = state.value ?? const CustomSymbolImages();
    final path = slot == CustomSymbolSlot.sun
        ? current.sunPath
        : current.moonPath;
    if (path != null) {
      unawaited(File(path).delete().catchError((_) => File(path)));
    }
    if (slot == CustomSymbolSlot.sun) {
      await storage.clearCustomSunImagePath();
      state = AsyncData(CustomSymbolImages(moonPath: current.moonPath));
    } else {
      await storage.clearCustomMoonImagePath();
      state = AsyncData(CustomSymbolImages(sunPath: current.sunPath));
    }
  }

  Future<String?> _existingPathOrNull(String? path) async {
    if (path == null || path.isEmpty) return null;
    return await File(path).exists() ? path : null;
  }

  Future<void> _deleteIfDifferent(String? oldPath, String newPath) async {
    if (oldPath == null || oldPath == newPath) return;
    await File(oldPath).delete().catchError((_) => File(oldPath));
  }
}

String customSymbolFileName(CustomSymbolSlot slot, {DateTime? timestamp}) {
  final slotName = slot == CustomSymbolSlot.sun ? 'sun' : 'moon';
  final stamp = (timestamp ?? DateTime.now()).microsecondsSinceEpoch;
  return '${slotName}_$stamp.jpg';
}

final customSymbolImagesProvider =
    AsyncNotifierProvider<CustomSymbolImagesController, CustomSymbolImages>(
      CustomSymbolImagesController.new,
    );

// ─── stats ──────────────────────────────────────────────────────────────

class StatsController extends Notifier<List<SolvedRecord>> {
  @override
  List<SolvedRecord> build() => ref.read(storageProvider).loadStats();

  Future<void> record(SolvedRecord r) async {
    final next = [...state, r];
    state = next;
    await ref.read(storageProvider).saveStats(next);
  }
}

final statsProvider = NotifierProvider<StatsController, List<SolvedRecord>>(
  StatsController.new,
);

final statsSummaryProvider = Provider<StatsSummary>(
  (ref) => StatsSummary.from(ref.watch(statsProvider)),
);

// ─── achievements ───────────────────────────────────────────────────────

class AchievementsController extends Notifier<List<UnlockedAchievement>> {
  @override
  List<UnlockedAchievement> build() =>
      ref.read(storageProvider).loadAchievements();

  Future<List<UnlockedAchievement>> evaluate({
    AchievementEvent? event,
    List<SolvedRecord>? records,
  }) async {
    final List<SolvedRecord> solved = records ?? ref.read(statsProvider);
    final ids = evaluateAchievementIds(solved, event: event);
    final unlockedIds = state.map((a) => a.id).toSet();
    final now = event?.occurredAt ?? DateTime.now();
    final fresh = ids
        .where((id) => !unlockedIds.contains(id))
        .map((id) => UnlockedAchievement(id: id, unlockedAt: now))
        .toList();
    if (fresh.isEmpty) return const [];

    final next = [...state, ...fresh]
      ..sort((a, b) => a.unlockedAt.compareTo(b.unlockedAt));
    state = next;
    await ref.read(storageProvider).saveAchievements(next);
    return fresh;
  }
}

final achievementsProvider =
    NotifierProvider<AchievementsController, List<UnlockedAchievement>>(
      AchievementsController.new,
    );

class FreshAchievementUnlocksController
    extends Notifier<List<UnlockedAchievement>> {
  @override
  List<UnlockedAchievement> build() => const [];

  void clear() => state = const [];

  void addAll(Iterable<UnlockedAchievement> unlocks) {
    final existing = state.map((a) => a.id).toSet();
    final fresh = unlocks.where((a) => !existing.contains(a.id)).toList();
    if (fresh.isEmpty) return;
    state = [...state, ...fresh];
  }
}

final freshAchievementUnlocksProvider =
    NotifierProvider<
      FreshAchievementUnlocksController,
      List<UnlockedAchievement>
    >(FreshAchievementUnlocksController.new);

final achievementProgressProvider = Provider<({int unlocked, int total})>((
  ref,
) {
  final unlocked = ref.watch(achievementsProvider).length;
  return (unlocked: unlocked, total: achievementDefinitions.length);
});

// ─── preferences ────────────────────────────────────────────────────────

class PreferredSizeController extends Notifier<int> {
  @override
  int build() => ref.read(storageProvider).preferredSize;
  void set(int v) => state = v;
}

final preferredSizeProvider = NotifierProvider<PreferredSizeController, int>(
  PreferredSizeController.new,
);

class PreferredDifficultyController extends Notifier<Difficulty> {
  @override
  Difficulty build() {
    final code = ref.read(storageProvider).preferredDifficulty;
    if (code == null) return Difficulty.easy;
    try {
      return Difficulty.fromCode(code);
    } catch (_) {
      // Stale or unknown difficulty code from an older build — fall back to
      // Easy rather than crashing the New Puzzle screen. See docs/SENTRY_TRIAGE.md.
      return Difficulty.easy;
    }
  }

  void set(Difficulty v) => state = v;
}

final preferredDifficultyProvider =
    NotifierProvider<PreferredDifficultyController, Difficulty>(
      PreferredDifficultyController.new,
    );

// ─── tutorial ───────────────────────────────────────────────────────────

class TutorialSeenController extends Notifier<bool> {
  @override
  bool build() => ref.read(storageProvider).tutorialSeen;

  Future<void> markSeen() async {
    state = true;
    await ref.read(storageProvider).setTutorialSeen(true);
  }
}

final tutorialSeenProvider = NotifierProvider<TutorialSeenController, bool>(
  TutorialSeenController.new,
);

// ─── premium teaser ───────────────────────────────────────────────────────

/// Whether the home-screen premium teaser card has been permanently dismissed.
/// Mirrors [TutorialSeenController]: a one-way flag persisted to storage.
class PremiumTeaserDismissedController extends Notifier<bool> {
  @override
  bool build() => ref.read(storageProvider).premiumTeaserDismissed;

  Future<void> dismiss() async {
    state = true;
    await ref.read(storageProvider).setPremiumTeaserDismissed(true);
  }
}

final premiumTeaserDismissedProvider =
    NotifierProvider<PremiumTeaserDismissedController, bool>(
      PremiumTeaserDismissedController.new,
    );

// ─── current game ──────────────────────────────────────────────────────

class GameController extends Notifier<Game?> {
  static const _graceWindow = Duration(milliseconds: 1000);

  Timer? _ticker;
  DateTime? _resumedAt;
  int _resumeOffset = 0;
  bool _active = false;

  /// Cells currently in mistake-grace: every cell implicated in a rule
  /// violation at the moment of the latest tap is suppressed from the
  /// red-highlight set and from mistake counting until the grace timer
  /// fires (or another tap re-arms it). Cleared on undo/restart/abandon
  /// and on completion.
  Set<(int, int)> _graceCells = const {};
  Timer? _graceTimer;

  /// Violation set that was visible (red) at the most recent commit.
  /// Used to avoid double-counting a mistake when the player taps other
  /// cells without introducing new violations.
  Set<(int, int)> _lastCommittedViolations = const {};

  @override
  Game? build() {
    ref.onDispose(() {
      _ticker?.cancel();
      _graceTimer?.cancel();
    });
    // Reading the raw JSON string from SharedPreferences is ~0 ms. Reconstructing
    // the Game (Generator.fromCode) can take several seconds for expert 12×12 and
    // must not block the main thread — so we do it in an isolate.
    final raw = ref.read(storageProvider).rawCurrentGame();
    if (raw != null) unawaited(_loadFromRaw(raw));
    return null;
  }

  Future<void> _loadFromRaw(String raw) async {
    final storage = ref.read(storageProvider);
    final game = await compute(_gameFromRawJsonInIsolate, raw);
    if (game == null) {
      // Corrupt or undecodable blob — remove it.
      unawaited(storage.saveCurrentGame(null));
      return;
    }
    // Guard against a race where the player started a new game while we were
    // loading — in that case state is already non-null and we must not clobber it.
    if (state == null) {
      state = game;
      if (_active) _startTickingFrom(game);
    }
  }

  Set<(int, int)> get graceCells => _graceCells;

  void activate() {
    if (_active) return;
    _active = true;
    final g = state;
    if (g != null && !g.completed) {
      _startTickingFrom(g);
    }
  }

  void deactivate() {
    if (!_active) return;
    _active = false;
    // Cancel the ticker first so no more ticks fire while we're tearing down.
    _ticker?.cancel();
    _ticker = null;
    final snappedElapsed = state != null ? _currentElapsedMs(state!) : 0;
    _resumedAt = null;
    final g = state;
    if (g != null && !g.completed) {
      final snapped = g.tick(snappedElapsed);
      // Defer state mutation: calling state= during widget dispose/unmount
      // (BuildOwner.finalizeTree) triggers a Riverpod assertion. Schedule it
      // for the next microtask so the widget tree has finished tearing down.
      Future(() {
        state = snapped;
        unawaited(ref.read(storageProvider).saveCurrentGame(snapped));
      });
    }
  }

  /// Start a brand-new game, replacing any in-flight one. Generation runs in
  /// a background isolate so the UI doesn't stall on slow paths (expert /
  /// 12×12 in particular).
  Future<void> start(int size, Difficulty difficulty) async {
    final puzzle = await compute(_makeInIsolate, (size, difficulty));
    final game = Game.fresh(puzzle);
    ref.read(freshAchievementUnlocksProvider.notifier).clear();
    _clearGrace();
    _lastCommittedViolations = const {};
    state = game;
    if (_active) _startTickingFrom(game);
    await ref.read(storageProvider).saveCurrentGame(game);
    unawaited(
      ref
          .read(achievementsProvider.notifier)
          .evaluate(
            event: AchievementEvent(
              startedPuzzle: true,
              occurredAt: DateTime.now(),
            ),
          ),
    );
  }

  /// Start a specific puzzle by code (used by "play by code").
  Future<void> startFromCode(String code) async {
    final puzzle = await compute(_fromCodeInIsolate, code);
    final game = Game.fresh(puzzle);
    ref.read(freshAchievementUnlocksProvider.notifier).clear();
    _clearGrace();
    _lastCommittedViolations = const {};
    state = game;
    if (_active) _startTickingFrom(game);
    await ref.read(storageProvider).saveCurrentGame(game);
    unawaited(
      ref
          .read(achievementsProvider.notifier)
          .evaluate(
            event: AchievementEvent(
              startedPuzzle: true,
              occurredAt: DateTime.now(),
            ),
          ),
    );
  }

  void tap(int r, int c) {
    final cur = state;
    if (cur == null || cur.completed) return;
    if (cur.isClue(r, c)) return;
    _mutate(
      (g) => g.tap(r, c),
      beforeEmit: (next) {
        if (next.completed) {
          _clearGrace();
          return;
        }
        // Re-arm: every cell currently involved in a violation gets the
        // grace treatment, not just the tapped one.
        _graceCells = _firstViolationCells(next);
        _armGraceTimer();
      },
    );
  }

  void _armGraceTimer() {
    _graceTimer?.cancel();
    _graceTimer = Timer(_graceWindow, _commitGrace);
  }

  /// Commit pending grace: recompute the current violation set; if it
  /// introduces cells the previous commit didn't already account for,
  /// charge one mistake. Re-emit state so providers refresh.
  void _commitGrace() {
    _graceTimer?.cancel();
    _graceTimer = null;
    final g = state;
    if (g == null || g.completed) {
      _graceCells = const {};
      _lastCommittedViolations = const {};
      return;
    }
    final current = _firstViolationCells(g);
    final introducesNew = current.any(
      (c) => !_lastCommittedViolations.contains(c),
    );
    _graceCells = const {};
    _lastCommittedViolations = current;
    if (introducesNew && current.isNotEmpty) {
      state = g.incrementMistakes();
    } else {
      // Trigger a re-emission so graceCellsProvider picks up the cleared
      // grace set even if mistakes didn't change.
      state = g.tick(_currentElapsedMs(g));
    }
  }

  void _clearGrace() {
    _graceTimer?.cancel();
    _graceTimer = null;
    _graceCells = const {};
  }

  void undo() {
    _clearGrace();
    _lastCommittedViolations = const {};
    _mutate((g) => g.undo());
  }

  void restart() {
    final g = state;
    if (g == null) return;
    _clearGrace();
    _lastCommittedViolations = const {};
    final reset = g.restart();
    state = reset;
    if (_active) _startTickingFrom(reset);
    unawaited(ref.read(storageProvider).saveCurrentGame(reset));
  }

  /// Get the next deduction from the current board, or null if none.
  Deduction? peekHint() {
    final g = state;
    if (g == null) return null;
    return Solver.nextDeduction(g.board);
  }

  /// Bumps the hint counter without revealing a cell (stages 1 & 2).
  void recordHint() => _mutate((g) => g.recordHint());

  /// Consume a hint: reveal the deduced cell. Counter was already bumped
  /// at stages 1 and 2, so this does not increment it.
  void revealHint(Deduction d) {
    unawaited(
      ref
          .read(achievementsProvider.notifier)
          .evaluate(
            event: AchievementEvent(
              usedAllHintStages: true,
              occurredAt: DateTime.now(),
            ),
          ),
    );
    _mutate((g) => g.revealHint(d.r, d.c, d.value));
  }

  void recordShare() {
    unawaited(
      ref
          .read(achievementsProvider.notifier)
          .evaluate(
            event: AchievementEvent(
              sharedPuzzle: true,
              occurredAt: DateTime.now(),
            ),
          ),
    );
  }

  void abandon() {
    state = null;
    _clearGrace();
    _lastCommittedViolations = const {};
    _ticker?.cancel();
    unawaited(ref.read(storageProvider).saveCurrentGame(null));
  }

  // ─── completion handling ─────────────────────────────────────────────

  Future<void> _completeIfDone(Game prev, Game next) async {
    if (next.completed && !prev.completed) {
      _ticker?.cancel();
      _clearGrace();
      await ref
          .read(statsProvider.notifier)
          .record(
            SolvedRecord(
              code: next.puzzle.code,
              size: next.size,
              difficulty: next.puzzle.difficulty,
              elapsedMs: next.elapsedMs,
              hintsUsed: next.hintsUsed,
              mistakes: next.mistakes,
              solvedAt: DateTime.now(),
            ),
          );
      await ref
          .read(achievementsProvider.notifier)
          .evaluate(
            event: AchievementEvent(
              usedUndo: next.undoUsed,
              occurredAt: DateTime.now(),
            ),
          );
    }
  }

  // ─── ticker plumbing ─────────────────────────────────────────────────

  void _startTickingFrom(Game g) {
    _ticker?.cancel();
    _resumeOffset = g.elapsedMs;
    _resumedAt = DateTime.now();
    if (g.completed) return;
    _ticker = Timer.periodic(const Duration(milliseconds: 250), (_) {
      // Guard: deactivate() may have fired between the last tick and this one.
      if (!_active) {
        _ticker?.cancel();
        return;
      }
      final cur = state;
      if (cur == null || cur.completed) {
        _ticker?.cancel();
        return;
      }
      state = cur.tick(_currentElapsedMs(cur));
    });
  }

  int _currentElapsedMs(Game g) {
    final startedTick = _resumedAt;
    if (startedTick == null) return g.elapsedMs;
    return _resumeOffset +
        DateTime.now().difference(startedTick).inMilliseconds;
  }

  void _mutate(Game Function(Game) f, {void Function(Game)? beforeEmit}) {
    final g = state;
    if (g == null) return;
    final ticked = g.tick(_currentElapsedMs(g));
    final next = f(ticked);
    beforeEmit?.call(next);
    state = next;
    unawaited(ref.read(storageProvider).saveCurrentGame(next));
    unawaited(_completeIfDone(ticked, next));
  }
}

final gameProvider = NotifierProvider<GameController, Game?>(
  GameController.new,
);

// Convenience: the current hint for the live board, or null if no game is
// active. The result is conclusive — for a legal, mistake-free state it always
// carries a forced move; for an invalid state it reports a contradiction so the
// UI can warn the player.
//
// Every technique the solver surfaces here is human-reproducible: it reasons
// about a single row/column or a single near-identical pair, so the player can
// follow it (the hardest, linePatterns, just asks them to list the legal ways
// one line can be filled). We deliberately do NOT exclude linePatterns: hiding
// it used to force the hint onto a whole-board contradiction search the player
// could never replicate. Solver.hint keeps that search only as an unreachable
// safety net for non-generated boards.
final nextHintProvider = Provider<HintResult?>((ref) {
  final g = ref.watch(gameProvider);
  if (g == null || g.completed) return null;
  return Solver.hint(g.board);
});

// Convenience: simple flag for whether placing the last move violated rules.
final hasViolationProvider = Provider<bool>((ref) {
  final g = ref.watch(gameProvider);
  if (g == null) return false;
  return !_boardOk(g);
});

bool _boardOk(Game g) {
  // Use the rules check; we only care about whether the current state is
  // partially valid.
  return g.board.cells.every((_) => true) && _firstViolationCells(g).isEmpty;
}

/// Returns the cells implicated in the current violation, for UI highlights.
/// All cells in the active grace set (those tagged by the most-recent
/// tap for a 1-second commit window) are filtered out so the red flash
/// happens to every affected cell simultaneously at commit time.
final graceCellsProvider = Provider<Set<(int, int)>>((ref) {
  ref.watch(gameProvider);
  return ref.read(gameProvider.notifier).graceCells;
});

final violationCellsProvider = Provider<Set<(int, int)>>((ref) {
  final g = ref.watch(
    gameProvider.select(
      (g) => g == null ? null : (board: g.board, size: g.size),
    ),
  );
  if (g == null) return const {};
  final cells = _firstViolationCellsFromBoard(g.board, g.size);
  final grace = ref.watch(graceCellsProvider);
  if (grace.isEmpty || cells.isEmpty) return cells;
  return cells.where((c) => !grace.contains(c)).toSet();
});

Set<(int, int)> _firstViolationCells(Game g) =>
    _firstViolationCellsFromBoard(g.board, g.size);

Set<(int, int)> _firstViolationCellsFromBoard(Board board, int n) {
  // Use a custom scan rather than the violation enum so we can highlight every
  // offending cell at once, not just one.
  final out = <(int, int)>{};
  final half = n ~/ 2;

  // Count overflow → highlight all cells in the offending row/col.
  for (var i = 0; i < n; i++) {
    final row = board.row(i);
    final col = board.col(i);
    final rs = row.where((c) => c == Cell.sun).length;
    final rm = row.where((c) => c == Cell.moon).length;
    final cs = col.where((c) => c == Cell.sun).length;
    final cm = col.where((c) => c == Cell.moon).length;
    if (rs > half || rm > half) {
      for (var c = 0; c < n; c++) {
        out.add((i, c));
      }
    }
    if (cs > half || cm > half) {
      for (var r = 0; r < n; r++) {
        out.add((r, i));
      }
    }
  }

  // No-three windows.
  for (var r = 0; r < n; r++) {
    for (var c = 0; c <= n - 3; c++) {
      final a = board.at(r, c), b = board.at(r, c + 1), d = board.at(r, c + 2);
      if (a.isFilled && a == b && b == d) {
        out
          ..add((r, c))
          ..add((r, c + 1))
          ..add((r, c + 2));
      }
    }
  }
  for (var c = 0; c < n; c++) {
    for (var r = 0; r <= n - 3; r++) {
      final a = board.at(r, c), b = board.at(r + 1, c), d = board.at(r + 2, c);
      if (a.isFilled && a == b && b == d) {
        out
          ..add((r, c))
          ..add((r + 1, c))
          ..add((r + 2, c));
      }
    }
  }

  // Edge violations.
  for (final e in board.constraints.edges.entries) {
    final pos = e.key;
    final (or, oc) = pos.otherCell;
    final a = board.at(pos.r, pos.c);
    final b = board.at(or, oc);
    if (!e.value.satisfiedBy(a, b)) {
      out
        ..add((pos.r, pos.c))
        ..add((or, oc));
    }
  }

  // Duplicate fully-filled rows / cols.
  for (var i = 0; i < n; i++) {
    final rowI = board.row(i);
    if (!rowI.every((c) => c.isFilled)) continue;
    for (var j = i + 1; j < n; j++) {
      final rowJ = board.row(j);
      if (!rowJ.every((c) => c.isFilled)) continue;
      if (_listEq(rowI, rowJ)) {
        for (var c = 0; c < n; c++) {
          out
            ..add((i, c))
            ..add((j, c));
        }
      }
    }
  }
  for (var i = 0; i < n; i++) {
    final colI = board.col(i);
    if (!colI.every((c) => c.isFilled)) continue;
    for (var j = i + 1; j < n; j++) {
      final colJ = board.col(j);
      if (!colJ.every((c) => c.isFilled)) continue;
      if (_listEq(colI, colJ)) {
        for (var r = 0; r < n; r++) {
          out
            ..add((r, i))
            ..add((r, j));
        }
      }
    }
  }

  return out;
}

bool _listEq(List<Cell> a, List<Cell> b) {
  if (a.length != b.length) return false;
  for (var i = 0; i < a.length; i++) {
    if (a[i] != b[i]) return false;
  }
  return true;
}

```

# Read lib/ui/screens/game_screen.dart
```dart
import 'dart:math' as math;

import 'package:confetti/confetti.dart';
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:share_plus/share_plus.dart';

import '../../ads/ad_gate.dart';
import '../../domain/achievement.dart';
import '../../domain/game/game.dart';
import '../../domain/solver/solver.dart';
import '../../domain/solver/technique.dart';
import '../state/providers.dart';
import '../theme/sundown_theme.dart';
import '../share_links.dart';
import '../widgets/board_widget.dart';
import '../widgets/control_button.dart';
import '../widgets/puzzle_code_chip.dart';
import 'how_to_play_screen.dart';
import 'watch_ad_dialog.dart';

/// Main play screen. Drives a single [Game] through to completion.
class GameScreen extends ConsumerStatefulWidget {
  const GameScreen({super.key});

  @override
  ConsumerState<GameScreen> createState() => _GameScreenState();
}

class _GameScreenState extends ConsumerState<GameScreen>
    with WidgetsBindingObserver {
  /// Hint reveal stage:
  ///   0 = no hint pending
  ///   1 = region highlight
  ///   2 = technique label shown
  ///   3 = cell about to be revealed (next tap consumes)
  int _hintStage = 0;
  Deduction? _pendingHint;

  late final ConfettiController _confetti = ConfettiController(
    duration: const Duration(milliseconds: 1800),
  );

  // Cached so dispose() can call deactivate() without going through ref,
  // which throws a StateError when the widget is already unmounted.
  late final GameController _gameController;

  @override
  void initState() {
    super.initState();
    _gameController = ref.read(gameProvider.notifier);
    // Observe app lifecycle so the timer pauses when the app is backgrounded
    // *while the board is on screen* — and never runs on the home screen, since
    // this observer only exists while the GameScreen is mounted.
    WidgetsBinding.instance.addObserver(this);
    // Every entry into the game screen counts as a play. Enforce the daily
    // ad gate before the board becomes interactive, then start the timer.
    WidgetsBinding.instance.addPostFrameCallback(
      (_) => _enforceGateThenActivate(),
    );
  }

  /// Pause the puzzle timer while the app is backgrounded so off-screen wall
  /// time doesn't inflate the player's solve time; resume on return. Scoped to
  /// the game screen — the home screen has no such observer.
  @override
  void didChangeAppLifecycleState(AppLifecycleState s) {
    switch (s) {
      case AppLifecycleState.paused:
      case AppLifecycleState.inactive:
      case AppLifecycleState.hidden:
        _gameController.deactivate();
      case AppLifecycleState.resumed:
        _gameController.activate();
      case AppLifecycleState.detached:
        break;
    }
  }

  /// If the player is out of free plays for the day, present the rewarded-ad
  /// gate. Watching grants bonus plays and lets the game proceed; backing out
  /// returns to the home screen without consuming a play.
  Future<void> _enforceGateThenActivate() async {
    final gate = ref.read(adGateProvider.notifier);
    if (gate.needsAd) {
      final earned = await WatchAdDialog.show(
        context,
        fallbackTier: ref.read(adFallbackTierProvider),
      );
      if (!mounted) return;
      if (!earned) {
        Navigator.of(context).maybePop();
        return;
      }
    }
    gate.registerPlay();
    _gameController.activate();
  }

  @override
  void dispose() {
    WidgetsBinding.instance.removeObserver(this);
    _gameController.deactivate();
    _confetti.dispose();
    super.dispose();
  }

  void _onHint() {
    final g = ref.read(gameProvider);
    if (g == null || g.completed) return;
    // If the player edits the board between hint stages, recompute. The solver
    // returns a conclusive result: a forced move for a legal state, or a
    // contradiction for one carrying a mistake (overt or silent).
    final result = ref.read(nextHintProvider);
    if (result == null) return;

    switch (result.status) {
      case HintStatus.contradiction:
        _showHintMessage(
          'There are mistakes on the board — fix them before using a hint.',
        );
        return;
      case HintStatus.ambiguous:
        // Only reachable on a non-unique (hand-built) board; nothing is forced.
        _showHintMessage('No deduction available from this state.');
        return;
      case HintStatus.solved:
        return; // board already complete — completion handling takes over
      case HintStatus.ok:
        break;
    }
    final fresh = result.deduction!;

    // Click 1 — new hint target: show witness cells and count it.
    if (_pendingHint == null ||
        _pendingHint!.r != fresh.r ||
        _pendingHint!.c != fresh.c ||
        _pendingHint!.value != fresh.value) {
      _pendingHint = fresh;
      ref.read(gameProvider.notifier).recordHint();
      setState(() => _hintStage = 1);
      return;
    }
    if (_hintStage < 3) {
      setState(() => _hintStage += 1);
      if (_hintStage == 2) {
        // Click 2 — show reason text: count it.
        ref.read(gameProvider.notifier).recordHint();
      }
      if (_hintStage == 3) {
        // Click 3 — reveal: answer already paid for; do NOT count again.
        ref.read(gameProvider.notifier).revealHint(fresh);
        setState(() {
          _hintStage = 0;
          _pendingHint = null;
        });
      }
    }
  }

  void _showHintMessage(String message) {
    ScaffoldMessenger.of(
      context,
    ).showSnackBar(SnackBar(content: Text(message)));
  }

  void _onTap(int r, int c) {
    if (_hintStage != 0) {
      setState(() {
        _hintStage = 0;
        _pendingHint = null;
      });
    }
    ref.read(gameProvider.notifier).tap(r, c);
  }

  @override
  Widget build(BuildContext context) {
    ref.listen(achievementsProvider, (previous, next) {
      final before = previous?.map((a) => a.id).toSet() ?? const <String>{};
      final fresh = next.where((a) => !before.contains(a.id)).toList();
      if (fresh.isEmpty || !mounted) return;
      ref.read(freshAchievementUnlocksProvider.notifier).addAll(fresh);
      final name = achievementDefinitions
          .firstWhere((d) => d.id == fresh.last.id)
          .name;
      ScaffoldMessenger.of(
        context,
      ).showSnackBar(SnackBar(content: Text('Achievement unlocked: $name')));
    });
    ref.listen<Game?>(gameProvider, (previous, next) {
      if (next == null || !next.completed || previous?.completed == true) {
        return;
      }
      WidgetsBinding.instance.addPostFrameCallback((_) {
        Future.delayed(const Duration(milliseconds: 330), () {
          if (!context.mounted) return;
          _confetti.play();
          _showSolvedDialog(context, ref, next);
        });
      });
    });
    final t = context.tokens;
    final hasGame = ref.watch(gameProvider.select((g) => g != null));

    if (!hasGame) {
      return Scaffold(
        backgroundColor: t.background,
        body: Center(child: Text('No active game.', style: t.body)),
      );
    }

    return Scaffold(
      backgroundColor: t.background,
      body: Stack(
        children: [
          SafeArea(
            child: Column(
              children: [
                const _Header(),
                const SizedBox(height: 8),
                Expanded(
                  child: _GameBoardArea(
                    onTap: _onTap,
                    hintStage: _hintStage,
                    pendingHint: _pendingHint,
                  ),
                ),
                const SizedBox(height: 8),
                _Controls(onHint: _onHint, hintStage: _hintStage),
                const SizedBox(height: 12),
              ],
            ),
          ),
          // Confetti emitter centered top, blasts downward.
          Align(
            alignment: Alignment.topCenter,
            child: ConfettiWidget(
              confettiController: _confetti,
              blastDirection: math.pi / 2,
              maxBlastForce: 24,
              minBlastForce: 12,
              emissionFrequency: 0.04,
              numberOfParticles: 18,
              gravity: 0.25,
              shouldLoop: false,
              colors: [t.sun, t.moon, t.accent, t.success],
            ),
          ),
        ],
      ),
    );
  }
}

class _Header extends ConsumerWidget {
  const _Header();

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final t = context.tokens;
    final meta = ref.watch(
      gameProvider.select(
        (g) => g == null
            ? null
            : (
                size: g.size,
                difficulty: g.puzzle.difficulty.code,
                hintsUsed: g.hintsUsed,
                mistakes: g.mistakes,
                code: g.puzzle.code,
                completed: g.completed,
              ),
      ),
    );
    final elapsed = _fmt(
      ref.watch(gameProvider.select((g) => g?.elapsedMs ?? 0)),
    );
    if (meta == null) {
      return const SizedBox.shrink();
    }
    return Padding(
      padding: EdgeInsets.fromLTRB(t.screenPadding, 8, t.screenPadding, 0),
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.stretch,
        children: [
          // Row 1: nav buttons on the left, puzzle meta on the right.
          // Keeping the 44 px icon targets on the far left and the difficulty
          // + code on the far right makes intentional use of the full width
          // while guaranteeing neither side squeezes the other.
          Row(
            crossAxisAlignment: CrossAxisAlignment.center,
            children: [
              IconButton(
                tooltip: 'Back',
                icon: const Icon(Icons.arrow_back_rounded),
                color: t.textPrimary,
                padding: EdgeInsets.zero,
                constraints: const BoxConstraints(minWidth: 44, minHeight: 44),
                onPressed: () => Navigator.of(context).maybePop(),
              ),
              const SizedBox(width: 4),
              IconButton(
                tooltip: 'How to Play',
                icon: const Icon(Icons.help_outline_rounded),
                color: t.textPrimary,
                padding: EdgeInsets.zero,
                constraints: const BoxConstraints(minWidth: 44, minHeight: 44),
                onPressed: () async {
                  final controller = ref.read(gameProvider.notifier);
                  controller.deactivate();
                  await Navigator.of(context).push(
                    MaterialPageRoute(builder: (_) => const HowToPlayScreen()),
                  );
                  if (context.mounted && !meta.completed) {
                    controller.activate();
                  }
                },
              ),
              const Spacer(),
              // Difficulty label + puzzle code, right-aligned.
              // Wrap handles text-scale overflow gracefully without extra rows.
              Flexible(
                child: Wrap(
                  spacing: 6,
                  runSpacing: 4,
                  alignment: WrapAlignment.end,
                  crossAxisAlignment: WrapCrossAlignment.center,
                  children: [
                    Text(
                      '${meta.size}×${meta.size} · ${_diffName(meta.difficulty)}',
                      style: t.label,
                      textAlign: TextAlign.end,
                    ),
                    PuzzleCodeChip(code: meta.code),
                  ],
                ),
              ),
            ],
          ),
          const SizedBox(height: 8),
          // Row 2: stat chips spread evenly across the full width.
          // IntrinsicHeight + Row(crossAxisAlignment.stretch) makes all three
          // chips the same height regardless of value length.
          IntrinsicHeight(
            child: Row(
              crossAxisAlignment: CrossAxisAlignment.stretch,
              children: [
                Expanded(
                  child: StatChip(
                    label: 'time',
                    value: elapsed,
                    icon: Icons.timer_outlined,
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: StatChip(
                    label: 'hints',
                    value: '${meta.hintsUsed}',
                    icon: Icons.lightbulb_outline,
                  ),
                ),
                const SizedBox(width: 8),
                Expanded(
                  child: StatChip(
                    label: 'mistakes',
                    value: '${meta.mistakes}',
                    icon: Icons.error_outline,
                  ),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}

class _GameBoardArea extends ConsumerWidget {
  final CellTap onTap;
  final int hintStage;
  final Deduction? pendingHint;

  const _GameBoardArea({
    required this.onTap,
    required this.hintStage,
    required this.pendingHint,
  });

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final t = context.tokens;
    final game = ref.watch(
      gameProvider.select(
        (g) => g == null
            ? null
            : (puzzle: g.puzzle, board: g.board, completed: g.completed),
      ),
    );
    if (game == null) return const SizedBox.shrink();

    final highlights = <(int, int)>{};
    String? hintReason;
    if (pendingHint != null) {
      if (hintStage >= 1) {
        highlights.addAll(pendingHint!.witnessCells);
      }
      if (hintStage >= 2) hintReason = pendingHint!.reason;
    }
    final errors = ref.watch(violationCellsProvider);

    return Padding(
      padding: EdgeInsets.symmetric(horizontal: t.screenPadding),
      child: Stack(
        children: [
          BoardWidget(
            puzzle: game.puzzle,
            board: game.board,
            onTap: onTap,
            highlightCells: highlights,
            errorCells: errors,
            locked: game.completed,
          ),
          Positioned(
            left: 0,
            right: 0,
            bottom: 8,
            child: AnimatedSwitcher(
              duration: const Duration(milliseconds: 200),
              transitionBuilder: (child, anim) => FadeTransition(
                opacity: anim,
                child: SlideTransition(
                  position: Tween<Offset>(
                    begin: const Offset(0, 0.2),
                    end: Offset.zero,
                  ).animate(anim),
                  child: child,
                ),
              ),
              child: hintReason == null
                  ? const SizedBox.shrink(key: ValueKey('none'))
                  : _HintBanner(key: ValueKey(hintReason), text: hintReason),
            ),
          ),
        ],
      ),
    );
  }
}

class _Controls extends ConsumerWidget {
  final VoidCallback? onHint;
  final int hintStage;
  const _Controls({required this.onHint, required this.hintStage});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final game = ref.watch(
      gameProvider.select(
        (g) => g == null
            ? null
            : (historyEmpty: g.history.isEmpty, completed: g.completed),
      ),
    );
    final t = context.tokens;
    if (game == null) return const SizedBox.shrink();
    final hintLabel = switch (hintStage) {
      0 => 'Hint',
      1 => 'Where',
      2 => 'Why',
      _ => 'Reveal',
    };
    return Padding(
      padding: EdgeInsets.symmetric(horizontal: t.screenPadding),
      child: Wrap(
        alignment: WrapAlignment.center,
        spacing: 8,
        runSpacing: 8,
        children: [
          ControlButton(
            icon: Icons.undo_rounded,
            label: 'Undo',
            onPressed: game.historyEmpty || game.completed
                ? null
                : () => ref.read(gameProvider.notifier).undo(),
          ),
          ControlButton(
            icon: Icons.refresh_rounded,
            label: 'Restart',
            onPressed: game.historyEmpty || game.completed
                ? null
                : () => ref.read(gameProvider.notifier).restart(),
          ),
          ControlButton(
            icon: Icons.lightbulb_outline,
            label: hintLabel,
            primary: hintStage > 0,
            onPressed: game.completed ? null : onHint,
            badge: hintStage > 0 ? hintStage : null,
          ),
        ],
      ),
    );
  }
}

class _HintBanner extends StatelessWidget {
  final String text;
  const _HintBanner({super.key, required this.text});
  @override
  Widget build(BuildContext context) {
    final t = context.tokens;
    return Padding(
      padding: EdgeInsets.symmetric(horizontal: t.screenPadding, vertical: 6),
      child: Container(
        padding: const EdgeInsets.all(12),
        decoration: BoxDecoration(
          color: t.accent.withValues(alpha: 0.15),
          borderRadius: BorderRadius.circular(t.cardRadius * 0.7),
          border: Border.all(color: t.accent, width: 0.7),
        ),
        child: Row(
          children: [
            Icon(Icons.tips_and_updates_outlined, color: t.accent, size: 18),
            const SizedBox(width: 8),
            Expanded(child: Text(text, style: t.body)),
          ],
        ),
      ),
    );
  }
}

void _showSolvedDialog(BuildContext context, WidgetRef ref, Game game) {
  final t = context.tokens;
  final freshAchievements = ref.read(freshAchievementUnlocksProvider);
  showDialog<void>(
    context: context,
    barrierDismissible: true,
    builder: (dialogCtx) => SafeArea(
      child: Align(
        alignment: Alignment.topCenter,
        child: Padding(
          padding: const EdgeInsets.only(top: 72),
          child: Dialog(
            backgroundColor: t.surface,
            shape: RoundedRectangleBorder(
              borderRadius: BorderRadius.circular(t.cardRadius),
              side: BorderSide(color: t.success, width: 0.7),
            ),
            child: ConstrainedBox(
              constraints: const BoxConstraints(maxWidth: 420, maxHeight: 560),
              child: Padding(
                padding: const EdgeInsets.all(20),
                child: Column(
                  mainAxisSize: MainAxisSize.min,
                  crossAxisAlignment: CrossAxisAlignment.start,
                  children: [
                    Row(
                      children: [
                        Icon(
                          Icons.celebration_outlined,
                          color: t.success,
                          size: 24,
                        ),
                        const SizedBox(width: 10),
                        Expanded(
                          child: Text(
                            'Solved!',
                            style: t.title.copyWith(color: t.success),
                          ),
                        ),
                        // Close the dialog to view the finished board (it's
                        // still behind, fully solved). Re-openable? No - but
                        // Home/Share remain reachable from the board controls.
                        IconButton(
                          onPressed: () => Navigator.of(dialogCtx).pop(),
                          icon: Icon(Icons.close_rounded, color: t.textMuted),
                          tooltip: 'View puzzle',
                          visualDensity: VisualDensity.compact,
                          padding: EdgeInsets.zero,
                          constraints: const BoxConstraints(),
                        ),
                      ],
                    ),
                    Flexible(
                      child: SingleChildScrollView(
                        padding: const EdgeInsets.only(top: 10),
                        child: Column(
                          mainAxisSize: MainAxisSize.min,
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              '${_fmt(game.elapsedMs)} · ${game.hintsUsed} hint${game.hintsUsed == 1 ? "" : "s"} · ${game.mistakes} mistake${game.mistakes == 1 ? "" : "s"}',
                              style: t.body,
                            ),
                            const SizedBox(height: 16),
                            Wrap(
                              spacing: 8,
                              runSpacing: 8,
                              children: [
                                ControlButton(
                                  icon: Icons.home_outlined,
                                  label: 'Home',
                                  onPressed: () {
                                    Navigator.of(dialogCtx).pop();
                                    Navigator.of(context).maybePop();
                                  },
                                ),
                                ControlButton(
                                  icon: Icons.share_outlined,
                                  label: 'Share',
                                  onPressed: () {
                                    ref
                                        .read(gameProvider.notifier)
                                        .recordShare();
                                    SharePlus.instance.share(
                                      ShareParams(
                                        text: puzzleShareText(game.puzzle.code),
                                        subject: 'Sundown ${game.puzzle.code}',
                                      ),
                                    );
                                  },
                                ),
                              ],
                            ),
                            if (freshAchievements.isNotEmpty) ...[
                              const SizedBox(height: 18),
                              Text('New achievements', style: t.label),
                              const SizedBox(height: 8),
                              for (final unlock in freshAchievements) ...[
                                _AchievementUnlockTile(unlock: unlock),
                                if (unlock != freshAchievements.last)
                                  const SizedBox(height: 8),
                              ],
                            ],
                          ],
                        ),
                      ),
                    ),
                  ],
                ),
              ),
            ),
          ),
        ),
      ),
    ),
  );
}

class _AchievementUnlockTile extends StatelessWidget {
  final UnlockedAchievement unlock;
  const _AchievementUnlockTile({required this.unlock});

  @override
  Widget build(BuildContext context) {
    final t = context.tokens;
    final definition = achievementDefinitions.firstWhere(
      (d) => d.id == unlock.id,
      orElse: () => AchievementDefinition(
        id: unlock.id,
        name: unlock.id,
        description: 'Achievement unlocked.',
        category: AchievementCategory.persistence,
      ),
    );
    return Container(
      padding: const EdgeInsets.all(12),
      decoration: BoxDecoration(
        color: t.success.withValues(alpha: 0.10),
        borderRadius: BorderRadius.circular(t.cardRadius * 0.7),
        border: Border.all(color: t.success.withValues(alpha: 0.35)),
      ),
      child: Row(
        children: [
          Icon(Icons.emoji_events_outlined, color: t.success, size: 20),
          const SizedBox(width: 10),
          Expanded(
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                Text(
                  definition.name,
                  style: t.body.copyWith(fontWeight: FontWeight.w700),
                ),
                const SizedBox(height: 2),
                Text(
                  definition.description,
                  style: t.label.copyWith(color: t.textMuted),
                ),
              ],
            ),
          ),
        ],
      ),
    );
  }
}

String _fmt(int ms) {
  final s = (ms / 1000).floor();
  final m = s ~/ 60;
  final sec = s % 60;
  return '${m.toString()}:${sec.toString().padLeft(2, '0')}';
}

String _diffName(String code) => switch (code) {
  'E' => 'Easy',
  'M' => 'Medium',
  'H' => 'Hard',
  'X' => 'Expert',
  _ => code,
};

```

# Read lib/ui/widgets/board_widget.dart
```dart
import 'dart:io';
import 'dart:math' as math;

import 'package:flutter/widgets.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

import '../../domain/board/board.dart';
import '../../domain/board/cell.dart';
import '../../domain/board/puzzle.dart';
import '../state/providers.dart';
import '../theme/sundown_theme.dart';
import 'cell_widget.dart';
import 'edges_overlay.dart';

typedef CellTap = void Function(int r, int c);

/// Renders the game board: cells, edge marks, error/hint highlights.
class BoardWidget extends StatelessWidget {
  final Puzzle puzzle;
  final Board board;
  final CellTap onTap;
  final Set<(int, int)> highlightCells;
  final Set<(int, int)> errorCells;

  /// When true, the board is non-interactive (e.g. after solve).
  final bool locked;

  const BoardWidget({
    super.key,
    required this.puzzle,
    required this.board,
    required this.onTap,
    this.highlightCells = const {},
    this.errorCells = const {},
    this.locked = false,
  });

  @override
  Widget build(BuildContext context) {
    final theme = context.theme;
    final t = theme.tokens;
    final n = puzzle.size;

    return LayoutBuilder(
      builder: (ctx, constraints) {
        final available = math.min(constraints.maxWidth, constraints.maxHeight);
        final innerSide = available - t.boardPadding * 2;
        final cellSide = (innerSide - t.cellGap * (n - 1)) / n;
        final boardSide = cellSide * n + t.cellGap * (n - 1);

        return Center(
          child: Container(
            padding: EdgeInsets.all(t.boardPadding),
            decoration: BoxDecoration(
              color: t.surface,
              borderRadius: BorderRadius.circular(t.cardRadius),
              border: Border.all(color: t.gridLine, width: 1),
            ),
            child: SizedBox(
              width: boardSide,
              height: boardSide,
              child: Stack(
                children: [
                  for (var r = 0; r < n; r++)
                    for (var c = 0; c < n; c++)
                      _buildCell(
                        row: r,
                        column: c,
                        cellSide: cellSide,
                        clues: puzzle.clues,
                        board: board,
                        locked: locked,
                        highlightCells: highlightCells,
                        errorCells: errorCells,
                        onTap: onTap,
                        cellGap: t.cellGap,
                      ),
                  EdgesOverlay(
                    constraints: puzzle.constraints,
                    size: n,
                    cellSide: cellSide,
                    cellGap: t.cellGap,
                    tokens: t,
                  ),
                ],
              ),
            ),
          ),
        );
      },
    );
  }

  Widget _buildCell({
    required int row,
    required int column,
    required double cellSide,
    required Board clues,
    required Board board,
    required bool locked,
    required Set<(int, int)> highlightCells,
    required Set<(int, int)> errorCells,
    required CellTap onTap,
    required double cellGap,
  }) {
    final isClue = clues.at(row, column).isFilled;
    return Positioned(
      left: column * (cellSide + cellGap),
      top: row * (cellSide + cellGap),
      width: cellSide,
      height: cellSide,
      child: CellWidget(
        row: row,
        column: column,
        cell: board.at(row, column),
        isClue: isClue,
        highlighted: highlightCells.contains((row, column)),
        errorHighlighted: errorCells.contains((row, column)),
        side: cellSide,
        onTap: locked || isClue ? null : () => onTap(row, column),
      ),
    );
  }
}

/// Non-interactive solid-color preview of just the clue cells — used in
/// home-screen "continue" tile, share previews, etc.
class BoardThumbnail extends ConsumerWidget {
  final Puzzle puzzle;
  final double size;
  const BoardThumbnail({super.key, required this.puzzle, required this.size});

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final theme = context.theme;
    final t = theme.tokens;
    final customImages = ref.watch(customSymbolImagesProvider).value;
    final useCustomImages = ref.watch(premiumProvider);
    final n = puzzle.size;
    final gap = math.max(1.0, t.cellGap * 0.6);
    final cellSide = (size - gap * (n - 1)) / n;
    return SizedBox(
      width: size,
      height: size,
      child: Stack(
        children: [
          for (var r = 0; r < n; r++)
            for (var c = 0; c < n; c++)
              Positioned(
                left: c * (cellSide + gap),
                top: r * (cellSide + gap),
                width: cellSide,
                height: cellSide,
                child: Container(
                  decoration: BoxDecoration(
                    color: t.surfaceElevated,
                    borderRadius: BorderRadius.circular(t.cellRadius * 0.5),
                  ),
                  child: () {
                    final cell = puzzle.clues.at(r, c);
                    if (cell == Cell.empty) return const SizedBox.shrink();
                    final imagePath = useCustomImages
                        ? switch (cell) {
                            Cell.sun => customImages?.sunPath,
                            Cell.moon => customImages?.moonPath,
                            Cell.empty => null,
                          }
                        : null;
                    return _ThumbSymbolView(
                      cell: cell,
                      color: cell == Cell.sun ? t.sun : t.moon,
                      theme: theme,
                      imagePath: imagePath,
                      side: cellSide,
                    );
                  }(),
                ),
              ),
        ],
      ),
    );
  }
}

class _ThumbSymbolView extends StatelessWidget {
  final Cell cell;
  final Color color;
  final SundownTheme theme;
  final String? imagePath;
  final double side;

  const _ThumbSymbolView({
    required this.cell,
    required this.color,
    required this.theme,
    required this.imagePath,
    required this.side,
  });

  @override
  Widget build(BuildContext context) {
    final path = imagePath;
    if (path != null) {
      final cacheSize = math
          .min(384, (side * MediaQuery.of(context).devicePixelRatio).round())
          .toInt();
      return Padding(
        padding: const EdgeInsets.all(2),
        child: ClipOval(
          child: Image(
            image: ResizeImage(
              FileImage(File(path)),
              width: cacheSize,
              height: cacheSize,
            ),
            fit: BoxFit.cover,
            errorBuilder: (_, _, _) => CustomPaint(
              painter: _ThumbSymbolPainter(
                cell: cell,
                color: color,
                theme: theme,
              ),
            ),
          ),
        ),
      );
    }
    return CustomPaint(
      painter: _ThumbSymbolPainter(cell: cell, color: color, theme: theme),
    );
  }
}

class _ThumbSymbolPainter extends CustomPainter {
  final Cell cell;
  final Color color;
  final SundownTheme theme;
  _ThumbSymbolPainter({
    required this.cell,
    required this.color,
    required this.theme,
  });

  @override
  void paint(Canvas canvas, Size size) =>
      theme.symbols.paint(canvas, size, cell, color);

  @override
  bool shouldRepaint(_ThumbSymbolPainter old) =>
      old.cell != cell || old.color != color;
}

```

# Read lib/ui/widgets/cell_widget.dart
```dart
import 'dart:io';
import 'dart:math' as math;

import 'package:flutter/widgets.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

import '../../domain/board/cell.dart';
import '../state/providers.dart';
import '../theme/sundown_theme.dart';

/// One cell on the board. Animates between sun/moon/empty using the
/// theme's [SymbolPainter] so symbol style is theme-controlled.
///
/// Fixed (clue) vs user-placed tiles pull from distinct theme tokens:
/// fixed tiles are recessed (darker/desaturated background, optional
/// inner stroke) so they feel etched into the board; user tiles are
/// raised (lighter background, optional subtle ring) so they feel
/// placed on top. The sun/moon glyph color is identical in both cases.
class CellWidget extends ConsumerWidget {
  final int row;
  final int column;
  final Cell cell;
  final bool isClue;
  final bool highlighted;
  final bool errorHighlighted;
  final VoidCallback? onTap;
  final double side;

  const CellWidget({
    super.key,
    required this.row,
    required this.column,
    required this.cell,
    required this.isClue,
    required this.highlighted,
    required this.errorHighlighted,
    required this.onTap,
    required this.side,
  });

  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final theme = context.theme;
    final t = theme.tokens;
    final customImages = ref.watch(customSymbolImagesProvider).value;
    final useCustomImages = ref.watch(premiumProvider);
    final customImagePath = useCustomImages
        ? switch (cell) {
            Cell.sun => customImages?.sunPath,
            Cell.moon => customImages?.moonPath,
            Cell.empty => null,
          }
        : null;
    final bg = cell == Cell.empty
        ? t.emptyCellBg
        : isClue
        ? t.fixedTileBg
        : t.userTileBg;
    final outline = errorHighlighted
        ? t.error
        : highlighted
        ? t.accent
        : null;
    return Semantics(
      container: true,
      label: _semanticLabel(),
      button: onTap != null,
      onTap: onTap,
      child: GestureDetector(
        behavior: HitTestBehavior.opaque,
        onTap: onTap,
        child: ExcludeSemantics(
          child: AnimatedContainer(
            duration: const Duration(milliseconds: 160),
            curve: Curves.easeOut,
            width: side,
            height: side,
            decoration: BoxDecoration(
              color: bg,
              borderRadius: BorderRadius.circular(t.cellRadius),
              border: outline != null
                  ? Border.all(color: outline, width: 2)
                  : Border.all(color: t.gridLine, width: 0.6),
            ),
            child: CustomPaint(
              painter: _TileTreatmentPainter(
                cell: cell,
                isClue: isClue,
                fixedInner: t.fixedTileInnerBorder,
                userRing: t.userTileRing,
                radius: t.cellRadius,
              ),
              child: AnimatedSwitcher(
                duration: const Duration(milliseconds: 180),
                switchInCurve: Curves.easeOut,
                switchOutCurve: Curves.easeIn,
                transitionBuilder: (child, animation) => ScaleTransition(
                  scale: animation,
                  child: FadeTransition(opacity: animation, child: child),
                ),
                child: cell == Cell.empty
                    ? const SizedBox.shrink(key: ValueKey('e'))
                    : _SymbolView(
                        key: ValueKey(
                          '${cell.glyph}${isClue ? 'c' : 'p'}$customImagePath',
                        ),
                        cell: cell,
                        color: cell == Cell.sun ? t.sun : t.moon,
                        theme: theme,
                        imagePath: customImagePath,
                        side: side,
                      ),
              ),
            ),
          ),
        ),
      ),
    );
  }

  String _semanticLabel() {
    final value = switch (cell) {
      Cell.empty => 'empty',
      Cell.sun => 'sun',
      Cell.moon => 'moon',
    };
    final role = isClue ? 'clue' : 'player';
    final states = <String>[];
    if (highlighted) states.add('Highlighted');
    if (errorHighlighted) states.add('Error');
    final stateSuffix = states.isEmpty ? '.' : '. ${states.join('. ')}.';
    return 'Row ${row + 1}, column ${column + 1}. ${role.capitalize()} cell, $value$stateSuffix';
  }
}

extension on String {
  String capitalize() =>
      isEmpty ? this : '${this[0].toUpperCase()}${substring(1)}';
}

class _SymbolView extends StatelessWidget {
  final Cell cell;
  final Color color;
  final SundownTheme theme;
  final String? imagePath;
  final double side;

  const _SymbolView({
    super.key,
    required this.cell,
    required this.color,
    required this.theme,
    required this.imagePath,
    required this.side,
  });

  @override
  Widget build(BuildContext context) {
    final path = imagePath;
    if (path != null) {
      final cacheSize = math
          .min(384, (side * MediaQuery.of(context).devicePixelRatio).round())
          .toInt();
      return Padding(
        padding: EdgeInsets.all(side * 0.12),
        child: ClipOval(
          child: Image(
            image: ResizeImage(
              FileImage(File(path)),
              width: cacheSize,
              height: cacheSize,
            ),
            fit: BoxFit.cover,
            errorBuilder: (_, _, _) => CustomPaint(
              size: Size(side, side),
              painter: _SymbolPainterAdapter(
                cell: cell,
                color: color,
                theme: theme,
              ),
            ),
          ),
        ),
      );
    }
    return CustomPaint(
      size: Size(side, side),
      painter: _SymbolPainterAdapter(cell: cell, color: color, theme: theme),
    );
  }
}

/// Draws the structural treatment that sells "fixed = etched, user =
/// raised": a 1px inner stroke on fixed tiles, or a subtle inner ring
/// on user tiles. Both are tokenized (nullable) so themes can opt out.
class _TileTreatmentPainter extends CustomPainter {
  final Cell cell;
  final bool isClue;
  final Color? fixedInner;
  final Color? userRing;
  final double radius;

  _TileTreatmentPainter({
    required this.cell,
    required this.isClue,
    required this.fixedInner,
    required this.userRing,
    required this.radius,
  });

  @override
  void paint(Canvas canvas, Size size) {
    if (cell == Cell.empty) return;
    final color = isClue ? fixedInner : userRing;
    if (color == null) return;
    const inset = 1.5;
    final rect = Rect.fromLTWH(
      inset,
      inset,
      size.width - inset * 2,
      size.height - inset * 2,
    );
    final paint = Paint()
      ..style = PaintingStyle.stroke
      ..strokeWidth = 1
      ..color = color;
    canvas.drawRRect(
      RRect.fromRectAndRadius(rect, Radius.circular(radius - inset)),
      paint,
    );
  }

  @override
  bool shouldRepaint(_TileTreatmentPainter old) =>
      old.cell != cell ||
      old.isClue != isClue ||
      old.fixedInner != fixedInner ||
      old.userRing != userRing ||
      old.radius != radius;
}

class _SymbolPainterAdapter extends CustomPainter {
  final Cell cell;
  final Color color;
  final SundownTheme theme;
  _SymbolPainterAdapter({
    required this.cell,
    required this.color,
    required this.theme,
  });

  @override
  void paint(Canvas canvas, Size size) {
    // Symbol color is identical for clue and player cells — the
    // clue-vs-player distinction lives in the cell background and the
    // tile-treatment overlay.
    theme.symbols.paint(canvas, size, cell, color);
  }

  @override
  bool shouldRepaint(_SymbolPainterAdapter old) =>
      old.cell != cell || old.color != color || old.theme.id != theme.id;
}

```

# Read lib/ui/widgets/control_button.dart
```dart
import 'package:flutter/material.dart';

import '../theme/sundown_theme.dart';

/// Themed button used across the bottom control bar and modal sheets.
class ControlButton extends StatelessWidget {
  final IconData? icon;
  final String label;
  final VoidCallback? onPressed;
  final bool primary;
  final int? badge;

  const ControlButton({
    super.key,
    this.icon,
    required this.label,
    required this.onPressed,
    this.primary = false,
    this.badge,
  });

  @override
  Widget build(BuildContext context) {
    final t = context.tokens;
    final enabled = onPressed != null;
    final bg = primary ? t.accent : t.surfaceElevated;
    final fg = primary ? t.background : t.textPrimary;
    final semanticLabel = badge != null && badge! > 0
        ? '$label, $badge'
        : label;
    return Semantics(
      button: true,
      enabled: enabled,
      label: semanticLabel,
      onTap: onPressed,
      excludeSemantics: true,
      child: Opacity(
        opacity: enabled ? 1 : 0.45,
        child: ConstrainedBox(
          constraints: const BoxConstraints(minWidth: 48, minHeight: 48),
          child: Material(
            color: Colors.transparent,
            child: InkWell(
              onTap: onPressed,
              borderRadius: BorderRadius.circular(t.cardRadius * 0.6),
              child: AnimatedContainer(
                duration: const Duration(milliseconds: 120),
                padding: const EdgeInsets.symmetric(
                  horizontal: 14,
                  vertical: 12,
                ),
                decoration: BoxDecoration(
                  color: bg,
                  borderRadius: BorderRadius.circular(t.cardRadius * 0.6),
                  border: Border.all(color: t.gridLine, width: 0.5),
                ),
                child: Stack(
                  clipBehavior: Clip.none,
                  children: [
                    Row(
                      mainAxisSize: MainAxisSize.min,
                      children: [
                        if (icon != null) ...[
                          Icon(icon, color: fg, size: 18),
                          const SizedBox(width: 8),
                        ],
                        Text(
                          label,
                          maxLines: 1,
                          softWrap: false,
                          overflow: TextOverflow.ellipsis,
                          style: t.body.copyWith(
                            color: fg,
                            fontWeight: FontWeight.w600,
                          ),
                        ),
                      ],
                    ),
                    if (badge != null && badge! > 0)
                      PositionedDirectional(
                        top: -8,
                        end: -8,
                        child: IgnorePointer(
                          child: ConstrainedBox(
                            constraints: const BoxConstraints(
                              minWidth: 18,
                              minHeight: 18,
                            ),
                            child: Container(
                              padding: const EdgeInsets.symmetric(
                                horizontal: 5,
                              ),
                              alignment: Alignment.center,
                              decoration: BoxDecoration(
                                color: t.accent,
                                borderRadius: BorderRadius.circular(100),
                              ),
                              child: Text(
                                '$badge',
                                textAlign: TextAlign.center,
                                style: t.label.copyWith(
                                  color: t.background,
                                  fontWeight: FontWeight.w700,
                                ),
                              ),
                            ),
                          ),
                        ),
                      ),
                  ],
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}

/// Simple two-line stat card used in HUD.
class StatChip extends StatelessWidget {
  final String label;
  final String value;
  final IconData? icon;
  const StatChip({
    super.key,
    required this.label,
    required this.value,
    this.icon,
  });

  @override
  Widget build(BuildContext context) {
    final t = context.tokens;
    return Semantics(
      container: true,
      readOnly: true,
      label: '$label $value',
      child: ExcludeSemantics(
        child: Container(
          padding: const EdgeInsets.symmetric(horizontal: 12, vertical: 8),
          decoration: BoxDecoration(
            color: t.surfaceElevated,
            borderRadius: BorderRadius.circular(t.cardRadius * 0.5),
            border: Border.all(color: t.gridLine, width: 0.5),
          ),
          child: Column(
            mainAxisSize: MainAxisSize.min,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              FittedBox(
                fit: BoxFit.scaleDown,
                alignment: Alignment.centerLeft,
                child: Row(
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    if (icon != null) ...[
                      Icon(icon, color: t.textMuted, size: 14),
                      const SizedBox(width: 6),
                    ],
                    Text(
                      value,
                      maxLines: 1,
                      softWrap: false,
                      overflow: TextOverflow.ellipsis,
                      style: t.body.copyWith(fontWeight: FontWeight.w700),
                    ),
                  ],
                ),
              ),
              const SizedBox(height: 2),
              Text(
                label,
                maxLines: 1,
                softWrap: false,
                overflow: TextOverflow.ellipsis,
                style: t.label,
              ),
            ],
          ),
        ),
      ),
    );
  }
}

```

# Read test/ui/state/providers_test.dart
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:shared_preferences/shared_preferences.dart';

import 'package:sundown/domain/difficulty.dart';
import 'package:sundown/storage/storage_service.dart';
import 'package:sundown/ui/state/providers.dart';

/// Regression test for the crash triaged in docs/SENTRY_TRIAGE.md
/// (Sentry issue 7501931998): a stored `preferredDifficulty` code from an
/// older build that no longer maps to a current [Difficulty] used to make
/// `Difficulty.fromCode` throw, which propagated out of
/// `PreferredDifficultyController.build()` and crashed the New Puzzle screen.
void main() {
  Future<ProviderContainer> containerWith(Map<String, Object> prefs) async {
    SharedPreferences.setMockInitialValues(prefs);
    final storage = await StorageService.open();
    return ProviderContainer(
      overrides: [storageProvider.overrideWithValue(storage)],
    );
  }

  test('falls back to Easy for an unknown stored difficulty code', () async {
    final container = await containerWith({
      'sundown.preferredDifficulty': 'LEGACY',
    });
    addTearDown(container.dispose);

    expect(container.read(preferredDifficultyProvider), Difficulty.easy);
  });

  test('falls back to Easy when no difficulty code is stored', () async {
    final container = await containerWith({});
    addTearDown(container.dispose);

    expect(container.read(preferredDifficultyProvider), Difficulty.easy);
  });

  test('honors a valid stored difficulty code', () async {
    final container = await containerWith({'sundown.preferredDifficulty': 'H'});
    addTearDown(container.dispose);

    expect(container.read(preferredDifficultyProvider), Difficulty.hard);
  });

  test('generates unique custom-symbol file names per replacement', () {
    final first = customSymbolFileName(
      CustomSymbolSlot.sun,
      timestamp: DateTime.fromMicrosecondsSinceEpoch(1000),
    );
    final second = customSymbolFileName(
      CustomSymbolSlot.sun,
      timestamp: DateTime.fromMicrosecondsSinceEpoch(2000),
    );

    expect(first, startsWith('sun_'));
    expect(second, startsWith('sun_'));
    expect(first, isNot(second));
    expect(first, endsWith('.jpg'));
  });
}

```

# Read test/ui/screens/game_header_test.dart
```dart
// Tests for the _Header widget rendered inside GameScreen.
//
// UX-1: verifies no RenderFlex overflow at 320 px wide with a long elapsed
//        time and non-trivial hints/mistakes values.
// UX-2: verifies the "How to Play" button pauses the timer on the way out and
//        resumes it on return (unless the puzzle is already completed).
// UX-3: verifies the two-row layout — nav+meta row and stat-chips row — renders
//        correctly on both narrow (320 px) and normal (390 px) screens, with all
//        three stat chips present and the nav buttons accessible.

import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:shared_preferences/shared_preferences.dart';

import 'package:sundown/domain/board/board.dart';
import 'package:sundown/domain/board/puzzle.dart';
import 'package:sundown/domain/difficulty.dart';
import 'package:sundown/domain/game/game.dart';
import 'package:sundown/storage/storage_service.dart';
import 'package:sundown/ui/screens/game_screen.dart';
import 'package:sundown/ui/screens/how_to_play_screen.dart';
import 'package:sundown/ui/state/providers.dart';
import 'package:sundown/ui/theme/sundown_theme.dart';
import 'package:sundown/ui/theme/themes.dart';

// ---------------------------------------------------------------------------
// Helpers
// ---------------------------------------------------------------------------

/// A minimal 4×4 puzzle used only to satisfy the Game constructor — the cells
/// are not important for header layout or timer tests.
Puzzle _smallPuzzle() {
  final clues = Board.fromGlyphs(
    'S...\n'
    '....\n'
    '....\n'
    '....',
  );
  return Puzzle(clues: clues, difficulty: Difficulty.easy, code: 'S4E-TEST');
}

/// A [GameController] subclass that immediately returns a preset [Game] from
/// [build], bypassing the SharedPreferences load.  Tracks [deactivate] and
/// [activate] call counts so tests can assert timer-pause behaviour.
class _FakeGameController extends GameController {
  final Game _preset;

  int deactivateCount = 0;
  int activateCount = 0;

  _FakeGameController(this._preset);

  @override
  Game? build() => _preset;

  @override
  void deactivate() {
    deactivateCount++;
    // Call super so any running ticker is cleanly cancelled.
    super.deactivate();
  }

  @override
  void activate() {
    activateCount++;
    // Do NOT call super.activate() in tests: the parent would start a
    // periodic 250 ms ticker that keeps the widget dirty forever and causes
    // tester.pumpAndSettle() to time out.
  }
}

/// Pumps [GameScreen] inside a [ProviderScope] that injects the given fake
/// controller and a mocked [StorageService].  Returns the controller so tests
/// can inspect it after interactions.
Future<_FakeGameController> _pumpGameScreen(
  WidgetTester tester, {
  required _FakeGameController controller,
  Size surfaceSize = const Size(390, 844),
  double textScale = 1.0,
}) async {
  SharedPreferences.setMockInitialValues({});
  final storage = await StorageService.open();

  await tester.binding.setSurfaceSize(surfaceSize);
  addTearDown(() => tester.binding.setSurfaceSize(null));

  await tester.pumpWidget(
    MediaQuery(
      data: MediaQueryData.fromView(
        tester.view,
      ).copyWith(textScaler: TextScaler.linear(textScale)),
      child: ProviderScope(
        overrides: [
          storageProvider.overrideWithValue(storage),
          gameProvider.overrideWith(() => controller),
        ],
        child: SundownThemeScope(
          theme: SundownThemes.classic,
          child: const MaterialApp(home: GameScreen()),
        ),
      ),
    ),
  );

  // Process initState callbacks (including _enforceGateThenActivate).
  // pumpAndSettle also drains any timers started by activate().
  await tester.pumpAndSettle();
  return controller;
}

// ---------------------------------------------------------------------------
// Tests
// ---------------------------------------------------------------------------

void main() {
  // ── UX-1: overflow at 320 px ───────────────────────────────────────────

  testWidgets('no RenderFlex overflow at 320 px with long elapsed time and '
      'non-trivial hints and mistakes', (tester) async {
    // 12:34:56 → "746 minutes" → "746:56", a wide time string that used to
    // push the "mistakes" chip off the right edge of a 320 px screen.
    final longGame = Game(
      puzzle: _smallPuzzle(),
      board: Board.fromGlyphs('S...\n....\n....\n....'),
      history: const [],
      hintsUsed: 3,
      mistakes: 5,
      elapsedMs: 45_296_000, // 12 h 34 m 56 s
      completed: false,
      startedAt: DateTime(2025, 1, 1),
      undoUsed: false,
    );

    await _pumpGameScreen(
      tester,
      controller: _FakeGameController(longGame),
      surfaceSize: const Size(320, 800),
    );

    // All three stat values must be present in the widget tree.
    // 45_296_000 ms = 45296 s = 754 min 56 s → "754:56"
    expect(
      find.text('754:56'),
      findsOneWidget,
      reason: 'time chip must be rendered',
    );
    expect(
      find.text('3'),
      findsOneWidget,
      reason: 'hints chip must be rendered',
    );
    expect(
      find.text('5'),
      findsOneWidget,
      reason: 'mistakes chip must be rendered',
    );

    // No overflow exception should have been thrown.
    expect(
      tester.takeException(),
      isNull,
      reason: 'no RenderFlex overflow must occur at 320 px',
    );
  });

  testWidgets(
    'header keeps the puzzle code visible at 320 px with larger text scale',
    (tester) async {
      final scaledGame = Game(
        puzzle: Puzzle(
          clues: Board.fromGlyphs(
            'S...\n'
            '....\n'
            '....\n'
            '....',
          ),
          difficulty: Difficulty.medium,
          code: 'S4M-WIDE-CODE',
        ),
        board: Board.fromGlyphs('S...\n....\n....\n....'),
        history: const [],
        hintsUsed: 12,
        mistakes: 9,
        elapsedMs: 3_600_000,
        completed: false,
        startedAt: DateTime(2025, 1, 1),
        undoUsed: false,
      );

      await _pumpGameScreen(
        tester,
        controller: _FakeGameController(scaledGame),
        surfaceSize: const Size(320, 800),
        textScale: 1.8,
      );

      expect(
        find.text('S4M-WIDE-CODE'),
        findsOneWidget,
        reason: 'puzzle code must remain visible at large text scale',
      );
      expect(
        tester.takeException(),
        isNull,
        reason: 'no overflow must occur at 320 px with larger text scale',
      );
    },
  );

  // ── UX-2: help button pause / resume ──────────────────────────────────

  testWidgets(
    'tapping How to Play deactivates the timer, navigates to HowToPlayScreen, '
    'and reactivates on return (puzzle not completed)',
    (tester) async {
      final activeGame = Game(
        puzzle: _smallPuzzle(),
        board: Board.fromGlyphs('S...\n....\n....\n....'),
        history: const [],
        hintsUsed: 0,
        mistakes: 0,
        elapsedMs: 1_000,
        completed: false,
        startedAt: DateTime(2025, 1, 1),
        undoUsed: false,
      );

      final controller = _FakeGameController(activeGame);
      await _pumpGameScreen(tester, controller: controller);

      // Reset counts accumulated during _enforceGateThenActivate.
      controller.deactivateCount = 0;
      controller.activateCount = 0;

      // Tap the "How to Play" button.
      final helpBtn = find.byTooltip('How to Play').hitTestable();
      expect(
        helpBtn,
        findsOneWidget,
        reason: '"How to Play" button must be present with its tooltip',
      );

      await tester.tap(helpBtn);
      await tester.pumpAndSettle();

      // After tapping, the timer must have been paused and we should be on the
      // HowToPlayScreen.
      expect(
        controller.deactivateCount,
        equals(1),
        reason: 'deactivate() must be called once when help is opened',
      );
      expect(
        find.byType(HowToPlayScreen),
        findsOneWidget,
        reason: 'HowToPlayScreen must be pushed onto the navigator',
      );

      // Navigate back.
      final backBtn = find
          .descendant(
            of: find.byType(HowToPlayScreen),
            matching: find.byTooltip('Back'),
          )
          .hitTestable();
      await tester.tap(backBtn);
      await tester.pumpAndSettle();

      // The timer must resume because the puzzle was not completed.
      expect(
        controller.activateCount,
        equals(1),
        reason:
            'activate() must be called once when returning from help '
            'with an incomplete puzzle',
      );
    },
  );

  testWidgets(
    'tapping How to Play deactivates the timer but does NOT reactivate '
    'when the puzzle is already completed',
    (tester) async {
      final completedGame = Game(
        puzzle: _smallPuzzle(),
        board: Board.fromGlyphs('S...\n....\n....\n....'),
        history: const [],
        hintsUsed: 0,
        mistakes: 0,
        elapsedMs: 60_000,
        completed: true, // <-- puzzle done
        startedAt: DateTime(2025, 1, 1),
        undoUsed: false,
      );

      final controller = _FakeGameController(completedGame);
      await _pumpGameScreen(tester, controller: controller);

      controller.deactivateCount = 0;
      controller.activateCount = 0;

      await tester.tap(find.byTooltip('How to Play').hitTestable());
      await tester.pumpAndSettle();

      expect(controller.deactivateCount, equals(1));
      expect(find.byType(HowToPlayScreen), findsOneWidget);

      // Navigate back.
      await tester.tap(
        find
            .descendant(
              of: find.byType(HowToPlayScreen),
              matching: find.byTooltip('Back'),
            )
            .hitTestable(),
      );
      await tester.pumpAndSettle();

      // Because game.completed is true, activate() must NOT be called.
      expect(
        controller.activateCount,
        equals(0),
        reason: 'activate() must NOT be called when the puzzle is completed',
      );
    },
  );

  // ── UX-3: two-row layout on narrow and normal widths ──────────────────

  testWidgets(
    'all three stat chips and both nav buttons are present at 320 px',
    (tester) async {
      final game = Game(
        puzzle: _smallPuzzle(),
        board: Board.fromGlyphs('S...\n....\n....\n....'),
        history: const [],
        hintsUsed: 2,
        mistakes: 1,
        elapsedMs: 90_000, // 1:30
        completed: false,
        startedAt: DateTime(2025, 1, 1),
        undoUsed: false,
      );

      await _pumpGameScreen(
        tester,
        controller: _FakeGameController(game),
        surfaceSize: const Size(320, 800),
      );

      // All stat chip labels and values must be visible.
      expect(find.text('1:30'), findsOneWidget, reason: 'time value');
      expect(find.text('2'), findsOneWidget, reason: 'hints value');
      expect(find.text('1'), findsOneWidget, reason: 'mistakes value');
      expect(find.text('time'), findsOneWidget, reason: 'time label');
      expect(find.text('hints'), findsOneWidget, reason: 'hints label');
      expect(find.text('mistakes'), findsOneWidget, reason: 'mistakes label');

      // Nav buttons must be present and tappable.
      expect(find.byTooltip('Back').hitTestable(), findsOneWidget);
      expect(find.byTooltip('How to Play').hitTestable(), findsOneWidget);

      expect(tester.takeException(), isNull, reason: 'no overflow at 320 px');
    },
  );

  testWidgets(
    'difficulty label and puzzle code are visible at 320 px',
    (tester) async {
      final game = Game(
        puzzle: Puzzle(
          clues: Board.fromGlyphs('S...\n....\n....\n....'),
          difficulty: Difficulty.hard,
          code: 'S4H-NARR',
        ),
        board: Board.fromGlyphs('S...\n....\n....\n....'),
        history: const [],
        hintsUsed: 0,
        mistakes: 0,
        elapsedMs: 0,
        completed: false,
        startedAt: DateTime(2025, 1, 1),
        undoUsed: false,
      );

      await _pumpGameScreen(
        tester,
        controller: _FakeGameController(game),
        surfaceSize: const Size(320, 800),
      );

      expect(find.textContaining('Hard'), findsWidgets,
          reason: 'difficulty label must appear');
      expect(find.text('S4H-NARR'), findsOneWidget,
          reason: 'puzzle code must appear');
      expect(tester.takeException(), isNull);
    },
  );

  testWidgets(
    'stat chips each occupy roughly equal width at 390 px (no layout overflow)',
    (tester) async {
      final game = Game(
        puzzle: _smallPuzzle(),
        board: Board.fromGlyphs('S...\n....\n....\n....'),
        history: const [],
        hintsUsed: 0,
        mistakes: 0,
        elapsedMs: 0,
        completed: false,
        startedAt: DateTime(2025, 1, 1),
        undoUsed: false,
      );

      await _pumpGameScreen(
        tester,
        controller: _FakeGameController(game),
        surfaceSize: const Size(390, 844), // standard iPhone 14 width
      );

      expect(find.text('0:00'), findsOneWidget);
      expect(find.text('time'), findsOneWidget);
      expect(find.text('hints'), findsOneWidget);
      expect(find.text('mistakes'), findsOneWidget);
      expect(tester.takeException(), isNull, reason: 'no overflow at 390 px');
    },
  );

  testWidgets(
    'header renders without overflow at 375 px with moderately large text scale',
    (tester) async {
      final game = Game(
        puzzle: _smallPuzzle(),
        board: Board.fromGlyphs('S...\n....\n....\n....'),
        history: const [],
        hintsUsed: 5,
        mistakes: 3,
        elapsedMs: 120_000, // 2:00
        completed: false,
        startedAt: DateTime(2025, 1, 1),
        undoUsed: false,
      );

      await _pumpGameScreen(
        tester,
        controller: _FakeGameController(game),
        surfaceSize: const Size(375, 812),
        textScale: 1.3,
      );

      expect(find.text('2:00'), findsOneWidget);
      expect(tester.takeException(), isNull,
          reason: 'no overflow at 375 px with 1.3× text scale');
    },
  );

  testWidgets(
    'nav buttons keep at least 44 px tap targets at 320 px',
    (tester) async {
      final game = Game(
        puzzle: _smallPuzzle(),
        board: Board.fromGlyphs('S...\n....\n....\n....'),
        history: const [],
        hintsUsed: 0,
        mistakes: 0,
        elapsedMs: 0,
        completed: false,
        startedAt: DateTime(2025, 1, 1),
        undoUsed: false,
      );

      await _pumpGameScreen(
        tester,
        controller: _FakeGameController(game),
        surfaceSize: const Size(320, 800),
      );

      // Accessibility floor: even on the narrowest screen the nav icon hit
      // areas must stay >= 44 px so they remain comfortably tappable.
      for (final tooltip in const ['Back', 'How to Play']) {
        final size = tester.getSize(find.byTooltip(tooltip));
        expect(size.width, greaterThanOrEqualTo(44.0),
            reason: '$tooltip width below 44 px tap target');
        expect(size.height, greaterThanOrEqualTo(44.0),
            reason: '$tooltip height below 44 px tap target');
      }
    },
  );
}

```

# Read test/widget_smoke_test.dart
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:shared_preferences/shared_preferences.dart';

import 'package:sundown/storage/storage_service.dart';
import 'package:sundown/ui/screens/home_screen.dart';
import 'package:sundown/ui/state/providers.dart';
import 'package:sundown/ui/theme/sundown_theme.dart';
import 'package:sundown/ui/theme/themes.dart';

/// Mounts the home screen with a clean in-memory storage layer to ensure the
/// UI tree compiles and assembles without errors. Not a functional test of
/// gameplay — that lives in the domain unit tests.
void main() {
  testWidgets('HomeScreen mounts', (tester) async {
    SharedPreferences.setMockInitialValues({});
    final storage = await StorageService.open();
    await tester.pumpWidget(
      ProviderScope(
        overrides: [storageProvider.overrideWithValue(storage)],
        child: SundownThemeScope(
          theme: SundownThemes.classic,
          child: const MaterialApp(home: HomeScreen()),
        ),
      ),
    );
    // "Sundown" appears as the page title and (when dusk is active) as the
    // theme name in the settings card — at least one must be present.
    expect(find.text('Sundown'), findsWidgets);
    expect(find.text('New puzzle'), findsOneWidget);
    expect(find.text('Play by code'), findsOneWidget);
    expect(find.text('Challenge pack'), findsNothing);
  });

  testWidgets('App Store build hides developer challenge entry', (
    tester,
  ) async {
    SharedPreferences.setMockInitialValues({
      'sundown.developerTestingMode': true,
      'sundown.tutorialSeen': true,
    });
    final storage = await StorageService.open();
    await tester.pumpWidget(
      ProviderScope(
        overrides: [storageProvider.overrideWithValue(storage)],
        child: SundownThemeScope(
          theme: SundownThemes.classic,
          child: const MaterialApp(home: HomeScreen()),
        ),
      ),
    );
    await tester.pumpAndSettle();

    expect(find.text('Challenge pack'), findsNothing);
    expect(find.text('DEVELOPER TESTING MODE'), findsNothing);
  });
}

```