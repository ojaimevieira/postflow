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
- **`postflow-instagram-beta.json`** - ✅ Versão BETA para fotos e posts simples
- **`postflow-instagram-beta-videos.json`** - ⚠️ Vídeos (pode sobrecarregar servidor)
- **`postflow-instagram-beta-videos-upload.json`** - ⚠️ Upload híbrido (pode sobrecarregar servidor)
- **`postflow-instagram-smart-videos.json`** - ❌ Tentativa leve (Instagram rejeita Google Drive URLs)
- **`postflow-instagram-stable-videos.json`** - ⭐ **RECOMENDADO** para vídeos

### Documentação:
- **`docs/configuracao.md`** - Guia de configuração completo
- **`docs/importacao.md`** - Como importar workflows no N8n
- **`docs/status-final.md`** - Status do projeto e funcionalidades

## 🎬 Suporte a Vídeos

### ⚠️ Problema Instagram + Google Drive
**Erro comum:**
```
"Não foi possível obter a mídia deste URI: https://drive.google.com/uc?export=download&id=..."
```

**Causa:** Instagram API não consegue acessar diretamente URLs do Google Drive.

**Solução:** Use o workflow `postflow-instagram-stable-videos.json` (RECOMENDADO)

### 🛡️ Workflow Estável (Recomendado)
- **Arquivo:** `postflow-instagram-stable-videos.json`
- **Frequência:** A cada 2 minutos (ao invés de 30 segundos)
- **Limite:** 1 post por execução para evitar sobrecarga
- **Timeouts:** Estendidos (120s download, 180s upload)
- **Método:** Sempre baixa e faz upload do arquivo

### Detecção Automática de Mídia
- **Fotos:** `.jpg`, `.jpeg`, `.png`, `.webp`, `.gif`
- **Vídeos:** `.mp4`, `.mov`, `.avi`, `.mkv`, `.webm`, `.m4v`

### ⚠️ Proteção do Servidor
Para evitar "conexão do n8n caindo na etapa de download", o workflow estável:
- Processa apenas 1 post por vez
- Executa a cada 2 minutos
- Timeouts estendidos
- Logs detalhados para monitoramento

## 🔐 Segurança

- Mantenha o arquivo `.env` seguro e não versionado
- Use tokens com permissões mínimas necessárias
- Monitore logs de publicação

---

**Desenvolvido por:** Jaime Vieira  
**Versão:** 1.0.0  
**Licença:** MIT
