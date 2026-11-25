# Swift / iOS Agent Skills — Personal Collection

مجموعة مهارات (Agent Skills) مجمّعة من مشاريع هذا الجهاز الأربعة، بمعيار
[Agent Skills](https://agentskills.io): كل مهارة مجلد فيه `SKILL.md` بترويسة
`name` و`description`، وملفات مراجع اختيارية داخل `references/` أو `reference/`.

المجموع: **64 مهارة**.

## التركيب

### مشغّل الوسائط والفيديو
| المهارة | الغرض |
|---|---|
| `avkit` | AVPlayerViewController، PiP، AirPlay، الترجمة، أزرار التحكم |
| `avfoundation-hls-pip` | HLS، الـbitrate التكيفي، دورة حياة PiP، إعادة المحاولة |
| `libmpv-ffmpeg-ios` | libmpv على iOS: ترتيب الإنهاء، فك الترميز العتادي، PiP مخصص |
| `video-instant-startup` | تقصير زمن أول إطار: prewarm، preroll، الهيدرز داخل الـasset |
| `subtitle-sync-parsing` | ترجمة عربية (UTF-8 ثم Windows-1256)، بحث ثنائي عن المقطع |
| `stream-cache-expiry` | روابط موقّعة تنتهي صلاحيتها: قراءة الانتهاء، التجديد، التنظيف |
| `source-racing-speed` | سباق المرايا وإلغاء الخاسرين وترتيب المزوّدين بالزمن المقاس |

### المصادر والـaddons
| المهارة | الغرض |
|---|---|
| `source-plugin-resilience` | توزيع البحث على المزوّدين بالتوازي، مهلة لكل واحد، قاطع دائرة |
| `plugin-sandbox-validation` | تحقّق من manifest خارجي دون انهيار، رفض مخططات URL غير آمنة |
| `ktor-client-performance` | عميل Ktor واحد مشترك: محرّكات، تجمّع اتصالات، مهلات، بثّ |
| `kmp-ktor` | إعداد Ktor في KMP: محرّكات المنصّات، تسلسل، Bearer refresh، اختبارات |
| `kmp-network-layer` | طبقة شبكة كاملة في `:core:network` مع `NetworkResult` وKoin |

### SwiftUI والواجهة
| المهارة | الغرض |
|---|---|
| `swiftui-pro` | مراجعة شاملة لأفضل ممارسات SwiftUI الحديثة |
| `swiftui-animation` | حركات صريحة ومقيّدة، springs، PhaseAnimator، KeyframeAnimator |
| `swiftui-navigation` | NavigationStack/SplitView، sheets، تبويبات، deep linking |
| `swiftui-layout-components` | Stacks، Grids، List، ScrollView، Form، overlays |
| `swiftui-uikit-interop` | UIViewRepresentable، UIHostingController، Coordinator |
| `swiftui-view-refactor` | تفكيك الملفات الكبيرة، تثبيت تدفّق البيانات، Observation |
| `swiftui-performance-audit` | تشخيص بطء العرض من الكود أولاً |
| `swiftui-scroll-fluency` | كتالوجات بـ120Hz: حاويات lazy، هوية مستقرة، كاش محدود |
| `swiftui-liquid-glass` | زجاج iOS 26: glassEffect، الحاويات، الانتقالات المورفية |
| `swiftui-iphone-duo` | iPhone Duo القابل للطي: نوافذ متغيرة، الأشرطة، الحافة |
| `ios-ui-fluidity` | لا تحليل ولا فك ترميز على الخيط الرئيسي، صور مصغّرة، إلغاء البحث |
| `instant-search-fluency` | فهرس بحث محلي مطبّع، إجابة في إطار واحد، طيّ الحروف العربية |
| `swiftui-microinteractions` | حركات دقيقة بأسلوب Animo: فيزياء spring، CoreHaptics، زجاج، ورسم SF Symbols 7 |
| `swiftui-patterns` | @Observable وملكية الحالة، تركيب العروض، تحميل async، وصقل التصميم |
| `swiftui-gestures` | Tap/Drag/Magnify/Rotate، التركيب، GestureState، وحل تعارض الإيماءات (أساس إيماءات المشغّل) |
| `swiftui-webkit` | WebView/WebPage، سياسات التنقّل، تنفيذ JavaScript، محتوى محلي ومخططات URL (متصرّف التطبيق) |
| `swift-charts` | رسوم Bar/Line/Area/Pie/3D، التحديد والتمرير والتحاور (شاشة الإحصاءات) |

### Swift وSwiftData
| المهارة | الغرض |
|---|---|
| `swift-concurrency` | Swift 6: Sendable، actors، العمل المتوازي، أمان البيانات |
| `swift-codable` | Codable/JSONDecoder، CodingKeys، الفك المخصص والمتداخل |
| `swift-formatstyle` | تنسيق الأرقام والعملات والتواريخ والمدد حسب اللغة |
| `swiftdata` | نماذج SwiftData، الوراثة، العلاقات متعددة الأشكال |
| `swiftdata-migration` | ترحيل القوائم والسجلّات من UserDefaults إلى SwiftData |
| `swift-architecture` | اختيار/مراجعة/ترحيل المعمارية: MV، MVVM، MVI، TCA، Clean، Coordinator |
| `swift-testing` | Swift Testing: @Test، @Suite، #expect، الاختبارات المُعاملة والمزاح |

### iOS الهندسية والمنصّة
| المهارة | الغرض |
|---|---|
| `ios-networking` | URLSession async/await، REST، تنزيل/رفع، WebSocket، إعادة المحاولة |
| `ios-localization` | String Catalogs، الجمع، RTL، التنسيق حسب اللغة |
| `ios-accessibility` | VoiceOver، Dynamic Type، rotors، إعلانات إتاحة المتجر |
| `background-processing` | BGTaskScheduler، التنزيل في الخلفية، push صامت |
| `activitykit` | Live Activities والجزيرة الديناميكية |
| `widgetkit` | ودجت الشاشة والقفل وControl Center، timeline providers |
| `app-intents` | Siri وShortcuts وSpotlight وApple Intelligence |
| `ios-app-intents` | تعريض التشغيل والبحث لـSiri وAppEntity وApp Shortcuts |
| `xcode-project-setup` | تعديل `.pbxproj` بأمان: حزم Swift وربط الملفات |
| `storekit` | StoreKit 2: المنتجات، الاشتراكات، SubscriptionStoreView، التحقق من المعاملات (العضوية) |
| `push-notifications` | UNUserNotificationCenter، APNs، الإشعارات الغنية والصامتة، الردود والخدمة |
| `swift-security` | Keychain، CryptoKit، Secure Enclave، ربط الشهادات، تخزين بيانات Trakt والاعتمادات |
| `authentication` | Sign in with Apple، المفاتيح (passkeys)، الحماية بالبصمة، إدارة الاعتمادات |
| `metrickit` | MetricKit: تقارير الأداء والانهيارات في الإنتاج وقمع الأعطال |
| `app-store-review` | إرشادات المراجعة، PrivacyInfo.xcprivacy، منع الرفض، وقوائم الفحص |
| `debugging-instruments` | LLDB وInstruments و`os_signpost` وترميز الانهيارات |
| `swiftlint` | SwiftLint: الإعداد، القواعد المخصصة، الـbaselines، والـCI |
| `tipkit` | TipKit: تلميحات الميزات وقواعدها وأحداثها ومجموعاتها |
| `speech-recognition` | SpeechAnalyzer/SFSpeechRecognizer: بحث صوتي عربي وتفريغ الصوت |
| `natural-language` | NLTokenizer/NLTagger: تطبيع النص، كشف اللغة، الترجمة |
| `shareplay-activities` | GroupActivity/GroupSession: مشاهدة جماعية متزامنة (Party) |
| `swift-api-design-guidelines` | اصطلاحات التسمية والتوثيق ومعايير تصميم الـAPI |

### التصميم والأدوات
| المهارة | الغرض |
|---|---|
| `anti-ui-slop` | منع الواجهات العامة: عقد تصميم، حالات مطلوبة، بوابة إنهاء |
| `impeccable` | تصميم/نقد/صقل واجهات الويب والتطبيقات وأنظمة التصميم |
| `app-paywall-pilot` | تصميم وتدقيق paywall متوافق مع App Store: الاشتراكات، التجارب، التسعير، LTV |
| `gemini-image-generation` | توليد صور عبر Gemini 3 Pro / 2.5 Flash — يحتاج `GEMINI_API_KEY` |
| `app-store-optimization` | ASO: بحث الكلمات، وصف المتجر، لقطات الشاشة، الصفحات المخصصة واختبار A/B |
| `find-skills` | البحث عن مهارات قابلة للتثبيت عند الحاجة لوظيفة جديدة |

## التركيب في مشروع

```sh
mkdir -p .claude && cp -R .claude/skills /path/to/project/.claude/
```

يقرأ المشروع مهاراته من `.claude/skills` أو `.agents/skills` أو `.codex/skills`.
مع Claude Code يمكن أيضاً وضعها في `~/.claude/skills` لتكون عامة.

## المصادر والتراخيص

- `avkit`, `swift-codable`, `ios-networking`, `ios-localization`, `swift-formatstyle`,
  `swiftui-liquid-glass`, `widgetkit`, `activitykit`, `background-processing`,
  `ios-accessibility` — من [dpearson2699/swift-ios-skills](https://github.com/dpearson2699/swift-ios-skills)
  بترخيص **PolyForm Perimeter 1.0.0**: الاستخدام لبناء التطبيقات مسموح، وإعادة بيعها
  كمنتج مهارات منافس غير مسموح.
- `swiftui-pro`, `swiftui-animation`, `swiftui-navigation`, `swiftui-layout-components`,
  `swiftui-uikit-interop`, `swiftui-view-refactor`, `swiftui-performance-audit`,
  `swiftui-scroll-fluency`, `swiftui-iphone-duo`, `swiftdata`, `swift-concurrency` —
  من Paul Hudson وThomas Ricouard ومصادر مجتمعية أخرى عبر
  [twostraws/swift-agent-skills](https://github.com/twostraws/swift-agent-skills)؛ راجع
  `LICENSE` داخل كل مهارة عند إعادة التوزيع.
- `impeccable`, `anti-ui-slop`, `find-skills` — تراخيصها مع ملفاتها (`anti-ui-slop/LICENSE`،
  `anti-ui-slop/NOTICE`).
- `avfoundation-hls-pip`, `libmpv-ffmpeg-ios`, `video-instant-startup`,
  `subtitle-sync-parsing`, `source-racing-speed`, `source-plugin-resilience`,
  `stream-cache-expiry`, `plugin-sandbox-validation`, `instant-search-fluency`,
  `ios-ui-fluidity`, `kmp-ktor`, `kmp-network-layer`, `ktor-client-performance`,
  `ios-app-intents`, `swiftdata-migration`, `xcode-project-setup`, `app-intents` —
  مهارات خاصة بمشاريع هذا الجهاز.
- `swiftui-microinteractions` — من [iAmVishal16/swiftui-microinteractions](https://github.com/iAmVishal16/swiftui-microinteractions)، MIT.
- `app-paywall-pilot` — من [Nikolai-Iakubovskii/app-paywall-pilot](https://github.com/Nikolai-Iakubovskii/app-paywall-pilot)، MIT.
- `gemini-image-generation` — من [chirag2653/free-ai-agent-skills](https://github.com/chirag2653/free-ai-agent-skills)، MIT (سكربت التشغيل بايثون ويتطلب مفتاح Gemini).
- `swiftui-patterns`, `swiftui-gestures`, `swiftui-webkit`, `swift-charts`, `swift-testing`,
  `swift-architecture`, `storekit`, `push-notifications`, `swift-security`, `authentication`,
  `metrickit`, `app-store-review`, `app-store-optimization` — أيضاً من
  [dpearson2699/swift-ios-skills](https://github.com/dpearson2699/swift-ios-skills) بترخيص
  PolyForm Perimeter 1.0.0.
- `debugging-instruments`, `swiftlint`, `tipkit`, `speech-recognition`, `natural-language`,
  `shareplay-activities`, `swift-api-design-guidelines` — كذلك من
  [dpearson2699/swift-ios-skills](https://github.com/dpearson2699/swift-ios-skills) بترخيص
  PolyForm Perimeter 1.0.0.

هذا الفهرس هو نسخة تجميعية للاستخدام الشخصي، وليس إصداراً رسمياً من أي من المصادر أعلاه.
