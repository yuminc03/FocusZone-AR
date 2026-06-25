# FocusZone Figma AI 디자인 프롬프트 명세
Figma Make(Figma AI)에 그대로 입력하여 FocusZone iOS 앱의 UI 템플릿과 디자인 시스템을 고성능으로 생성해낼 수 있는 구체적인 영문 프롬프트 세트입니다.

## 1. 메인 AR 스캔 화면 (Main AR Noise-scanning Screen)
```text
Create a high-fidelity iOS mobile app design for an AR acoustic mapping app named "FocusZone".
The UI should be premium, clean, and minimalist, utilizing Apple's native design system and human interface guidelines.
- Color Theme: Ultra-dark mode (pure black #000000 background) with a vibrant, neon-colored AR viewport.
- Layout Structure:
  1. A full-screen AR camera viewport displaying a semi-transparent 3D room mesh. Inside the mesh, show a smooth, glowing heat map transition (gradient from calm neon blue #00C2FF for quiet areas to intense neon red #FF0055 for noisy areas).
  2. Overlay thin, glowing 3D vector lines representing sound reflection rays bouncing from a window source to the walls with soft arrow indicators.
- Overlay UI Panels (Glassmorphism):
  1. Top Status Bar: Transparent background. Displays the current average ambient decibel (e.g., "42 dB - Calm") in a soft green pill badge. Add a subtle settings gear icon on the top right.
  2. Bottom Control Panel: A floating, rounded-corner card with heavy background blur (SF Pro Material style, white fill at 10% opacity, 1px thin white border at 20% opacity).
- Elements inside Bottom Panel:
  1. Left side: A real-time mini decibel line graph (last 10 seconds) rendered in a glowing neon-cyan stroke.
  2. Center: A large, circular, glowing "Analyze Space" button with a microphone icon.
  3. Right side: A status message "Scanning Area... 78% mapped" in a light-gray color.
- Typography: San Francisco (SF Pro) font family. High contrast, sharp readability, medium/bold weight for critical readouts. No placeholders, complete realistic copy.
```

## 2. 권한 승인 및 온보딩 화면 (Permissions & Onboarding Screen)
```text
Create a minimalist iOS mobile app onboarding and permissions screen for "FocusZone".
- Color Theme: Pure dark theme (#09090B background).
- Layout Structure:
  1. Top Section: A large, sleek 3D-like abstract icon representing sound waves intersecting with a camera lens. The icon has a glowing gradient of purple to blue, with no device frames and no text inside the icon graphics.
  2. Middle Section: A clean, centered title "Discover Your Focus Space" using bold San Francisco font, followed by a short description: "FocusZone scans your room in 3D to map real-time noise levels. Your scan data is processed purely in memory and disappears instantly when you exit the app."
  3. Bottom Section: Two distinct permission request cards with a glassmorphism style (10% white opacity, subtle border).
     - Card 1: "Camera & LiDAR Access" - Brief explanation for 3D room mapping. Includes a toggle switch.
     - Card 2: "Microphone Access" - Brief explanation for real-time FFT decibel analysis. Includes a toggle switch.
  4. CTA Button: A full-width, pill-shaped primary action button at the bottom labeled "Start Scanning" with a solid gradient background (neon blue to violet) and smooth rounded corners.
- Typography: San Francisco font, strict adherence to Apple HIG spacing, padding, and font scale.
```
