## 4. Financeiro: Automação de envio de boletos por e-mail

### 🛠 Problema real
Em muitas empresas, a equipe financeira precisa **enviar boletos manualmente** para cada cliente após a emissão do título (duplicata). Isso gera um grande volume de trabalho repetitivo, principalmente quando há dezenas ou centenas de boletos emitidos por dia.

Além disso, esquecimentos ou atrasos no envio podem gerar inadimplência e cobranças desnecessárias.

### 📉 Impacto
- Alto volume de tarefas manuais no contas a receber
- Risco de boletos não enviados ou atrasados
- Clientes reclamando da ausência de cobrança
- Atraso nos recebimentos
- Perda de tempo com reenvios manuais

### 💡 Solução aplicada
Automatização do processo de envio de boletos assim que o título é gerado (a partir da SE2 ou via rotina agendada). A automação consiste em:

- Gerar o PDF do boleto (pode usar integração com bancos ou layout próprio)
- Montar o corpo do e-mail com dados do cliente e vencimento
- Enviar o e-mail automaticamente com o boleto em anexo

> A rotina pode ser agendada diariamente ou ativada após a emissão de faturas.

### 🧾 Código exemplo simplificado (ADVPL)
```advpl
User Function EnviaBoleto()
    Local cEmail := SA1->A1_EMAIL
    Local cBoletoPDF := "C:\boletos\" + SE2->E2_NUM + ".pdf"
    Local cAssunto := "Boleto referente ao título " + SE2->E2_NUM
    Local cCorpo := "Olá " + SA1->A1_NOME + "," + CRLF +;
                    "Segue em anexo o boleto com vencimento em " + DTOC(SE2->E2_VENCTO)

    If File(cBoletoPDF)
        MtaSendMail(cEmail, cAssunto, cCorpo, {}, {cBoletoPDF})
        MsgInfo("Boleto enviado com sucesso para " + cEmail)
    Else
        MsgStop("Arquivo PDF do boleto não encontrado.")
    EndIf
Return
```
Esta é uma versão simplificada. Em ambiente real, é ideal controlar status de envio, logs, e até reenvio automático.

## 🧪 Testes realizados

| Situação                          | Resultado Esperado                     |
|-----------------------------------|----------------------------------------|
| Cliente com e-mail válido e boleto gerado | ✅ E-mail enviado com sucesso  |
| Cliente sem e-mail               | ⚠️ Aviso de ausência de e-mail         |
| Boleto não gerado (sem PDF)      | ❌ Aviso de erro no sistema            |

🎯 Benefícios
Economia de tempo e esforço para o financeiro

Maior agilidade na cobrança

Redução de inadimplência por falha no envio

Imagem mais profissional da empresa

🏷️ Tags
#Protheus #Financeiro #SE2 #Boleto #Cobrança #Automação #ADVPL #E-mail
