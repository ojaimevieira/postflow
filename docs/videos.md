# 🎬 PostFlow BETA - Suporte a Vídeos

## Overview

Existem **duas versões** do workflow para vídeos:

### 1. `postflow-instagram-beta-videos.json` ⚡ (Versão URL)
- ✅ **Mais rápida** - Usa URLs diretamente
- ⚠️ **Limitações** - Google Drive URLs podem não funcionar
- 🎯 **Melhor para** - URLs diretas de mídia

### 2. `postflow-instagram-beta-videos-upload.json` 🔄 (Versão Upload)
- ✅ **Mais compatível** - Baixa e faz upload dos arquivos
- ✅ **Funciona com Google Drive** - Download automático
- 🎯 **Melhor para** - URLs de cloud storage

## ⚠️ Erro Comum: Google Drive URLs

### Problema
```json
{
  "error": "Only photo or video can be accepted as media type.",
  "message": "Não foi possível obter a mídia deste URI: https://drive.google.com/uc?export=download&id=..."
}
```

### Causa
O Instagram não consegue acessar diretamente URLs do Google Drive, mesmo no formato de download direto.

### ✅ Solução: Use a Versão Upload
O arquivo `postflow-instagram-beta-videos-upload.json` resolve este problema:

1. **Detecta** se a URL é do Google Drive
2. **Baixa** o arquivo automaticamente 
3. **Faz upload** direto para o Instagram

## 🔧 Como Escolher a Versão

### Use `postflow-instagram-beta-videos.json` se:
- ✅ Suas mídias estão em URLs **diretas** (não cloud storage)
- ✅ URLs são **acessíveis publicamente** pelo Instagram
- ✅ Quer **máxima velocidade** de processamento

### Use `postflow-instagram-beta-videos-upload.json` se:
- ✅ Usa **Google Drive**, Dropbox, OneDrive
- ✅ Usa URLs **temporárias** do Notion
- ✅ Quer **máxima compatibilidade**

## 🔍 Detecção de Mídia

### Formatos Suportados

**Fotos:**
- `.jpg`, `.jpeg` - JPEG images
- `.png` - PNG images  
- `.webp` - WebP images
- `.gif` - GIF animations

**Vídeos:**
- `.mp4` - MP4 videos (recomendado)
- `.mov` - QuickTime videos
- `.avi` - AVI videos
- `.mkv` - Matroska videos
- `.webm` - WebM videos
- `.m4v` - iTunes videos

### Lógica de Detecção

```javascript
function getMediaType(mediaUrl) {
  const url = mediaUrl.toLowerCase();
  const videoExtensions = ['.mp4', '.mov', '.avi', '.mkv', '.webm', '.m4v'];
  const imageExtensions = ['.jpg', '.jpeg', '.png', '.webp', '.gif'];
  
  for (const ext of videoExtensions) {
    if (url.includes(ext)) return 'VIDEO';
  }
  
  for (const ext of imageExtensions) {
    if (url.includes(ext)) return 'IMAGE';
  }
  
  // Default: IMAGE (comportamento do Instagram)
  return 'IMAGE';
}
```

## 🔄 Conversão do Google Drive

### Problema Original
URLs do Google Drive no formato de compartilhamento não funcionam diretamente com a API do Instagram:

```
❌ https://drive.google.com/file/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74mZa/view
```

### Solução Automática
O workflow converte automaticamente para o formato de download direto:

```
✅ https://drive.google.com/uc?export=download&id=1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74mZa
```

### Código de Conversão

```javascript
function convertGoogleDriveUrl(url) {
  const driveMatch = url.match(/\/file\/d\/([a-zA-Z0-9_-]+)/);
  if (driveMatch) {
    console.log(`🔄 Converting Google Drive URL to direct download`);
    return `https://drive.google.com/uc?export=download&id=${driveMatch[1]}`;
  }
  return url; // Return original if not Google Drive
}
```

## 📡 API do Instagram

### Endpoints Dinâmicos

O workflow usa endpoints diferentes baseado no tipo de mídia:

**Para Fotos:**
```http
POST https://graph.facebook.com/v18.0/{account-id}/media
Content-Type: application/x-www-form-urlencoded

image_url={media_url}&caption={caption}&media_type=IMAGE&access_token={token}
```

**Para Vídeos:**
```http
POST https://graph.facebook.com/v18.0/{account-id}/media
Content-Type: application/x-www-form-urlencoded

video_url={media_url}&caption={caption}&media_type=VIDEO&access_token={token}
```

### Código do N8n

```javascript
// Query Parameters dinâmicos
{
  "name": "={{ $json.media_type === 'VIDEO' ? 'video_url' : 'image_url' }}",
  "value": "={{ $json.media_url }}"
},
{
  "name": "media_type", 
  "value": "={{ $json.media_type === 'VIDEO' ? 'VIDEO' : 'IMAGE' }}"
}
```

## 📋 Como Usar

### 1. Preparar Mídia no Google Drive

1. **Upload do vídeo/foto** para o Google Drive
2. **Compartilhar** o arquivo (tornar público ou com link)
3. **Copiar URL** de compartilhamento
4. **Colar no Notion** no campo "Mídia"

### 2. Formato no Notion

No campo **Mídia** do Notion, adicione a URL do Google Drive:

```
https://drive.google.com/file/d/1BxiMVs0XRA5nFMdKvBdBZjgmUUqptlbs74mZa/view?usp=sharing
```

### 3. Publicação Automática

O workflow irá:
1. ✅ **Detectar** automaticamente se é foto ou vídeo
2. ✅ **Converter** URL do Google Drive se necessário
3. ✅ **Usar endpoint correto** do Instagram
4. ✅ **Publicar** com logs detalhados

## 🔧 Configuração

### Credenciais Necessárias

Mesmo setup da versão BETA normal:

1. **Notion API Token**
2. **Facebook/Instagram Access Token** 
3. **Instagram Business Account ID**

### Importar Workflow

1. Baixar `postflow-instagram-beta-videos.json`
2. Importar no N8n
3. Configurar credenciais
4. Ativar workflow

## 🐛 Troubleshooting

### Logs de Depuração

O workflow fornece logs detalhados:

```
🎬 BETA EXECUTION - Processing 1 items (Photos + Videos support)
📄 Found 1 pages in Notion response
📝 Post: "Meu Vídeo Test"
🔗 Original Media URL: https://drive.google.com/file/d/1BxiMVs0.../view
🔄 Converting Google Drive URL to direct download
🔗 Processed Media URL: https://drive.google.com/uc?export=download&id=1BxiMVs0...
🎬 Media Type: VIDEO
🚀 POST APPROVED: "Meu Vídeo Test" ready for Instagram VIDEO posting!
```

### Problemas Comuns

**❌ Vídeo não detectado:**
- Verificar se a URL contém extensão de vídeo (`.mp4`, etc.)
- Renomear arquivo no Google Drive para incluir extensão

**❌ URL do Google Drive não funciona:**
- Verificar se arquivo está público ou compartilhado
- Testar URL convertida manualmente no navegador

**❌ Instagram rejeita mídia:**
- Verificar tamanho e formato do arquivo
- Videos: máximo 100MB, formatos MP4/MOV recomendados
- Fotos: máximo 8MB, formatos JPG/PNG recomendados

## 📊 Diferenças vs Versão Normal

| Recurso | BETA Normal | BETA Videos |
|---------|-------------|-------------|
| Suporte a fotos | ✅ | ✅ |
| Suporte a vídeos | ❌ | ✅ |
| Google Drive URLs | ❌ | ✅ |
| Detecção automática | ❌ | ✅ |
| Logs detalhados | ✅ | ✅ |
| Endpoints dinâmicos | ❌ | ✅ |

---

**Versão:** 1.0.0  
**Data:** 2024  
**Testado com:** N8n 1.0+, Instagram Graph API v18.0
