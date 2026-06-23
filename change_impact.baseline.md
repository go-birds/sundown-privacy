# Prompt
Suppose we change the Game model to track a new per-cell annotation state. What code would likely need updates, what tests should be run or added, and what are the risky dependencies?


# rg -n '\\bGame\\b|copyWith|cell|Cell|Board|Puzzle|GameCodec|toJson|fromJson'
lib/domain/premium/premium_token.dart:43:  factory PremiumToken.fromJson(Map<String, dynamic> json) {
lib/domain/premium/premium_token.dart:51:  Map<String, dynamic> toJson() => {
test/domain/challenge_pack_test.dart:6:import 'package:sundown/domain/board/cell.dart';
test/domain/challenge_pack_test.dart:12:  late List<ChallengePuzzle> puzzles;
test/domain/challenge_pack_test.dart:19:        ChallengePuzzle.fromJson(puzzle as Map<String, dynamic>),
test/domain/challenge_pack_test.dart:69:    final spaces = <(int, int), List<List<Cell>>>{};
test/domain/challenge_pack_test.dart:78:          .where((cells) => _matches(puzzle, cells))
test/domain/challenge_pack_test.dart:89:      final cells = puzzle.initialCells();
test/domain/challenge_pack_test.dart:94:          cells[puzzle.index(r, c)] = puzzle.solutionAt(r, c);
test/domain/challenge_pack_test.dart:96:            ChallengeRules.violationCells(puzzle, cells),
test/domain/challenge_pack_test.dart:102:      expect(ChallengeRules.isSolved(puzzle, cells), isTrue, reason: puzzle.id);
test/domain/challenge_pack_test.dart:109:      (row: 0, column: 1): Cell.sun,
test/domain/challenge_pack_test.dart:110:      (row: 1, column: 1): Cell.sun,
test/domain/challenge_pack_test.dart:123:      (cage) => cage.cells.toSet().containsAll(topRight),
test/domain/challenge_pack_test.dart:126:    expect(cage.cells.toSet(), topRight);
test/domain/challenge_pack_test.dart:156:  test('rule evaluation rejects a board with the wrong cell count', () {
test/domain/challenge_pack_test.dart:159:      () => ChallengeRules.violationCells(puzzles.first, const []),
test/domain/challenge_pack_test.dart:165:List<List<Cell>> _baseSolutions(int rows, int columns) {
test/domain/challenge_pack_test.dart:166:  final patterns = <List<Cell>>[];
test/domain/challenge_pack_test.dart:170:        bits & (1 << c) == 0 ? Cell.moon : Cell.sun,
test/domain/challenge_pack_test.dart:172:    if (_count(line, Cell.sun) != columns ~/ 2) continue;
test/domain/challenge_pack_test.dart:176:  final solutions = <List<Cell>>[];
test/domain/challenge_pack_test.dart:178:  void walk(List<List<Cell>> grid) {
test/domain/challenge_pack_test.dart:184:      if (columnsList.any((line) => _count(line, Cell.sun) != rows ~/ 2)) {
test/domain/challenge_pack_test.dart:197:        if (_count(partial, Cell.sun) > rows ~/ 2 ||
test/domain/challenge_pack_test.dart:198:            _count(partial, Cell.moon) > rows ~/ 2 ||
test/domain/challenge_pack_test.dart:212:bool _matches(ChallengePuzzle puzzle, List<Cell> cells) {
test/domain/challenge_pack_test.dart:214:    if (cells[puzzle.index(entry.key.row, entry.key.column)] != entry.value) {
test/domain/challenge_pack_test.dart:218:  return ChallengeRules.isSolved(puzzle, cells);
test/domain/challenge_pack_test.dart:221:int _count(List<Cell> cells, Cell value) =>
test/domain/challenge_pack_test.dart:222:    cells.where((cell) => cell == value).length;
test/domain/challenge_pack_test.dart:224:bool _hasTriple(List<Cell> cells) {
test/domain/challenge_pack_test.dart:225:  for (var i = 0; i <= cells.length - 3; i++) {
test/domain/challenge_pack_test.dart:226:    if (cells[i] == cells[i + 1] && cells[i] == cells[i + 2]) return true;
test/domain/challenge_pack_test.dart:231:bool _hasDuplicate(List<List<Cell>> lines) {
test/domain/challenge_pack_test.dart:240:bool _same(List<Cell> a, List<Cell> b) {
test/domain/rules_test.dart:9:      expect(Rules.firstViolation(Board.empty(6)), isNull);
test/domain/rules_test.dart:14:      final b = Board.fromGlyphs('''
test/domain/rules_test.dart:26:      final b = Board.fromGlyphs('''
test/domain/rules_test.dart:38:      final b = Board.fromGlyphs('''
test/domain/rules_test.dart:50:      final b = Board.fromGlyphs(
test/domain/rules_test.dart:65:      final b = Board.fromGlyphs(
test/domain/rules_test.dart:83:      final b = Board.fromGlyphs('''
test/domain/rules_test.dart:94:      final b = Board.fromGlyphs('''
test/domain/rules_test.dart:107:      final b = Board.fromGlyphs('''
test/domain/rules_test.dart:120:      final b = Board.fromGlyphs('''
lib/domain/premium/premium_service.dart:15:    final token = PremiumToken.fromJson(tokenJson);
lib/domain/premium/premium_service.dart:19:    await storage.setString('_premium_token', jsonEncode(token.toJson()));
lib/domain/premium/premium_service.dart:32:      final token = PremiumToken.fromJson(tokenJson);
test/domain/difficulty_test.dart:13:        final cells = size * size;
test/domain/difficulty_test.dart:14:        expect(Difficulty.fromScore(size, (0.5 * cells).round()),
test/domain/difficulty_test.dart:16:        expect(Difficulty.fromScore(size, (1.1 * cells).round()),
test/domain/difficulty_test.dart:18:        expect(Difficulty.fromScore(size, (1.6 * cells).round()),
test/domain/difficulty_test.dart:20:        expect(Difficulty.fromScore(size, (2.5 * cells).round()),
lib/domain/premium/custom_symbol_images.dart:11:  CustomSymbolImages copyWith({String? sunPath, String? moonPath}) =>
test/domain/achievements_test.dart:21:        event: AchievementEvent(sharedPuzzle: true, occurredAt: DateTime(2026)),
test/domain/puzzle_code_test.dart:7:  group('PuzzleCode', () {
test/domain/puzzle_code_test.dart:11:          for (final seed in [0, 1, 42, 1234, PuzzleCode.seedMax - 1]) {
test/domain/puzzle_code_test.dart:12:            final pc = PuzzleCode(size: size, difficulty: d, seed: seed);
test/domain/puzzle_code_test.dart:14:            final decoded = PuzzleCode.decode(encoded);
test/domain/puzzle_code_test.dart:24:      final pc = PuzzleCode(size: 6, difficulty: Difficulty.easy, seed: 0);
test/domain/puzzle_code_test.dart:37:          PuzzleCode(size: 10, difficulty: Difficulty.hard, seed: 1234)
test/domain/puzzle_code_test.dart:40:      final decoded = PuzzleCode.decode(messy);
test/domain/puzzle_code_test.dart:49:          PuzzleCode(size: 8, difficulty: Difficulty.medium, seed: 42)
test/domain/puzzle_code_test.dart:57:        () => PuzzleCode.decode(mutated),
test/domain/puzzle_code_test.dart:58:        throwsA(isA<InvalidPuzzleCodeException>()),
test/domain/puzzle_code_test.dart:64:        () => PuzzleCode.decode('NOTAWORD-RED-DOG'),
test/domain/puzzle_code_test.dart:65:        throwsA(isA<InvalidPuzzleCodeException>()),
test/domain/puzzle_code_test.dart:68:        () => PuzzleCode.decode('OLD-NOTACOLOR-DOG'),
test/domain/puzzle_code_test.dart:69:        throwsA(isA<InvalidPuzzleCodeException>()),
test/domain/puzzle_code_test.dart:75:        () => PuzzleCode.decode('OLD-RED'),
test/domain/puzzle_code_test.dart:76:        throwsA(isA<InvalidPuzzleCodeException>()),
test/domain/puzzle_code_test.dart:79:        () => PuzzleCode.decode('OLD-RED-DOG-EXTRA'),
test/domain/puzzle_code_test.dart:80:        throwsA(isA<InvalidPuzzleCodeException>()),
lib/domain/challenge/challenge_puzzle.dart:1:import '../board/cell.dart';
lib/domain/challenge/challenge_puzzle.dart:6:  final List<ChallengePosition> cells;
lib/domain/challenge/challenge_puzzle.dart:9:  const ChallengeCage({required this.cells, required this.suns});
lib/domain/challenge/challenge_puzzle.dart:11:  factory ChallengeCage.fromJson(Map<String, dynamic> json) {
lib/domain/challenge/challenge_puzzle.dart:12:    final cells = (json['cells'] as List).cast<Map<String, dynamic>>();
lib/domain/challenge/challenge_puzzle.dart:14:      cells: [
lib/domain/challenge/challenge_puzzle.dart:15:        for (final cell in cells)
lib/domain/challenge/challenge_puzzle.dart:16:          (row: cell['row'] as int, column: cell['column'] as int),
lib/domain/challenge/challenge_puzzle.dart:23:class ChallengePuzzle {
lib/domain/challenge/challenge_puzzle.dart:32:  final Map<ChallengePosition, Cell> givens;
lib/domain/challenge/challenge_puzzle.dart:38:  const ChallengePuzzle({
lib/domain/challenge/challenge_puzzle.dart:54:  factory ChallengePuzzle.fromJson(Map<String, dynamic> json) {
lib/domain/challenge/challenge_puzzle.dart:55:    final givens = <ChallengePosition, Cell>{};
lib/domain/challenge/challenge_puzzle.dart:59:          Cell.fromGlyph(given['value'] as String);
lib/domain/challenge/challenge_puzzle.dart:65:    final puzzle = ChallengePuzzle(
lib/domain/challenge/challenge_puzzle.dart:77:          ChallengeCage.fromJson(cage as Map<String, dynamic>),
lib/domain/challenge/challenge_puzzle.dart:89:  List<Cell> initialCells() {
lib/domain/challenge/challenge_puzzle.dart:90:    final cells = List<Cell>.filled(rows * columns, Cell.empty);
lib/domain/challenge/challenge_puzzle.dart:92:      cells[index(entry.key.row, entry.key.column)] = entry.value;
lib/domain/challenge/challenge_puzzle.dart:94:    return cells;
lib/domain/challenge/challenge_puzzle.dart:97:  Cell solutionAt(int row, int column) => Cell.fromGlyph(solution[row][column]);
lib/domain/challenge/challenge_puzzle.dart:119:      if (entry.value == Cell.empty ||
lib/domain/challenge/challenge_puzzle.dart:127:      if (cage.cells.isEmpty ||
lib/domain/challenge/challenge_puzzle.dart:129:          cage.suns > cage.cells.length) {
lib/domain/challenge/challenge_puzzle.dart:132:      for (final position in cage.cells) {
lib/domain/challenge/challenge_puzzle.dart:139:      throw FormatException('$id constellations must cover every cell once');
lib/domain/challenge/challenge_puzzle.dart:155:    final solvedCells = [
lib/domain/challenge/challenge_puzzle.dart:160:    if (!ChallengeRules.isSolved(this, solvedCells)) {
lib/domain/challenge/challenge_puzzle.dart:167:  static Set<ChallengePosition> violationCells(
lib/domain/challenge/challenge_puzzle.dart:168:    ChallengePuzzle puzzle,
lib/domain/challenge/challenge_puzzle.dart:169:    List<Cell> cells,
lib/domain/challenge/challenge_puzzle.dart:171:    if (cells.length != puzzle.rows * puzzle.columns) {
lib/domain/challenge/challenge_puzzle.dart:173:        cells.length,
lib/domain/challenge/challenge_puzzle.dart:174:        'cells.length',
lib/domain/challenge/challenge_puzzle.dart:179:    Cell at(int r, int c) => cells[puzzle.index(r, c)];
lib/domain/challenge/challenge_puzzle.dart:194:      if (_count(row, Cell.sun) > puzzle.columns ~/ 2 ||
lib/domain/challenge/challenge_puzzle.dart:195:          _count(row, Cell.moon) > puzzle.columns ~/ 2) {
lib/domain/challenge/challenge_puzzle.dart:211:      if (_count(column, Cell.sun) > puzzle.rows ~/ 2 ||
lib/domain/challenge/challenge_puzzle.dart:212:          _count(column, Cell.moon) > puzzle.rows ~/ 2) {
lib/domain/challenge/challenge_puzzle.dart:228:      final values = [for (final cell in cage.cells) at(cell.row, cell.column)];
lib/domain/challenge/challenge_puzzle.dart:229:      final suns = _count(values, Cell.sun);
lib/domain/challenge/challenge_puzzle.dart:230:      final empty = _count(values, Cell.empty);
lib/domain/challenge/challenge_puzzle.dart:232:        bad.addAll(cage.cells);
lib/domain/challenge/challenge_puzzle.dart:253:      if (values.every((cell) => cell.isFilled) &&
lib/domain/challenge/challenge_puzzle.dart:260:      if (values.every((cell) => cell.isFilled) &&
lib/domain/challenge/challenge_puzzle.dart:266:    _markDuplicates(puzzle, cells, bad, rows: true);
lib/domain/challenge/challenge_puzzle.dart:267:    _markDuplicates(puzzle, cells, bad, rows: false);
lib/domain/challenge/challenge_puzzle.dart:271:  static bool isSolved(ChallengePuzzle puzzle, List<Cell> cells) =>
lib/domain/challenge/challenge_puzzle.dart:272:      cells.length == puzzle.rows * puzzle.columns &&
lib/domain/challenge/challenge_puzzle.dart:273:      cells.every((cell) => cell.isFilled) &&
lib/domain/challenge/challenge_puzzle.dart:274:      violationCells(puzzle, cells).isEmpty;
lib/domain/challenge/challenge_puzzle.dart:276:  static int _count(List<Cell> cells, Cell value) =>
lib/domain/challenge/challenge_puzzle.dart:277:      cells.where((cell) => cell == value).length;
lib/domain/challenge/challenge_puzzle.dart:279:  static int _runs(List<Cell> cells) {
lib/domain/challenge/challenge_puzzle.dart:281:    for (var i = 1; i < cells.length; i++) {
lib/domain/challenge/challenge_puzzle.dart:282:      if (cells[i] != cells[i - 1]) count++;
lib/domain/challenge/challenge_puzzle.dart:288:    ChallengePuzzle puzzle,
lib/domain/challenge/challenge_puzzle.dart:289:    List<Cell> cells,
lib/domain/challenge/challenge_puzzle.dart:295:    List<Cell> line(int i) => [
lib/domain/challenge/challenge_puzzle.dart:297:        cells[puzzle.index(rows ? i : j, rows ? j : i)],
lib/domain/challenge/challenge_puzzle.dart:301:      if (first.any((cell) => !cell.isFilled)) continue;
lib/domain/challenge/challenge_puzzle.dart:304:        if (second.any((cell) => !cell.isFilled)) continue;
lib/domain/challenge/challenge_puzzle.dart:315:  static bool _same(List<Cell> a, List<Cell> b) {
lib/domain/achievement.dart:36:  Map<String, dynamic> toJson() => {
lib/domain/achievement.dart:41:  static UnlockedAchievement fromJson(Map<String, dynamic> json) =>
lib/domain/achievement.dart:49:  final bool sharedPuzzle;
lib/domain/achievement.dart:50:  final bool startedPuzzle;
lib/domain/achievement.dart:56:    this.sharedPuzzle = false,
lib/domain/achievement.dart:57:    this.startedPuzzle = false,
lib/domain/achievement.dart:299:    if (event.sharedPuzzle) ids.add('binary_star');
lib/domain/achievement.dart:300:    if (event.startedPuzzle && event.occurredAt.hour < 5) {
test/domain/solver_test.dart:5:import 'package:sundown/domain/board/cell.dart';
test/domain/solver_test.dart:15:      final b = Board.fromGlyphs('''
test/domain/solver_test.dart:25:      expect(ds.any((d) => d.r == 0 && d.c == 0 && d.value == Cell.moon), isTrue);
test/domain/solver_test.dart:26:      expect(ds.any((d) => d.r == 0 && d.c == 3 && d.value == Cell.moon), isTrue);
test/domain/solver_test.dart:30:      final b = Board.fromGlyphs('''
test/domain/solver_test.dart:39:      expect(ds.any((d) => d.r == 2 && d.c == 1 && d.value == Cell.sun), isTrue);
test/domain/solver_test.dart:45:      final b = Board.fromGlyphs('''
test/domain/solver_test.dart:54:      expect(ds.any((d) => d.r == 0 && d.c == 1 && d.value == Cell.moon), isTrue);
test/domain/solver_test.dart:60:      final b = Board.fromGlyphs(
test/domain/solver_test.dart:74:      expect(ds.single.value, Cell.sun);
test/domain/solver_test.dart:78:      final b = Board.fromGlyphs(
test/domain/solver_test.dart:90:      expect(ds.single.value, Cell.moon);
test/domain/solver_test.dart:97:      final b = Board.fromGlyphs(
test/domain/solver_test.dart:109:      expect(ds.any((d) => d.r == 0 && d.c == 1 && d.value == Cell.moon), isTrue);
test/domain/solver_test.dart:110:      expect(ds.any((d) => d.r == 0 && d.c == 2 && d.value == Cell.moon), isTrue);
test/domain/solver_test.dart:118:      final b = Board.fromGlyphs('''
test/domain/solver_test.dart:127:      // half=3, three suns already → remaining row cells must be moons.
test/domain/solver_test.dart:128:      expect(ds.where((d) => d.r == 0 && d.value == Cell.moon).length, 3);
test/domain/solver_test.dart:133:    test('forces cells where every completion of a row agrees', () {
test/domain/solver_test.dart:135:      // edges, count + no-three constraints leave certain cells forced when
test/domain/solver_test.dart:136:      // intersected. We verify that linePatterns produces ≥1 forced cell
test/domain/solver_test.dart:138:      final b = Board.fromGlyphs('''
test/domain/solver_test.dart:148:      // contradict the cell at (0,2).
test/domain/solver_test.dart:151:          expect(d.value, Cell.sun);
test/domain/solver_test.dart:159:      final b = Board.fromGlyphs(
test/domain/solver_test.dart:181:      // Start with a valid solution and clear two cells in a way that's
test/domain/solver_test.dart:183:      final b = Board.fromGlyphs('''
test/domain/solver_test.dart:193:      expect(r.finalBoard.isComplete, isTrue);
test/domain/solver_test.dart:197:      final b = Board.fromGlyphs('''
test/domain/solver_test.dart:210:      final r = Solver.solve(Board.empty(6));
test/domain/solver_test.dart:218:      final b = Board.fromGlyphs(
test/domain/solver_test.dart:244:      expect(Solver.hint(Board.empty(4)).status, HintStatus.ambiguous);
test/domain/solver_test.dart:261:          // the solution's remaining cells.
test/domain/solver_test.dart:266:                if (board.at(r, c) == Cell.empty && rng.nextBool()) {
test/domain/solver_test.dart:308:      // Place the WRONG value in the first empty non-clue cell. Whether or not
test/domain/solver_test.dart:315:          if (board.at(r, c) == Cell.empty) {
test/domain/generator_test.dart:51:        expect(PuzzleCode.decode(p.code).difficulty, d);
test/domain/generator_test.dart:59:      final pc = PuzzleCode.decode(Generator.generateFromSeed(6, 3).code);
test/domain/generator_test.dart:62:            PuzzleCode(size: pc.size, difficulty: d, seed: pc.seed).encode();
lib/domain/generator.dart:4:import 'board/cell.dart';
lib/domain/generator.dart:31:  static Puzzle fromCode(String code) {
lib/domain/generator.dart:32:    final pc = PuzzleCode.decode(code);
lib/domain/generator.dart:36:    return p.copyWith(code: pc.encode());
lib/domain/generator.dart:41:  static Puzzle make(
lib/domain/generator.dart:49:      final seed = rng.nextInt(PuzzleCode.seedMax);
lib/domain/generator.dart:52:        return puzzle.copyWith(
lib/domain/generator.dart:53:          code: PuzzleCode(size: size, difficulty: target, seed: seed).encode(),
lib/domain/generator.dart:63:  static Puzzle generateFromSeed(int size, int seed) {
lib/domain/generator.dart:65:    return p.copyWith(
lib/domain/generator.dart:66:      code: PuzzleCode(size: size, difficulty: p.difficulty, seed: seed).encode(),
lib/domain/generator.dart:75:  static Puzzle _generate(int size, int seed, Difficulty? cap) {
lib/domain/generator.dart:87:    return Puzzle(
lib/domain/generator.dart:98:  // hold for filled cells. This is the same as constraint propagation but
lib/domain/generator.dart:101:  static Board _fillSolution(int size, Random rng) {
lib/domain/generator.dart:102:    final cells = List<Cell>.filled(size * size, Cell.empty);
lib/domain/generator.dart:108:          rng.nextBool() ? [Cell.sun, Cell.moon] : [Cell.moon, Cell.sun];
lib/domain/generator.dart:110:        cells[idx] = v;
lib/domain/generator.dart:111:        if (_partiallyOk(Board(size, cells), r, c)) {
lib/domain/generator.dart:115:      cells[idx] = Cell.empty;
lib/domain/generator.dart:122:    return Board(size, cells);
lib/domain/generator.dart:125:  static bool _partiallyOk(Board b, int r, int c) {
lib/domain/generator.dart:129:    if (v == Cell.empty) return true;
lib/domain/generator.dart:135:      if (x == Cell.sun) rs++;
lib/domain/generator.dart:136:      if (x == Cell.moon) rm++;
lib/domain/generator.dart:143:      if (x == Cell.sun) cs++;
lib/domain/generator.dart:144:      if (x == Cell.moon) cm++;
lib/domain/generator.dart:156:    // Uniqueness: a row finishes when we place its last cell (c == n-1);
lib/domain/generator.dart:157:    // a column finishes when we place its last cell (r == n-1, given the
lib/domain/generator.dart:184:  static Board _sprinkleEdges(Board solution, Random rng) {
lib/domain/generator.dart:201:      final (or, oc) = pos.otherCell;
lib/domain/generator.dart:213:  static Board _minimizeClues(Board full, Random rng, Difficulty? cap) {
lib/domain/generator.dart:220:      if (board.at(r, c) == Cell.empty) continue;
lib/domain/generator.dart:221:      final candidate = board.set(r, c, Cell.empty);
lib/domain/generator.dart:231:  static Board _minimizeEdges(Board b, Random rng, Difficulty? cap) {
lib/domain/rules.dart:2:import 'board/cell.dart';
lib/domain/rules.dart:11:  /// Returns true if every filled cell on [board] is consistent with the rules.
lib/domain/rules.dart:12:  /// Empty cells are ignored except for "count cannot already exceed N/2".
lib/domain/rules.dart:13:  static bool isPartiallyValid(Board board) => firstViolation(board) == null;
lib/domain/rules.dart:16:  static bool isSolved(Board board) =>
lib/domain/rules.dart:21:  static Violation? firstViolation(Board board) {
lib/domain/rules.dart:29:      final rs = _count(row, Cell.sun);
lib/domain/rules.dart:30:      final rm = _count(row, Cell.moon);
lib/domain/rules.dart:31:      final cs = _count(col, Cell.sun);
lib/domain/rules.dart:32:      final cm = _count(col, Cell.moon);
lib/domain/rules.dart:33:      if (rs > half) return Violation.tooMany(Axis.row, i, Cell.sun);
lib/domain/rules.dart:34:      if (rm > half) return Violation.tooMany(Axis.row, i, Cell.moon);
lib/domain/rules.dart:35:      if (cs > half) return Violation.tooMany(Axis.col, i, Cell.sun);
lib/domain/rules.dart:36:      if (cm > half) return Violation.tooMany(Axis.col, i, Cell.moon);
lib/domain/rules.dart:92:      final (or, oc) = pos.otherCell;
lib/domain/rules.dart:106:  static int _count(List<Cell> cells, Cell target) =>
lib/domain/rules.dart:107:      cells.where((c) => c == target).length;
lib/domain/rules.dart:120:  final Cell? cell;
lib/domain/rules.dart:130:    this.cell,
lib/domain/rules.dart:135:  factory Violation.tooMany(Axis axis, int idx, Cell cell) =>
lib/domain/rules.dart:136:      Violation._(kind: ViolationKind.tooMany, axis: axis, index: idx, cell: cell);
lib/domain/rules.dart:138:  factory Violation.threeInARow(Axis axis, int idx, int offset, Cell cell) =>
lib/domain/rules.dart:144:        cell: cell,
lib/domain/rules.dart:164:          'too many ${cell!.name} in ${axis!.name} $index',
lib/domain/rules.dart:166:          'three ${cell!.name} in ${axis!.name} $index at offset $offset',
test/ui/widgets/board_widget_semantics_test.dart:14:import 'package:sundown/ui/widgets/cell_widget.dart';
test/ui/widgets/board_widget_semantics_test.dart:21:Finder _cellAt(int row, int column) => find.byWidgetPredicate(
test/ui/widgets/board_widget_semantics_test.dart:23:      widget is CellWidget && widget.row == row && widget.column == column,
test/ui/widgets/board_widget_semantics_test.dart:26:Future<void> _pumpBoard(
test/ui/widgets/board_widget_semantics_test.dart:28:  required Puzzle puzzle,
test/ui/widgets/board_widget_semantics_test.dart:29:  required Board board,
test/ui/widgets/board_widget_semantics_test.dart:31:  Set<(int, int)> highlightCells = const {},
test/ui/widgets/board_widget_semantics_test.dart:32:  Set<(int, int)> errorCells = const {},
test/ui/widgets/board_widget_semantics_test.dart:45:              child: BoardWidget(
test/ui/widgets/board_widget_semantics_test.dart:49:                highlightCells: highlightCells,
test/ui/widgets/board_widget_semantics_test.dart:50:                errorCells: errorCells,
test/ui/widgets/board_widget_semantics_test.dart:63:  testWidgets('Board cells expose a single actionable semantics node', (
test/ui/widgets/board_widget_semantics_test.dart:66:    final puzzle = Puzzle(
test/ui/widgets/board_widget_semantics_test.dart:67:      clues: Board.fromGlyphs('S.\n..'),
test/ui/widgets/board_widget_semantics_test.dart:71:    final board = Board.fromGlyphs('S.\n.M');
test/ui/widgets/board_widget_semantics_test.dart:73:    await _pumpBoard(
test/ui/widgets/board_widget_semantics_test.dart:78:      highlightCells: {(1, 0)},
test/ui/widgets/board_widget_semantics_test.dart:79:      errorCells: {(1, 1)},
test/ui/widgets/board_widget_semantics_test.dart:85:            .descendant(of: _cellAt(0, 0), matching: find.byType(Semantics))
test/ui/widgets/board_widget_semantics_test.dart:88:      matchesSemantics(label: 'Row 1, column 1. Clue cell, sun.'),
test/ui/widgets/board_widget_semantics_test.dart:93:            .descendant(of: _cellAt(0, 1), matching: find.byType(Semantics))
test/ui/widgets/board_widget_semantics_test.dart:97:        label: 'Row 1, column 2. Player cell, empty.',
test/ui/widgets/board_widget_semantics_test.dart:105:            .descendant(of: _cellAt(1, 0), matching: find.byType(Semantics))
test/ui/widgets/board_widget_semantics_test.dart:109:        label: 'Row 2, column 1. Player cell, empty. Highlighted.',
test/ui/widgets/board_widget_semantics_test.dart:117:            .descendant(of: _cellAt(1, 1), matching: find.byType(Semantics))
test/ui/widgets/board_widget_semantics_test.dart:121:        label: 'Row 2, column 2. Player cell, moon. Error.',
test/ui/widgets/board_widget_semantics_test.dart:136:  testWidgets('Locked cells are not announced as actionable', (tester) async {
test/ui/widgets/board_widget_semantics_test.dart:137:    final puzzle = Puzzle(
test/ui/widgets/board_widget_semantics_test.dart:138:      clues: Board.fromGlyphs('S.\n..'),
test/ui/widgets/board_widget_semantics_test.dart:142:    final board = Board.fromGlyphs('S.\n.M');
test/ui/widgets/board_widget_semantics_test.dart:144:    await _pumpBoard(tester, puzzle: puzzle, board: board, locked: true);
test/ui/widgets/board_widget_semantics_test.dart:149:            .descendant(of: _cellAt(0, 1), matching: find.byType(Semantics))
test/ui/widgets/board_widget_semantics_test.dart:152:      matchesSemantics(label: 'Row 1, column 2. Player cell, empty.'),
test/ui/widgets/board_widget_semantics_test.dart:157:            .descendant(of: _cellAt(1, 1), matching: find.byType(Semantics))
test/ui/widgets/board_widget_semantics_test.dart:160:      matchesSemantics(label: 'Row 2, column 2. Player cell, moon.'),
test/ui/widgets/board_widget_semantics_test.dart:166:            .descendant(of: _cellAt(0, 1), matching: find.byType(Semantics))
lib/domain/game/game.dart:2:import '../board/cell.dart';
lib/domain/game/game.dart:9:/// All fields are immutable; mutations produce a new [Game] via [copyWith]
lib/domain/game/game.dart:12:class Game {
lib/domain/game/game.dart:13:  final Puzzle puzzle;
lib/domain/game/game.dart:16:  /// placements overlaid on top of the original clue cells. Clue cells are
lib/domain/game/game.dart:18:  final Board board;
lib/domain/game/game.dart:24:  /// cells are first shown (stage 1) and once more when the reason text is
lib/domain/game/game.dart:28:  /// Number of times the player placed a cell that immediately produced a
lib/domain/game/game.dart:47:  const Game({
lib/domain/game/game.dart:59:  factory Game.fresh(Puzzle p, {DateTime? now}) => Game(
lib/domain/game/game.dart:76:  /// Returns the next [Game] after the player taps cell `(r,c)`. Cycle order:
lib/domain/game/game.dart:77:  /// empty → sun → moon → empty. Tapping a clue cell is a no-op.
lib/domain/game/game.dart:78:  Game tap(int r, int c) {
lib/domain/game/game.dart:83:      Cell.empty => Cell.sun,
lib/domain/game/game.dart:84:      Cell.sun => Cell.moon,
lib/domain/game/game.dart:85:      Cell.moon => Cell.empty,
lib/domain/game/game.dart:91:  Game place(int r, int c, Cell value) {
lib/domain/game/game.dart:99:  Game _applyMove(int r, int c, Cell prev, Cell next) {
lib/domain/game/game.dart:100:    final newBoard = board.set(r, c, next);
lib/domain/game/game.dart:102:    final done = Rules.isSolved(newBoard);
lib/domain/game/game.dart:103:    return copyWith(board: newBoard, history: newHistory, completed: done);
lib/domain/game/game.dart:106:  Game incrementMistakes() => copyWith(mistakes: mistakes + 1);
lib/domain/game/game.dart:109:  Game undo() {
lib/domain/game/game.dart:112:    return copyWith(
lib/domain/game/game.dart:121:  Game restart() => Game.fresh(puzzle, now: startedAt);
lib/domain/game/game.dart:123:  /// Increments the hint counter without revealing a cell.
lib/domain/game/game.dart:124:  /// Called when the player asks to see witness cells (stage 1) or
lib/domain/game/game.dart:126:  Game recordHint() => copyWith(hintsUsed: hintsUsed + 1);
lib/domain/game/game.dart:128:  /// Applies a [Cell] reveal at `(r,c)`. Counter is NOT bumped here;
lib/domain/game/game.dart:130:  Game revealHint(int r, int c, Cell value) => place(r, c, value);
lib/domain/game/game.dart:132:  Game tick(int newElapsedMs) =>
lib/domain/game/game.dart:133:      completed ? this : copyWith(elapsedMs: newElapsedMs);
lib/domain/game/game.dart:135:  Game copyWith({
lib/domain/game/game.dart:136:    Puzzle? puzzle,
lib/domain/game/game.dart:137:    Board? board,
lib/domain/game/game.dart:145:  }) => Game(
lib/domain/game/game.dart:158:/// One step in [Game.history]. `previous` is what the cell was BEFORE the
lib/domain/game/game.dart:163:  final Cell previous;
lib/domain/game/game_codec.dart:2:import '../board/cell.dart';
lib/domain/game/game_codec.dart:6:/// JSON codec for a [Game]. The puzzle itself is recovered by regenerating
lib/domain/game/game_codec.dart:8:class GameCodec {
lib/domain/game/game_codec.dart:9:  static Map<String, dynamic> toJson(Game g) => {
lib/domain/game/game_codec.dart:11:    'cells': g.board.toGlyphs(),
lib/domain/game/game_codec.dart:23:  static Game fromJson(Map<String, dynamic> j) {
lib/domain/game/game_codec.dart:25:    final board = Board.fromGlyphs(j['cells'] as String, puzzle.constraints);
lib/domain/game/game_codec.dart:31:          previous: Cell.fromGlyph(m['prev'] as String),
lib/domain/game/game_codec.dart:34:    return Game(
lib/domain/game/stats.dart:23:  Map<String, dynamic> toJson() => {
lib/domain/game/stats.dart:33:  static SolvedRecord fromJson(Map<String, dynamic> j) => SolvedRecord(
test/ui/screens/challenge_pack_screen_test.dart:28:    const puzzle = ChallengePuzzle(
test/ui/screens/challenge_pack_screen_test.dart:40:          cells: [
test/ui/screens/tutorial_screen_test.dart:197:  // ── interactive tap-cycle cell on page 2 ─────────────────────────────────
test/ui/screens/tutorial_screen_test.dart:198:  testWidgets('tap-cycle cell on page 2 shows touch icon when empty, hides it after tap', (
test/ui/screens/tutorial_screen_test.dart:211:    // The touch_app icon is shown when the cell is empty (interactive hint).
test/ui/screens/tutorial_screen_test.dart:214:    // Tap the touch_app icon — GestureDetector covers the whole cell.
lib/domain/solver/techniques.dart:2:import '../board/cell.dart';
lib/domain/solver/techniques.dart:6:/// Pure functions that scan a [Board] and return all deductions they can find
lib/domain/solver/techniques.dart:9:/// A "deduction" is always for a currently-empty cell. Techniques only emit
lib/domain/solver/techniques.dart:11:/// current cel

# rg -n 'mark|toggle|annotation|selected|mistake|hint|test\\('
test/domain/challenge_pack_test.dart:23:  test('contains 30 diverse puzzles in a non-monotonic size sequence', () {
test/domain/challenge_pack_test.dart:44:  test('every ground truth satisfies all base and experimental rules', () {
test/domain/challenge_pack_test.dart:68:  test('every puzzle is solvable and has exactly one valid solution', () {
test/domain/challenge_pack_test.dart:87:  test('ground truths can be entered from each initial board', () {
test/domain/challenge_pack_test.dart:106:  test('finale has visible starter anchors', () {
test/domain/challenge_pack_test.dart:114:  test('finale top-right constellation clue matches its visible 2x2 cage', () {
test/domain/challenge_pack_test.dart:130:  test('mechanics are introduced progressively', () {
test/domain/challenge_pack_test.dart:156:  test('rule evaluation rejects a board with the wrong cell count', () {
test/domain/rules_test.dart:8:    test('empty board has no violations', () {
test/domain/rules_test.dart:12:    test('detects too many of one symbol in a row', () {
test/domain/rules_test.dart:25:    test('detects three in a row horizontally', () {
test/domain/rules_test.dart:37:    test('detects three in a column', () {
test/domain/rules_test.dart:49:    test('detects = edge violation', () {
test/domain/rules_test.dart:64:    test('detects x edge violation', () {
test/domain/rules_test.dart:79:    test('detects two identical fully-filled rows', () {
test/domain/rules_test.dart:92:    test('detects two identical fully-filled columns', () {
test/domain/rules_test.dart:104:    test('does NOT flag partially-filled rows that happen to match', () {
test/domain/rules_test.dart:118:    test('accepts a known-valid completed board', () {
test/domain/difficulty_test.dart:8:    test('bands are relative to board area, so a label means the same '
test/domain/difficulty_test.dart:25:    test('monotonic in score', () {
test/domain/difficulty_test.dart:34:    test('a higher score never yields an easier label', () {
test/domain/difficulty_test.dart:46:    test('is the sum of applied technique weights', () {
test/domain/achievements_test.dart:7:  test('unlocks solve, perfect, size, and speed achievements from records', () {
test/domain/achievements_test.dart:16:  test(
test/domain/achievements_test.dart:38:  int hintsUsed = 0,
test/domain/achievements_test.dart:39:  int mistakes = 0,
test/domain/achievements_test.dart:45:  hintsUsed: hintsUsed,
test/domain/achievements_test.dart:46:  mistakes: mistakes,
lib/domain/challenge/challenge_puzzle.dart:180:    void markRow(int r) {
lib/domain/challenge/challenge_puzzle.dart:186:    void markColumn(int c) {
lib/domain/challenge/challenge_puzzle.dart:196:        markRow(r);
lib/domain/challenge/challenge_puzzle.dart:213:        markColumn(c);
lib/domain/challenge/challenge_puzzle.dart:255:        markRow(entry.key);
lib/domain/challenge/challenge_puzzle.dart:262:        markColumn(entry.key);
lib/domain/challenge/challenge_puzzle.dart:266:    _markDuplicates(puzzle, cells, bad, rows: true);
lib/domain/challenge/challenge_puzzle.dart:267:    _markDuplicates(puzzle, cells, bad, rows: false);
lib/domain/challenge/challenge_puzzle.dart:287:  static void _markDuplicates(
test/domain/puzzle_code_test.dart:8:    test('round trips for every size × difficulty across the seed range', () {
test/domain/puzzle_code_test.dart:23:    test('encoded form is three uppercase words separated by hyphens', () {
test/domain/puzzle_code_test.dart:34:    test('decode tolerates whitespace, lowercase, and alternative separators',
test/domain/puzzle_code_test.dart:46:    test('decode rejects swapped word (checksum mismatch or wrong category)',
test/domain/puzzle_code_test.dart:62:    test('decode rejects unknown words', () {
test/domain/puzzle_code_test.dart:73:    test('decode rejects wrong word count', () {
test/domain/puzzle_code_test.dart:84:    test('wordlists are exactly 256 entries each, no duplicates', () {
lib/domain/achievement.dart:123:    description: 'Complete any puzzle with 0 hints and 0 mistakes.',
lib/domain/achievement.dart:129:    description: 'Complete 5 puzzles in a row with 0 mistakes.',
lib/domain/achievement.dart:135:    description: 'Complete 10 puzzles total with 0 hints and 0 mistakes.',
lib/domain/achievement.dart:213:    description: 'Use all 3 hint stages in a single puzzle.',
lib/domain/achievement.dart:262:  if (records.any((r) => r.hintsUsed == 0 && r.mistakes == 0)) {
lib/domain/achievement.dart:266:  if (records.where((r) => r.hintsUsed == 0 && r.mistakes == 0).length >= 10) {
lib/domain/achievement.dart:312:    if (r.mistakes == 0) {
test/domain/premium_service_test.dart:8:  test('GREENBEAN is not recognized in App Store builds', () async {
test/domain/premium_service_test.dart:23:  test('normal promo codes do not enable developer testing mode', () async {
test/domain/premium_service_test.dart:35:  test('testing deactivation clears premium and developer state', () async {
test/domain/solver_test.dart:14:    test('horizontal pair forces flanks', () {
test/domain/solver_test.dart:29:    test('vertical pair forces flanks', () {
test/domain/solver_test.dart:44:    test('S _ S forces moon in middle', () {
test/domain/solver_test.dart:59:    test('= edge with one side filled propagates', () {
test/domain/solver_test.dart:77:    test('x edge with one side filled propagates to opposite', () {
test/domain/solver_test.dart:95:    test('outside neighbor forces the locked pair to opposite', () {
test/domain/solver_test.dart:115:    test('row with N/2 of one symbol fills rest with opposite', () {
test/domain/solver_test.dart:133:    test('forces cells where every completion of a row agrees', () {
test/domain/solver_test.dart:156:    test('combines edge constraints with line enumeration', () {
test/domain/solver_test.dart:180:    test('completes a near-solved board', () {
test/domain/solver_test.dart:196:    test('detects contradictory state', () {
test/domain/solver_test.dart:209:    test('empty board: cannot make progress', () {
test/domain/solver_test.dart:216:  group('Solver.nextDeduction (hints)', () {
test/domain/solver_test.dart:217:    test('returns the easiest available technique first', () {
test/domain/solver_test.dart:237:  group('Solver.hint (conclusive hints)', () {
test/domain/solver_test.dart:238:    test('a solved board reports solved', () {
test/domain/solver_test.dart:240:      expect(Solver.hint(p.solution!).status, HintStatus.solved);
test/domain/solver_test.dart:243:    test('an empty (non-unique) board reports ambiguous', () {
test/domain/solver_test.dart:244:      expect(Solver.hint(Board.empty(4)).status, HintStatus.ambiguous);
test/domain/solver_test.dart:247:    // The central invariant: from any legal, mistake-free state of a uniquely-
test/domain/solver_test.dart:248:    // solvable puzzle the hint engine must ALWAYS return a forced move that
test/domain/solver_test.dart:254:        test('every mistake-free state yields a correct, followable hint '
test/domain/solver_test.dart:273:            final result = Solver.hint(board);
test/domain/solver_test.dart:275:                reason: 'no hint for a legal state:\n${board.toGlyphs()}');
test/domain/solver_test.dart:278:                reason: 'hint disagreed with the solution at (${d.r},${d.c})');
test/domain/solver_test.dart:280:                reason: 'hint fell through to the un-followable fallback:\n'
test/domain/solver_test.dart:287:    test('walking the hint chain solves with only followable techniques', () {
test/domain/solver_test.dart:294:            reason: 'hint chain failed to terminate');
test/domain/solver_test.dart:295:        final result = Solver.hint(board);
test/domain/solver_test.dart:305:    test('a silent mistake is reported as a contradiction, not a dead end', () {
test/domain/solver_test.dart:310:      // so the hint must report a contradiction rather than "no deduction".
test/domain/solver_test.dart:321:      expect(Solver.hint(board).status, HintStatus.contradiction);
test/domain/generator_test.dart:10:    test('generates valid, uniquely-solvable 6×6 puzzles from many seeds', () {
test/domain/generator_test.dart:22:    test('classified difficulty is reproducible from the same seed', () {
test/domain/generator_test.dart:30:    test('fromCode round-trips: code → puzzle → same code', () {
test/domain/generator_test.dart:38:    test('generates valid 8×8 and 10×10 puzzles', () {
test/domain/generator_test.dart:47:    test('make() returns a puzzle of the requested difficulty', () {
test/domain/generator_test.dart:55:    test('fromCode opens any well-formed code, returning the scored band', () {
lib/domain/game/game.dart:7:/// history, hint usage, mistake count, and timing.
lib/domain/game/game.dart:23:  /// Number of hints the player has requested. Incremented once when witness
lib/domain/game/game.dart:26:  final int hintsUsed;
lib/domain/game/game.dart:30:  final int mistakes;
lib/domain/game/game.dart:51:    required this.hintsUsed,
lib/domain/game/game.dart:52:    required this.mistakes,
lib/domain/game/game.dart:63:    hintsUsed: 0,
lib/domain/game/game.dart:64:    mistakes: 0,
lib/domain/game/game.dart:90:  /// Sets `(r,c)` directly to [value] (used by hint reveals).
lib/domain/game/game.dart:106:  Game incrementMistakes() => copyWith(mistakes: mistakes + 1);
lib/domain/game/game.dart:120:  /// timer/mistakes/hints too.
lib/domain/game/game.dart:123:  /// Increments the hint counter without revealing a cell.
lib/domain/game/game.dart:126:  Game recordHint() => copyWith(hintsUsed: hintsUsed + 1);
lib/domain/game/game.dart:139:    int? hintsUsed,
lib/domain/game/game.dart:140:    int? mistakes,
lib/domain/game/game.dart:149:    hintsUsed: hintsUsed ?? this.hintsUsed,
lib/domain/game/game.dart:150:    mistakes: mistakes ?? this.mistakes,
lib/domain/game/game_codec.dart:15:    'hintsUsed': g.hintsUsed,
lib/domain/game/game_codec.dart:16:    'mistakes': g.mistakes,
lib/domain/game/game_codec.dart:38:      hintsUsed: j['hintsUsed'] as int,
lib/domain/game/game_codec.dart:39:      mistakes: j['mistakes'] as int,
lib/domain/game/stats.dart:9:  final int hintsUsed;
lib/domain/game/stats.dart:10:  final int mistakes;
lib/domain/game/stats.dart:18:    required this.hintsUsed,
lib/domain/game/stats.dart:19:    required this.mistakes,
lib/domain/game/stats.dart:28:        'hintsUsed': hintsUsed,
lib/domain/game/stats.dart:29:        'mistakes': mistakes,
lib/domain/game/stats.dart:38:        hintsUsed: j['hintsUsed'] as int,
lib/domain/game/stats.dart:39:        mistakes: j['mistakes'] as int,
test/ui/screens/settings_screen_test.dart:34:  testWidgets('SettingsScreen announces selected and locked themes', (
test/ui/screens/settings_screen_test.dart:61:        hint: 'Select theme',
test/ui/screens/settings_screen_test.dart:82:        hint: 'Unlock with Premium',
test/ui/screens/settings_screen_test.dart:200:  testWidgets('developer toggle is hidden until unlocked', (tester) async {
test/ui/screens/settings_screen_test.dart:222:  testWidgets('App Store build hides developer toggle even when unlocked', (
lib/domain/solver/solver.dart:35:/// Outcome of [Solver.hint]: either a forced move, or a reason no single move
lib/domain/solver/solver.dart:41:  /// The board cannot be completed — it holds a mistake, whether an outright
lib/domain/solver/solver.dart:116:  /// Suitable for the hint button. Pass [exclude] to suppress specific
lib/domain/solver/solver.dart:118:  /// appropriate to show as a hint).
lib/domain/solver/solver.dart:123:  /// Produce a hint for [state], guaranteed to be conclusive for any board
lib/domain/solver/solver.dart:127:  /// mistake-free state. It works in three stages:
lib/domain/solver/solver.dart:131:  ///      board carries a silent mistake — a placement that breaks no rule yet
lib/domain/solver/solver.dart:134:  ///   3. Otherwise the board is completable and mistake-free. Prefer a
lib/domain/solver/solver.dart:140:  static HintResult hint(Board state, {Set<Technique> exclude = const {}}) {
lib/domain/solver/solver.dart:155:    // Board is mistake-free and completable: any technique that fires is sound.
lib/domain/solver/technique.dart:103:/// One forced placement, with provenance for hint UIs.
lib/domain/solver/technique.dart:110:  /// Human-readable explanation (used for hint level 2).
lib/domain/solver/technique.dart:113:  /// Cells implicated in the deduction (used for hint level 1: region highlight).
test/ui/screens/tutorial_screen_test.dart:75:  testWidgets('tapping Skip marks tutorialSeen and pops', (tester) async {
test/ui/screens/tutorial_screen_test.dart:150:      // Skip (which calls markSeen) — should stay true, not flip false.
test/ui/screens/tutorial_screen_test.dart:211:    // The touch_app icon is shown when the cell is empty (interactive hint).
test/ui/screens/game_header_test.dart:4://        time and non-trivial hints/mistakes values.
test/ui/screens/game_header_test.dart:120:      'non-trivial hints and mistakes', (tester) async {
test/ui/screens/game_header_test.dart:122:    // push the "mistakes" chip off the right edge of a 320 px screen.
test/ui/screens/game_header_test.dart:127:      hintsUsed: 3,
test/ui/screens/game_header_test.dart:128:      mistakes: 5,
test/ui/screens/game_header_test.dart:151:      reason: 'hints chip must be rendered',
test/ui/screens/game_header_test.dart:156:      reason: 'mistakes chip must be rendered',
test/ui/screens/game_header_test.dart:183:        hintsUsed: 12,
test/ui/screens/game_header_test.dart:184:        mistakes: 9,
test/ui/screens/game_header_test.dart:221:        hintsUsed: 0,
test/ui/screens/game_header_test.dart:222:        mistakes: 0,
test/ui/screens/game_header_test.dart:289:        hintsUsed: 0,
test/ui/screens/game_header_test.dart:290:        mistakes: 0,
test/ui/screens/game_header_test.dart:338:        hintsUsed: 2,
test/ui/screens/game_header_test.dart:339:        mistakes: 1,
test/ui/screens/game_header_test.dart:354:      expect(find.text('2'), findsOneWidget, reason: 'hints value');
test/ui/screens/game_header_test.dart:355:      expect(find.text('1'), findsOneWidget, reason: 'mistakes value');
test/ui/screens/game_header_test.dart:357:      expect(find.text('hints'), findsOneWidget, reason: 'hints label');
test/ui/screens/game_header_test.dart:358:      expect(find.text('mistakes'), findsOneWidget, reason: 'mistakes label');
test/ui/screens/game_header_test.dart:379:        hintsUsed: 0,
test/ui/screens/game_header_test.dart:380:        mistakes: 0,
test/ui/screens/game_header_test.dart:408:        hintsUsed: 0,
test/ui/screens/game_header_test.dart:409:        mistakes: 0,
test/ui/screens/game_header_test.dart:424:      expect(find.text('hints'), findsOneWidget);
test/ui/screens/game_header_test.dart:425:      expect(find.text('mistakes'), findsOneWidget);
test/ui/screens/game_header_test.dart:437:        hintsUsed: 5,
test/ui/screens/game_header_test.dart:438:        mistakes: 3,
test/ui/screens/game_header_test.dart:465:        hintsUsed: 0,
test/ui/screens/game_header_test.dart:466:        mistakes: 0,
test/ui/state/custom_symbol_image_normalizer_test.dart:10:  test('normalizes a non-square photo into a bounded square JPEG tile', () {
test/ui/state/custom_symbol_image_normalizer_test.dart:41:  test('uses the requested square crop instead of rejecting wide photos', () {
test/ui/state/custom_symbol_image_normalizer_test.dart:70:  test('honours a crop flush against the right edge', () {
test/ui/state/custom_symbol_image_normalizer_test.dart:100:  test('honours a crop flush against the bottom edge', () {
test/ui/state/custom_symbol_image_normalizer_test.dart:130:  test('clamps an over-reaching crop back inside the image bounds', () {
test/ui/state/custom_symbol_image_normalizer_test.dart:152:  test('loads an orientation-baked preview for the crop screen', () async {
test/ui/state/providers_test.dart:23:  test('falls back to Easy for an unknown stored difficulty code', () async {
test/ui/state/providers_test.dart:32:  test('falls back to Easy when no difficulty code is stored', () async {
test/ui/state/providers_test.dart:39:  test('honors a valid stored difficulty code', () async {
test/ui/state/providers_test.dart:46:  test('generates unique custom-symbol file names per replacement', () {
test/ui/stats_screen_test.dart:46:        hintsUsed: 1,
test/ui/stats_screen_test.dart:47:        mistakes: 3,
lib/ui/widgets/puzzle_code_chip.dart:36:      hint: 'Tap to copy. Long press to share.',
lib/ui/state/providers.dart:95:/// developer marker instead of ads. Unlocked by redeeming the GREENBEAN code;
lib/ui/state/providers.dart:96:/// toggled on/off from Settings thereafter.
lib/ui/state/providers.dart:116:/// visibility of the developer-mode toggle in Settings. Recomputed after a
lib/ui/state/providers.dart:514:  Future<void> markSeen() async {
lib/ui/state/providers.dart:553:  /// Cells currently in mistake-grace: every cell implicated in a rule
lib/ui/state/providers.dart:555:  /// red-highlight set and from mistake counting until the grace timer
lib/ui/state/providers.dart:562:  /// Used to avoid double-counting a mistake when the player taps other
lib/ui/state/providers.dart:700:  /// charge one mistake. Re-emit state so providers refresh.
lib/ui/state/providers.dart:720:      // grace set even if mistakes didn't change.
lib/ui/state/providers.dart:755:  /// Bumps the hint counter without revealing a cell (stages 1 & 2).
lib/ui/state/providers.dart:758:  /// Consume a hint: reveal the deduced cell. Counter was already bumped
lib/ui/state/providers.dart:809:              hintsUsed: next.hintsUsed,
lib/ui/state/providers.dart:810:              mistakes: next.mistakes,
lib/ui/state/providers.dart:870:// Convenience: the current hint for the live board, or null if no game is
lib/ui/state/providers.dart:871:// active. The result is conclusive — for a legal, mistake-free state it always
lib/ui/state/providers.dart:879:// it used to force the hint onto a whole-board contradiction search the player
lib/ui/state/providers.dart:880:// could never replicate. Solver.hint keeps that search only as an unreachable
lib/ui/state/providers.dart:885:  return Solver.hint(g.board);
lib/ui/widgets/edges_overlay.dart:6:/// Renders all edge constraint marks (`=` / `×`) as a single overlay.
lib/ui/widgets/edges_overlay.dart:9:/// place a mark between two adjacent cells.
lib/ui/widgets/edges_overlay.dart:65:    final markSize = cellGap * 0.8 + cellSide * 0.22;
lib/ui/widgets/edges_overlay.dart:77:      // Background chip behind the mark so it sits cleanly over either cell.
lib/ui/widgets/edges_overlay.dart:78:      final chipR = markSize / 2 + 1.5;
lib/ui/widgets/edges_overlay.dart:89:        ..strokeWidth = markSize * 0.14
lib/ui/widgets/edges_overlay.dart:93:        final off = markSize * 0.18;
lib/ui/widgets/edges_overlay.dart:95:          Offset(mx - markSize / 2, my - off),
lib/ui/widgets/edges_overlay.dart:96:          Offset(mx + markSize / 2, my - off),
lib/ui/widgets/edges_overlay.dart:100:          Offset(mx - markSize / 2, my + off),
lib/ui/widgets/edges_overlay.dart:101:          Offset(mx + markSize / 2, my + off),
lib/ui/widgets/edges_overlay.dart:106:          Offset(mx - markSize / 2, my - markSize / 2),
lib/ui/widgets/edges_overlay.dart:107:          Offset(mx + markSize / 2, my + markSize / 2),
lib/ui/widgets/edges_overlay.dart:111:          Offset(mx - markSize / 2, my + markSize / 2),
lib/ui/widgets/edges_overlay.dart:112:          Offset(mx + markSize / 2, my - markSize / 2),
lib/ui/widgets/action_tile.dart:41:      hint: semanticHint,
test/ads/ad_event_queue_test.dart:28:  test('round-trips ad events in append order', () async {
test/ads/ad_event_queue_test.dart:43:  test('clears acknowledged events and can clear the whole queue', () async {
test/ads/ad_event_queue_test.dart:60:  test('drops corrupt queue blobs and resets storage access', () async {
lib/ui/widgets/board_widget.dart:17:/// Renders the game board: cells, edge marks, error/hint highlights.
lib/ui/theme/design_tokens.dart:39:  /// Accent (CTA buttons, focused outline, hint highlight).
lib/ui/theme/design_tokens.dart:49:  /// Color of `=` and `x` edge marks.
test/ads/ad_fallback_tier_test.dart:5:  test('routes to liveAd only when premium is off and internet is present', () {
test/ads/ad_fallback_tier_test.dart:16:  test('routes to onlineFallback when live ads are unavailable online', () {
test/ads/ad_fallback_tier_test.dart:35:  test('routes to offlineFallback when no internet is present', () {
lib/storage/storage_service.dart:245:  /// Once unlocked, the Settings toggle stays available even when developer
test/ads/ad_gate_test.dart:31:  test('offline reward is granted only once per local day', () async {
test/ads/ad_gate_test.dart:42:  test('offline bookkeeping resets when the local day rolls over', () async {
test/ads/play_licensing_test.dart:6:    test('embedded LVL public key is present and well-formed base64', () {
test/ads/play_licensing_test.dart:11:    test('verifyPlayPurchase returns false (never throws) on garbage input', () {
lib/ui/screens/tutorial_screen.dart:17:  /// Convenience: push the tutorial onto the navigator and mark it seen on
lib/ui/screens/tutorial_screen.dart:41:  Future<void> _markAndPop() async {
lib/ui/screens/tutorial_screen.dart:42:    await ref.read(tutorialSeenProvider.notifier).markSeen();
lib/ui/screens/tutorial_screen.dart:53:      _markAndPop();
lib/ui/screens/tutorial_screen.dart:81:                    hint: 'Return to the home screen',
lib/ui/screens/tutorial_screen.dart:82:                    onTap: _markAndPop,
lib/ui/screens/tutorial_screen.dart:84:                      onPressed: _markAndPop,
lib/ui/screens/tutorial_screen.dart:124:                  hint: isLast ? 'Close tutorial and go to Home' : 'Next step',
lib/ui/screens/tutorial_screen.dart:485:            'Two pairs of cells with edge marks. First pair has equals mark, second pair has times mark.',
lib/ui/theme/sundown_theme.dart:21:  /// True for dark-mode themes — Flutter's system chrome uses this hint to
lib/ui/theme/sundown_theme.dart:42:/// scope widget marks dependents dirty.
test/ads/network_status_service_test.dart:8:  test('maps disconnected transport to offline without probing', () async {
test/ads/network_status_service_test.dart:22:  test(
test/ads/network_status_service_test.dart:41:  test('listens for connectivity changes and re-probes each change', () async {
lib/ui/screens/settings_screen.dart:64:                  selected: current.id == theme.id,
lib/ui/screens/settings_screen.dart:80:/// Developer-mode toggle. Hidden until the GREENBEAN password has been redeemed
lib/ui/screens/settings_screen.dart:459:      hint: 'Unlock with Premium',
lib/ui/screens/settings_screen.dart:553:      selected: imagePath != null,
lib/ui/screens/settings_screen.dart:555:      value: imagePath == null ? 'Default symbol' : 'Custom image selected',
lib/ui/screens/settings_screen.dart:596:                      : 'Custom image selected',
lib/ui/screens/settings_screen.dart:965:  final bool selected;
lib/ui/screens/settings_screen.dart:972:    required this.selected,
lib/ui/screens/settings_screen.dart:983:        : selected
lib/ui/screens/settings_screen.dart:989:      selected: selected,
lib/ui/screens/settings_screen.dart:992:      hint: locked ? 'Unlock with Premium' : 'Select theme',
lib/ui/screens/settings_screen.dart:1005:                  color: selected ? tokens.accent : tokens.gridLine,
lib/ui/screens/settings_screen.dart:1006:                  width: selected ? 2 : 0.5,
lib/ui/screens/settings_screen.dart:1028:                  else if (selected)
lib/ui/screens/play_by_code_dialog.dart:63:                hintText: 'WORD-WORD-WORD or sundown://puzzle?code=...',
lib/ui/screens/play_by_code_dialog.dart:64:                hintStyle: t.mono.copyWith(color: t.textMuted),
lib/ui/screens/game_screen.dart:33:  ///   0 = no hint pending
lib/ui/screens/game_screen.dart:37:  int _hintStage = 0;
lib/ui/screens/game_screen.dart:111:    // If the player edits the board between hint stages, recompute. The solver
lib/ui/screens/game_screen.dart:113:    // contradiction for one carrying a mistake (overt or silent).
lib/ui/screens/game_screen.dart:120:          'There are mistakes on the board — fix them before using a hint.',
lib/ui/screens/game_screen.dart:134:    // Click 1 — new hint target: show witness cells and count it.
lib/ui/screens/game_screen.dart:141:      setState(() => _hintStage = 1);
lib/ui/screens/game_screen.dart:144:    if (_hintStage < 3) {
lib/ui/screens/game_screen.dart:145:      setState(() => _hintStage += 1);
lib/ui/screens/game_screen.dart:146:      if (_hintStage == 2) {
lib/ui/screens/game_screen.dart:150:      if (_hintStage == 3) {
lib/ui/screens/game_screen.dart:154:          _hintStage = 0;
lib/ui/screens/game_screen.dart:168:    if (_hintStage != 0) {
lib/ui/screens/game_screen.dart:170:        _hintStage = 0;
lib/ui/screens/game_screen.dart:225:                    hintStage: _hintStage,
lib/ui/screens/game_screen.dart:230:                _Controls(onHint: _onHint, hintStage: _hintStage),
lib/ui/screens/game_screen.dart:269:                hintsUsed: g.hintsUsed,
lib/ui/screens/game_screen.dart:270:                mistakes: g.mistakes,
lib/ui/screens/game_screen.dart:359:                    label: 'hints',
lib/ui/screens/game_screen.dart:360:                    value: '${meta.hintsUsed}',
lib/ui/screens/game_screen.dart:367:                    label: 'mistakes',
lib/ui/screens/game_screen.dart:368:                    value: '${meta.mistakes}',
lib/ui/screens/game_screen.dart:383:  final int hintStage;
lib/ui/screens/game_screen.dart:388:    required this.hintStage,
lib/ui/screens/game_screen.dart:405:    String? hintReason;
lib/ui/screens/game_screen.dart:407:      if (hintStage >= 1) {
lib/ui/screens/game_screen.dart:410:      if (hintStage >= 2) hintReason = pendingHint!.reason;
lib/ui/screens/game_screen.dart:442:              child: hintReason == null
lib/ui/screens/game_screen.dart:444:                  : _HintBanner(key: ValueKey(hintReason), text: hintReason),
lib/ui/screens/game_screen.dart:455:  final int hintStage;
lib/ui/screens/game_screen.dart:456:  const _Controls({required this.onHint, required this.hintStage});
lib/ui/screens/game_screen.dart:469:    final hintLabel = switch (hintStage) {
lib/ui/screens/game_screen.dart:498:            label: hintLabel,
lib/ui/screens/game_screen.dart:499:            primary: hintStage > 0,
lib/ui/screens/game_screen.dart:501:            badge: hintStage > 0 ? hintStage : null,
lib/ui/screens/game_screen.dart:596:                              '${_fmt(game.elapsedMs)} · ${game.hintsUsed} hint${game.hintsUsed == 1 ? "" : "s"} · ${game.mistakes} mistake${game.mistakes == 1 ? "" : "s"}',
lib/ads/ad_gate.dart:104:    // Internal-only kill switch (no user-facing toggle).
lib/ui/screens/redeem_code_dialog.dart:67:                hintText: 'ENTER CODE',
lib/ui/screens/redeem_code_dialog.dart:68:                hintStyle: t.mono.copyWith(color: t.textMuted),
lib/ui/screens/new_game_screen.dart:53:                      selected: size,
lib/ui/screens/new_game_screen.dart:78:                          selected: diff,
lib/ui/screens/new_game_screen.dart:172:  final T selected;
lib/ui/screens/new_game_screen.dart:185:    required this.selected,
lib/ui/screens/new_game_screen.dart:196:    final on = value == selected;
lib/ui/screens/new_game_screen.dart:220:      selected: on,


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

# Read lib/domain/game/game_codec.dart
```dart
import '../board/board.dart';
import '../board/cell.dart';
import '../generator.dart';
import 'game.dart';

/// JSON codec for a [Game]. The puzzle itself is recovered by regenerating
/// from its code (deterministic), so we only persist the player's overlay.
class GameCodec {
  static Map<String, dynamic> toJson(Game g) => {
    'code': g.puzzle.code,
    'cells': g.board.toGlyphs(),
    'history': [
      for (final m in g.history) {'r': m.r, 'c': m.c, 'prev': m.previous.glyph},
    ],
    'hintsUsed': g.hintsUsed,
    'mistakes': g.mistakes,
    'elapsedMs': g.elapsedMs,
    'completed': g.completed,
    'startedAt': g.startedAt.toIso8601String(),
    'undoUsed': g.undoUsed,
  };

  static Game fromJson(Map<String, dynamic> j) {
    final puzzle = Generator.fromCode(j['code'] as String);
    final board = Board.fromGlyphs(j['cells'] as String, puzzle.constraints);
    final history = <Move>[
      for (final m in (j['history'] as List).cast<Map>())
        Move(
          r: m['r'] as int,
          c: m['c'] as int,
          previous: Cell.fromGlyph(m['prev'] as String),
        ),
    ];
    return Game(
      puzzle: puzzle,
      board: board,
      history: history,
      hintsUsed: j['hintsUsed'] as int,
      mistakes: j['mistakes'] as int,
      elapsedMs: j['elapsedMs'] as int,
      completed: j['completed'] as bool,
      startedAt: DateTime.parse(j['startedAt'] as String),
      undoUsed: j['undoUsed'] as bool? ?? false,
    );
  }
}

```

# Read lib/domain/board/board.dart
```dart
import 'cell.dart';
import 'edge.dart';

/// An immutable N×N grid of [Cell] values plus edge constraints.
///
/// `N` must be even (enforced at construction). Cells are stored row-major
/// in a flat list of length `N*N`.
class Board {
  final int size;
  final List<Cell> cells;
  final Constraints constraints;

  Board._(this.size, this.cells, this.constraints);

  factory Board(int size, List<Cell> cells,
      [Constraints constraints = const Constraints.empty()]) {
    if (size <= 0 || size.isOdd) {
      throw ArgumentError('Board size must be a positive even integer: $size');
    }
    if (cells.length != size * size) {
      throw ArgumentError('cells.length (${cells.length}) must equal size² (${size * size})');
    }
    return Board._(size, List.unmodifiable(cells), constraints);
  }

  /// Empty board of the given size with optional edge constraints.
  factory Board.empty(int size, [Constraints constraints = const Constraints.empty()]) =>
      Board(size, List<Cell>.filled(size * size, Cell.empty), constraints);

  /// Construct a board from a multi-line glyph string (rows separated by `\n`,
  /// each row of size characters from `{., S, M}`). Constraints are not parsed.
  factory Board.fromGlyphs(String s, [Constraints constraints = const Constraints.empty()]) {
    final rows = s.trim().split('\n').map((r) => r.trim()).where((r) => r.isNotEmpty).toList();
    final size = rows.length;
    if (size == 0) throw ArgumentError('empty glyph string');
    if (rows.any((r) => r.length != size)) {
      throw ArgumentError('glyph rows must be square; expected $size cols');
    }
    final cells = <Cell>[];
    for (final row in rows) {
      for (final ch in row.split('')) {
        cells.add(Cell.fromGlyph(ch));
      }
    }
    return Board(size, cells, constraints);
  }

  int _idx(int r, int c) => r * size + c;

  bool inBounds(int r, int c) => r >= 0 && r < size && c >= 0 && c < size;

  Cell at(int r, int c) {
    if (!inBounds(r, c)) throw RangeError('($r,$c) out of bounds for size $size');
    return cells[_idx(r, c)];
  }

  /// Returns a new board with cell `(r,c)` set to [value]. O(N²) copy.
  Board set(int r, int c, Cell value) {
    final next = [...cells];
    next[_idx(r, c)] = value;
    return Board._(size, List.unmodifiable(next), constraints);
  }

  /// Apply many cell mutations at once. More efficient than chained [set]s.
  Board setAll(Iterable<({int r, int c, Cell value})> updates) {
    final next = [...cells];
    for (final u in updates) {
      next[_idx(u.r, u.c)] = u.value;
    }
    return Board._(size, List.unmodifiable(next), constraints);
  }

  Board withConstraints(Constraints c) => Board._(size, cells, c);

  List<Cell> row(int r) => List<Cell>.generate(size, (c) => at(r, c));
  List<Cell> col(int c) => List<Cell>.generate(size, (r) => at(r, c));

  /// All cells filled in (no [Cell.empty] remaining).
  bool get isComplete => cells.every((c) => c.isFilled);

  /// Number of cells equal to [value].
  int countCells(Cell value) => cells.where((c) => c == value).length;

  /// Glyph representation, one line per row.
  String toGlyphs() {
    final b = StringBuffer();
    for (var r = 0; r < size; r++) {
      for (var c = 0; c < size; c++) {
        b.write(at(r, c).glyph);
      }
      if (r < size - 1) b.write('\n');
    }
    return b.toString();
  }

  @override
  String toString() => 'Board($size×$size, ${constraints.count} edges)\n${toGlyphs()}';

  @override
  bool operator ==(Object other) {
    if (other is! Board || other.size != size) return false;
    for (var i = 0; i < cells.length; i++) {
      if (cells[i] != other.cells[i]) return false;
    }
    // Constraints equality is identity-by-content.
    if (constraints.edges.length != other.constraints.edges.length) return false;
    for (final e in constraints.edges.entries) {
      if (other.constraints.edges[e.key] != e.value) return false;
    }
    return true;
  }

  @override
  int get hashCode => Object.hashAll([size, ...cells, constraints.edges.length]);
}

```

# Read lib/domain/board/cell.dart
```dart
/// The state of a single cell on the board.
///
/// Pure data — no UI here. Symbol rendering (☀/🌙, monochrome dots, etc.)
/// is a theme concern in the UI layer.
enum Cell {
  empty,
  sun,
  moon;

  bool get isFilled => this != Cell.empty;

  /// Returns the other filled value (sun↔moon). For [empty], returns [empty].
  Cell get opposite => switch (this) {
        Cell.sun => Cell.moon,
        Cell.moon => Cell.sun,
        Cell.empty => Cell.empty,
      };

  /// Compact single-character representation for debugging and serialization.
  /// `.` = empty, `S` = sun, `M` = moon.
  String get glyph => switch (this) {
        Cell.empty => '.',
        Cell.sun => 'S',
        Cell.moon => 'M',
      };

  static Cell fromGlyph(String s) => switch (s) {
        '.' => Cell.empty,
        'S' => Cell.sun,
        'M' => Cell.moon,
        _ => throw ArgumentError('not a cell glyph: $s'),
      };
}

```

# Read lib/domain/board/puzzle.dart
```dart
import '../difficulty.dart';
import 'board.dart';
import 'cell.dart';
import 'edge.dart';

/// A complete puzzle definition.
///
/// - [clues]: a [Board] containing the initial revealed cells (with [Cell.empty]
///   for cells the player must solve) and the edge [Constraints].
/// - [solution]: the unique completed solution. Optional — generators produce it,
///   but a [Puzzle] parsed from a code does not require it until solved.
/// - [difficulty]: the classified difficulty (the hardest technique required).
/// - [code]: the shareable puzzle code (`S6E-…`).
class Puzzle {
  final Board clues;
  final Board? solution;
  final Difficulty difficulty;
  final String code;

  const Puzzle({
    required this.clues,
    required this.difficulty,
    required this.code,
    this.solution,
  });

  int get size => clues.size;
  Constraints get constraints => clues.constraints;
  int get clueCount => clues.cells.where((c) => c.isFilled).length;

  Puzzle copyWith({Board? clues, Board? solution, Difficulty? difficulty, String? code}) =>
      Puzzle(
        clues: clues ?? this.clues,
        solution: solution ?? this.solution,
        difficulty: difficulty ?? this.difficulty,
        code: code ?? this.code,
      );
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

# Read test/domain/rules_test.dart
```dart
import 'package:flutter_test/flutter_test.dart';
import 'package:sundown/domain/board/board.dart';
import 'package:sundown/domain/board/edge.dart';
import 'package:sundown/domain/rules.dart';

void main() {
  group('Rules.firstViolation', () {
    test('empty board has no violations', () {
      expect(Rules.firstViolation(Board.empty(6)), isNull);
    });

    test('detects too many of one symbol in a row', () {
      // 6×6 row with four suns is illegal (half = 3).
      final b = Board.fromGlyphs('''
SSSS..
......
......
......
......
......
''');
      expect(Rules.firstViolation(b)?.kind, ViolationKind.tooMany);
    });

    test('detects three in a row horizontally', () {
      final b = Board.fromGlyphs('''
SSS...
......
......
......
......
......
''');
      expect(Rules.firstViolation(b)?.kind, ViolationKind.threeInARow);
    });

    test('detects three in a column', () {
      final b = Board.fromGlyphs('''
S.....
S.....
S.....
......
......
......
''');
      expect(Rules.firstViolation(b)?.kind, ViolationKind.threeInARow);
    });

    test('detects = edge violation', () {
      final b = Board.fromGlyphs(
        '''
SM....
......
......
......
......
......
''',
        Constraints({const EdgePos(0, 0, EdgeDir.right): EdgeKind.equal}),
      );
      expect(Rules.firstViolation(b)?.kind, ViolationKind.edge);
    });

    test('detects x edge violation', () {
      final b = Board.fromGlyphs(
        '''
SS....
......
......
......
......
......
''',
        Constraints({const EdgePos(0, 0, EdgeDir.right): EdgeKind.opposite}),
      );
      expect(Rules.firstViolation(b)?.kind, ViolationKind.edge);
    });

    test('detects two identical fully-filled rows', () {
      // 4×4 grid where rows 0 and 1 are identical and fully filled. The
      // other constraints are not violated (counts of 2 each, no
      // three-in-a-row), but the uniqueness rule must catch it.
      final b = Board.fromGlyphs('''
SMSM
SMSM
MSMS
MSMS
''');
      expect(Rules.firstViolation(b)?.kind, ViolationKind.duplicate);
    });

    test('detects two identical fully-filled columns', () {
      // 4×4 grid where cols 0 and 2 are identical and fully filled.
      final b = Board.fromGlyphs('''
SMSM
MSMS
SMSM
MSMS
''');
      // Rows 0&2 and 1&3 are duplicates too, but the row scan runs first.
      expect(Rules.firstViolation(b)?.kind, ViolationKind.duplicate);
    });

    test('does NOT flag partially-filled rows that happen to match', () {
      // Two rows are identical so far but both have empties, so they
      // could resolve to non-duplicates. Must be partial-valid.
      final b = Board.fromGlyphs('''
SM....
SM....
......
......
......
......
''');
      expect(Rules.isPartiallyValid(b), isTrue);
    });

    test('accepts a known-valid completed board', () {
      // hand-crafted valid 6×6 (3 suns + 3 moons per row/col, no three-in-a-row)
      final b = Board.fromGlyphs('''
SSMMSM
MMSSMS
SMSMSM
MSMSMS
SSMSMM
MMSMSS
''');
      expect(Rules.isSolved(b), isTrue);
    });
  });
}

```

# Read test/ui/widgets/board_widget_semantics_test.dart
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:flutter_test/flutter_test.dart';
import 'package:shared_preferences/shared_preferences.dart';

import 'package:sundown/domain/board/board.dart';
import 'package:sundown/domain/board/puzzle.dart';
import 'package:sundown/domain/difficulty.dart';
import 'package:sundown/storage/storage_service.dart';
import 'package:sundown/ui/state/providers.dart';
import 'package:sundown/ui/theme/sundown_theme.dart';
import 'package:sundown/ui/theme/themes.dart';
import 'package:sundown/ui/widgets/board_widget.dart';
import 'package:sundown/ui/widgets/cell_widget.dart';

Future<StorageService> _storage() async {
  SharedPreferences.setMockInitialValues({});
  return StorageService.open();
}

Finder _cellAt(int row, int column) => find.byWidgetPredicate(
  (widget) =>
      widget is CellWidget && widget.row == row && widget.column == column,
);

Future<void> _pumpBoard(
  WidgetTester tester, {
  required Puzzle puzzle,
  required Board board,
  required bool locked,
  Set<(int, int)> highlightCells = const {},
  Set<(int, int)> errorCells = const {},
}) async {
  final storage = await _storage();
  await tester.pumpWidget(
    ProviderScope(
      overrides: [storageProvider.overrideWithValue(storage)],
      child: SundownThemeScope(
        theme: SundownThemes.classic,
        child: Directionality(
          textDirection: TextDirection.ltr,
          child: Center(
            child: SizedBox.square(
              dimension: 240,
              child: BoardWidget(
                puzzle: puzzle,
                board: board,
                onTap: (row, column) {},
                highlightCells: highlightCells,
                errorCells: errorCells,
                locked: locked,
              ),
            ),
          ),
        ),
      ),
    ),
  );
  await tester.pumpAndSettle();
}

void main() {
  testWidgets('Board cells expose a single actionable semantics node', (
    tester,
  ) async {
    final puzzle = Puzzle(
      clues: Board.fromGlyphs('S.\n..'),
      difficulty: Difficulty.easy,
      code: 'S2E-TEST',
    );
    final board = Board.fromGlyphs('S.\n.M');
    final semanticsHandle = tester.ensureSemantics();
    await _pumpBoard(
      tester,
      puzzle: puzzle,
      board: board,
      locked: false,
      highlightCells: {(1, 0)},
      errorCells: {(1, 1)},
    );

    expect(
      tester.getSemantics(
        find
            .descendant(of: _cellAt(0, 0), matching: find.byType(Semantics))
            .first,
      ),
      matchesSemantics(label: 'Row 1, column 1. Clue cell, sun.'),
    );
    expect(
      tester.getSemantics(
        find
            .descendant(of: _cellAt(0, 1), matching: find.byType(Semantics))
            .first,
      ),
      matchesSemantics(
        label: 'Row 1, column 2. Player cell, empty.',
        isButton: true,
        hasTapAction: true,
      ),
    );
    expect(
      tester.getSemantics(
        find
            .descendant(of: _cellAt(1, 0), matching: find.byType(Semantics))
            .first,
      ),
      matchesSemantics(
        label: 'Row 2, column 1. Player cell, empty. Highlighted.',
        isButton: true,
        hasTapAction: true,
      ),
    );
    expect(
      tester.getSemantics(
        find
            .descendant(of: _cellAt(1, 1), matching: find.byType(Semantics))
            .first,
      ),
      matchesSemantics(
        label: 'Row 2, column 2. Player cell, moon. Error.',
        isButton: true,
        hasTapAction: true,
      ),
    );

    expect(
      find.byType(Semantics),
      findsNWidgets(4),
      reason: 'painted symbols should not introduce extra semantics nodes',
    );

    semanticsHandle.dispose();
  });

  testWidgets('Locked cells are not announced as actionable', (tester) async {
    final puzzle = Puzzle(
      clues: Board.fromGlyphs('S.\n..'),
      difficulty: Difficulty.easy,
      code: 'S2E-TEST',
    );
    final board = Board.fromGlyphs('S.\n.M');
    final semanticsHandle = tester.ensureSemantics();
    await _pumpBoard(tester, puzzle: puzzle, board: board, locked: true);

    expect(
      tester.getSemantics(
        find
            .descendant(of: _cellAt(0, 1), matching: find.byType(Semantics))
            .first,
      ),
      matchesSemantics(label: 'Row 1, column 2. Player cell, empty.'),
    );
    expect(
      tester.getSemantics(
        find
            .descendant(of: _cellAt(1, 1), matching: find.byType(Semantics))
            .first,
      ),
      matchesSemantics(label: 'Row 2, column 2. Player cell, moon.'),
    );

    expect(
      tester.getSemantics(
        find
            .descendant(of: _cellAt(0, 1), matching: find.byType(Semantics))
            .first,
      ),
      isNot(matchesSemantics(isButton: true, hasTapAction: true)),
    );

    semanticsHandle.dispose();
  });
}

```