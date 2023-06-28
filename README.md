# Atualização de Item EnSpace [LOTE]

## Automação n8n para atualização de itens EnSpace em lote

### Como usar
1. Baixe o [n8n](https://drive.google.com/file/d/1wEh4zD1b4WhE-fZurRlPBQOLXHT8STGB/view?usp=drive_link) e extraia o arquivo. (Pule para o passo 3 caso já tenha o n8n)
2. Acesse a pasta e execute o arquivo `n8n.exe` aguarde iniciar.
3. Copie este repositório e cole na pasta `data` em seu n8n. (Mantenha o nome da pasta)
4. Na pasta Input, coloque o arquivo `.xlsx` com os dados dos formulários a serem respondidos. 
5. Renomeie o arquivo `config.json.example` para `config.json`.
6. Preencha o arquivo `config.json` com os dados necessários.
7. Abra o n8n e importe o arquivo `workflow.json` para o seu n8n.
8. Execute o workflow.
9. Ao final, na pasta Output será gerado um arquivo `.xlsx` com a relação de formulários respondidos.

### Detalhes do arquivo `config.json`
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


### Exemplo de arquivo `config.json`
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

### Exemplo de formulário EnSpace
### Campos
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

### Exemplo de arquivo `.xlsx`
| reference | nome | registro | salario | tipo_doc | documento | endereco | telefones |
| --- | --- | --- | --- | --- | --- | --- | --- |
| USER-B7 | João | 01/01/2021 | 1000.01 | cpf | 123.456.789-00 | ITEM-END-1 | ITEM-TEL-3 |
| USER-E1 | Maria | 01/01/2021 | 999.99 | cnpj | 12.345.678/0001-00 | ITEM-END-2 | ITEM-TEL-2, ITEM-TEL-3 |
| USER-W4 | José | 01/01/2021 | 999.99 | rg | 12.345.678-9 | ITEM-END-3 | ITEM-TEL-1 |
