# Magic Cue in Notifications — spec notes (extracted from Figma)

Source: Figma desktop (Dev Mode MCP). `get_design_context` hung on this file (the launcher wallpaper
is a 5400×12120 image), so values below come from `get_metadata`, `get_variable_defs`, node renders
(`get_screenshot`) and pixel sampling of those renders. Reference renders of the key frames are in
`figma-ref/` (1×, 412×919).

## Frames
| State | Node | Ref render |
|---|---|---|
| Single recall chip – animating (glow) | `6:135282` | `figma-ref/frame-6-135282.png` |
| Two recall chips – animating | `10383:115705` | `figma-ref/frame-10383-115705.png` |
| Single recall chip – settled | `228:352358`, `10386:156914` | `frame-228-352358.png`, `frame-10386-156914.png` |
| Message sent (Sam) | `19:14686` | `frame-19-14686.png` |
| Message sent (Liam / Philz) | `1439:337140` | `frame-1439-337140.png` |
| Add second recall chip | `17:17814` | `frame-17-17814.png` |
| Magic action – "Pick up oat milk" | `1131:65565` | `frame-1131-65565.png` |
| "Complete action using" sheet | `1180:114283` | `frame-1180-114283.png` |
| Liam recall + "View Sunday" | `200:178549` | `frame-200-178549.png` |
| Sam recall + "Find restaurants" | `10410:585763` | `frame-10410-585763.png` |
| "Find restaurant" + "Sure" smart reply | `10410:119929` | `frame-10410-119929.png` |
| KG Floaty – query / thinking / Maps connecting / answer | `10410:120303` / `10410:120129` / `10410:120216` / `10410:120397` | `frame-10410-*.png` |
| Calendar Day View (handoff landing) | `10410:585420` | `frame-10410-585420.png` |

## Launcher (Blazer P10 Pro, 412 × 919)
- Wallpaper: `google_wallpapers_2025_frankel_blue_LT_5400X12120_360DPI_aRGB` (instance "Frankel P10", 412×925).
  `wallpaper-frankel-blue.png` is reconstructed at 1× from the frame renders: notification area restored by
  inverting the launcher scrim (measured exactly `rgba(0,0,0,.40)`: B = 0.600·A), UI areas inpainted
  (areas under widgets / hotseat / search bar are hidden by those elements).
- Status bar: 52 px, light icons. Time "9:30" at x 34; camera cutout 30 px circle centred (206, 33).
- Clock widget "MVP/Clock/Flower": box (214, 286.5) 182×182, 12-sided cookie r(θ) = 87.8 + 3.3·cos 12θ,
  fill `widgetBackground #F1EFFF`; hour hand `#0043A1`, minute hand `#1771FF` (12.5 px round strokes),
  second dot `#AD5AAF` r 8 at (43, 142); "Tue 19" on an arc (Google Sans Flex 21/500, #232D67).
- Weather "MVP/Weather/S/008": box (16, 505) 182×182, pill (stadium) rotated −45°, half-width 77, fill `#F1EFFF`;
  temperature "68°" box (67.6, 31) 99×79, `weatherTemp #0053C3`; icon 03_MostlySunny (30, 79.8) 72×73.
- Hotseat: y 724, 60 px circles at x 33/129/225/321, `themeApp #6F9BFF`, glyphs `onThemeApp #000F30`,
  App Shadows `0 0 6px #00000008, 0 1px 3px #0000000A`.
- Search bar (PSB): (29.37, 817) 353.26×52, radius 26, `sContainerLow #F1EFFF`; G (left 16), mic, Lens 24 px
  (themed: brandA #0053C3, brandB #97A5FF, brandC #005EDC, brandD #AD5AAF).
- Gesture handle: 108×4 at (152, 905), white (dark #1F1F1F over light sheets).

## Heads-up notification ("Design Structures")
- Card: x 16, y 68, width 380, radius 28, fill `surfaceContainer #E6E6FF`, padding 16.
- Icon 40×40 avatar at (16,16); Messages badge 20 px white circle at avatar (22,22), blue bubble.
- Content column x 72, width 292:
  - Primary line h 20: header text (h 24, y −2): name **Title/Medium/Emphasis** Google Sans Flex 16/24 w650
    `onSurface #232D67`; " • now • " Body/Small 12/16 `onSurfaceVariant #525C98`; bell (notifications_active) 16 px.
  - Expand/Collapse pill 28×20 radius 10, `surfaceBright #F8F5FF`, chevron 18 px.
  - Text block at y 28 (gap 8): Body/Medium 14/20 `onSurfaceVariant`; extra lines separated by 4 px
    (2nd message, "You" (14/20 w600) + sent value).
  - Suggestions + Actions: padding-top 8; chip row (48 or 32) then gap 16; actions row h 24:
    "Mark as read" / "Reply" 16/24 w650 `primary #0053C3`, gap 16; snooze (filled) 24 px right.
  - Heights: one chip 204 · two messages + chips 228 · message sent 188.

## Magic Cue chips (Section 1 `12918:38199`)
- 1-line: h 32 · padding 12 | icon 20×20 | gap 8 | 12 · Label Medium 12/16 (Google Sans Flex 500) onSurface.
- 2-line (recall): h 48 · padding 8 vertical · label onSurface + attribution onSurfaceVariant (both Label Medium).
- Chips in one row share the tallest height (48). Gap between chips 8. Recall chip widths e.g. 143 / 128.
- Fill = card surface (transparent), 1 px AI gradient outline `gStepLinear #FDA0FC → #578CFF → #0053C3`.
- Animating state adds `APUX Ai innerGlow`: inset 6 2 8 `#FDA0FC40`, inset −6 −2 8 `#0053C326`.
- App icons sit in a 20 px white circle (Calendar logo 12 px inside); recall glyph is a 20 px outline icon.
- #1 rule (section `9554:26328`): the entire text to be sent must be visible in the chip, otherwise don't show
  the chip; attribution may truncate. Action labels may wrap to 2 lines.

## Action catalog & fulfillment (`10386:157287`)
| Action | Fulfillment | Landing | Copy |
|---|---|---|---|
| View Calendar | Handoff | Calendar Day View | View <Day/Date> |
| Add to Calendar | Handoff | Calendar: Create Event | Add to calendar |
| Add reminder | In situ | Tasks: bottom sheet | Remind me |
| Summarize | In situ | Floaty | Summarize |
| Find restaurant | In situ | Floaty | Find <parameter> |
| Call / Text | Handoff | Dialer / Messages | Call / Text <parameter> |
| Send payment | Handoff | Pay contact | Pay <Person> <Amount> |
| Open location / Search in Maps | Handoff | Maps | Location / Find <parameter> |
| Open email | Handoff | Gmail | Open in Gmail |

## Sheet / Floaty
- "Complete action using": top 589 (h 330), radius 28 top, fill `#F1EFFF`; title 24/32 at (24, 613);
  rows 64 h, icon 40, label 16/24 w500; selected row pill `secondaryContainer #C9CFFF` inset 16, content +8;
  "Just once" / "Always" text buttons 14/20 w500 primary at y≈853.
- Floaty query: (16, 791) 380×96 white radius 28; "+" left, text 16/24 #1F1F1F, send button 44 px `#F0F4F9`.
- Thinking / Connecting: (16, 815) 380×72; 32 px orb (gradient arc + sparkle / Maps pin); title 16/24, sub 14/20 #5F6368.
- Answer: (16, 360) 380×527 white radius 24; header (sparkle, "Google Maps ▾" pill 40 h `#F0F4F9`, volume, open);
  body 16/26; disclaimer 11/16; input pill 72 h radius 36, 1 px `#C4C7C5`.
- Scrim: black 40%.
