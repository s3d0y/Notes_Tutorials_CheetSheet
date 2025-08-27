**SASL** - это framework для аутентификации и безопасности в сетевых протоколах. В контексте Postfix - это механизм аутентификации при отправке почты через внешний SMTP сервер (как Mail.ru).

### Как работает в Postfix:

``` sh
# Настройки в main.cf
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl/sasl_passwd
smtp_sasl_security_options = noanonymous
```


### Компоненты SASL:

1. **Аутентификация** - проверка логина/пароля
    
2. **Безопасность** - шифрование учетных данных
    
3. **Механизмы** - plain, login, cram-md5 и др.