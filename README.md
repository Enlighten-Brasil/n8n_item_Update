# Atualização de Item EnSpace [LOTE]

## Automação n8n para atualização de itens EnSpace em lote

### Como usar
1. Baixe o [n8n]([https://drive.google.com/file/d/1wEh4zD1b4WhE-fZurRlPBQOLXHT8STGB/view?usp=drive_link](https://drive.google.com/file/d/18eq-ZwPnkV6etOPOKQhsCPcCVIr_fwqf/view?usp=sharing) (Pule para o passo 4 caso já tenha o n8n)
2. Extraia o arquivo baixado em uma pasta de sua preferência. [Extrair aquivos Windows 11](https://conectandonet.com.br/blog/como-extrair-arquivos-rar-no-windows-11/)
3. Acesse a pasta criada e execute o arquivo `n8n.exe`. (Aguarde a inicialização do n8n)
4. Baixe este [repositório](https://github.com/Enlighten-Brasil/n8n_item_Update/archive/refs/heads/main.zip) e extraia o conteúdo na pasta `data` do n8n. (Ex. `n8n\data\n8n_item_Update-main`)
5. Crie um Workflow em branco no n8n.
6. No canto superior direito, clique nos 3 pontinhos e selecione a opção `Import From File` e selecione o arquivo `workflow.json` na pasta `n8n_item_Update-main`.
7. Clique no botão `Save` no canto superior direito.
8. Leia as instruções abaixo para entender como preencher os arquivo `config.json` e o input a ser usado `.xlsx`.
9. Renomeie o arquivo `config.json.example` para `config.json`.
10. Preencha o arquivo `config.json` com os dados necessários.
11. Na pasta `Input`, coloque o arquivo `.xlsx` com os dados a serem atualizados usando o nome configurado `inputFilePath` em `config.json`.
12. Clique no botão `Execute Workflow` no centro inferior da tela.
13. Aguarde a execução do Workflow.
14. Se tudo ocorrer bem, um arquivo `.xlsx` será gerado na pasta `Output` com o resultado.

## Instruções

### Arquivo `config.json`
- `apiUrl`: URL da API EnSpace. (Ex: `https://api.stage.enspace.io`).
  - (Produção) `https://api.enspace.io`
  - (Stage) `https://api.stage.enspace.io`
  - (Local) `http://localhost:1337`
- `inputFilePath`: Nome do arquivo `.xlsx` com os dados dos formulários a serem respondidos.
- `type_id`: ID da Categoria no EnSpace.
- `user_email`: E-mail do usuário que irá se autenticar na API EnSpace.
- `user_token`: Token do usuário que irá se autenticar na API EnSpace.
- `inputDateFormat`: Formato da data usado no arquivo `.xlsx`. (Ex: `dd/MM/yyyy`, `MM/dd/yyyy`, `yyyy-MM-dd`)

#### Instruções para Formato de Data
- `dd`: Dia com 2 dígitos
- `MM`: Mês com 2 dígitos
- `yyyy`: Ano com 4 dígitos
- `HH`: Hora com 2 dígitos
- `mm`: Minuto com 2 dígitos
- `ss`: Segundo com 2 dígitos

##### Exemplos de Formato de Data / Hora
- `dd/MM/yyyy`: 30/01/2021
- `MM/dd/yyyy`: 01/30/2021
- `yyyy-MM-dd`: 2021-01-30
- `dd/MM/yyyy HH:mm:ss`: 30/01/2021 00:00:00

#### Exemplo de arquivo `config.json`
```json
{
  "apiUrl": "https://api.stage.enspace.io",
  "inputFilePath": "input.xlsx",
  "type_id": 1,
  "user_email": "exemplo@exemplo.com",
  "user_token": "exemplo",
  "inputDateFormat": "dd/MM/yyyy"
}
```
---

### Tipos de campos suportados e formato de entrada
- Texto, Texto longo
  - Tipo: `string` ou `texto`
  - Exemplo: `Lorem ipsum dolor sit amet`
- Número (Moeda, Porcentagem, decimal)
  - Tipo: `number` ou `número`
  - Exemplo: `123`
  - Formato: `0.00`
- Calendário, Data, Hora* e Data e Hora*
  - Tipo: `string` ou `texto`
  - Exemplo: `01/01/2021`
  - Formato: `dd/MM/yyyy` (configurável no arquivo `config.json`)
  - *Hora e Data e Hora não são suportados no momento (sempre será preenchido como `00:00:00` sem fuso horário)
- Campos de seleção unica (Selação, Radio)
  - Tipo: `string` ou `texto`
  - Exemplo: `Opção 1`
  - Formato: Valor deve ser idêntico ao `valor` (`value`) da opção configurada no formulário
- Campos de seleção multipla - `*Em Progresso` 
  - Tipo: `string` ou `texto`
  - Exemplo: `Opção 1, Opção 2`
  - Formato: Valores devem ser idênticos aos `valores` (`values`) das opções configuradas no formulário, separados por vírgula
  - *Em Progresso: Ainda não é possível selecionar mais de uma opção
- Campos de seleção de arquivos - `*Não suportado`
  - Tipo: `string` ou `texto`
  - Exemplo: `https://www.google.com.br/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png`
  - Formato: URL do arquivo
  - *Não suportado: Ainda não é possível enviar arquivos
- Seleção de usuário
  - Tipo: `number` ou `numero`
  - Exemplo: `01`
  - Formato: ID do usuário
- Campos de Relacionamento (EnRel e EnRelMulti)
  - Tipo: `string` ou `texto`
  - Exemplos: `EXEMPLO-1`, `EXEMPLO-1, EXEMPLO-2, EXEMPLO-3`
  - Formato: Referência do item relacionado, separados por vírgula quando mais de um.
- Mascara de texto
  - Tipo: `string` ou `texto`
  - Exemplo: `123.456.789-00`
  - Formato: Valor deve respeitar a máscara configurada no campo.
- Editor HTML
  - Tipo: `string` ou `texto`
  - Exemplo: `<p>Lorem ipsum dolor sit amet</p>`
  - Formato: HTML
- Documento (OnlyOffice) - `*Não suportado`
  - *Não suportado: Sem previsão de suporte

## Exemplos 

### Configuração dos campos usados no exemplo
- Nome (ref: `nome`): Texto
- Registro (ref: `registro`)  : Calendário
- Salário (ref: `salario`)  : Número
- Tipo Doc. (ref: `tipo_doc`)  : Seleção única
  - CPF (value: `cpf`)
  - CNPJ (value: `cnpj`)
  - R.G. (value: `rg`)
- Documento (ref: `documento`)  : Texto
- Endereço (ref: `endereco`)  : Relacionamento
- Telefones (ref: `telefones`)  : Relacionamento Multi

### Exemplo de arquivo de input `.xlsx`
| reference | nome | registro | salario | tipo_doc | documento | endereco | telefones |
| --- | --- | --- | --- | --- | --- | --- | --- |
| USER-B7 | João | 01/01/2021 | 1000.01 | cpf | 123.456.789-00 | ITEM-END-1 | ITEM-TEL-3 |
| USER-E1 | Maria | 01/01/2021 | 999.99 | cnpj | 12.345.678/0001-00 | ITEM-END-2 | ITEM-TEL-2, ITEM-TEL-3 |
| USER-W4 | José | 01/01/2021 | 999.99 | rg | 12.345.678-9 | ITEM-END-3 | ITEM-TEL-1 |
