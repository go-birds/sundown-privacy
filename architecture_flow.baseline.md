# Prompt
Explain how a new game session is created, persisted, restored, and shown on the game screen. Include the main classes/functions and the file paths.


# rg -n '\\bGame\\b|GameCodec|newGame|create.*game|restore|persist|save|load'
lib/main.dart:95:    RewardedAdService.instance.preload();
lib/main.dart:110:  // Game-timer pause/resume on app background is handled by GameScreen itself
lib/main.dart:112:  // actually on screen — never on the home screen with a saved game.
test/ads/ad_event_queue_test.dart:36:    final loaded = storage.loadAdEventQueue();
test/ads/ad_event_queue_test.dart:37:    expect(loaded, hasLength(2));
test/ads/ad_event_queue_test.dart:38:    expect(loaded.map((event) => event.id), ['a', 'b']);
test/ads/ad_event_queue_test.dart:39:    expect(loaded[0].tier, AdFallbackTier.liveAd);
test/ads/ad_event_queue_test.dart:40:    expect(loaded[1].premiumState, isFalse);
test/ads/ad_event_queue_test.dart:54:    expect(storage.loadAdEventQueue().map((event) => event.id), ['a', 'c']);
test/ads/ad_event_queue_test.dart:57:    expect(storage.loadAdEventQueue(), isEmpty);
test/ads/ad_event_queue_test.dart:65:    expect(storage.loadAdEventQueue(), isEmpty);
lib/domain/game/game.dart:9:/// All fields are immutable; mutations produce a new [Game] via [copyWith]
lib/domain/game/game.dart:12:class Game {
lib/domain/game/game.dart:47:  const Game({
lib/domain/game/game.dart:59:  factory Game.fresh(Puzzle p, {DateTime? now}) => Game(
lib/domain/game/game.dart:76:  /// Returns the next [Game] after the player taps cell `(r,c)`. Cycle order:
lib/domain/game/game.dart:78:  Game tap(int r, int c) {
lib/domain/game/game.dart:91:  Game place(int r, int c, Cell value) {
lib/domain/game/game.dart:99:  Game _applyMove(int r, int c, Cell prev, Cell next) {
lib/domain/game/game.dart:106:  Game incrementMistakes() => copyWith(mistakes: mistakes + 1);
lib/domain/game/game.dart:109:  Game undo() {
lib/domain/game/game.dart:121:  Game restart() => Game.fresh(puzzle, now: startedAt);
lib/domain/game/game.dart:126:  Game recordHint() => copyWith(hintsUsed: hintsUsed + 1);
lib/domain/game/game.dart:130:  Game revealHint(int r, int c, Cell value) => place(r, c, value);
lib/domain/game/game.dart:132:  Game tick(int newElapsedMs) =>
lib/domain/game/game.dart:135:  Game copyWith({
lib/domain/game/game.dart:145:  }) => Game(
lib/domain/game/game.dart:158:/// One step in [Game.history]. `previous` is what the cell was BEFORE the
lib/storage/storage_service.dart:55:  Future<void> saveCurrentGame(Game? g) async {
lib/storage/storage_service.dart:60:    await _prefs.setString(_kCurrentGame, jsonEncode(GameCodec.toJson(g)));
lib/storage/storage_service.dart:63:  /// Raw JSON string for the saved game, or null. This is a fast SharedPreferences
lib/storage/storage_service.dart:64:  /// lookup — callers that need a [Game] should reconstruct it in an isolate via
lib/storage/storage_service.dart:65:  /// [GameCodec.fromJson] rather than calling [loadCurrentGame] on the main thread.
lib/storage/storage_service.dart:68:  /// Synchronous convenience loader. Avoid on the main thread for large puzzles;
lib/storage/storage_service.dart:70:  Game? loadCurrentGame() {
lib/storage/storage_service.dart:74:      return GameCodec.fromJson(jsonDecode(s) as Map<String, dynamic>);
lib/storage/storage_service.dart:83:  List<SolvedRecord> loadStats() {
lib/storage/storage_service.dart:97:  Future<void> saveStats(List<SolvedRecord> records) async {
lib/storage/storage_service.dart:105:    final list = loadStats()..add(rec);
lib/storage/storage_service.dart:106:    await saveStats(list);
lib/storage/storage_service.dart:110:  List<AdEventRecord> loadAdEventQueue() {
lib/storage/storage_service.dart:125:  Future<void> _saveAdEventQueue(List<AdEventRecord> events) async {
lib/storage/storage_service.dart:133:    final list = loadAdEventQueue()..add(event);
lib/storage/storage_service.dart:134:    await _saveAdEventQueue(list);
lib/storage/storage_service.dart:142:    final next = loadAdEventQueue()
lib/storage/storage_service.dart:149:    await _saveAdEventQueue(next);
lib/storage/storage_service.dart:155:  List<UnlockedAchievement> loadAchievements() {
lib/storage/storage_service.dart:169:  Future<void> saveAchievements(List<UnlockedAchievement> achievements) async {
test/ui/widgets/banner_ad_widget_test.dart:45:  Future<void> restorePurchases() async {}
lib/domain/code/puzzle_code.dart:7:/// Wire format: three 8-bit indices, packed into a 24-bit payload that
lib/domain/code/puzzle_code.dart:16:/// `ADJ-COL-NOUN`. Each index is one byte of the 24-bit payload,
lib/domain/code/puzzle_code.dart:46:    final payload = (_sizeToIndex[size]! << 22) |
lib/domain/code/puzzle_code.dart:50:    final i0 = (payload >> 16) & 0xFF;
lib/domain/code/puzzle_code.dart:51:    final i1 = (payload >> 8) & 0xFF;
lib/domain/code/puzzle_code.dart:52:    final i2 = payload & 0xFF;
lib/domain/code/puzzle_code.dart:73:    final payload = (i0 << 16) | (i1 << 8) | i2;
lib/domain/code/puzzle_code.dart:74:    final sizeIdx = (payload >> 22) & 0x3;
lib/domain/code/puzzle_code.dart:75:    final diffIdx = (payload >> 20) & 0x3;
lib/domain/code/puzzle_code.dart:76:    final seed = (payload >> _checksumBits) & ((1 << seedBits) - 1);
lib/domain/code/puzzle_code.dart:77:    final checksum = payload & _checksumMask;
test/ads/play_licensing_test.dart:12:      // A bogus payload/signature must be rejected, and the embedded key must
lib/ads/rewarded_ad_service.dart:21:/// Keeps one ad preloaded so the gate can present instantly, and reloads after
lib/ads/rewarded_ad_service.dart:33:  bool _loading = false;
lib/ads/rewarded_ad_service.dart:35:  /// Preload a rewarded ad if one isn't already loaded/loading. Safe to call
lib/ads/rewarded_ad_service.dart:37:  void preload() {
lib/ads/rewarded_ad_service.dart:41:        _loading) {
lib/ads/rewarded_ad_service.dart:44:    _loading = true;
lib/ads/rewarded_ad_service.dart:45:    RewardedAd.load(
lib/ads/rewarded_ad_service.dart:51:          _loading = false;
lib/ads/rewarded_ad_service.dart:55:          _loading = false;
lib/ads/rewarded_ad_service.dart:56:          debugPrint('RewardedAd failed to load: $err');
lib/ads/rewarded_ad_service.dart:73:      // Nothing to show — kick off a load for next time and let the play
lib/ads/rewarded_ad_service.dart:75:      preload();
lib/ads/rewarded_ad_service.dart:85:        preload();
lib/ads/rewarded_ad_service.dart:90:        preload();
lib/ads/rewarded_ad_service.dart:102:      preload();
lib/domain/game/game_codec.dart:6:/// JSON codec for a [Game]. The puzzle itself is recovered by regenerating
lib/domain/game/game_codec.dart:7:/// from its code (deterministic), so we only persist the player's overlay.
lib/domain/game/game_codec.dart:8:class GameCodec {
lib/domain/game/game_codec.dart:9:  static Map<String, dynamic> toJson(Game g) => {
lib/domain/game/game_codec.dart:23:  static Game fromJson(Map<String, dynamic> j) {
lib/domain/game/game_codec.dart:34:    return Game(
lib/domain/achievement.dart:11:  persistence,
lib/domain/achievement.dart:214:    category: AchievementCategory.persistence,
lib/domain/achievement.dart:220:    category: AchievementCategory.persistence,
lib/domain/achievement.dart:226:    category: AchievementCategory.persistence,
lib/domain/achievement.dart:232:    category: AchievementCategory.persistence,
lib/ads/play_licensing.dart:9:/// signs every Play purchase payload with the matching private key; this app
lib/ads/ump_consent_service.dart:78:      await _loadAndShowConsentFormIfRequired();
lib/ads/ump_consent_service.dart:102:  Future<void> _loadAndShowConsentFormIfRequired() async {
lib/ads/ump_consent_service.dart:105:      await ConsentForm.loadAndShowConsentFormIfRequired(
lib/ads/ad_gate.dart:15:  /// Game-screen entries counted today.
test/ui/screens/game_header_test.dart:31:/// A minimal 4×4 puzzle used only to satisfy the Game constructor — the cells
test/ui/screens/game_header_test.dart:43:/// A [GameController] subclass that immediately returns a preset [Game] from
test/ui/screens/game_header_test.dart:44:/// [build], bypassing the SharedPreferences load.  Tracks [deactivate] and
test/ui/screens/game_header_test.dart:47:  final Game _preset;
test/ui/screens/game_header_test.dart:55:  Game? build() => _preset;
test/ui/screens/game_header_test.dart:123:    final longGame = Game(
test/ui/screens/game_header_test.dart:170:      final scaledGame = Game(
test/ui/screens/game_header_test.dart:217:      final activeGame = Game(
test/ui/screens/game_header_test.dart:285:      final completedGame = Game(
test/ui/screens/game_header_test.dart:334:      final game = Game(
test/ui/screens/game_header_test.dart:371:      final game = Game(
test/ui/screens/game_header_test.dart:404:      final game = Game(
test/ui/screens/game_header_test.dart:433:      final game = Game(
test/ui/screens/game_header_test.dart:461:      final game = Game(
lib/ads/iap_service.dart:229:      // Build the receipt payload.
lib/ads/iap_service.dart:299:  Future<void> restorePurchases() async {
lib/ads/iap_service.dart:302:      await InAppPurchase.instance.restorePurchases();
lib/ui/theme/symbol_painter.dart:14:  /// Identifier for analytics / persistence.
lib/ui/state/challenge_providers.dart:12:  final source = await rootBundle.loadString('assets/challenge_pack.json');
lib/ui/theme/sundown_theme.dart:14:  /// Stable id used for persistence ("classic", "dusk", "nocturne", "paper").
test/ui/stats_screen_test.dart:18:    await storage.saveStats([]);
test/ui/stats_screen_test.dart:40:    await storage.saveStats([
lib/ui/widgets/banner_ad_widget.dart:34:  bool _loaded = false;
lib/ui/widgets/banner_ad_widget.dart:59:    _loaded = false;
lib/ui/widgets/banner_ad_widget.dart:95:    final loader = ref.read(bannerAdLoaderProvider);
lib/ui/widgets/banner_ad_widget.dart:96:    final ad = loader(
lib/ui/widgets/banner_ad_widget.dart:106:            _loaded = true;
lib/ui/widgets/banner_ad_widget.dart:136:      if (!mounted || _loaded || _showFallback) return;
lib/ui/widgets/banner_ad_widget.dart:174:      '${DateTime.now().microsecondsSinceEpoch}-$kind-${_loaded ? 'live' : 'fallback'}';
lib/ui/widgets/banner_ad_widget.dart:226:    if (_loaded && _ad != null) {
lib/ui/state/custom_symbol_image_normalizer.dart:108:Future<CustomSymbolCropPreview> loadCustomSymbolCropPreviewFile(String path) {
lib/ui/state/custom_symbol_image_normalizer.dart:109:  return compute(_loadCustomSymbolCropPreviewFileInIsolate, path);
lib/ui/state/custom_symbol_image_normalizer.dart:134:CustomSymbolCropPreview _loadCustomSymbolCropPreviewFileInIsolate(String path) {
test/ui/state/custom_symbol_image_normalizer_test.dart:152:  test('loads an orientation-baked preview for the crop screen', () async {
test/ui/state/custom_symbol_image_normalizer_test.dart:159:    final preview = await loadCustomSymbolCropPreviewFile(file.path);
lib/ui/screens/settings_screen.dart:223:            loading: () => const _BuyButtonSkeleton(),
lib/ui/screens/settings_screen.dart:244:                  onPressed: () => _restore(context, ref),
lib/ui/screens/settings_screen.dart:283:  Future<void> _restore(BuildContext context, WidgetRef ref) async {
lib/ui/screens/settings_screen.dart:284:    await ref.read(iapServiceProvider).restorePurchases();
lib/ui/screens/settings_screen.dart:413:        loading: () => const Center(child: CircularProgressIndicator()),
lib/ui/screens/settings_screen.dart:415:            Text('Custom images could not be loaded.', style: t.body),
lib/ui/screens/settings_screen.dart:632:      await notifier.save(slot, image.path, crop: crop);
lib/ui/screens/settings_screen.dart:641:        const SnackBar(content: Text('Image could not be saved.')),
lib/ui/screens/settings_screen.dart:676:    _previewFuture = loadCustomSymbolCropPreviewFile(widget.sourcePath);
lib/ui/screens/settings_screen.dart:837:    // Round (matching x/y below) so the saved tile and on-screen preview agree
lib/ui/screens/challenge_pack_screen.dart:32:        loading: () => const Center(child: CircularProgressIndicator()),
lib/ui/screens/challenge_pack_screen.dart:34:          child: Text('Challenge pack could not be loaded.', style: t.body),
lib/ui/screens/play_by_code_dialog.dart:143:        _error = 'Could not load: ${_errorMessage(e)}';
lib/ui/state/providers.dart:49:// Reconstructs a saved Game from its raw JSON string in an isolate so that
lib/ui/state/providers.dart:52:Game? _gameFromRawJsonInIsolate(String raw) {
lib/ui/state/providers.dart:54:    return GameCodec.fromJson(jsonDecode(raw) as Map<String, dynamic>);
lib/ui/state/providers.dart:69:/// premium-theme unlocking. Synchronous so UI gates read it without a loading
lib/ui/state/providers.dart:85:  /// Recompute after a promo-code redemption or a restore-purchases call.
lib/ui/state/providers.dart:103:  /// Toggle developer mode on or off and persist the choice.
lib/ui/state/providers.dart:171:        loading: () => NetworkStatus.online,
lib/ui/state/providers.dart:184:  List<AdEventRecord> build() => ref.read(storageProvider).loadAdEventQueue();
lib/ui/state/providers.dart:211:/// Banner loader seam. The default implementation uses Google Mobile Ads, but
lib/ui/state/providers.dart:222:        ad.load();
lib/ui/state/providers.dart:299:  Future<void> save(
lib/ui/state/providers.dart:341:      throw const CustomSymbolImageException('Image could not be saved.');
lib/ui/state/providers.dart:348:    await save(slot, image.path);
lib/ui/state/providers.dart:395:  List<SolvedRecord> build() => ref.read(storageProvider).loadStats();
lib/ui/state/providers.dart:400:    await ref.read(storageProvider).saveStats(next);
lib/ui/state/providers.dart:417:      ref.read(storageProvider).loadAchievements();
lib/ui/state/providers.dart:436:    await ref.read(storageProvider).saveAchievements(next);
lib/ui/state/providers.dart:527:/// Mirrors [TutorialSeenController]: a one-way flag persisted to storage.
lib/ui/state/providers.dart:545:class GameController extends Notifier<Game?> {
lib/ui/state/providers.dart:567:  Game? build() {
lib/ui/state/providers.dart:573:    // the Game (Generator.fromCode) can take several seconds for expert 12×12 and
lib/ui/state/providers.dart:576:    if (raw != null) unawaited(_loadFromRaw(raw));
lib/ui/state/providers.dart:580:  Future<void> _loadFromRaw(String raw) async {
lib/ui/state/providers.dart:585:      unawaited(storage.saveCurrentGame(null));
lib/ui/state/providers.dart:589:    // loading — in that case state is already non-null and we must not clobber it.
lib/ui/state/providers.dart:623:        unawaited(ref.read(storageProvider).saveCurrentGame(snapped));
lib/ui/state/providers.dart:633:    final game = Game.fresh(puzzle);
lib/ui/state/providers.dart:639:    await ref.read(storageProvider).saveCurrentGame(game);
lib/ui/state/providers.dart:655:    final game = Game.fresh(puzzle);
lib/ui/state/providers.dart:661:    await ref.read(storageProvider).saveCurrentGame(game);
lib/ui/state/providers.dart:745:    unawaited(ref.read(storageProvider).saveCurrentGame(reset));
lib/ui/state/providers.dart:792:    unawaited(ref.read(storageProvider).saveCurrentGame(null));
lib/ui/state/providers.dart:797:  Future<void> _completeIfDone(Game prev, Game next) async {
lib/ui/state/providers.dart:827:  void _startTickingFrom(Game g) {
lib/ui/state/providers.dart:847:  int _currentElapsedMs(Game g) {
lib/ui/state/providers.dart:854:  void _mutate(Game Function(Game) f, {void Function(Game)? beforeEmit}) {
lib/ui/state/providers.dart:861:    unawaited(ref.read(storageProvider).saveCurrentGame(next));
lib/ui/state/providers.dart:866:final gameProvider = NotifierProvider<GameController, Game?>(
lib/ui/state/providers.dart:895:bool _boardOk(Game g) {
lib/ui/state/providers.dart:923:Set<(int, int)> _firstViolationCells(Game g) =>
lib/ui/screens/premium_upsell_dialog.dart:67:          loading: () =>
lib/ui/screens/game_screen.dart:22:/// Main play screen. Drives a single [Game] through to completion.
lib/ui/screens/game_screen.dart:191:    ref.listen<Game?>(gameProvider, (previous, next) {
lib/ui/screens/game_screen.dart:536:void _showSolvedDialog(BuildContext context, WidgetRef ref, Game game) {
lib/ui/screens/game_screen.dart:667:        category: AchievementCategory.persistence,
lib/ui/screens/achievements_screen.dart:268:  AchievementCategory.persistence => 'Persistence & fun',
lib/ui/screens/watch_ad_dialog.dart:302:          loading: () => _PremiumButtons(


# rg -n 'gameProvider|GameController|currentGame|StorageService|GameScreen'
lib/domain/premium/premium_service.dart:8:  final StorageService storage;
test/domain/premium_service_test.dart:10:    final storage = await StorageService.open();
test/domain/premium_service_test.dart:25:    final storage = await StorageService.open();
test/domain/premium_service_test.dart:37:    final storage = await StorageService.open();
test/widget_smoke_test.dart:18:    final storage = await StorageService.open();
test/widget_smoke_test.dart:43:    final storage = await StorageService.open();
lib/main.dart:46:  final storage = await StorageService.open();
lib/main.dart:110:  // Game-timer pause/resume on app background is handled by GameScreen itself
lib/storage/storage_service.dart:15:class StorageService {
lib/storage/storage_service.dart:16:  static const _kCurrentGame = 'sundown.currentGame';
lib/storage/storage_service.dart:48:  StorageService._(this._prefs);
lib/storage/storage_service.dart:50:  static Future<StorageService> open() async {
lib/storage/storage_service.dart:51:    return StorageService._(await SharedPreferences.getInstance());
test/ui/stats_screen_test.dart:17:    final storage = await StorageService.open();
test/ui/stats_screen_test.dart:39:    final storage = await StorageService.open();
test/ui/widgets/board_widget_semantics_test.dart:16:Future<StorageService> _storage() async {
test/ui/widgets/board_widget_semantics_test.dart:18:  return StorageService.open();
lib/ads/iap_service.dart:64:  Future<void> initialize(StorageService storage) async {
lib/ads/iap_service.dart:145:    StorageService storage,
lib/ads/iap_service.dart:222:    StorageService storage,
test/ui/widgets/banner_ad_widget_test.dart:42:  Future<void> initialize(StorageService storage) async {}
test/ui/widgets/banner_ad_widget_test.dart:61:Future<StorageService> _storage() async {
test/ui/widgets/banner_ad_widget_test.dart:63:  return StorageService.open();
test/ui/screens/settings_screen_test.dart:29:  Future<StorageService> storage() async {
test/ui/screens/settings_screen_test.dart:31:    return StorageService.open();
test/ui/state/providers_test.dart:17:    final storage = await StorageService.open();
test/ui/screens/challenge_pack_screen_test.dart:27:    final storage = await StorageService.open();
test/ui/screens/challenge_pack_screen_test.dart:62:          child: const MaterialApp(home: ChallengeGameScreen(puzzle: puzzle)),
test/ads/ad_gate_test.dart:25:    final storage = await StorageService.open();
lib/ui/screens/play_by_code_dialog.dart:133:      await ref.read(gameProvider.notifier).startFromCode(normalized);
lib/ui/screens/play_by_code_dialog.dart:138:      ).push(MaterialPageRoute(builder: (_) => const GameScreen()));
test/ads/ad_event_queue_test.dart:23:  Future<StorageService> storageWith(Map<String, Object> prefs) async {
test/ads/ad_event_queue_test.dart:25:    return StorageService.open();
lib/ui/screens/home_screen.dart:50:    final inProgress = ref.watch(gameProvider);
lib/ui/screens/home_screen.dart:101:              ).push(MaterialPageRoute(builder: (_) => const NewGameScreen())),
lib/ui/screens/home_screen.dart:404:    final g = ref.watch(gameProvider)!;
lib/ui/screens/home_screen.dart:408:      ).push(MaterialPageRoute(builder: (_) => const GameScreen())),
test/ui/screens/tutorial_screen_test.dart:18:  StorageService storage,
test/ui/screens/tutorial_screen_test.dart:32:Future<StorageService> storageWith(Map<String, Object> values) async {
test/ui/screens/tutorial_screen_test.dart:34:  return StorageService.open();
test/ui/screens/game_header_test.dart:1:// Tests for the _Header widget rendered inside GameScreen.
test/ui/screens/game_header_test.dart:43:/// A [GameController] subclass that immediately returns a preset [Game] from
test/ui/screens/game_header_test.dart:46:class _FakeGameController extends GameController {
test/ui/screens/game_header_test.dart:52:  _FakeGameController(this._preset);
test/ui/screens/game_header_test.dart:73:/// Pumps [GameScreen] inside a [ProviderScope] that injects the given fake
test/ui/screens/game_header_test.dart:74:/// controller and a mocked [StorageService].  Returns the controller so tests
test/ui/screens/game_header_test.dart:76:Future<_FakeGameController> _pumpGameScreen(
test/ui/screens/game_header_test.dart:78:  required _FakeGameController controller,
test/ui/screens/game_header_test.dart:83:  final storage = await StorageService.open();
test/ui/screens/game_header_test.dart:96:          gameProvider.overrideWith(() => controller),
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
test/ui/screens/watch_ad_dialog_test.dart:55:Future<StorageService> _storage() async {
test/ui/screens/watch_ad_dialog_test.dart:57:  return StorageService.open();
test/ui/screens/watch_ad_dialog_test.dart:63:  required StorageService storage,
test/ui/screens/watch_ad_dialog_test.dart:189:    final storage = await StorageService.open();
lib/ui/screens/new_game_screen.dart:11:class NewGameScreen extends ConsumerStatefulWidget {
lib/ui/screens/new_game_screen.dart:12:  const NewGameScreen({super.key});
lib/ui/screens/new_game_screen.dart:14:  ConsumerState<NewGameScreen> createState() => _NewGameScreenState();
lib/ui/screens/new_game_screen.dart:17:class _NewGameScreenState extends ConsumerState<NewGameScreen> {
lib/ui/screens/new_game_screen.dart:141:      await ref.read(gameProvider.notifier).start(size, d);
lib/ui/screens/new_game_screen.dart:147:      ).pushReplacement(MaterialPageRoute(builder: (_) => const GameScreen()));
lib/ui/screens/challenge_pack_screen.dart:148:        MaterialPageRoute(builder: (_) => ChallengeGameScreen(puzzle: puzzle)),
lib/ui/screens/challenge_game_screen.dart:10:class ChallengeGameScreen extends ConsumerStatefulWidget {
lib/ui/screens/challenge_game_screen.dart:12:  const ChallengeGameScreen({super.key, required this.puzzle});
lib/ui/screens/challenge_game_screen.dart:15:  ConsumerState<ChallengeGameScreen> createState() =>
lib/ui/screens/challenge_game_screen.dart:16:      _ChallengeGameScreenState();
lib/ui/screens/challenge_game_screen.dart:19:class _ChallengeGameScreenState extends ConsumerState<ChallengeGameScreen> {
lib/ui/screens/stats_screen.dart:75:                    MaterialPageRoute(builder: (_) => const NewGameScreen()),
lib/ui/screens/game_screen.dart:23:class GameScreen extends ConsumerStatefulWidget {
lib/ui/screens/game_screen.dart:24:  const GameScreen({super.key});
lib/ui/screens/game_screen.dart:27:  ConsumerState<GameScreen> createState() => _GameScreenState();
lib/ui/screens/game_screen.dart:30:class _GameScreenState extends ConsumerState<GameScreen>
lib/ui/screens/game_screen.dart:46:  late final GameController _gameController;
lib/ui/screens/game_screen.dart:51:    _gameController = ref.read(gameProvider.notifier);
lib/ui/screens/game_screen.dart:54:    // this observer only exists while the GameScreen is mounted.
lib/ui/screens/game_screen.dart:109:    final g = ref.read(gameProvider);
lib/ui/screens/game_screen.dart:140:      ref.read(gameProvider.notifier).recordHint();
lib/ui/screens/game_screen.dart:148:        ref.read(gameProvider.notifier).recordHint();
lib/ui/screens/game_screen.dart:152:        ref.read(gameProvider.notifier).revealHint(fresh);
lib/ui/screens/game_screen.dart:174:    ref.read(gameProvider.notifier).tap(r, c);
lib/ui/screens/game_screen.dart:191:    ref.listen<Game?>(gameProvider, (previous, next) {
lib/ui/screens/game_screen.dart:204:    final hasGame = ref.watch(gameProvider.select((g) => g != null));
lib/ui/screens/game_screen.dart:263:      gameProvider.select(
lib/ui/screens/game_screen.dart:277:      ref.watch(gameProvider.select((g) => g?.elapsedMs ?? 0)),
lib/ui/screens/game_screen.dart:310:                  final controller = ref.read(gameProvider.notifier);
lib/ui/screens/game_screen.dart:396:      gameProvider.select(
lib/ui/screens/game_screen.dart:461:      gameProvider.select(
lib/ui/screens/game_screen.dart:487:                : () => ref.read(gameProvider.notifier).undo(),
lib/ui/screens/game_screen.dart:494:                : () => ref.read(gameProvider.notifier).restart(),
lib/ui/screens/game_screen.dart:617:                                        .read(gameProvider.notifier)
lib/ui/state/providers.dart:39:final storageProvider = Provider<StorageService>(
lib/ui/state/providers.dart:545:class GameController extends Notifier<Game?> {
lib/ui/state/providers.dart:866:final gameProvider = NotifierProvider<GameController, Game?>(
lib/ui/state/providers.dart:867:  GameController.new,
lib/ui/state/providers.dart:883:  final g = ref.watch(gameProvider);
lib/ui/state/providers.dart:890:  final g = ref.watch(gameProvider);
lib/ui/state/providers.dart:906:  ref.watch(gameProvider);
lib/ui/state/providers.dart:907:  return ref.read(gameProvider.notifier).graceCells;
lib/ui/state/providers.dart:912:    gameProvider.select(


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

# Read lib/storage/storage_service.dart
```dart
import 'dart:convert';

import 'package:shared_preferences/shared_preferences.dart';

import '../domain/game/game.dart';
import '../domain/game/game_codec.dart';
import '../domain/game/stats.dart';
import '../domain/achievement.dart';
import '../ads/ad_event_queue.dart';

/// Thin wrapper around [SharedPreferences] that the rest of the app uses.
///
/// Schema is JSON in named keys — small enough for v1; can move to drift
/// later if stats history grows large.
class StorageService {
  static const _kCurrentGame = 'sundown.currentGame';
  static const _kStats = 'sundown.stats';
  static const _kThemeId = 'sundown.themeId';
  static const _kPreferredSize = 'sundown.preferredSize';
  static const _kPreferredDifficulty = 'sundown.preferredDifficulty';
  static const _kCustomSunImagePath = 'sundown.customSunImagePath';
  static const _kCustomMoonImagePath = 'sundown.customMoonImagePath';
  static const _kAchievements = 'sundown.achievements';

  // Ad-gate bookkeeping. `adPlayDay` is the local calendar day (yyyy-mm-dd)
  // the counts below belong to; both reset when a new day rolls over.
  static const _kAdPlayDay = 'sundown.adPlayDay';
  static const _kAdPlayCount = 'sundown.adPlayCount';
  static const _kAdBonusPlays = 'sundown.adBonusPlays';
  static const _kOfflineRewardDay = 'sundown.offlineRewardDay';
  static const _kOfflineRewardGranted = 'sundown.offlineRewardGranted';
  // "Unlimited Play" entitlement — set true after the IAP is purchased. Grants
  // the same premium tier as redeeming a promo code (no ads, no play gate).
  static const _kUnlimitedPlay = 'sundown.unlimitedPlay';
  // Internal-only ad kill switch (no user-facing UI). Lets the codebase suppress
  // ads during testing. Defaults to on.
  static const _kShowAds = 'sundown.showAds';
  // Whether the first-run tutorial has been shown (completed or skipped).
  static const _kTutorialSeen = 'sundown.tutorialSeen';
  // Whether the home-screen premium teaser card has been permanently dismissed.
  static const _kPremiumTeaserDismissed = 'sundown.premiumTeaserDismissed';
  static const _kAdEventQueue = 'sundown.adEventQueue';
  static const _kDeveloperTestingMode = 'sundown.developerTestingMode';
  static const _kDeveloperModeUnlocked = 'sundown.developerModeUnlocked';
  static const _kChallengeCompleted = 'sundown.challengeCompleted';

  final SharedPreferences _prefs;
  StorageService._(this._prefs);

  static Future<StorageService> open() async {
    return StorageService._(await SharedPreferences.getInstance());
  }

  // ─── current game ─────────────────────────────────────────────────────
  Future<void> saveCurrentGame(Game? g) async {
    if (g == null) {
      await _prefs.remove(_kCurrentGame);
      return;
    }
    await _prefs.setString(_kCurrentGame, jsonEncode(GameCodec.toJson(g)));
  }

  /// Raw JSON string for the saved game, or null. This is a fast SharedPreferences
  /// lookup — callers that need a [Game] should reconstruct it in an isolate via
  /// [GameCodec.fromJson] rather than calling [loadCurrentGame] on the main thread.
  String? rawCurrentGame() => _prefs.getString(_kCurrentGame);

  /// Synchronous convenience loader. Avoid on the main thread for large puzzles;
  /// prefer [rawCurrentGame] + compute instead.
  Game? loadCurrentGame() {
    final s = rawCurrentGame();
    if (s == null) return null;
    try {
      return GameCodec.fromJson(jsonDecode(s) as Map<String, dynamic>);
    } catch (_) {
      // corrupt blob — drop it
      _prefs.remove(_kCurrentGame);
      return null;
    }
  }

  // ─── stats ────────────────────────────────────────────────────────────
  List<SolvedRecord> loadStats() {
    final s = _prefs.getString(_kStats);
    if (s == null) return [];
    try {
      final raw = jsonDecode(s) as List;
      return [
        for (final r in raw.cast<Map>())
          SolvedRecord.fromJson(r.cast<String, dynamic>()),
      ];
    } catch (_) {
      return [];
    }
  }

  Future<void> saveStats(List<SolvedRecord> records) async {
    await _prefs.setString(
      _kStats,
      jsonEncode([for (final r in records) r.toJson()]),
    );
  }

  Future<void> appendSolve(SolvedRecord rec) async {
    final list = loadStats()..add(rec);
    await saveStats(list);
  }

  // ─── ad event queue ───────────────────────────────────────────────────
  List<AdEventRecord> loadAdEventQueue() {
    final s = _prefs.getString(_kAdEventQueue);
    if (s == null) return [];
    try {
      final raw = jsonDecode(s) as List;
      return [
        for (final r in raw.cast<Map>())
          AdEventRecord.fromJson(r.cast<String, dynamic>()),
      ];
    } catch (_) {
      _prefs.remove(_kAdEventQueue);
      return [];
    }
  }

  Future<void> _saveAdEventQueue(List<AdEventRecord> events) async {
    await _prefs.setString(
      _kAdEventQueue,
      jsonEncode([for (final event in events) event.toJson()]),
    );
  }

  Future<void> appendAdEvent(AdEventRecord event) async {
    final list = loadAdEventQueue()..add(event);
    await _saveAdEventQueue(list);
  }

  Future<void> clearAcknowledgedAdEvents(
    Iterable<String> acknowledgedIds,
  ) async {
    final ids = acknowledgedIds.toSet();
    if (ids.isEmpty) return;
    final next = loadAdEventQueue()
        .where((event) => !ids.contains(event.id))
        .toList(growable: false);
    if (next.isEmpty) {
      await _prefs.remove(_kAdEventQueue);
      return;
    }
    await _saveAdEventQueue(next);
  }

  Future<void> clearAdEventQueue() => _prefs.remove(_kAdEventQueue);

  // ─── achievements ─────────────────────────────────────────────────────
  List<UnlockedAchievement> loadAchievements() {
    final s = _prefs.getString(_kAchievements);
    if (s == null) return [];
    try {
      final raw = jsonDecode(s) as List;
      return [
        for (final r in raw.cast<Map>())
          UnlockedAchievement.fromJson(r.cast<String, dynamic>()),
      ];
    } catch (_) {
      return [];
    }
  }

  Future<void> saveAchievements(List<UnlockedAchievement> achievements) async {
    await _prefs.setString(
      _kAchievements,
      jsonEncode([for (final a in achievements) a.toJson()]),
    );
  }

  // ─── settings ─────────────────────────────────────────────────────────
  String? get themeId => _prefs.getString(_kThemeId);
  Future<void> setThemeId(String id) => _prefs.setString(_kThemeId, id);

  int get preferredSize => _prefs.getInt(_kPreferredSize) ?? 6;
  Future<void> setPreferredSize(int n) => _prefs.setInt(_kPreferredSize, n);

  String? get preferredDifficulty => _prefs.getString(_kPreferredDifficulty);
  Future<void> setPreferredDifficulty(String code) =>
      _prefs.setString(_kPreferredDifficulty, code);

  String? get customSunImagePath => _prefs.getString(_kCustomSunImagePath);
  Future<void> setCustomSunImagePath(String path) =>
      _prefs.setString(_kCustomSunImagePath, path);
  Future<void> clearCustomSunImagePath() => _prefs.remove(_kCustomSunImagePath);

  String? get customMoonImagePath => _prefs.getString(_kCustomMoonImagePath);
  Future<void> setCustomMoonImagePath(String path) =>
      _prefs.setString(_kCustomMoonImagePath, path);
  Future<void> clearCustomMoonImagePath() =>
      _prefs.remove(_kCustomMoonImagePath);

  // ─── ad gate ──────────────────────────────────────────────────────────
  String? get adPlayDay => _prefs.getString(_kAdPlayDay);
  Future<void> setAdPlayDay(String day) => _prefs.setString(_kAdPlayDay, day);

  int get adPlayCount => _prefs.getInt(_kAdPlayCount) ?? 0;
  Future<void> setAdPlayCount(int n) => _prefs.setInt(_kAdPlayCount, n);

  int get adBonusPlays => _prefs.getInt(_kAdBonusPlays) ?? 0;
  Future<void> setAdBonusPlays(int n) => _prefs.setInt(_kAdBonusPlays, n);

  String? get offlineRewardDay => _prefs.getString(_kOfflineRewardDay);
  Future<void> setOfflineRewardDay(String day) =>
      _prefs.setString(_kOfflineRewardDay, day);

  bool get offlineRewardGranted =>
      _prefs.getBool(_kOfflineRewardGranted) ?? false;
  Future<void> setOfflineRewardGranted(bool v) =>
      _prefs.setBool(_kOfflineRewardGranted, v);

  /// True once the player has purchased "Unlimited Play" (removes ads, removes
  /// the play gate, and unlocks premium themes). Same tier as a promo code.
  bool get unlimitedPlay => _prefs.getBool(_kUnlimitedPlay) ?? false;
  Future<void> setUnlimitedPlay(bool v) => _prefs.setBool(_kUnlimitedPlay, v);

  /// Internal-only ad kill switch (no user-facing UI). Default: true (ads on).
  bool? getShowAds() => _prefs.getBool(_kShowAds);
  Future<void> setShowAds(bool v) => _prefs.setBool(_kShowAds, v);

  // ─── tutorial ─────────────────────────────────────────────────────────
  /// True once the first-run tutorial has been completed or skipped.
  bool get tutorialSeen => _prefs.getBool(_kTutorialSeen) ?? false;
  Future<void> setTutorialSeen(bool v) => _prefs.setBool(_kTutorialSeen, v);

  /// True once the player has dismissed the home-screen premium teaser card.
  /// Hides it permanently so the upsell never nags after a deliberate dismiss.
  bool get premiumTeaserDismissed =>
      _prefs.getBool(_kPremiumTeaserDismissed) ?? false;
  Future<void> setPremiumTeaserDismissed(bool v) =>
      _prefs.setBool(_kPremiumTeaserDismissed, v);

  // ─── developer challenge pack ────────────────────────────────────────
  bool get developerTestingMode =>
      _prefs.getBool(_kDeveloperTestingMode) ?? false;
  Future<void> setDeveloperTestingMode(bool value) =>
      _prefs.setBool(_kDeveloperTestingMode, value);

  /// Whether the developer-testing password (GREENBEAN) has ever been entered.
  /// Once unlocked, the Settings toggle stays available even when developer
  /// mode is switched off, so the user can turn it back on without re-entering
  /// the code.
  bool get developerModeUnlocked =>
      _prefs.getBool(_kDeveloperModeUnlocked) ?? false;
  Future<void> setDeveloperModeUnlocked(bool value) =>
      _prefs.setBool(_kDeveloperModeUnlocked, value);

  Set<String> get challengeCompleted =>
      (_prefs.getStringList(_kChallengeCompleted) ?? const <String>[]).toSet();
  Future<void> setChallengeCompleted(Set<String> ids) =>
      _prefs.setStringList(_kChallengeCompleted, ids.toList()..sort());

  // ─── generic key-value ─────────────────────────────────────────────────
  /// Generic string getter (used by premium token storage).
  String? getString(String key) => _prefs.getString(key);

  /// Generic string setter (used by premium token storage).
  Future<void> setString(String key, String value) =>
      _prefs.setString(key, value);

  /// Generic key remover (used by premium token storage).
  Future<void> remove(String key) => _prefs.remove(key);
}

```

# Read lib/ui/screens/new_game_screen.dart
```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';

import '../../domain/difficulty.dart';
import '../state/providers.dart';
import '../theme/sundown_theme.dart';
import '../widgets/control_button.dart';
import 'game_screen.dart';
import 'premium_upsell_dialog.dart';

class NewGameScreen extends ConsumerStatefulWidget {
  const NewGameScreen({super.key});
  @override
  ConsumerState<NewGameScreen> createState() => _NewGameScreenState();
}

class _NewGameScreenState extends ConsumerState<NewGameScreen> {
  bool _busy = false;

  @override
  Widget build(BuildContext context) {
    final t = context.tokens;
    final size = ref.watch(preferredSizeProvider);
    final diff = ref.watch(preferredDifficultyProvider);
    final premium = ref.watch(premiumProvider);
    return Scaffold(
      backgroundColor: t.background,
      appBar: AppBar(
        backgroundColor: t.background,
        foregroundColor: t.textPrimary,
        title: Text('New Puzzle', style: t.title),
        elevation: 0,
      ),
      body: SafeArea(
        child: Padding(
          padding: EdgeInsets.all(t.screenPadding),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.stretch,
            children: [
              Text('Grid size', style: t.label),
              const SizedBox(height: 4),
              Text(
                'Smaller grids finish faster; larger ones add more of a stretch.',
                style: t.label.copyWith(color: t.textMuted),
              ),
              const SizedBox(height: 8),
              Wrap(
                spacing: 8,
                children: [
                  for (final n in [6, 8, 10, 12])
                    _Choice<int>(
                      value: n,
                      selected: size,
                      label: '$n × $n',
                      onTap: (v) =>
                          ref.read(preferredSizeProvider.notifier).set(v),
                    ),
                ],
              ),
              const SizedBox(height: 24),
              Text('Difficulty', style: t.label),
              const SizedBox(height: 4),
              Text(
                'Expert is a Premium feature.',
                style: t.label.copyWith(color: t.textMuted),
              ),
              const SizedBox(height: 8),
              Row(
                children: [
                  for (var i = 0; i < Difficulty.values.length; i++) ...[
                    if (i > 0) const SizedBox(width: 8),
                    Expanded(
                      child: () {
                        final d = Difficulty.values[i];
                        final locked = d == Difficulty.expert && !premium;
                        return _Choice<Difficulty>(
                          value: d,
                          selected: diff,
                          label: _diffName(d),
                          fill: true,
                          locked: locked,
                          semanticsLabel: locked
                              ? '${_diffName(d)}, Premium feature'
                              : _diffName(d),
                          onTap: (v) {
                            if (locked) {
                              _promptExpertPremium();
                            } else {
                              ref
                                  .read(preferredDifficultyProvider.notifier)
                                  .set(v);
                            }
                          },
                        );
                      }(),
                    ),
                  ],
                ],
              ),
              const Spacer(),
              if (_busy)
                Padding(
                  padding: const EdgeInsets.all(20),
                  child: Column(
                    children: [
                      CircularProgressIndicator(color: t.accent),
                      const SizedBox(height: 12),
                      Text('Generating puzzle…', style: t.body),
                      Text(
                        '${size == 12
                            ? "12×12"
                            : size == 10
                            ? "10×10"
                            : "this"} can take a moment',
                        style: t.label,
                      ),
                    ],
                  ),
                ),
              Center(
                child: ControlButton(
                  icon: Icons.play_arrow_rounded,
                  label: 'Start',
                  primary: true,
                  onPressed: _busy ? null : () => _start(size, diff),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }

  Future<void> _start(int size, Difficulty d) async {
    setState(() => _busy = true);
    try {
      // Run in microtask so spinner paints first; generator is sync but can
      // take several hundred ms at 10×10/12×12.
      await Future<void>.delayed(const Duration(milliseconds: 16));
      await ref.read(gameProvider.notifier).start(size, d);
      await ref.read(storageProvider).setPreferredSize(size);
      await ref.read(storageProvider).setPreferredDifficulty(d.code);
      if (!mounted) return;
      Navigator.of(
        context,
      ).pushReplacement(MaterialPageRoute(builder: (_) => const GameScreen()));
    } catch (e) {
      if (!mounted) return;
      setState(() => _busy = false);
      ScaffoldMessenger.of(
        context,
      ).showSnackBar(SnackBar(content: Text('Could not generate: $e')));
    }
  }

  /// Tapping a locked Expert chip opens the Premium upsell so the player can
  /// buy Unlimited Play (or redeem a code) without leaving the New Puzzle flow.
  Future<void> _promptExpertPremium() {
    return PremiumUpsellDialog.show(
      context,
      title: 'Expert is Premium',
      message:
          'Unlock Expert puzzles, all premium themes, custom symbols, and '
          'unlimited play with a one-time purchase.',
    );
  }
}

class _Choice<T> extends StatelessWidget {
  final T value;
  final T selected;
  final String label;
  final String? semanticsLabel;

  /// Fill the available width (for an equal-width row) instead of sizing to
  /// the label. Content is centered and scaled down to fit on narrow screens.
  final bool fill;

  /// Show a Premium gem badge (e.g. locked Expert for non-premium players).
  final bool locked;
  final void Function(T) onTap;
  const _Choice({
    required this.value,
    required this.selected,
    required this.label,
    this.semanticsLabel,
    this.fill = false,
    this.locked = false,
    required this.onTap,
  });

  @override
  Widget build(BuildContext context) {
    final t = context.tokens;
    final on = value == selected;
    final content = Row(
      mainAxisSize: MainAxisSize.min,
      children: [
        Text(
          label,
          maxLines: 1,
          style: t.body.copyWith(
            color: on ? t.background : t.textPrimary,
            fontWeight: FontWeight.w600,
          ),
        ),
        if (locked) ...[
          const SizedBox(width: 5),
          Icon(
            Icons.diamond_rounded,
            size: 14,
            color: on ? t.background : t.accent,
          ),
        ],
      ],
    );
    return Semantics(
      button: true,
      selected: on,
      label: semanticsLabel ?? label,
      child: Material(
        color: Colors.transparent,
        child: InkWell(
          borderRadius: BorderRadius.circular(t.cardRadius * 0.5),
          onTap: () => onTap(value),
          child: AnimatedContainer(
            duration: const Duration(milliseconds: 120),
            alignment: fill ? Alignment.center : null,
            padding: EdgeInsets.symmetric(
              horizontal: fill ? 8 : 16,
              vertical: 12,
            ),
            decoration: BoxDecoration(
              color: on ? t.accent : t.surfaceElevated,
              borderRadius: BorderRadius.circular(t.cardRadius * 0.5),
              border: Border.all(color: on ? t.accent : t.gridLine, width: 1),
            ),
            child: fill
                ? FittedBox(fit: BoxFit.scaleDown, child: content)
                : content,
          ),
        ),
      ),
    );
  }
}

String _diffName(Difficulty d) => switch (d) {
  Difficulty.easy => 'Easy',
  Difficulty.medium => 'Medium',
  Difficulty.hard => 'Hard',
  Difficulty.expert => 'Expert',
};

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

# Read lib/main.dart
```dart
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:flutter/services.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:google_mobile_ads/google_mobile_ads.dart';
import 'package:sentry_flutter/sentry_flutter.dart';

import 'ads/ad_config.dart';
import 'ads/ad_privacy_service.dart';
import 'ads/iap_service.dart';
import 'ads/rewarded_ad_service.dart';
import 'ads/ump_consent_service.dart';
import 'storage/storage_service.dart';
import 'ui/screens/home_screen.dart';
import 'ui/state/providers.dart';
import 'ui/theme/sundown_theme.dart';

Future<void> main() async {
  WidgetsFlutterBinding.ensureInitialized();

  // DSN is injected at build time via --dart-define=SENTRY_DSN=...
  // Empty string = SDK no-ops, so local debug builds don't ship reports.
  const dsn = String.fromEnvironment('SENTRY_DSN');
  if (dsn.isEmpty) {
    debugPrint('Sentry disabled: SENTRY_DSN was not provided.');
  }

  await SentryFlutter.init((options) {
    options.dsn = dsn;
    options.sendDefaultPii = false;
    // We only use Sentry for crash reporting. Disable the
    // perf-tracing / user-interaction features that ship enabled by
    // default — they eat free-tier quota and (in the case of
    // user-interaction breadcrumbs) record every tap, which would
    // contradict the "no behavioral tracking" promise in
    // docs/PRIVACY.md.
    options.tracesSampleRate = 0.0;
    options.enableAutoPerformanceTracing = false;
    options.enableUserInteractionTracing = false;
    options.enableUserInteractionBreadcrumbs = false;
  }, appRunner: _bootstrapAndRunApp);
}

Future<void> _bootstrapAndRunApp() async {
  final storage = await StorageService.open();
  await SystemChrome.setPreferredOrientations(const [
    DeviceOrientation.portraitUp,
    DeviceOrientation.portraitDown,
  ]);
  await SystemChrome.setEnabledSystemUIMode(SystemUiMode.edgeToEdge);
  // Initialize in-app purchases (Android & iOS).
  try {
    await IapService().initialize(storage);
  } catch (e, st) {
    debugPrint('IAP initialization failed: $e');
    await Sentry.captureException(e, stackTrace: st);
  }

  runApp(
    ProviderScope(
      overrides: [storageProvider.overrideWithValue(storage)],
      child: const SundownApp(),
    ),
  );

  // Ads are Android/iOS-only for now (see AdConfig). Start consent and SDK
  // warmup on the next frame so launch is not held up by optional work.
  WidgetsBinding.instance.addPostFrameCallback((_) {
    unawaited(_initializeAds());
  });
}

Future<void> _initializeAds() async {
  if (!AdConfig.supported) return;
  try {
    final personalizedAdsAllowed = await AdPrivacyService.instance
        .requestPersonalizedAdsPermission();
    await MobileAds.instance.setSameAppKeyEnabled(personalizedAdsAllowed);

    final canRequestAds = await UmpConsentService.instance
        .gatherConsent()
        .timeout(
          const Duration(seconds: 5),
          onTimeout: () {
            debugPrint('UMP consent timed out; skipping mobile ads for now.');
            return false;
          },
        );
    if (!canRequestAds) {
      debugPrint('Mobile Ads skipped: UMP consent is not ready.');
      return;
    }
    await MobileAds.instance.initialize();
    RewardedAdService.instance.preload();
  } catch (e, st) {
    debugPrint('Mobile Ads init failed: $e');
    await Sentry.captureException(e, stackTrace: st);
  }
}

class SundownApp extends ConsumerStatefulWidget {
  const SundownApp({super.key});

  @override
  ConsumerState<SundownApp> createState() => _SundownAppState();
}

class _SundownAppState extends ConsumerState<SundownApp> {
  // Game-timer pause/resume on app background is handled by GameScreen itself
  // (via its own WidgetsBindingObserver), so it only ticks while the board is
  // actually on screen — never on the home screen with a saved game.
  @override
  Widget build(BuildContext context) {
    final theme = ref.watch(themeProvider);
    return AnnotatedRegion<SystemUiOverlayStyle>(
      value: _systemOverlayStyleFrom(theme),
      child: SundownThemeScope(
        theme: theme,
        child: MaterialApp(
          title: 'Sundown',
          debugShowCheckedModeBanner: false,
          theme: _materialThemeFrom(theme),
          home: const HomeScreen(),
        ),
      ),
    );
  }

  /// Bridge our tokens to Material — used by AppBar, dialogs, snackbars, etc.
  ThemeData _materialThemeFrom(SundownTheme t) {
    final tk = t.tokens;
    final base = t.isDark
        ? ThemeData.dark(useMaterial3: true)
        : ThemeData.light(useMaterial3: true);
    return base.copyWith(
      scaffoldBackgroundColor: tk.background,
      colorScheme: base.colorScheme.copyWith(
        primary: tk.accent,
        secondary: tk.accent,
        surface: tk.surface,
        error: tk.error,
        onPrimary: tk.background,
        onSurface: tk.textPrimary,
      ),
      textTheme: base.textTheme.apply(
        bodyColor: tk.textPrimary,
        displayColor: tk.textPrimary,
      ),
      dialogTheme: DialogThemeData(
        backgroundColor: tk.surface,
        surfaceTintColor: Colors.transparent,
      ),
      snackBarTheme: SnackBarThemeData(
        backgroundColor: tk.surfaceElevated,
        contentTextStyle: tk.body,
        behavior: SnackBarBehavior.floating,
      ),
      filledButtonTheme: FilledButtonThemeData(
        style: FilledButton.styleFrom(
          backgroundColor: tk.accent,
          foregroundColor: tk.background,
        ),
      ),
      progressIndicatorTheme: ProgressIndicatorThemeData(color: tk.accent),
      appBarTheme: AppBarTheme(systemOverlayStyle: _systemOverlayStyleFrom(t)),
    );
  }

  SystemUiOverlayStyle _systemOverlayStyleFrom(SundownTheme theme) {
    final iconBrightness = theme.isDark ? Brightness.light : Brightness.dark;
    return SystemUiOverlayStyle(
      statusBarIconBrightness: iconBrightness,
      statusBarBrightness: theme.isDark ? Brightness.dark : Brightness.light,
      systemNavigationBarIconBrightness: iconBrightness,
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