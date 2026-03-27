# 余命時計 — Rork プロンプト

---

## 最初に投げるプロンプト（アプリ全体の指示）

```
Build a dark-themed mobile app called "余命時計" (Yomei Tokei / Life Countdown).

## Concept
A Memento Mori countdown app that shows the user's remaining active lifetime in real time, excluding sleep hours. The goal is to make users feel the weight of finite time and motivate behavior change.

## Design
- Always dark theme only. No light mode.
- Background: #080808 (deep black)
- Primary color: #8b0000 to #c0392b (deep crimson red)
- Text color: #c8b8b0 (muted off-white)
- Accent: #2a1a1a (dark reddish)
- Heading font: serif (Georgia or similar)
- Number font: monospace with tabular numerals
- Tone: minimal, oppressive silence — the countdown dominates the screen

## Screens

### 1. Google Sign-In Screen
- Simple dark screen with app name "余命時計" in serif font
- Subtitle: "Memento mori — 死を忘れるな" in small italic text
- Google Sign-In button styled in deep red

### 2. Onboarding (first time only, 3 steps)
- Step 1: Date of birth input (date picker)
- Step 2: Target age input (e.g. 80 years old)
- Step 3: Wake time and sleep time input (time pickers)
- Minimal dark UI, serif headings, red accent buttons
- Progress indicator at top

### 3. Paywall Screen
- Show the countdown blurred (filter: blur 6px, opacity 0.55) in the background
- Overlay with a floating Grim Reaper illustration (simple SVG, hooded figure with scythe, glowing red eyes, swaying animation)
- Copy text (display exactly as written, line breaks as shown):
  「あなたに残された時間は、
  今この瞬間も減っている

  余命時計を見るたびに気づく

  「人生は有限だ、」と。

  スマホを無意識にスクロールする時間が、
  静かに消えていく。

  あなたがやりたかった、
  本当に大切なことは何ですか？

  時計を解き放ち、
  今、ここから始めよう。」
- Feature list (just before CTA button):
  - 余命までの残り時間を表示（年・日・時間・分・秒をリアルタイム確認）
  - ホーム画面ウィジェット対応（スマホを開くたびに、残り時間が目に入る）
- Price display: ¥500 / 買い切り・永久アクセス
- CTA button: 「余命時計を解除する」 in deep red (#8b0000)
- Below button: small text link 「購入を復元する」

### 4. Main Screen (Countdown)
- Full screen dark background
- Small hourglass SVG illustration at top center, with sand-falling animation (particles dropping)
- Label: 「残り活動可能時間」 in tiny uppercase spaced letters, muted red
- Countdown display showing all units simultaneously:
  [年] · [日] · [時間] · [分] · [秒]
  Each unit: large number (32px, deep crimson) above small label (8px, uppercase, very dark red)
- Numbers update in real time (seconds tick down)
- Numbers have a subtle flicker animation occasionally
- Bottom: italic serif text 「Memento mori — 死を忘れるな」 in very dark red, separated by a thin line

### 5. Settings Screen
- Target age (editable)
- Wake time / Sleep time (editable)
- Notification settings (on/off, time picker for custom reminder)
- Account info (signed-in Google account)
- Reset data option (with confirmation dialog)

## Countdown Calculation Logic
remaining_seconds = (birthday_at_target_age - now) in seconds
active_ratio = (24 - sleep_hours) / 24
active_remaining_seconds = remaining_seconds * active_ratio

Display:
- 年 = floor(active_remaining_seconds / 31536000)
- 日 = floor((active_remaining_seconds % 31536000) / 86400)
- 時間 = floor((active_remaining_seconds % 86400) / 3600)
- 分 = floor((active_remaining_seconds % 3600) / 60)
- 秒 = floor(active_remaining_seconds % 60)

Update every second with setInterval.

## Navigation
- After sign-in: check if onboarding is complete → if not, show onboarding → then show paywall (if not purchased) → then show main screen
- Bottom tab or settings icon (top right) to access settings screen
- No bottom nav bar on main screen — keep it clean

## State / Storage
- Store user settings (DOB, target age, wake/sleep time) in AsyncStorage
- Store onboarding completion flag in AsyncStorage
- Simulate purchase state with a local boolean for prototype (isPurchased: true/false)
```

---

## 画面ごとの追加プロンプト（うまくいかない部分に使う）

### カウントダウンの計算がおかしい場合
```
Fix the countdown calculation. 
- Date of birth: use the stored date of birth
- Target age: add target age years to date of birth to get the end date
- Active hours per day: 24 minus sleep hours (e.g. if sleep is 7 hours, active = 17 hours)
- active_ratio = active_hours / 24
- remaining_active_seconds = (end_date - now).totalSeconds * active_ratio
- Display years, days, hours, minutes, seconds from this value
- Update every second using setInterval
```

### ペイウォールのデザインを直す場合
```
Redesign the paywall screen:
- Background behind the blurred countdown should be rgba(4,2,2,0.82)
- The Grim Reaper SVG should float with a CSS animation (translateY 0 to -8px, 4s ease-in-out infinite)
- The scythe should sway (rotate -3deg to 3deg, 4s ease-in-out infinite)
- Copy text must follow exact line breaks as specified — use explicit line break elements
- Feature items before the CTA button should have small icon SVGs (clock icon, widget icon) on the left
- CTA button background: #8b0000, text color: #e8c0b0, font: serif
```

### 数字のデザインを直す場合
```
Update the countdown number display:
- Each unit (年/日/時間/分/秒) should be in its own block
- Number: font-size 32px, font-weight 500, color #9b2020, font-variant-numeric tabular-nums
- Unit label: font-size 8px, uppercase, letter-spacing 0.1em, color #3d1a1a
- Separator between units: · (middle dot), color #1e0e0e
- All units on one row, centered, with flex layout
- Numbers should flicker occasionally (CSS keyframe animation, opacity 1 to 0.7, random timing)
```

### オンボーディングのデザインを直す場合
```
Redesign the onboarding screens with dark theme:
- Background: #080808
- Step indicator at top: small dots, active dot in #8b0000
- Heading: serif font, color #c8a8a0, font-size 20px
- Input fields: dark background #141414, border #2a1a1a, text color #c8b8b0
- Next button: background #8b0000, text color #e8c0b0, serif font
- Step 1: "生年月日を入力してください" with date picker
- Step 2: "目標とする年齢を入力してください" with number input (default: 80)
- Step 3: "起床時間と就寝時間を入力してください" with two time pickers
```

---

## 注意点

- **課金処理はモックでOK**: プロトタイプなので `isPurchased` のローカルフラグで切り替えるだけでよい。RevenueCatの実装は本番時にCursorで追加する
- **ウィジェットはスコープ外**: RorkはネイティブWidgetKitに対応していないため、プロトタイプではウィジェット画面は省略する
- **Firebase AuthもモックでOK**: Googleサインインのモックとして、ボタンタップで認証完了扱いにしてよい
