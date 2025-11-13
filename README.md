# horscope-
import 'package:flutter/material.dart';

void main() => runApp(HoroscopeApp());

class HoroscopeApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Daily Horoscope',
      theme: ThemeData(
        useMaterial3: true,
        colorSchemeSeed: Colors.deepPurple,
      ),
      debugShowCheckedModeBanner: false,
      home: SplashScreen(),
    );
  }
}

// Splash Screen with Fade Animation
class SplashScreen extends StatefulWidget {
  @override
  State<SplashScreen> createState() => _SplashScreenState();
}

class _SplashScreenState extends State<SplashScreen>
    with SingleTickerProviderStateMixin {
  late AnimationController _controller;
  late Animation<double> _animation;

  @override
  void initState() {
    super.initState();
    _controller =
        AnimationController(vsync: this, duration: Duration(seconds: 2));
    _animation = CurvedAnimation(parent: _controller, curve: Curves.easeIn);
    _controller.forward();

    Future.delayed(Duration(seconds: 3), () {
      Navigator.pushReplacement(
          context, MaterialPageRoute(builder: (_) => HomeScreen()));
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.deepPurple,
      body: Center(
        child: FadeTransition(
          opacity: _animation,
          child: Text(
            '✨ Horoscope ✨',
            style: TextStyle(
              color: Colors.white,
              fontSize: 32,
              fontWeight: FontWeight.bold,
            ),
          ),
        ),
      ),
    );
  }
}

// Home Screen - Zodiac Grid
class HomeScreen extends StatelessWidget {
  final List<Map<String, String>> zodiacSigns = [
    {'name': 'Aries', 'emoji': '♈'},
    {'name': 'Taurus', 'emoji': '♉'},
    {'name': 'Gemini', 'emoji': '♊'},
    {'name': 'Cancer', 'emoji': '♋'},
    {'name': 'Leo', 'emoji': '♌'},
    {'name': 'Virgo', 'emoji': '♍'},
    {'name': 'Libra', 'emoji': '♎'},
    {'name': 'Scorpio', 'emoji': '♏'},
    {'name': 'Sagittarius', 'emoji': '♐'},
    {'name': 'Capricorn', 'emoji': '♑'},
    {'name': 'Aquarius', 'emoji': '♒'},
    {'name': 'Pisces', 'emoji': '♓'},
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Choose Your Zodiac'),
        centerTitle: true,
      ),
      body: GridView.builder(
        padding: EdgeInsets.all(16),
        gridDelegate:
            SliverGridDelegateWithFixedCrossAxisCount(crossAxisCount: 3),
        itemCount: zodiacSigns.length,
        itemBuilder: (context, index) {
          final sign = zodiacSigns[index];
          return GestureDetector(
            onTap: () {
              Navigator.push(
                context,
                PageRouteBuilder(
                  pageBuilder: (_, __, ___) =>
                      DetailScreen(sign: sign['name']!, emoji: sign['emoji']!),
                  transitionsBuilder: (_, animation, __, child) {
                    return FadeTransition(opacity: animation, child: child);
                  },
                ),
              );
            },
            child: Card(
              shape: RoundedRectangleBorder(
                  borderRadius: BorderRadius.circular(16)),
              elevation: 4,
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text(sign['emoji']!, style: TextStyle(fontSize: 40)),
                  SizedBox(height: 8),
                  Text(sign['name']!,
                      style:
                          TextStyle(fontWeight: FontWeight.bold, fontSize: 16)),
                ],
              ),
            ),
          );
        },
      ),
    );
  }
}

// Detail Screen
class DetailScreen extends StatelessWidget {
  final String sign;
  final String emoji;

  const DetailScreen({required this.sign, required this.emoji});

  String getHoroscope(String sign) {
    switch (sign) {
      case 'Aries':
        return 'Today is a good day for bold decisions.';
      case 'Taurus':
        return 'Patience will bring you closer to success.';
      case 'Gemini':
        return 'Communication opens new doors today.';
      case 'Cancer':
        return 'Trust your instincts and protect your peace.';
      case 'Leo':
        return 'Your energy inspires others around you.';
      case 'Virgo':
        return 'Focus on details, but don’t lose sight of the big picture.';
      case 'Libra':
        return 'Balance work and personal time wisely.';
      case 'Scorpio':
        return 'Your determination will lead to progress.';
      case 'Sagittarius':
        return 'Adventure awaits — say yes to new things.';
      case 'Capricorn':
        return 'Hard work brings quiet rewards.';
      case 'Aquarius':
        return 'Creative thinking solves a major issue.';
      case 'Pisces':
        return 'Listen to your emotions — they guide you right.';
      default:
        return 'Let the stars guide you today.';
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('$emoji $sign'),
        backgroundColor: Colors.deepPurple,
      ),
      body: Center(
        child: Padding(
          padding: EdgeInsets.all(24),
          child: AnimatedContainer(
            duration: Duration(seconds: 1),
            curve: Curves.easeInOut,
            decoration: BoxDecoration(
              color: Colors.deepPurple.shade50,
              borderRadius: BorderRadius.circular(20),
              boxShadow: [
                BoxShadow(
                    color: Colors.deepPurple.withOpacity(0.2),
                    blurRadius: 10,
                    offset: Offset(0, 5))
              ],
            ),
            padding: EdgeInsets.all(20),
            child: Column(
              mainAxisSize: MainAxisSize.min,
              children: [
                Text(emoji, style: TextStyle(fontSize: 60)),
                SizedBox(height: 10),
                Text(sign,
                    style:
                        TextStyle(fontSize: 28, fontWeight: FontWeight.bold)),
                SizedBox(height: 20),
                Text(
                  getHoroscope(sign),
                  textAlign: TextAlign.center,
                  style: TextStyle(fontSize: 18, color: Colors.black87),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
