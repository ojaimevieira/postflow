# 📥 Guia de Importação - PostFlow Instagram

## ✅ **WORKFLOW PRONTO PARA IMPORTAR**

O arquivo `workflows/postflow-instagram.json` está validado e pronto!

## 📋 **PASSO A PASSO PARA IMPORTAR:**

### **1. No N8n:**
- Clique no **"+"** para criar novo workflow
- Clique em **"Import from file"**
- Selecione o arquivo: `postflow-instagram.json`

### **2. Configurar Credencial Notion:**
- No primeiro nó "Query Approved Posts", clique na credencial
- Selecione sua credencial do Notion já configurada
- No último nó "Update Post Status", faça o mesmo

### **3. Ativar Workflow:**
- Clique no botão **"Active"** no canto superior direito
- O workflow começará a rodar a cada 5 minutos

## 🧪 **CRIAR POST DE TESTE:**

No seu Notion "Banco de Postagens":

```
✅ Nome: "Teste PostFlow Instagram 🚀"
✅ Plataforma: Instagram (selecionar)
✅ Status: Aprovado
✅ Data do Agendamento: AGORA + 3 minutos
✅ Texto do Post: "Testando automação PostFlow! 🎯 Funcionando perfeitamente!"
✅ Hashtags: automation, test, postflow
✅ Mídia: Anexar uma foto
✅ Publicar?: ✅ Marcado
```

## ⏰ **MONITORAR EXECUÇÃO:**

- No N8n, vá em **"Executions"** 
- Você verá as execuções a cada 5 minutos
- Quando o post for detectado, verá todo o fluxo executando
- O post será publicado no Instagram
- Status mudará para "Publicado" no Notion

## 🎯 **ESTÁ TUDO PRONTO!**

**JSON válido ✅**  
**Credenciais configuradas ✅**  
**Workflow funcional ✅**

**Pode importar sem problemas!** 🚀
