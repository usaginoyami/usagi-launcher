# Usagi Launcher - APK automático e assinatura

O workflow `Usagi Launcher - Build APK` gera o APK a cada push na branch `usagi-launcher`
(ou manualmente em Actions > Run workflow). O APK fica em "Artifacts" da execução.

## Assinar com a sua chave (uma vez só)

1. No PC, crie a chave (guarde o arquivo e as senhas em local seguro, e nunca suba o `.jks` no GitHub):

       keytool -genkeypair -v -keystore usagi.jks -alias usagi -keyalg RSA -keysize 2048 -validity 10000

2. Converta para base64 (PowerShell):

       [Convert]::ToBase64String([IO.File]::ReadAllBytes("usagi.jks")) | Set-Clipboard

3. No GitHub: Settings > Secrets and variables > Actions > New repository secret. Crie:

   | Secret | Valor |
   |---|---|
   | `KEYSTORE` | o base64 copiado |
   | `KEYSTORE_PASSWORD` | senha do keystore |
   | `KEY_ALIAS` | `usagi` |
   | `KEY_PASSWORD` | senha da chave |

Observação: ao trocar a assinatura, o Android não atualiza por cima um app assinado com outra chave.
Desinstale a versão antiga uma vez.

## Publicar em Releases

Crie uma tag que comece com `v` (ex.: `v1.0.0`) e dê push: o workflow anexa o APK à Release.

    git tag v1.0.0
    git push origin v1.0.0
