# 🔧 Configuração do PostFlow Instagram

## Pré-requisitos

Antes de importar o workflow, você precisa configurar:

### 1. 📱 Instagram/Facebook Setup

#### Credenciais que você já tem:
- **App ID:** `1324691276112631`
- **App Secret:** `02fd07953f4dfac72b9f7fa429974b3b`
- **Página Facebook:** `perfilx0`

#### ⚠️ Ainda precisa configurar:

1. **Page ID do Facebook:**
   - Acesse: https://developers.facebook.com/tools/explorer/
   - Selecione seu app: `postflow-IG`
   - GET request: `/me/accounts`
   - Copie o `id` da página `perfilx0`

2. **Page Access Token:**
   - Na mesma resposta, copie o `access_token` da página
   - Este token é específico da página, não do app

### 2. 🔑 Tokens no N8n

Configure essas credenciais no N8n:

#### Notion API:
```
Credential Type: Notion API
API Key: [Sua chave do Notion]
```

#### Environment Variables (Settings → Variables):
```
FACEBOOK_APP_ID = 1324691276112631
FACEBOOK_APP_SECRET = 02fd07953f4dfac72b9f7fa429974b3b  
FACEBOOK_PAGE_ID = [ID da página que você vai obter]
FACEBOOK_ACCESS_TOKEN = [Token da página que você vai obter]
NOTION_DATABASE_ID = 1c868fa96b04811ab439da9ae42cf538
```

## 📥 Importação do Workflow

1. Abra o N8n
2. Clique em "Import from file"
3. Selecione: `workflows/postflow-instagram.json`
4. Configure as credenciais mencionadas acima
5. Ative o workflow

## 🧪 Como Testar

### Teste Rápido:
1. **Crie um post de teste no Notion:**
   - Nome: "Teste PostFlow"
   - Plataforma: ✅ Instagram
   - Tipo: Tutoriais
   - Status: **Aprovado**
   - Data do Agendamento: **Agora + 2 minutos**
   - Texto: "Testando automação PostFlow 🚀"
   - Hashtags: automation, test
   - Mídia: **Anexar uma imagem**
   - Publicar?: ✅ **Marcado**

2. **Aguarde:**
   - Em até 5 minutos o workflow detectará o post
   - Verificará se chegou a hora
   - Publicará no Instagram
   - Atualizará o status para "Publicado"

## ⚙️ Fluxo do Workflow

```
🕐 Schedule (5 min) 
    ↓
📋 Query Notion (posts aprovados)
    ↓  
⏰ Check Time (data chegou?)
    ↓
📝 Prepare Data (formatar caption)
    ↓
📸 Create Media (upload Instagram)
    ↓
🚀 Publish Post (publicar)
    ↓
✅ Update Notion (marcar como publicado)
```

## 🔍 Logs e Debug

- Acesse "Executions" no N8n para ver logs
- Cada execução mostra o status de cada etapa
- Em caso de erro, o post fica com Status = "Erro"

## ⚠️ Próximos Passos Necessários

**ANTES DE TESTAR, você precisa:**

1. ✅ Obter o Page ID do Facebook
2. ✅ Obter o Page Access Token  
3. ✅ Configurar as variáveis no N8n
4. ✅ Configurar credencial do Notion

**Quer que eu te ajude a obter essas informações?** 🚀
