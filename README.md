# Nexus

> Seçim ekranındaki Yin ve Yang'ın içinde nokta yok. Çünkü nokta, sizsiniz.

2D pixel art aksiyon-RPG platformer · Unreal Engine 5 · Deep Jam II başvurusu

**Durum:** Ön prodüksiyon. Hareket ve animasyon altyapısı kuruldu, dövüş sistemi sırada. Bu sayfa projenin vizyonunu, şu anki gerçek durumunu ve planını içerir.

---

## Konsept

Nexus'un seçim ekranında Yin ve Yang vardır, ama klasik simgenin aksine içlerinde nokta yoktur: bir yarı saf beyaz, diğeri saf siyah. Birini seçtiğiniz an, seçtiğiniz yarının içinde karşıt renkte küçük bir tomoe belirir. O nokta oyuncudur: Yang'ın içindeki kötülük de Yin'in içindeki iyilik de sizsiniz.

- **Nokta sizsiniz.** Seçiminiz saf karaktere karşıtını ekler; oyuncu iki saf dünyanın dengesini bozan unsurdur.
- **Hiçbir düşmanlık gerekçesiz değildir.** Yang'ın dünyası içimizdeki karanlığı, Yin'in dünyası içimizdeki ışığı hissettiği için bize saldırır.
- **İki yön, bir buluşma.** Yang aydınlıktan karanlığa (soldan sağa), Yin karanlıktan aydınlığa (sağdan sola) ilerler; ikisi ortadaki Nexus'ta karşılaşır.
- **Final aynı anın iki perspektifi.** Yang'da yendiğiniz son düşman, Yin'de Nexus'a ulaştığınız andaki sizsiniz. Yang'ın yükselip ışın topladığı ve patlattığı an iki koşuda da aynı sahnedir: Yang'da ekran kararır, Yin'de kararmaz, Yin kaçıp savaşı sürdürür.
- **Güç alma biçimi tarafı belirler.** Yendiğiniz mistik yaratıkları özümseyip aktif yetenek, pasif ve dönüşüm formu olarak kullanırsınız: seri ve hızlı Yang'da onların saygısını kazanıp anlaşarak, ağır ve güçlü Yin'de onları zincirleyerek.
- **Dört çekirdek son.** İki kapışmanın sonucu belirler: ikisini de kazanırsanız iki taraf birbirini yok eder, ikisini de kaybederseniz üst tanrılara karşı birleşirler, biri kazanıp biri kaybederseniz kazanan taraf hükmeder.

---

## Şu anki durum

- [x] Enhanced Input ile hareket ve zıplama
- [x] Hareket yönüne göre karakter dönüşü
- [x] PaperZD animasyon state machine: Idle / Running / Jump
- [x] Animation Source'ta tanımlı sekanslar: Attack1-3, Hurt, Idle, Jump (Start/Loop/End), Run
- [x] Ekip arkadaşım Azra'nın özgün Yang idle animasyonu aynı animasyon hattına eklendi ve test edildi
- [ ] 3'lü kombo, hasar, hit react ve düşman
- [ ] Hit stop, camera shake, knockback (game feel katmanı)
- [ ] Parry, dash, boss
- [ ] Tomoe'lu seçim ekranı

**Not:** Bu aşamadaki altyapı (hareket, AnimBP, state machine) bir Unreal Engine 5 2D Blueprint kursu takip edilerek kuruldu ve şu an kurs asset'leriyle çalışıyor. Özgün sistemler (kombo çerçevesi, game feel, boss, seçim ekranı) sırada.

---

## Teknik altyapı

### Girdi ve hareket
![Hareket ve zıplama girdisi](screenshots/bp-input-eventgraph.png)

`IA_Move` yön girdisini karaktere iletir, `IA_Jump` zıplamayı başlatır. `BeginPlay`'de mapping context eklenir.

![Mapping context](screenshots/bp-add-mapping-context.png)

Oyuncu kontrolcüsü üzerinden Enhanced Input Local Player Subsystem'e `IMC_RedHood` eklenir.

![Yön çevirme](screenshots/bp-rotation.png)

İvmenin X bileşenine göre kontrol rotasyonu 0° ya da 180° olarak ayarlanır, böylece karakter sola ya da sağa bakar.

### Animasyon veri akışı (PaperZD)
![AnimBP olay grafiği](screenshots/abp-eventgraph.png)
![Değişkenlerin saklanması](screenshots/abp-store-variables.png)
![Değişkenlerin güncellenmesi](screenshots/abp-update-variables.png)

Başlangıçta karakter referansı saklanır; her tick'te hızın XY uzunluğu (`IsMoving`) ve düşme durumu (`IsFalling`) okunup animasyon grafiğine aktarılır.

![Locomotion state machine](screenshots/abp-locomotion-state-machine.png)

Idle, Running ve Jump durumları arasındaki geçişler PaperZD state machine'iyle yönetilir.

### Animation Source ve asset değişimi
![Animation Source](screenshots/as-redhood-animation-source.png)

Karakterin tüm animasyonları Animation Source üzerinden veriye bağlı tanımlanır. Azra'nın özgün Yang idle'ı (`yang_FINAL-Sheet1`) "NewIdle" olarak aynı hatta test edildi: sistemler değişmeden, sprite'lar yenisiyle değiştirilebilir.

### Proje yapısı
![İçerik yapısı](screenshots/content-structure.png)
![Asset klasörleri](screenshots/content-assets.png)
![Karakter klasörü](screenshots/content-characters.png)

---

## Vertical slice kapsamı (8 hafta)

- 8-12 dakikalık cilalı dikey kesit
- 5 alan (onboarding, Arena A, geçiş, Arena B, boss kapısı) + 1 boss
- 27 animasyon: oyuncu 13, temel düşman 6, boss 8 (yaklaşık 178 frame); elit düşman palet değişimiyle
- Boss = Nexus kapışması (Yang'a karşı Yin'in gölgesi, 2 faz)

## 8 haftalık plan

| Hafta | Hedef |
|---|---|
| 1 | Çekirdek hareket (coyote time, jump buffer), oyuncu temel animasyonları |
| 2 | Dövüş çekirdeği: LLL/LLH kombo, dash + i-frame, hit stop, knockback |
| 3 | Düşman AI ve elit varyant, hasar/can sistemi, ilk dış playtest |
| 4 | Parry, stagger, 5 alanlı seviyenin greybox'ı, checkpoint; yeni mekanik eklemeye kapanır |
| 5 | Boss (Nexus kapışması), final sekansı; yeni içerik eklemeye kapanır |
| 6 | Gerçek sanatın entegrasyonu, parallax, seviye akışı |
| 7 | VFX, ses, UI, tomoe'lu seçim ekranı, özümseme VFX'inin minik hali |
| 8 | Dış playtest, balans, paketleme, trailer |

---

## Asset ve yapay zeka beyanı

- **Geçici (placeholder) asset'ler:** Stephen Ulibarri'nin UE5 Blueprint kursunun ders asset'leri (RedHood, Skeleton, Dungeon, Torch, Coin, UI). Nihai oyunda kullanılmayacak, Azra'nın özgün sanatıyla değiştirilecek.
- **Özgün sanat:** Karakter ve ortam sanatı Azra tarafından üretiliyor; şu ana kadar bu repoda yalnızca Yang idle çalışması test edildi.
- **Yapay zeka kullanımı:** Konsept geliştirme, kapsam planlaması ve başvuru/README metinlerinin taslaklanmasında LLM tabanlı asistanlar (Claude, ChatGPT) kullanıldı; kararları ve metinlerin son halini ekip verdi.

## Ekip

- **Niyazi:** Game Designer, Gameplay Programmer (Unreal Engine 5), proje yönetimi
- **Azra:** Art Director, Karakter Tasarımcısı, 2D Pixel Artist

Ekip, Kantei Dublaj topluluğunda birlikte seslendirme ve video prodüksiyonu işleri yaptı.

## Lisans

Tüm hakları saklıdır. Bu depo bir portfolyo ve başvuru sayfasıdır, oyun kaynak kodu içermez.
