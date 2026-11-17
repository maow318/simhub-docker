# SIMHub NAS — Docker

[English](#english) · [日本語](#日本語) · [Русский](#русский)

[Official website](https://simhubkit.com) · [Docker image](https://github.com/users/maow318/packages/container/package/simhub-docker)

---

## Docker Compose

[Download / ダウンロード / Скачать compose.yaml](compose.yaml)

Copy this configuration into your NAS Compose editor, or save it as `compose.yaml` in a dedicated project directory. / NAS の Compose エディターに貼り付けるか、専用ディレクトリに `compose.yaml` として保存してください。 / Вставьте конфигурацию в редактор Compose на NAS или сохраните её как `compose.yaml` в отдельном каталоге проекта.

```yaml
services:
  simhub:
    image: ghcr.io/maow318/simhub-docker:1.1.2
    pull_policy: missing
    container_name: simhub-nas
    restart: unless-stopped
    network_mode: host
    volumes:
      - ./data:/data
      - /dev/bus/usb:/dev/bus/usb
    device_cgroup_rules:
      - "c 189:* rmw"
    environment:
      TZ: "${TZ:-UTC}"
```

Linux amd64 and arm64; Docker selects the matching architecture automatically. / Linux amd64・arm64 対応。Docker が適切なアーキテクチャを自動選択します。 / Linux amd64 и arm64; Docker автоматически выбирает подходящую архитектуру.

---

## English

### About SIMHub NAS

SIMHub NAS is self-hosted software for managing supported SIM/eSIM devices from a web interface on your NAS. Messaging and calling features depend on compatible hardware, the SIM plan, carrier support and network conditions. SIMHub is a management tool, not a mobile network operator.

This repository provides product information for the Docker distribution. It does not publish SIMHub's proprietary backend source code, private keys or personal NAS data. Public availability of the image does not make SIMHub open-source or grant a free license. Third-party open-source components remain subject to their respective licenses and notices.

### Regional availability

**SIMHub NAS is intended for use and supported only in countries and regions outside mainland China. Mainland China is outside our supported market and deployment scope. Please do not purchase or deploy it for use in mainland China.**

Availability outside mainland China is not universal: local law, applicable sanctions, payment-provider restrictions and carrier policies may impose further limits. A publicly accessible download or a successful connection does not establish eligibility or official support. Confirm availability and compatibility before purchasing.

### Deployment and licensing

- This image supports **Linux x86-64 / amd64 and arm64** systems with Docker, and Docker selects the matching architecture automatically. 32-bit ARM is not supported.
- A valid SIMHub license and an internet connection are required for activation and periodic license validation. A lifetime license does **not** mean permanently offline operation; its scope is defined in the official terms.
- Keep your persistent data directory and back it up before updates. Do not expose an unconfigured management interface to the public internet.
- Confirm hardware and carrier compatibility before relying on any feature. No claim of compatibility with every NAS, SIM, reader or carrier is made.

### Upgrade to 1.1.2

This release is an **attempted fix for AT&T Wi-Fi Calling failing to come up** (AT&T and AT&T-network MVNOs such as Red Pocket GSMA). It replaces 1.1.1, which has been withdrawn.

- **New in 1.1.2 — AT&T registration over TCP:** in a real-card test of 1.1.1, ISIM authentication and IMS IPsec succeeded, but the second REGISTER (about 1.9 KB) never got a reply because it was split into IP fragments over UDP. For AT&T it now goes over **TCP inside IMS IPsec**, without fragmentation. On a 480 ("registration still in progress") SIMHub waits 32 seconds and retries once.
- **AT&T Wi-Fi Calling:** IMS authentication runs on the card's **ISIM** over its own logical channel (card readers and modems); previously the card returned SW 9862.
- **AT&T IMS registration:** REGISTER and IMS IPsec follow AT&T's requirements (AT&T identity domains, hmac-sha-1-96 + aes-cbc only, protected port 6000, AT&T Contact / header format).
- Red Pocket SIMs recognised by SPN or GID1; clearer error when the card rejects IMS authentication.
- No changes for other carriers. Activation-code support and +86 call/SMS blocking are retained.

**Not yet verified with a real AT&T card.** The line needs Wi-Fi Calling enabled with a registered E911 address, and VoWiFi traffic should leave from a US IP.

**Reader limit: at most two USB CCID readers can be active at the same time.** Additional readers wait for a slot. If your current installation uses more than two readers simultaneously, review this limit before upgrading. This update does not delete their configuration.


**This release retains v2 license-trust support.** `0.4.9-secure1` cannot read the new trust format. Production switched to v2 on 8 November 2026. Existing secure1 installations must upgrade to 1.1.2. After cutover, the operator no longer needs to renew the signed public-key list every month; normal online license checks, annual expiry and refund/revocation enforcement remain.

For an existing installation, stop the old instance and back up its data. Keep the same Compose project directory and the exact existing `/data` mount (`./data` in this example); change only the image tag to `1.1.2`. Then run:

```bash
docker compose pull
```

```bash
docker compose up -d
```

Confirm the application reports `1.1.2` and the existing license remains active. Do not delete the data directory, use volume-removal options or start a second instance against the same data. Once v2 trust has been accepted, do not downgrade to secure1 or restore an older trust cache.

### Quick installation

1. In fnOS, open **Docker → Compose → Add project**. Choose a new project name and a dedicated folder, select **Create docker-compose.yml**, and paste the configuration above. Start the project. Other NAS systems can use their equivalent Compose editor.
2. For command-line installation, save [compose.yaml](compose.yaml), open a terminal in its directory and run:

```bash
docker compose up -d
```

3. Open `http://<NAS-LAN-IP>:8080` (replace the placeholder with your NAS address), set your own administrator password and activate with your own SIMHub license key. Browser calling requires HTTPS, normally `https://<NAS-LAN-IP>:8443`, and microphone permission. Verify the NAS address before trusting its self-signed certificate.

**Before starting:** use a supported USB device, ensure `/dev/bus/usb` exists on the NAS, and keep ports **8080 and 8443** free. Host networking is intentional; no `ports:` mapping is needed. Set `TZ` to your local IANA time zone if desired; the default is UTC. Do not run two SIMHub instances against the same data or USB devices.

**Data and updates:** `./data` is stored alongside the Compose file. Keep and back up this directory. This example is for a new installation; do not point an existing deployment at an empty directory by mistake. Updates are manual: review the release notes, back up data, stop the old instance, change the image tag to the intended release, then run `docker compose pull` and `docker compose up -d`. The fixed tag does not automatically track future versions.

**Download errors:** if GHCR returns `unauthorized` or `denied`, the image may not yet be publicly accessible. Contact support; end users should never need the publisher's GitHub token. A timeout can instead indicate a network or registry problem.

### Responsible use and important limitations

Use only SIMs, eSIM profiles and devices that you own or are authorized to manage. Comply with local telecommunications, privacy and recording laws, and your carrier's terms. Do not use SIMHub for fraud, unsolicited messaging, unauthorized interception or bypassing legal, carrier or licensing restrictions. Carrier charges may apply.

**Do not rely on SIMHub for emergency calls, life-safety services or uninterrupted communications.** Keep an independent means of communication. Feature availability can change with hardware, carrier or network conditions.

This page is an informational summary, not a replacement for the [Terms](https://simhubkit.com/terms.html), [Privacy Policy](https://simhubkit.com/privacy.html) or [Refund Policy](https://simhubkit.com/refund.html). Nothing here excludes mandatory consumer rights or liability that cannot lawfully be excluded. Publishing an image is not a guarantee of universal compatibility or completion of every real-hardware test.

Operator: **SIMKIT LTD**. Support: **simkit@simhubkit.com**.

---

## 日本語

### SIMHub NAS について

SIMHub NAS は、NAS 上の Web インターフェースから対応する SIM／eSIM デバイスを管理するセルフホスト型ソフトウェアです。メッセージ・通話機能の利用可否は、対応ハードウェア、SIM の契約内容、通信事業者の対応、およびネットワーク環境によって異なります。SIMHub は管理ツールであり、移動体通信事業者ではありません。

このリポジトリは Docker 版の製品情報を提供します。SIMHub 独自のバックエンドソースコード、秘密鍵、個人の NAS データは公開しません。イメージの公開は、SIMHub のオープンソース化や無償ライセンスの付与を意味しません。同梱する第三者のオープンソースコンポーネントには、それぞれのライセンスと表示条件が適用されます。

### 提供地域

**SIMHub NAS は、中国本土以外の国・地域での利用のみを対象とし、サポートします。中国本土は販売・サポートおよび導入対象地域に含まれません。中国本土での利用を目的とした購入・導入はお控えください。**

中国本土以外のすべての地域で利用できるとは限りません。現地法、適用される制裁、決済事業者の制限、通信事業者の方針により、追加の制限が生じる場合があります。ダウンロードや接続ができても、利用資格や公式サポートが認められたことにはなりません。購入前に提供状況と互換性をご確認ください。

### 導入とライセンス

- 本イメージは Docker を利用できる **Linux x86-64／amd64 および arm64** に対応しており、Docker が適切なアーキテクチャを自動選択します。32 ビット ARM には対応していません。
- 有効な SIMHub ライセンスが必要です。初回認証および定期的なライセンス確認にはインターネット接続が必要です。永久ライセンスは、永久にオフラインで利用できることを意味しません。適用範囲は公式利用規約に定められます。
- 永続データディレクトリを保持し、更新前にバックアップしてください。初期設定が済んでいない管理画面をインターネットに公開しないでください。
- 利用前に機器・通信事業者との互換性を確認してください。すべての NAS、SIM、リーダー、通信事業者への対応を保証するものではありません。

### 1.1.2 への更新

本リリースは **AT&T の Wi-Fi Calling が接続できない問題の修正を試みたもの**です（AT&T および Red Pocket GSMA など AT&T 網の MVNO）。1.1.1 は取り下げ、本リリースに置き換えました。

- **1.1.2 の新機能 — AT&T の登録を TCP で送信：** 1.1.1 の実カード試験では ISIM 認証と IMS IPsec は成功したものの、2 回目の REGISTER（約 1.9 KB）が UDP で IP フラグメントに分割され、応答がありませんでした。AT&T では **IMS IPsec 内の TCP** で送るよう変更し、分割されなくなりました。480（前回の登録がまだ処理中）の場合は 32 秒待って 1 回再試行します。
- **AT&T Wi-Fi Calling：** IMS 認証をカードの **ISIM** で、専用の論理チャネル経由で行います（カードリーダー・モデムとも）。以前はカードが SW 9862 を返していました。
- **AT&T IMS 登録：** REGISTER と IMS IPsec を AT&T の要件に合わせました（AT&T 識別ドメイン、hmac-sha-1-96 + aes-cbc のみ、保護ポート 6000、AT&T 形式の Contact / ヘッダー）。
- SPN または GID1 で Red Pocket SIM を判定。カードが IMS 認証を拒否した場合のエラー表示を改善。
- 他のキャリアへの変更はありません。アクティベーションコード対応と +86 宛て通話・SMS のブロックは維持。

**AT&T 実カードでは未検証です。** 回線で Wi-Fi Calling と E911 住所の登録が必要で、VoWiFi の通信は米国 IP から出してください。

**USB CCID リーダーの同時利用は最大2台です。** 追加のリーダーは空きができるまで待機します。現在3台以上を同時利用している場合は、更新前にこの制限をご確認ください。設定は削除しません。


**本リリースもライセンス信頼リスト v2 に対応しています。** `0.4.9-secure1` は新形式に対応していません。本番環境は2026年11月8日に v2 へ切り替わりました。既存の secure1 は 1.1.2 へ更新してください。切り替え後は運営者による署名付き公開鍵リストの毎月の更新が不要になりますが、通常のオンライン認証、年額ライセンスの有効期限、返金・取り消しの確認は継続します。

既存環境では、旧インスタンスを停止してデータをバックアップします。同じ Compose プロジェクトのディレクトリと既存の `/data` マウント先（この例では `./data`）を保持し、イメージタグだけを `1.1.2` に変更して実行してください。

```bash
docker compose pull
```

```bash
docker compose up -d
```

アプリのバージョンが `1.1.2` で、既存ライセンスが有効なことを確認してください。データの削除、ボリュームを削除するオプションの使用、同じデータでの二重起動は避けてください。v2 を受け入れた後は secure1 へのダウングレードや古い信頼キャッシュの復元をしないでください。

### 簡単なインストール手順

1. fnOS の **Docker → Compose → プロジェクト追加**で新しいプロジェクト名と専用フォルダーを指定します。**docker-compose.yml を作成**を選び、上記の設定を貼り付けて起動します。他の NAS では対応する Compose エディターを利用してください。
2. コマンドラインを使う場合は [compose.yaml](compose.yaml) を保存し、そのディレクトリで実行します。

```bash
docker compose up -d
```

3. `http://<NAS-LAN-IP>:8080` の `<NAS-LAN-IP>` を NAS の LAN アドレスに置き換えて開き、自分の管理者パスワードを設定し、自分の SIMHub ライセンスキーで認証します。ブラウザー通話には HTTPS（通常は `https://<NAS-LAN-IP>:8443`）とマイクの許可が必要です。自己署名証明書を信頼する前に NAS のアドレスを確認してください。

**起動前：** 対応 USB デバイスを使用し、NAS に `/dev/bus/usb` が存在すること、ポート **8080 と 8443** が空いていることを確認してください。ホストネットワークを使用するため `ports:` の追加は不要です。`TZ` は地域の IANA タイムゾーンに変更できます（既定値は UTC）。同じデータや USB デバイスを複数の SIMHub で同時に使用しないでください。

**データと更新：** `./data` は Compose ファイルと同じディレクトリ内に作成されます。削除せず、バックアップしてください。この設定は新規導入用です。既存環境を誤って空のデータディレクトリに切り替えないでください。更新は手動です。リリースノートを確認し、バックアップして旧インスタンスを停止し、イメージタグを対象の版に変更してから `docker compose pull` と `docker compose up -d` を実行します。固定タグは将来の版を自動追従しません。

**ダウンロードエラー：** `unauthorized` や `denied` はイメージがまだ一般公開されていない場合にも発生します。サポートへお問い合わせください。利用者が公開者の GitHub トークンを使う必要はありません。タイムアウトはネットワークやレジストリの問題の場合があります。

### 適切な利用と重要な制限

ご自身が所有するか、管理権限を持つ SIM、eSIM プロファイル、デバイスのみをご利用ください。現地の電気通信・プライバシー・録音関連法令および通信事業者の利用条件を遵守してください。詐欺、迷惑メッセージ、不正な傍受、法令・通信事業者・ライセンス上の制限の回避に利用しないでください。通信料金が発生する場合があります。

**緊急通報、人命に関わるサービス、常時接続が不可欠な通信を SIMHub に依存させないでください。** 別の通信手段を確保してください。機能の利用可否は機器、通信事業者、ネットワーク環境の変化に影響されます。

本ページは情報提供のための概要であり、[利用規約](https://simhubkit.com/terms.html)、[プライバシーポリシー](https://simhubkit.com/privacy.html)、[返金ポリシー](https://simhubkit.com/refund.html)に代わるものではありません。法令上認められる消費者の権利や、法的に免除できない責任を制限するものではありません。イメージの公開は、あらゆる環境での互換性や、すべての実機試験の完了を保証するものではありません。

運営：**SIMKIT LTD**。お問い合わせ：**simkit@simhubkit.com**。

---

## Русский

### О SIMHub NAS

SIMHub NAS — программное обеспечение для самостоятельного размещения на NAS и управления поддерживаемыми SIM/eSIM-устройствами через веб-интерфейс. Работа сообщений и звонков зависит от совместимого оборудования, тарифа SIM-карты, поддержки оператора и условий сети. SIMHub — инструмент управления, а не оператор мобильной связи.

Этот репозиторий содержит информацию о Docker-версии продукта. Здесь не публикуются проприетарный исходный код серверной части SIMHub, закрытые ключи или личные данные NAS. Публичный доступ к образу не делает SIMHub программой с открытым исходным кодом и не предоставляет бесплатную лицензию. Сторонние компоненты с открытым исходным кодом распространяются на условиях собственных лицензий и уведомлений.

### Региональная доступность

**SIMHub NAS предназначен для использования и поддерживается только в странах и регионах за пределами материкового Китая. Материковый Китай не входит в поддерживаемый рынок и область развёртывания. Не приобретайте и не развёртывайте продукт для использования в материковом Китае.**

Доступность за пределами материкового Китая не является безусловной: местное законодательство, применимые санкции, ограничения платёжных провайдеров и правила операторов могут вводить дополнительные ограничения. Возможность скачать образ или установить соединение не подтверждает право на использование либо наличие официальной поддержки. До покупки уточните доступность и совместимость.

### Развёртывание и лицензирование

- Образ поддерживает системы **Linux x86-64 / amd64 и arm64** с Docker; Docker автоматически выбирает подходящую архитектуру. 32-битный ARM не поддерживается.
- Требуется действующая лицензия SIMHub. Для активации и периодической проверки лицензии необходимо подключение к интернету. Бессрочная лицензия **не** означает бессрочную автономную работу; её условия определяются официальными условиями использования.
- Сохраняйте каталог постоянных данных и создавайте резервную копию перед обновлением. Не открывайте доступ к ненастроенному интерфейсу управления из интернета.
- Перед использованием убедитесь в совместимости оборудования и оператора. Совместимость со всеми NAS, SIM-картами, считывателями и операторами не гарантируется.

### Обновление до 1.1.2

Этот выпуск — **попытка исправить неработающий Wi-Fi Calling у AT&T** (AT&T и MVNO в сети AT&T, например Red Pocket GSMA). Он заменяет отозванный выпуск 1.1.1.

- **Новое в 1.1.2 — регистрация AT&T по TCP:** при проверке 1.1.1 на реальной карте аутентификация ISIM и IMS IPsec прошли, но на второй REGISTER (около 1,9 КБ) ответа не было — по UDP он делился на IP-фрагменты. Для AT&T он теперь идёт по **TCP внутри IMS IPsec** без фрагментации. При ответе 480 (предыдущая регистрация ещё не завершена) SIMHub ждёт 32 секунды и повторяет попытку один раз.
- **AT&T Wi-Fi Calling:** IMS-аутентификация выполняется на **ISIM** карты через отдельный логический канал (считыватели и модемы); раньше карта возвращала SW 9862.
- **Регистрация IMS у AT&T:** REGISTER и IMS IPsec приведены к требованиям AT&T (домены AT&T, только hmac-sha-1-96 + aes-cbc, защищённый порт 6000, формат Contact / заголовков AT&T).
- Red Pocket определяется по SPN или GID1; понятная ошибка, если карта отклоняет IMS-аутентификацию.
- Для других операторов изменений нет. Коды активации и блокировка звонков/SMS на +86 сохранены.

**На реальной карте AT&T ещё не проверено.** На линии нужен включённый Wi-Fi Calling с адресом E911, трафик VoWiFi должен выходить с IP США.

**Одновременно могут работать не более двух USB CCID-считывателей.** Остальные ожидают свободного места. Если сейчас используются более двух считывателей одновременно, учтите это ограничение перед обновлением. Их настройки не удаляются.


**Этот выпуск сохраняет поддержку списка доверенных ключей v2.** Версия `0.4.9-secure1` не поддерживает новый формат. Рабочий сервер перешёл на v2 8 ноября 2026 года. Существующие установки secure1 необходимо обновить до 1.1.2. После перехода оператору больше не потребуется ежемесячно продлевать подписанный список открытых ключей; обычные онлайн-проверки лицензий, срок годовой лицензии и проверки возвратов и отзыва сохраняются.

При обновлении существующей установки остановите старый экземпляр и создайте резервную копию данных. Сохраните прежний каталог проекта Compose и точное подключение существующего каталога `/data` (в этом примере — `./data`); измените только тег образа на `1.1.2`. Затем выполните:

```bash
docker compose pull
```

```bash
docker compose up -d
```

Убедитесь, что приложение показывает версию `1.1.2`, а существующая лицензия остаётся активной. Не удаляйте данные, не используйте параметры удаления томов и не запускайте второй экземпляр с теми же данными. После принятия списка v2 не возвращайтесь к secure1 и не восстанавливайте старый кэш доверенных ключей.

### Быстрая установка

1. В fnOS откройте **Docker → Compose → Добавить проект**, задайте новое имя и отдельную папку. Выберите создание `docker-compose.yml`, вставьте конфигурацию выше и запустите проект. На других NAS используйте соответствующий редактор Compose.
2. Для установки через терминал сохраните [compose.yaml](compose.yaml) и выполните в его каталоге:

```bash
docker compose up -d
```

3. Откройте `http://<NAS-LAN-IP>:8080`, заменив `<NAS-LAN-IP>` локальным адресом NAS. Задайте собственный пароль администратора и активируйте программу своим ключом SIMHub. Для звонков в браузере необходимы HTTPS (обычно `https://<NAS-LAN-IP>:8443`) и разрешение на микрофон. Прежде чем доверять самоподписанному сертификату, проверьте адрес NAS.

**Перед запуском:** подключите поддерживаемое USB-устройство, проверьте наличие `/dev/bus/usb` на NAS и доступность портов **8080 и 8443**. Используется сеть хоста, поэтому раздел `ports:` не нужен. При необходимости измените `TZ` на свой часовой пояс IANA; по умолчанию используется UTC. Не запускайте два экземпляра SIMHub с одними и теми же данными или USB-устройствами.

**Данные и обновления:** каталог `./data` расположен рядом с файлом Compose. Сохраняйте его и делайте резервные копии. Пример предназначен для новой установки: не переключайте существующую систему на пустой каталог по ошибке. Обновление выполняется вручную: прочтите примечания к выпуску, сделайте резервную копию, остановите старый экземпляр, замените тег образа нужной версией, затем выполните `docker compose pull` и `docker compose up -d`. Фиксированный тег не переключается на будущие версии автоматически.

**Ошибки загрузки:** `unauthorized` или `denied` могут означать, что образ ещё не доступен публично. Обратитесь в поддержку; пользователям не нужен GitHub-токен издателя. Тайм-аут также может быть вызван сетью или реестром образов.

### Ответственное использование и важные ограничения

Используйте только SIM-карты, профили eSIM и устройства, которыми вы владеете или которыми вправе управлять. Соблюдайте местные нормы в области связи, конфиденциальности и записи разговоров, а также условия своего оператора. Не используйте SIMHub для мошенничества, нежелательных рассылок, несанкционированного перехвата или обхода правовых, операторских либо лицензионных ограничений. Оператор может взимать плату за услуги связи.

**Не полагайтесь на SIMHub для экстренных вызовов, систем обеспечения безопасности жизни или связи, требующей непрерывной доступности.** Сохраняйте независимый способ связи. Доступность функций может изменяться вместе с оборудованием, условиями оператора или сети.

Эта страница носит информационный характер и не заменяет [Условия использования](https://simhubkit.com/terms.html), [Политику конфиденциальности](https://simhubkit.com/privacy.html) и [Политику возврата средств](https://simhubkit.com/refund.html). Она не ограничивает обязательные права потребителей или ответственность, которую нельзя исключить по закону. Публикация образа не гарантирует универсальную совместимость или завершение всех испытаний на реальном оборудовании.

Оператор: **SIMKIT LTD**. Поддержка: **simkit@simhubkit.com**.
