---
title: "CupertinoPicker와 Gradient"
excerpt: "CupertinoPicker에 Gradient 그리기 및 Gradient issues에 대해"
date: 2021-02-24
---

# CupertinoPicker and Gradient

사용중이던 쿠퍼티노 피커가 상수로 지정되어 있어 아이패드 등 에서는 다소 작게 느껴져 미디어쿼리를 사용하여 기기 높이의 %로 생성되도록 변경하였다.

피커는 그라디언트 효과를 주기 위해 아래와 같은 그라디언트 위젯을 만들어 스택으로 쌓아서 사용중이었는데 이 그라디언트 위젯  또한 높이가 고정 값이기 때문에 변경이 필요했다.

- 그라디언트 높이도 피커의 높이에 맞춰 변경되어야 함
    - 디자인처럼 피커의 일정 비율이 그라데이션 되어야 함
    - 현재는 두개의 위젯을 위, 아래로 붙이므로 개별 값 필요 
    - ⇒ 피커 한 칸의 높이를 구하는건 불필요한 비용이 들고 가독성이 떨어질 것 같음

```dart
class _PickerGradient extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final color = Colors.white;
    final gradientColors = [color, color.withOpacity(0.30)];
    return Column(
      mainAxisAlignment: MainAxisAlignment.spaceBetween,
      children: List.generate(
        2,
        (i) => Container(
          height: 40,
          decoration: BoxDecoration(
            gradient: LinearGradient(
              begin: Alignment.topCenter,
              end: Alignment.bottomCenter,
              colors: i == 0 ? gradientColors : gradientColors.reversed.toList(),
            ),
          ),
        ),
      ),
    );
  }
}
```

그러던 중 CupertinoPicker의 그라데이션이 어느 없어졌다는 이슈글을 보게됐다.

원래 없던게 아니라 없어진거였어?

[CupertinoPicker top and bottom gradient screen no longer appearing · Issue #66357 · flutter/flutter](https://github.com/flutter/flutter/issues/66357)

해당 이슈는 현재 `Engineer reviewed` 단계에 있고,
가깝거나 먼 미래에 해결될 때 까지 삭제된 코드를 가져다가 쓰기로 하였다.

```dart
class _PickerGradient extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final gradientColor = Colors.white;
    return Positioned.fill(
      child: IgnorePointer(
        child: Container(
          decoration: BoxDecoration(
            gradient: LinearGradient(
              begin: Alignment.topCenter,
              end: Alignment.bottomCenter,
              colors: [
                gradientColor,
                gradientColor.withAlpha(0xF2),
                gradientColor.withAlpha(0xDD),
                gradientColor.withAlpha(0),
                gradientColor.withAlpha(0),
                gradientColor.withAlpha(0xDD),
                gradientColor.withAlpha(0xF2),
                gradientColor,
              ],
              stops: const <double>[0.0, 0.05, 0.09, 0.22, 0.78, 0.91, 0.95, 1.0],
            ),
          ),
        ),
      ),
    );
  }
}
```

왜냐하면 해당 코드는

- 두개의 그라데이션을 합친게 아닌  `Positioned.fill` 속성으로 전체를 채우는 방식
    - ⇒ 비율을 사용하여 조정할 수 있음
- 그라데이션 단계가 더 세분화 되어있음

의 이유로 기존 문제가 해결 및 개선됐기 때문이다.

기존 코드는 위, 아래로 위젯 두개가 합쳐져있고 가운데는 비어있는 상태였지만 변경 코드는 전체를 채우는 위젯이기 때문에 꼭 `IgnorePointer`를 사용하여 포인터 이벤트에서 제외시켜야 한다.

---

# 참고링크

[IgnorePointer class](https://api.flutter.dev/flutter/widgets/IgnorePointer-class.html)

자매품 [AbsorbPointer class](https://api.flutter.dev/flutter/widgets/AbsorbPointer-class.html)

---


