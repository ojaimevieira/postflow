# PostFlow 🚀

Workflow de automação para postagens no Instagram usando N8n e Notion.

## 📋 Sobre o Projeto

O **PostFlow** é um sistema de automação que integra o Notion como banco de dados com o Instagram via N8n, permitindo agendar e publicar posts automaticamente.

### 🎯 Funcionalidades

- ✅ Agendamento de posts via Notion
- ✅ Publicação automática no Instagram
- ✅ Suporte a fotos, vídeos e carrosséis
- ✅ Gerenciamento de status dos posts
- ✅ Verificação a cada 5 minutos
- 🔄 Expansão futura para TikTok e YouTube

## 🗂️ Estrutura da Database Notion

| Campo | Tipo | Descrição |
|-------|------|-----------|
| **Nome** | Título | Nome do post |
| **Plataforma** | Multi-select | Instagram, TikTok, Twitter, YouTube |
| **Tipo de Conteúdo** | Select | Notícias, Carrossel, Tutoriais |
| **Status** | Select | Ideia, Em revisão, Aprovado, Publicado, Referência |
| **Data do Agendamento** | Date | Data e hora para publicação |
| **Texto do Post** | Rich Text | Conteúdo/Caption do post |
| **Hashtags** | Multi-select | Tags do post |
| **Mídia** | Files | Arquivos de imagem/vídeo |
| **Publicar?** | Checkbox | Controle para ativar publicação |
| **Link da Publicação** | Files | URL do post publicado |

## ⚙️ Configuração

### 1. Credenciais Facebook/Instagram
```env
FACEBOOK_APP_ID=1324691276112631
FACEBOOK_APP_SECRET=02fd07953f4dfac72b9f7fa429974b3b
FACEBOOK_PAGE_NAME=perfilx0
```

### 2. Notion Database
```env
NOTION_DATABASE_ID=1c868fa96b04811ab439da9ae42cf538
```

### 3. Fluxo de Publicação

**Condições para publicar:**
- Status = "Aprovado"
- Publicar? = true
- Plataforma contém "Instagram"
- Data do Agendamento ≤ agora (com margem de 5 min)

**Após publicação:**
- Status → "Publicado"
- Publicar? → false
- Link da Publicação → URL do post Instagram

## 🔧 Instalação

1. Importe o workflow `postflow-instagram.json` no seu N8n
2. Configure as credenciais do Instagram/Facebook
3. Configure a API do Notion
4. Ative o workflow

## 📱 Tipos de Conteúdo Suportados

### Fase 1 (Atual)
- ✅ **Fotos simples** - Primeira implementação

### Próximas Fases
- 🔄 **Vídeos** 
- 🔄 **Carrosséis**

## 🚀 Como Usar

1. **Criar post no Notion:**
   - Preencher todos os campos obrigatórios
   - Definir Data do Agendamento
   - Selecionar Plataforma = "Instagram"

2. **Aprovar post:**
   - Status = "Aprovado"
   - Publicar? = ✅

3. **Aguardar publicação automática:**
   - N8n verifica a cada 5 minutos
   - Post é publicado automaticamente na data/hora definida

## 📁 Arquivos Disponíveis

### Workflows N8n:
- **`postflow-instagram-v1.json`** - Versão inicial completa
- **`postflow-instagram-beta.json`** - Versão BETA otimizada (apenas fotos)
- **`postflow-instagram-beta-videos.json`** - Versão BETA com suporte a fotos + vídeos

### Documentação:
- **`docs/configuracao.md`** - Guia de configuração completo
- **`docs/importacao.md`** - Como importar workflows no N8n
- **`docs/status-final.md`** - Status do projeto e funcionalidades

## 🎬 Suporte a Vídeos

### Google Drive Integration
O workflow `postflow-instagram-beta-videos.json` inclui conversão automática de URLs do Google Drive:

**URL Original (Google Drive):**
```
https://drive.google.com/file/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74mZa/view
```

**URL Convertida (para Instagram):**
```
https://drive.google.com/uc?export=download&id=1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74mZa
```

### Detecção Automática de Mídia
- **Fotos:** `.jpg`, `.jpeg`, `.png`, `.webp`, `.gif`
- **Vídeos:** `.mp4`, `.mov`, `.avi`, `.mkv`, `.webm`, `.m4v`

O workflow detecta automaticamente o tipo de mídia e usa o endpoint correto do Instagram (`image_url` vs `video_url`).

## 🔐 Segurança

- Mantenha o arquivo `.env` seguro e não versionado
- Use tokens com permissões mínimas necessárias
- Monitore logs de publicação

---

**Desenvolvido por:** Jaime Vieira  
**Versão:** 1.0.0  
**Licença:** MIT
