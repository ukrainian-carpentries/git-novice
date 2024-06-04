---
title: Інсталяція Git
---

## Інсталяція Git

Оскільки декілька уроків Carpentries покладаються на використання Git, перегляньте інструкції щодо встановлення Git для різних операційних систем у [цьому розділі шаблону семінару][workshop-setup].

- [Інсталяція Git у Windows][workshop-setup]
- [Інсталяція Git у MacOS][workshop-setup]
- [Інсталяція Git у Linux][workshop-setup]

## Створення облікового запису GitHub

Для того, щоб слідувати епізодам 7 та 8 у цьому уроці, вам знадобиться обліковий запис [GitHub](https://github.com).

1. Перейдіть до https://github.com і натисніть на "Зареєструватися" у верхньому правому куті вікна.
2. Дотримуйтесь інструкцій, щоб створити обліковий запис.
3. Підтвердить свою адресу електронної пошти на GitHub.
4. Налаштуйте багатофакторну автентифікацію (див. нижче).

### Багатофакторна автентифікація

У 2023 році GitHub запровадив вимогу, щоб усі облікові записи для додаткової безпеки використовували [багатофакторну автентифікацію (2FA)](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/about-two-factor-authentication).
Існує кілька варіантів налаштування 2FA, які наведено тут:

1. Якщо ви вже використовуєте програму автентифікації, наприклад [Google Authenticator](https://support.google.com/accounts/answer/1066447?hl=uk\&co=GENIE.Platform%3DiOS\&oco=0) або [Duo Mobile](https://duo.com/product/multi-factor-authentication-mfa/duo-mobile-app) на вашому смартфоні, [додайте до цієї програми GitHub](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-totp-mobile-app).
2. Якщо у вас є доступ до смартфону, але ви ще не використовуєте програму автентифікації, установіть її та [додайте до неї GitHub](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-totp-mobile-app).
3. Якщо у вас немає доступу до смартфону або ви не хочете встановлювати програму автентифікації, у вас є два варіанти:
   1. [налаштувати 2FA за допомогою текстового повідомлення](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-text-messages)
      ([перелік країн, де підтримується автентифікація за допомогою SMS](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/countries-where-sms-authentication-is-supported)), або
   2. [використовувати апаратний ключ безпеки](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication#configuring-two-factor-authentication-using-a-security-key) як [YubiKey](https://www.yubico.com/products/yubikey-5-overview/) або [ключ Google Titan](https://store.google.com/us/product/titan_security_key?hl=en-US\&pli=1).

Додаткову інформацію про налаштування 2FA наведено [у документації GitHub](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa/configuring-two-factor-authentication).

----------------

## Підготовка вашого робочого каталогу

Ми будемо працювати в каталозі `Desktop`, тому переконайтеся, що ви зміните робочий каталог на `Desktop` за допомогою:

```bash
$ cd
$ cd Desktop
```

[workshop-setup]: https://carpentries.github.io/workshop-template/install_instructions/#git
