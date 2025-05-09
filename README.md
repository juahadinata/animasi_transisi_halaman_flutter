## **Cara Menganimasikan Transisi Halaman pada Aplikasi dengan Flutter**

# Daftar Isi

- [Pendahuluan](#pendahuluan)
- [Instalasi](#instalasi)
- [Penggunaan](#penggunaan)
- [Kontribusi](#kontribusi)
- [Lisensi](#lisensi)

Pertama kita buat enum TransitinType nya, agar kita bebas memilih type yang ingin kita pakai :
```dart
enum TransitionType {
  none,
  size,
  scale,
  fade,
  rotate,
  slideUp,
  slideDown,
  slideLeft,
  slideRight
}
```
dan kemudian kita buat PageRouteBuilder<T> nya, karena PageRouteBuilder bisa di modifikasi daripada MaterialPageRoute :
```dart

class Transitions<T> extends PageRouteBuilder<T> {
  final TransitionType transitionType;
  final Curve curve;
  final Curve reverseCurve;
  final Duration duration;
  final Widget widget;

  Transitions({
    required this.transitionType,
    this.curve = Curves.elasticInOut,
    this.reverseCurve = Curves.easeOut,
    this.duration = const Duration(milliseconds: 500),
    required this.widget,
  }) : super(
          transitionDuration: duration,
          pageBuilder: (context, animation, secondaryAnimation) => widget,
          transitionsBuilder: (context, animation, secondaryAnimation, child) {
            final curvedAnimation = CurvedAnimation(
              parent: animation,
              curve: curve,
              reverseCurve: reverseCurve,
            );

            switch (transitionType) {
              case TransitionType.none:
                return child;
              case TransitionType.size:
                return Align(
                  child:
                      SizeTransition(sizeFactor: curvedAnimation, child: child),
                );
              case TransitionType.scale:
                return ScaleTransition(
                    scale: curvedAnimation,
                    alignment: Alignment.topRight,
                    child: child);
              case TransitionType.fade:
                return FadeTransition(opacity: curvedAnimation, child: child);
              case TransitionType.rotate:
                return RotationTransition(
                  alignment: Alignment.center,
                  turns: curvedAnimation,
                  child: ScaleTransition(
                    alignment: Alignment.center,
                    scale: curvedAnimation,
                    child: FadeTransition(
                      opacity: curvedAnimation,
                      child: child,
                    ),
                  ),
                );
              case TransitionType.slideDown:
                return SlideTransition(
                  position: Tween<Offset>(
                    begin: const Offset(0.0, -1.0),
                    end: Offset.zero,
                  ).animate(curvedAnimation),
                  child: child,
                );
              case TransitionType.slideUp:
                return SlideTransition(
                  position: Tween<Offset>(
                    begin: const Offset(0.0, 1.0),
                    end: Offset.zero,
                  ).animate(curvedAnimation),
                  child: child,
                );
              case TransitionType.slideLeft:
                return SlideTransition(
                  position: Tween<Offset>(
                    begin: const Offset(1.0, 0.0),
                    end: Offset.zero,
                  ).animate(curvedAnimation),
                  child: child,
                );
              case TransitionType.slideRight:
                return SlideTransition(
                  position: Tween<Offset>(
                    begin: const Offset(-1.0, 0.0),
                    end: Offset.zero,
                  ).animate(curvedAnimation),
                  child: child,
                );
              default:
                return FadeTransition(opacity: curvedAnimation, child: child);
            }
          },
        );
}

```

Ini contoh halaman yang akan dituju : 
```dart
class NextPage extends StatelessWidget {
  const NextPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Next Page")),
      body: const Center(child: Text("This is the next page!")),
    );
  }
}
```
Ini contoh halaman utama dan kita terapkan transisinya dari halaman ini :
```dart
class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Transitions Example")),
      body: Center(
        child: ElevatedButton(
          onPressed: () {
            Navigator.of(context).push(
              Transitions(
                transitionType: TransitionType.slideLeft, // pilih disini TransitionType yang ingin dipakai
                widget: const NextPage(),
              ),
            );
          },
          child: const Text("Go to Next Page"),
        ),
      ),
    );
  }
}

```

dan terakhir, kita bisa mencobanya :
```dart
void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Transitions Example',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: const MyHomePage(),
    );
  }
}

```


# Project Saya

## Pendahuluan
## Instalasi
## Penggunaan
## Kontribusi
## Lisensi
dan terakhir, kita bisa mencobanya :
```dart
void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Transitions Example',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: const MyHomePage(),
    );
  }
}

```
