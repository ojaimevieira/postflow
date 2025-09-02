# 🔐 INSTRUÇÕES DE SEGURANÇA

## ⚠️ IMPORTANTE: TOKENS DE ACESSO

**NUNCA** commite tokens reais no GitHub! Este repositório contém placeholders que devem ser substituídos.

### 🛠️ Como configurar tokens corretamente:

1. **Gere um novo token** no [Facebook Developers](https://developers.facebook.com/)
2. **Substitua** `YOUR_INSTAGRAM_ACCESS_TOKEN_HERE` pelo seu token real
3. **Configure variáveis de ambiente** ou use o sistema de credenciais do n8n

### 🔄 Workflows que precisam de tokens:

- `postflow-EXACT-SUCCESS.json`
- `postflow-GITHUB-HTTPS.json`

### 📍 Locais onde substituir:

Procure por `YOUR_INSTAGRAM_ACCESS_TOKEN_HERE` e substitua pelo seu token real em:
- Nodes de Upload (Instagram API)
- Nodes de Publish (Instagram API)

### 🚨 Se você expôs um token:

1. **REVOGUE** imediatamente o token no Facebook Developers
2. **GERE** um novo token
3. **ATUALIZE** apenas localmente (não commite)

### 💡 Dica de segurança:

Use o sistema de credenciais do n8n em vez de hardcode nos workflows:
1. Vá em Settings > Credentials
2. Adicione uma nova credencial "HTTP Header Auth"
3. Configure: `Authorization: Bearer SEU_TOKEN`
4. Referencie nos nodes em vez de hardcode
