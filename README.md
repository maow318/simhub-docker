# SIMHub NAS — Docker

[English](#english) · [日本語](#日本語) · [Русский](#русский)

[Official website](https://simhubkit.com) · [Docker image](https://github.com/users/maow318/packages/container/package/simhub-docker)

---

## English

### About SIMHub NAS

SIMHub NAS is self-hosted software for managing supported SIM/eSIM devices from a web interface on your NAS. Messaging and calling features depend on compatible hardware, the SIM plan, carrier support and network conditions. SIMHub is a management tool, not a mobile network operator.

This repository provides product information for the Docker distribution. It does not publish SIMHub's proprietary backend source code, private keys or personal NAS data. Public availability of the image does not make SIMHub open-source or grant a free license. Third-party open-source components remain subject to their respective licenses and notices.

### Regional availability

**SIMHub NAS is intended for use and supported only in countries and regions outside mainland China. Mainland China is outside our supported market and deployment scope. Please do not purchase or deploy it for use in mainland China.**

Availability outside mainland China is not universal: local law, applicable sanctions, payment-provider restrictions and carrier policies may impose further limits. A publicly accessible download or a successful connection does not establish eligibility or official support. Confirm availability and compatibility before purchasing.

### Deployment and licensing

- This image targets **Linux x86-64 / amd64** NAS systems with Docker; it is not an ARM image.
- A valid SIMHub license and an internet connection are required for activation and periodic license validation. A lifetime license does **not** mean permanently offline operation; its scope is defined in the official terms.
- Keep your persistent data directory and back it up before updates. Do not expose an unconfigured management interface to the public internet.
- Confirm hardware and carrier compatibility before relying on any feature. No claim of compatibility with every NAS, SIM, reader or carrier is made.

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

- 本イメージは Docker を利用できる **Linux x86-64／amd64** NAS 向けです。ARM 版ではありません。
- 有効な SIMHub ライセンスが必要です。初回認証および定期的なライセンス確認にはインターネット接続が必要です。永久ライセンスは、永久にオフラインで利用できることを意味しません。適用範囲は公式利用規約に定められます。
- 永続データディレクトリを保持し、更新前にバックアップしてください。初期設定が済んでいない管理画面をインターネットに公開しないでください。
- 利用前に機器・通信事業者との互換性を確認してください。すべての NAS、SIM、リーダー、通信事業者への対応を保証するものではありません。

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

- Образ предназначен для NAS на **Linux x86-64 / amd64** с Docker; это не версия для ARM.
- Требуется действующая лицензия SIMHub. Для активации и периодической проверки лицензии необходимо подключение к интернету. Бессрочная лицензия **не** означает бессрочную автономную работу; её условия определяются официальными условиями использования.
- Сохраняйте каталог постоянных данных и создавайте резервную копию перед обновлением. Не открывайте доступ к ненастроенному интерфейсу управления из интернета.
- Перед использованием убедитесь в совместимости оборудования и оператора. Совместимость со всеми NAS, SIM-картами, считывателями и операторами не гарантируется.

### Ответственное использование и важные ограничения

Используйте только SIM-карты, профили eSIM и устройства, которыми вы владеете или которыми вправе управлять. Соблюдайте местные нормы в области связи, конфиденциальности и записи разговоров, а также условия своего оператора. Не используйте SIMHub для мошенничества, нежелательных рассылок, несанкционированного перехвата или обхода правовых, операторских либо лицензионных ограничений. Оператор может взимать плату за услуги связи.

**Не полагайтесь на SIMHub для экстренных вызовов, систем обеспечения безопасности жизни или связи, требующей непрерывной доступности.** Сохраняйте независимый способ связи. Доступность функций может изменяться вместе с оборудованием, условиями оператора или сети.

Эта страница носит информационный характер и не заменяет [Условия использования](https://simhubkit.com/terms.html), [Политику конфиденциальности](https://simhubkit.com/privacy.html) и [Политику возврата средств](https://simhubkit.com/refund.html). Она не ограничивает обязательные права потребителей или ответственность, которую нельзя исключить по закону. Публикация образа не гарантирует универсальную совместимость или завершение всех испытаний на реальном оборудовании.

Оператор: **SIMKIT LTD**. Поддержка: **simkit@simhubkit.com**.
