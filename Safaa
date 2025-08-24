import 'dart:async';
import 'package:flutter/material.dart';

void main() => runApp(const SafaaApp());

class SafaaApp extends StatelessWidget {
  const SafaaApp({super.key});
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'صفاء',
      debugShowCheckedModeBanner: false,
      theme: ThemeData(
        useMaterial3: true,
        colorSchemeSeed: const Color(0xFF5CC8A1),
        fontFamily: 'Roboto',
      ),
      home: const Directionality(
        textDirection: TextDirection.rtl,
        child: HomePage(),
      ),
    );
  }
}

/// نموذج البيانات مبسط (تخزين مؤقت في الذاكرة)
class ThoughtEntry {
  final String text;
  final String reframed;
  ThoughtEntry({required this.text, required this.reframed});
}

class MoodEntry {
  final String mood; // calm, anxious, angry, sad, happy
  MoodEntry({required this.mood});
}

/// الشاشة الرئيسية
class HomePage extends StatefulWidget {
  const HomePage({super.key});
  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  final List<ThoughtEntry> thoughts = [];
  final List<MoodEntry> moods = [];

  String greeting() {
    final h = DateTime.now().hour;
    if (h < 12) return 'صباح الخير 🌞';
    if (h < 18) return 'مساء الخير 🌤️';
    return 'مساء هادئ 🌙';
  }

  @override
  Widget build(BuildContext context) {
    final tiles = [
      _QuickTile(
        title: 'مفكرة الأفكار',
        subtitle: 'اكتب هاجسًا وأعد صياغته',
        icon: Icons.edit_note,
        color: const Color(0xFF8EC5A1),
        onTap: () => Navigator.push(
            context,
            MaterialPageRoute(
                builder: (_) => JournalPage(
                      thoughts: thoughts,
                      onAdd: (t) => setState(() => thoughts.add(t)),
                    ))),
      ),
      _QuickTile(
        title: 'مؤشر الانفعال',
        subtitle: 'سجّل شعورك الآن',
        icon: Icons.emoji_emotions,
        color: const Color(0xFF7CB2E1),
        onTap: () => Navigator.push(
            context,
            MaterialPageRoute(
                builder: (_) => MoodPage(
                      moods: moods,
                      onAdd: (m) => setState(() => moods.add(m)),
                    ))),
      ),
      _QuickTile(
        title: 'تمارين التنفس',
        subtitle: 'تنفس هادئ لعدة ثوانٍ',
        icon: Icons.self_improvement,
        color: const Color(0xFFB39DDB),
        onTap: () =>
            Navigator.push(context, MaterialPageRoute(builder: (_) => const MindfulnessPage())),
      ),
      _QuickTile(
        title: 'وضع إيقاف التفكير',
        subtitle: 'جلسة هدوء قصيرة',
        icon: Icons.timer,
        color: const Color(0xFFF6A9A9),
        onTap: () =>
            Navigator.push(context, MaterialPageRoute(builder: (_) => const FocusModePage())),
      ),
      _QuickTile(
        title: 'إحصائياتي',
        subtitle: 'عدد الملاحظات والمزاج',
        icon: Icons.insights,
        color: const Color(0xFFFFD166),
        onTap: () =>
            Navigator.push(context, MaterialPageRoute(builder: (_) => StatsPage(thoughts, moods))),
      ),
    ];

    return Scaffold(
      appBar: AppBar(title: const Text('صفاء'), centerTitle: true),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            Text(greeting(), style: const TextStyle(fontSize: 22, fontWeight: FontWeight.w600)),
            const SizedBox(height: 12),
            Expanded(
              child: GridView.builder(
                itemCount: tiles.length,
                gridDelegate: const SliverGridDelegateWithFixedCrossAxisCount(
                    crossAxisCount: 2, mainAxisSpacing: 12, crossAxisSpacing: 12, childAspectRatio: 1.1),
                itemBuilder: (_, i) => tiles[i],
              ),
            ),
          ],
        ),
      ),
    );
  }
}

class _QuickTile extends StatelessWidget {
  final String title, subtitle;
  final IconData icon;
  final Color color;
  final VoidCallback onTap;
  const _QuickTile(
      {required this.title,
      required this.subtitle,
      required this.icon,
      required this.color,
      required this.onTap});

  @override
  Widget build(BuildContext context) {
    return Material(
      color: color.withOpacity(0.25),
      borderRadius: BorderRadius.circular(18),
      child: InkWell(
        onTap: onTap,
        borderRadius: BorderRadius.circular(18),
        child: Container(
          padding: const EdgeInsets.all(16),
          child: Column(
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              CircleAvatar(backgroundColor: color, child: Icon(icon, color: Colors.white)),
              const Spacer(),
              Text(title, style: const TextStyle(fontWeight: FontWeight.w700, fontSize: 16)),
              const SizedBox(height: 6),
              Text(subtitle, style: TextStyle(color: Colors.black.withOpacity(0.7), fontSize: 13)),
            ],
          ),
        ),
      ),
    );
  }
}

/// مفكرة الأفكار
class JournalPage extends StatefulWidget {
  final List<ThoughtEntry> thoughts;
  final void Function(ThoughtEntry) onAdd;
  const JournalPage({required this.thoughts, required this.onAdd, super.key});
  @override
  State<JournalPage> createState() => _JournalPageState();
}

class _JournalPageState extends State<JournalPage> {
  final ctrl = TextEditingController();
  String reframed = '';

  String _reframe(String input) {
    if (input.isEmpty) return '';
    return 'نسخة أكثر توازنًا:\n• ركز على ما يمكنك فعله الآن.\n• خطوة صغيرة أفضل من لا شيء.\n• لاحظ مشاعرك بدون حكم.';
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('مفكرة الأفكار')),
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: [
          TextField(
            controller: ctrl,
            maxLines: 3,
            decoration: InputDecoration(
              hintText: 'اكتب الفكرة أو الهاجس...',
              border: OutlineInputBorder(borderRadius: BorderRadius.circular(12)),
            ),
          ),
          const SizedBox(height: 8),
          Row(
            children: [
              Expanded(
                child: FilledButton.icon(
                  onPressed: () => setState(() => reframed = _reframe(ctrl.text)),
                  icon: const Icon(Icons.autorenew),
                  label: const Text('إعادة الصياغة'),
                ),
              ),
              const SizedBox(width: 8),
              Expanded(
                child: OutlinedButton.icon(
                  onPressed: () {
                    if (ctrl.text.isEmpty) return;
                    final t = ThoughtEntry(text: ctrl.text, reframed: reframed.isEmpty ? _reframe(ctrl.text) : reframed);
                    widget.onAdd(t);
                    ctrl.clear();
                    reframed = '';
                  },
                  icon: const Icon(Icons.save_alt),
                  label: const Text('حفظ'),
                ),
              ),
            ],
          ),
          if (reframed.isNotEmpty)
            Container(
              margin: const EdgeInsets.symmetric(vertical: 12),
              padding: const EdgeInsets.all(14),
              decoration: BoxDecoration(
                color: const Color(0xFFE8F5E9),
                borderRadius: BorderRadius.circular(12),
                border: Border.all(color: const Color(0xFFBDE3CD)),
              ),
              child: Text(reframed),
            ),
          const SizedBox(height: 12),
          const Text('السجل', style: TextStyle(fontWeight: FontWeight.bold)),
          const SizedBox(height: 8),
          ...widget.thoughts.reversed.map((e) => ListTile(title: Text(e.text), subtitle: Text(e.reframed))),
        ],
      ),
    );
  }
}

/// مؤشر الانفعال
class MoodPage extends StatelessWidget {
  final List<MoodEntry> moods;
  final void Function(MoodEntry) onAdd;
  const MoodPage({required this.moods, required this.onAdd, super.key});

  @override
  Widget build(BuildContext context) {
    final moodList = [
      {'key': 'calm', 'label': 'هادئ', 'color': Colors.green},
      {'key': 'anxious', 'label': 'قلق', 'color': Colors.orange},
      {'key': 'sad', 'label': 'حزين', 'color': Colors.blue},
      {'key': 'angry', 'label': 'غاضب', 'color': Colors.red},
      {'key': 'happy', 'label': 'سعيد', 'color': Colors.yellow[700]},
    ];

    return Scaffold(
      appBar: AppBar(title: const Text('مؤشر الانفعال')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(
          children: [
            const Text('كيف تشعر الآن؟', style: TextStyle(fontWeight: FontWeight.bold)),
            const SizedBox(height: 12),
            Wrap(
              spacing: 12,
              runSpacing: 12,
              children: moodList
                  .map((m) => GestureDetector(
                        onTap: () => onAdd(MoodEntry(mood: m['key'] as String)),
                        child: Container(
                          width: 120,
                          padding: const EdgeInsets.all(12),
                          decoration: BoxDecoration(
                            color: (m['color'] as Color).withOpacity(0.22),
                            borderRadius: BorderRadius.circular(14),
                          ),
                          child: Column(
                            children: [
                              CircleAvatar(backgroundColor: m['color'] as Color, child: Text(m['label']![0])),
                              const SizedBox(height: 8),
                              Text(m['label'] as String),
                            ],
                          ),
                        ),
                      ))
                  .toList(),
            ),
          ],
        ),
      ),
    );
  }
}

/// تمارين التنفس (مبسطة)
class MindfulnessPage extends StatefulWidget {
  const MindfulnessPage({super.key});
  @override
  State<MindfulnessPage> createState() => _MindfulnessPageState();
}

class _MindfulnessPageState extends State<MindfulnessPage> with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  bool running = false;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(vsync: this, duration: const Duration(seconds: 6))
      ..addStatusListener((s) {
        if (s == AnimationStatus.completed) _controller.reverse();
        if (s == AnimationStatus.dismissed && running) _controller.forward();
      });
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  void _toggle() => setState(() {
        running = !running;
        if (running) {
          _controller.forward();
        } else {
          _controller.stop();
        }
      });

  @override
  Widget build(BuildContext context) {
    final breath = Tween<double>(begin: 120, end: 220).animate(CurvedAnimation(parent: _controller, curve: Curves.easeInOut));
    return Scaffold(
      appBar: AppBar(title: const Text('تمارين التنفس')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            AnimatedBuilder(
              animation: breath,
              builder: (_, __) => Container(
                width: breath.value,
                height: breath.value,
                decoration: BoxDecoration(shape: BoxShape.circle, color: Colors.teal[200]),
                alignment: Alignment.center,
                child: Text(running ? 'تنفس...' : 'ابدأ'),
              ),
            ),
            const SizedBox(height: 20),
            FilledButton.icon(onPressed: _toggle, icon: Icon(running ? Icons.pause : Icons.play_arrow), label: Text(running ? 'إيقاف' : 'ابدأ')),
          ],
        ),
      ),
    );
  }
}

/// وضع إيقاف التفكير
class FocusModePage extends StatefulWidget {
  const FocusModePage({super.key});
  @override
  State<FocusModePage> createState() => _FocusModePageState();
}

class _FocusModePageState extends State<FocusModePage> {
  int seconds = 600;
  Timer? timer;
  bool running = false;

  void _start() {
    setState(() => running = true);
    timer?.cancel();
    timer = Timer.periodic(const Duration(seconds: 1), (_) {
      if (seconds <= 0) {
        timer?.cancel();
        setState(() => running = false);
      } else {
        setState(() => seconds--);
      }
    });
  }

  void _stop() {
    timer?.cancel();
    setState(() => running = false);
  }

  @override
  void dispose() {
    timer?.cancel();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    final min = (seconds ~/ 60).toString().padLeft(2, '0');
    final sec = (seconds % 60).toString().padLeft(2, '0');
    return Scaffold(
      appBar: AppBar(title: const Text('وضع إيقاف التفكير')),
      body: Padding(
        padding: const EdgeInsets.all(16),
        child: Column(children: [
          const Text('جلسة هدوء قصيرة'),
          const SizedBox(height: 12),
          LinearProgressIndicator(value: 1 - (seconds / 600)),
          const SizedBox(height: 8),
          Text('$min:$sec'),
          const SizedBox(height: 12),
          Row(children: [
            Expanded(child: FilledButton(onPressed: running ? null : _start, child: const Text('ابدأ'))),
            const SizedBox(width: 8),
            Expanded(child: OutlinedButton(onPressed: running ? _stop : null, child: const Text('إيقاف')))
          ])
        ]),
      ),
    );
  }
}

/// إحصائيات مبسطة
class StatsPage extends StatelessWidget {
  final List<ThoughtEntry> thoughts;
  final List<MoodEntry> moods;
  const StatsPage(this.thoughts, this.moods, {super.key});

  @override
  Widget build(BuildContext context) {
    final counts = <String, int>{};
    for (var m in moods) counts[m.mood] = (counts[m.mood] ?? 0) + 1;

    return Scaffold(
      appBar: AppBar(title: const Text('إحصائياتي')),
      body: ListView(padding: const EdgeInsets.all(16), children: [
        Card(child: ListTile(title: const Text('عدد الأفكار'), trailing: Text('${thoughts.length}'))),
        Card(
          child: Padding(
            padding: const EdgeInsets.all(14),
            child: Column(
              crossAxisAlignment: CrossAxisAlignment.start,
              children: [
                const Text('توزيع المزاج', style: TextStyle(fontWeight: FontWeight.bold)),
                ...counts.entries.map((e) => Row(
                      children: [
                        SizedBox(width: 80, child: Text(e.key)),
                        Expanded(child: LinearProgressIndicator(value: e.value / moods.length)),
                        const SizedBox(width: 8),
                        Text('${((e.value / moods.length) * 100).toStringAsFixed(0)}%')
                      ],
                    ))
              ],
            ),
          ),
        ),
      ]),
    );
  }
}
