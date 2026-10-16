# Dungeon Game

Unity ile geliştirilen, düğüm tabanlı dungeon ilerlemesini tur bazlı stratejik savaş, ekipman yönetimi, augment kombinasyonları ve risk-ödül odaklı karşılaşmalarla birleştiren 2D roguelite RPG.

Oyuncu her koşuda farklı bir dungeon akışından ilerler; savaşır, kaynak toplar, ekipman kuşanır, augment seçer, mağazadan alışveriş yapar ve final boss'a ulaşmaya çalışır. Kalıcı oyuncu gelişimi ile koşuya özel ilerleme ayrı veri katmanlarında tutulur.

> Bu repository aktif geliştirme aşamasındaki bir oyun prototipidir. README, mevcut kod ve içerik yapısına göre hazırlanmıştır.

## İçindekiler

- [Oyun Özeti](#oyun-özeti)
- [Öne Çıkan Sistemler](#öne-çıkan-sistemler)
- [Oynanış Döngüsü](#oynanış-döngüsü)
- [Savaş Sistemi](#savaş-sistemi)
- [İlerleme ve Build Oluşturma](#ilerleme-ve-build-oluşturma)
- [Dungeon ve Node Akışı](#dungeon-ve-node-akışı)
- [Ekipman ve Envanter](#ekipman-ve-envanter)
- [Karşılaşmalar](#karşılaşmalar)
- [Teknik Mimari](#teknik-mimari)
- [Kullanılan Teknolojiler](#kullanılan-teknolojiler)
- [Proje Yapısı](#proje-yapısı)
- [Kurulum ve Çalıştırma](#kurulum-ve-çalıştırma)
- [Kontroller](#kontroller)
- [Geliştirme Notları](#geliştirme-notları)
- [Roadmap](#roadmap)

## Oyun Özeti

**Dungeon Game**, her koşuda kaynakları dikkatli kullanmayı ve doğru build kararlarını ödüllendiren bir dungeon crawler deneyimi hedefler.

Temel hedefler:

- Rastgele oluşturulan dungeon node'larında ilerlemek
- Normal, güçlü ve elite düşmanları yenmek
- Mini boss ve final boss karşılaşmalarına hazırlanmak
- Savaşlardan XP, gold, ekipman ve augment kazanmak
- Element ve rarity seçimleriyle farklı karakter build'leri oluşturmak
- HP, guard, damage, crit ve ultimate kaynaklarını dengeli yönetmek
- Koşuyu tamamlayarak kalıcı ilerlemeye katkı sağlamak

## Öne Çıkan Sistemler

- Node tabanlı, tekrar oynanabilir dungeon üretimi
- Tur bazlı oyuncu ve düşman savaş sırası
- Hızlı saldırı, normal saldırı, güçlü saldırı, savunma ve ultimate yetenekleri
- İsabet, kritik vuruş, armor/guard azaltımı ve hasar azaltma hesaplamaları
- Burn, Poison, Freeze ve Shield status effect sistemi
- Ignis, Toxic, Thunder ve Frost elementleri
- Common'dan Legendary'ye augment rarity sistemi
- Savaş akışına hook'larla bağlanan veri odaklı augment mimarisi
- Silah, miğfer, eldiven, zırh, yüzük ve bot/relic ekipman slotları
- Aynı tip ve rarity ekipmanları birleştirme sistemi
- Gold kullanan augment mağazası ve ücretsiz/ücretli reroll sistemi
- Blackjack, Rock Paper Scissors, Shadow Thief ve Jackpot karşılaşma altyapısı
- JSON tabanlı oyuncu kayıt sistemi
- Screen fade, saldırı hareketleri, parallax arka plan, damage popup ve status VFX'leri
- Unity Event, C# Action ve event-driven UI güncellemeleri

## Oynanış Döngüsü

1. `0_Bootstrap` sahnesi kayıt dosyasını yükler veya yeni kayıt oluşturur.
2. Oyuncu `1_MainMenu` sahnesinden dungeon/area seçer.
3. Yeni koşu başlatılır ve koşu istatistikleri kalıcı oyuncu verisinden snapshot olarak alınır.
4. `2_GamePlayScene` içinde dungeon node'ları oluşturulur.
5. Oyuncu node'lar arasında ilerler ve node tipine göre içerik açılır:
   - Combat
   - Encounter
   - Shop
   - Mini Boss
   - Final Boss
6. Savaş ve karşılaşma ödülleri alınır; augment, gold, XP veya ekipman ile build güçlendirilir.
7. Koşu tamamlandığında veya sona erdiğinde ana menüye dönülür.
8. Kalıcı oyuncu verisi `player_save.json` dosyasına yazılır.

## Savaş Sistemi

Savaşlar `BattleSystem` tarafından state machine yaklaşımıyla yönetilir:

- `Start`
- `PlayerTurn`
- `EnemyTurn`
- `Victory`
- `Defeat`

Oyuncu yetenekleri:

| Tuş | Yetenek | Davranış |
|---|---|---|
| Q | Quick Attack | %100 isabet, %65 hasar, +10 ultimate |
| W | Normal Attack | %70 isabet, %100 hasar, +15 ultimate |
| E | Strong Attack | %40 isabet, %200 hasar, +25 ultimate |
| D | Defend | Saldırı yerine savunma, alınan hasarı azaltma |
| R | Ultimate Attack | 100 ultimate şarjında kullanılabilir, %90 isabet, %250 hasar |
| 1-4 | Augment Skills | Aktif augment yetenekleri için ayrılmış tuşlar |
|

Hasar çözümleme sırası `DamageCalculator` içinde şu aşamalardan geçer:

1. Yetenek isabet kontrolü
2. Temel hasar hesaplama
3. Augment hasar modifikasyonları
4. Kritik vuruş kontrolü ve çarpanı
5. Kritik sonrası augment etkileri
6. Düşman guard/defense hesabı
7. Hasarın uygulanması ve event/VFX güncellemeleri

Savaş UI'si oyuncu ve düşman HP barlarını, shield durumunu, ultimate şarjını, status effect stack'lerini, round bilgisini ve hasar popup'larını gösterir. Saldırı animasyonları ile hasar uygulaması coroutine akışı içinde senkronlanır.

## İlerleme ve Build Oluşturma

### Koşu ilerlemesi

`RunData` koşuya özel verileri yönetir:

- Run level ve run XP
- Power, vitality ve guard
- Crit chance ve crit damage
- Max/current HP
- Ultimate şarjı
- Gold
- Koşu içindeki augment listesi
- Koşu envanteri
- Dungeon node konumu

Run XP arttıkça seviye atlanır. Seviye atlama sırasında temel istatistikler ölçeklenir ve belirli seviyelerde oyuncuya HP iyileştirmesi veya power artışı arasında seçim sunulur.

### Augment sistemi

Augment'lar oyun içindeki build çeşitliliğinin ana kaynağıdır. Her augment:

- Bir elemente bağlıdır: Ignis, Toxic, Thunder, Frost veya Utility
- Bir rarity seviyesine sahiptir: Common, Uncommon, Rare, Epic, Legendary
- Savaş olaylarına bağlanabilen hook'lar içerir
- Hasar, kritik, ultimate, isabet, status effect ve hayatta kalma davranışlarını değiştirebilir

Örnek augment davranışları:

- Burn/Poison stack üretmek
- Kritik hasarı veya kritik ihtimalini artırmak
- Armor/guard yok saymak
- Ultimate daha hızlı doldurmak
- Ekstra vuruş veya execute etkisi vermek
- Shield, heal veya damage reduction sağlamak

Augment seçimleri level-up, başlangıç element seçimi, elite ödülü ve boss ödülü gibi farklı kaynaklardan gelebilir. Panel açıkken arka arkaya oluşan ödül talepleri kaybolmaması için kuyrukta tutulur.

### Elementler ve status effect'ler

Mevcut status effect altyapısı:

- **Burn:** Stack sayısına göre hasar verir, augment'larla güçlendirilebilir.
- **Poison:** Stack başına zehir hasarı üretir.
- **Freeze:** Stack başına gelen hasarı azaltır.
- **Shield:** Hasar almadan önce koruyucu kalkan olarak çalışır.

Status effect'ler tur başlangıcı ve tur sonu hook'larıyla işlenir; stack ve süre durumları UI ikonları ve VFX ile yansıtılır.

## Dungeon ve Node Akışı

`DungeonCreator`, her koşuda dungeon listesini runtime'da üretir. Akışta:

- Başlangıç node'u bulunur.
- Yaklaşık 15-17 combat node'u oluşturulur.
- Mini boss'lar aralıklı yerleştirilir.
- İlk encounter erken node'larda garanti edilir.
- Shop node'larının ilki erken-orta bölümde görünür.
- Final boss son node olarak korunur.

`NodeFlowController`, node tipine göre doğru paneli açar ve node tamamlandığında ekran geçişini yönetir. `ScreenFade` geçişleri savaş, encounter, augment ve dungeon bilgi panelleri arasında daha temiz bir akış sağlar.

Düşmanlar node tipine ve oyuncunun run level'ına göre üretilir. Normal savaşlarda seviye yükseldikçe Strong ve Elite düşman olasılığı artar. Mini boss ve final boss istatistikleri ayrıca ölçeklenir; area ve dungeon kimlikleri global çarpanlara dahil edilir.

## Ekipman ve Envanter

Ekipman sistemi `ItemData` ve `ItemInstance` ayrımı üzerine kuruludur:

- `ItemData`: eşyanın tanımı, ikonu, tipi ve temel statları
- `ItemInstance`: runtime kimliği, rarity'si ve gerçek stat değerleri

Ekipman tipleri:

- Weapon
- Helmet
- Glove
- Armor
- Ring
- Boot/Relic

Oyuncu eşyaları:

- Envantere ekleyebilir
- Slotlarına equip/unequip edebilir
- Damage, guard, max HP ve crit bonuslarını inceleyebilir
- Aynı tip ve rarity eşyaları merge ederek bir üst rarity oluşturabilir

Item rarity renkleri ve kart görünümleri `RarityColors` üzerinden merkezi olarak yönetilir. Envanter UI, event'lere abone olarak equip, unequip ve item ekleme işlemlerinde kendini yeniler.

## Karşılaşmalar

Encounter node'ları savaş dışı risk-ödül kararları için kullanılır. Kod tabanında aşağıdaki mini oyun sistemleri bulunur:

- **Blackjack:** Kart çekme, stand/hit, dealer akışı, ödülü alma veya ikiye katlama kararı.
- **Rock Paper Scissors:** Gold bahisli, üç round kazanma hedefli oyun. Rakip davranışı oyuncunun hamlelerine göre kısmen adapte olur.
- **Shadow Thief:** Üç turlu zar/kaçış karşılaşması. Başarısızlıkta artan gold kaybı, başarıda tur bazlı XP ödülü vardır.
- **Jackpot:** Slot benzeri rarity ve reroll sistemi. Reroll maliyeti gold miktarına göre hesaplanır.

Encounter sınıfları UI'dan ayrıdır. Örneğin `BlackJackEncounter`, `RockPaperScissorsEncounter` ve `ShadowThiefEncounter` oyun kurallarını tutarken ilgili UI sınıfları yalnızca girişleri ve görsel çıktıyı yönetir. Bu ayrım test edilebilirliği ve yeni encounter eklemeyi kolaylaştırır.

## Teknik Mimari

### Kalıcı ve geçici veri ayrımı

- `PlayerData`: oyuncunun koşular arasında korunan level, XP, temel stat, soul, tamamlanan dungeon ve inventory verileri
- `RunData`: yalnızca mevcut koşu boyunca yaşayan istatistik ve kaynaklar
- `GameSessionData`: sahneler arası seçili dungeon/area gibi geçici seçimler
- `SaveManager`: JSON serialize/deserialize ve inventory kayıtlarının yönetimi

`RunData` ve `SaveManager` singleton yaşam döngüsüyle sahneler arasında korunur ve `DontDestroyOnLoad` kullanır. Bootstrap sahnesi kayıt yüklemesini tamamladıktan sonra ana menüyü açar.

### Event-driven UI

Sistemler doğrudan birbirlerinin UI elemanlarına sıkı şekilde bağlı olmak yerine event'ler kullanır. Örnekler:

- `OnRunStatsChanged`
- `OnLevelUp`
- `OnAugmentsChanged`
- `OnInventoryChanged`
- `OnItemEquipped`
- Encounter sonuç event'leri
- Battle UI ve reward servis callback'leri

Bu yapı, oyun kuralları ile UI güncellemelerinin ayrılmasını ve yeni görsel katmanların daha kolay eklenmesini sağlar.

### Görsel efekt ve animasyon yaklaşımı

- Coroutine tabanlı saldırı ve geçiş animasyonları
- Screen shake ve damage popup feedback'i
- Status effect'e göre kalıcı VFX eşleştirmesi
- Augment ID üzerinden impact VFX lookup
- Sonsuz parallax arka plan katmanları
- Düşman prefab'larının `EnemyVisualData` üzerinden runtime seçilmesi

## Kullanılan Teknolojiler

- **Unity 6:** `6000.4.3f1`
- **C#:** Unity runtime ve oyun sistemleri
- **Universal Render Pipeline:** `17.4.0`
- **Unity Input System:** `1.19.0`
- **Unity uGUI:** `2.0.0`
- **TextMesh Pro:** UI metinleri
- **2D Animation:** `14.0.3`
- **2D Sprite / SpriteShape / Tilemap:** 2D içerik ve sahne desteği
- **Aseprite Importer:** `4.0.1`
- **PSD Importer:** `13.0.2`
- **Post Processing:** `3.5.4`
- **Timeline:** `1.8.12`
- **Visual Scripting:** `1.9.11`
- **Unity JSON Serialization:** oyuncu kayıt sistemi
- **Unity Test Framework:** `1.6.0` paket desteği

## Proje Yapısı

```text
Assets/
├── GameCore/
│   ├── Bootstrap/                 Kalıcı veri, bootstrap ve save sistemi
│   ├── DungeonGame/
│   │   ├── Combat/                Battle, damage, skill, enemy, augment, status ve VFX
│   │   ├── DungeonsMenu/          Dungeon üretimi, node akışı ve dungeon UI
│   │   ├── Encounters/            Mini oyunlar ve encounter panelleri
│   │   ├── Item/                  Item database, inventory, equip ve merge
│   │   └── ShopMenu/              Augment mağazası ve reroll akışı
│   ├── MainMenuScene/              Ana menü ve dungeon seçim ekranları
│   ├── Prefabs/                    Tekrar kullanılabilir prefab'lar
│   └── Shared/                     Ortak bileşenler
├── Scenes/
│   ├── 0_Bootstrap.unity
│   ├── 1_MainMenu.unity
│   ├── 2_GamePlayScene.unity
│   └── SampleScene.unity
├── Resources/                      Runtime Resources.Load içerikleri
└── Sprites/                        2D görsel içerikler
```


## Kontroller

| Girdi | Kullanım |
|---|---|
| Q | Quick Attack |
| W | Normal Attack |
| E | Strong Attack |
| D | Defend |
| R | Ultimate |
| 1-4 | Augment skill slotları |
| Space | Dungeon'da bir sonraki node'a ilerleme |
| Mouse | UI butonları, kartlar, ekipman ve encounter seçimleri |

## Geliştirme Notları

- Oyun akışı runtime üretilen listeler ve singleton servisler üzerinden ilerler.
- Inspector referansları özellikle UI, prefab ve VFX bileşenlerinde önemlidir.
- `Resources` altında kullanılan içerik yolları değiştirilirse ilgili `Resources.Load` çağrıları da güncellenmelidir.
- Yeni bir düşman görseli eklemek için `EnemyVisualData` asset'i ve ilgili prefab bağlantısı hazırlanmalıdır.
- Yeni bir augment eklemek için `Augment` sınıfından türeyen bir sınıf oluşturulmalı ve `AugmentGenerator` listesine eklenmelidir.
- Yeni bir status effect için `StatusEffectBase`, `StatusFactory`, UI ikonları ve gerekiyorsa `VFXManager` eşlemesi güncellenmelidir.
- Yeni bir item tanımı için `ItemData` veya merkezi `ItemDefinitions` akışı kullanılmalıdır.
- Sahneler arası testlerde Bootstrap sırasını atlamak `RunData`, `PlayerData` veya `SaveManager` bağımlılıklarının hazır olmamasına neden olabilir.

## Roadmap

Planlanan veya genişletilmeye uygun alanlar:

- Daha fazla dungeon area ve dungeon çeşidi
- Daha fazla düşman davranışı ve boss mekanikleri
- Element kombinasyonları ve fusion augment'lar
- Yeni encounter kategorileri: reward, curse, risk combat ve event seçimleri
- Daha kapsamlı loot drop ve item database sistemi
- Save/load doğrulaması ve kayıt sürümleme
- Ses, müzik ve daha geniş combat feedback katmanı
- Tutorial ve onboarding akışı
- Build alma, performans profilleme ve otomatik test kapsamının genişletilmesi
- Yeni platformlar için build pipeline'ı

## Proje Vizyonu

Bu proje; tekrar oynanabilir dungeon akışı, karar odaklı kaynak yönetimi, kombinlenebilir augment'lar ve etkili savaş feedback'ini tek bir 2D roguelite RPG döngüsünde birleştirmeyi amaçlar. Kod tarafında sistemlerin `RunData`, `BattleSystem`, `NodeFlowController`, `AugmentManager`, `InventorySystem` ve encounter domain sınıfları etrafında ayrıştırılması; yeni içeriklerin mevcut akışı bozmadan eklenebilmesi için temel bir genişleme zemini sağlar.
