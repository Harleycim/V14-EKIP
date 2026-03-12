# Harley Ekip V14

Discord.js v14 tabanlı bot + web panel botu keyfime göre paylaşıcam ama bot sağlam oldu

## Gereksinimler

- Node.js (LTS önerilir)
- MongoDB
- Discord Bot Token

## Kurulum

1) Bağımlılıkları yükle

```bash
npm install
```

2) `config.js` ayarla

`config.js` içinde en az şu alanlar gerekli:

- `token`: Discord bot token
- `mongoUri`: Mongo bağlantısı
- `botApiKey`: Panel/API koruması için anahtar
- `botApiPort`: API portu (default: 3001)
- `panelPort`: Panel portu (default: 3000)

> ÖNEMLİ: `config.js` içine token/anahtar koyuyorsan, GitHub’a pushlamadan önce dosyayı **.gitignore**’a ekle veya boş bir `config.example.js` kullan.

3) Çalıştır

### Normal çalıştırma

```bash
npm start
```

### PM2 ile çalıştırma

```bash
npm run pm2:start
npm run pm2:logs
```

Restart:

```bash
npm run pm2:restart
```

## Web Panel

Panel dosyaları: `panel/index.html`

Panel üzerinden;

- API URL / API KEY / Guild ID ile bağlanma
- Dashboard (üye/boost/kanal/rol, ping/uptime, kurulum durumu)
- Kayıt ayarları (mode/kanal/rol)
- Guard (Anti-Raid) ayarları
- Log kurulumu
- Bot durum (presence) ayarı

Paneli başlatma:

```bash
npm run start:panel
```

## Bot API

Bot, Express API’yi de ayağa kaldırır.

- Health:
  - `GET /health`
- Guild settings:
  - `GET /guilds/:guildId/settings`
  - `PATCH /guilds/:guildId/settings`
- Guild overview (Dashboard):
  - `GET /guilds/:guildId/overview`
- Log setup:
  - `POST /guilds/:guildId/logs/setup`

Tüm guild endpoint’leri `x-api-key` header ister:

- `x-api-key: <botApiKey>`

## Kayıt Sistemi

Ayarlar:

- `register.registerChannelId`
- `register.registerRoleId`
- (opsiyonel) `register.unregisteredRoleId`
- `register.mode`: `buttons` veya `reactions`

Davranış:

- Kayıt kanal/rol set edilince bot otomatik kayıt mesajı gönderir.
- Butona veya ✅ reaksiyona basılınca kullanıcıya kayıt rolü verilir.
- `unregisteredRoleId` ayarlıysa kayıt olunca otomatik kaldırılır.

Manuel başlatma komutu:

- `.kayıtbaşlat` / `.kayitbaslat`

## Guard (Anti-Raid)

Guild bazlı ayarlar: `guard`

- `enabled`
- `windowMs`
- `threshold`
- `timeoutMs`

Hızlı join tespit edilince yeni gelen üyeye timeout uygular.

## Log Sistemi

- Panelde **Logları Kur** butonu:
  - `POST /guilds/:guildId/logs/setup`
- Tek kategori altında event bazlı kanallar oluşturur.
- Event kanal mapping: `logsV2.channels[eventKey]`
- Eski sistem fallback: `logs.*`

> Not: Audit log bilgisi ("kim yaptı") için botta **View Audit Log** yetkisi gerekir.

## Bot Durumu (Presence)

Panel > Genel > **Bot Durumu**

Ayarlar: `presence`

- `status`: `online | idle | dnd | invisible`
- `activityType`: `PLAYING | LISTENING | WATCHING | COMPETING`
- `activityText`: durum yazısı

Bot açılışta ve 60sn’de bir bu ayarı okuyup uygular.

## Lisans

MIT License. Detaylar için `LICENSE` dosyasına bak.
