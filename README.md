# IRS Student Development Web App

GitHub Pages üzerinde çalışan, Firebase Authentication + Cloud Firestore ile kalıcı öğrenci ve değerlendirme verisi saklayabilen statik web uygulamasıdır.

## Özellikler
- Öğrenci ekleme / düzenleme / silme
- Sınıf, departman, Explorer / Contributor / Owner / Lead seviyesi
- Haftalık hızlı tikleme değerlendirmesi
- 0–3 sorumluluk, inisiyatif, takım çalışması, proje üretimi, liderlik puanları
- Otomatik haftalık skor (maksimum 26)
- Son 4 değerlendirmeye dayalı seviye ilerleme göstergesi
- Risk / dikkat listesi
- Değerlendirme geçmişi
- Mentor notu ve sonraki adım
- JSON yedek ve CSV dışa aktarma
- Firebase yapılandırılmadan önce localStorage demo modu
- Bordo / beyaz / siyah responsive arayüz

## 1. GitHub Pages
Bu klasördeki dosyaları repository köküne veya `/docs` klasörüne koyun.
GitHub > Settings > Pages bölümünden Branch deploy seçin.

## 2. Firebase Kurulumu
1. Firebase Console'da yeni proje oluşturun.
2. Build > Firestore Database > Create database.
3. Authentication > Sign-in method > Google seçeneğini etkinleştirin.
4. Project settings > Your apps > Web app oluşturun.
5. Verilen config değerlerini `firebase-config.js` içine yapıştırın.
6. `allowedEmails` listesine sistemi kullanacak hesapları ekleyin.
7. `firestore.rules` içindeki e-posta listesini aynı şekilde güncelleyin ve Firestore Rules ekranına yayınlayın.
8. Authentication > Settings > Authorized domains alanına GitHub Pages domaininizi (`kullanici.github.io`) ekleyin.

## 3. Veri Modeli
### students/{id}
`name, grade, department, level, startDate, active, note`

### evaluations/{id}
`studentId, date, 9 boolean gösterge, 5 adet 0-3 gelişim puanı, score, note, nextStep`

## 4. Puanlama
Hızlı göstergeler toplam 11 puan:
- Katılım +1
- Görev alma +1
- Görev tamamlama +2
- Zamanında tamamlama +1
- İnisiyatif +1
- Proje fikri +1
- Takıma destek +1
- Etkinlik / outreach +1
- Somut çıktı +2

Gelişim puanları toplam 15 puan: 5 alan x 0–3.
Toplam maksimum: 26.

Skor bir terfi kararı değildir. Arayüzdeki progress bar, son 4 değerlendirmeyi eşiklerle karşılaştıran bir karar desteğidir.

## 5. Önemli Güvenlik Notu
`allowedEmails` istemci tarafında yalnız UX filtresidir. Gerçek erişim kontrolü Firestore Security Rules ile yapılır. `firestore.rules` dosyasındaki allowlist'i mutlaka güncelleyin.

## 6. Yerel Test
Basit bir HTTP server ile açın; ES modules nedeniyle dosyayı `file://` ile açmayın.
Örnek: `python -m http.server 8000`

Firebase yapılandırılmadıysa uygulama localStorage modunda çalışır. Bu mod yalnız aynı tarayıcı/cihazda kalıcıdır.
