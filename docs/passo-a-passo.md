# Passo a Passo da Implementação

Este documento registra o roteiro utilizado para criar a primeira stack com AWS CloudFormation.

## 1. Preparação

Antes de iniciar, confirme:

- Acesso a uma conta AWS.
- Permissões para usar CloudFormation, EC2 e Security Groups.
- Existência de uma VPC padrão na região escolhida.
- Região AWS definida para o laboratório.
- Atenção aos custos dos recursos provisionados.

## 2. Acesso ao CloudFormation

1. Acesse o Console de Gerenciamento da AWS.
2. Pesquise por **CloudFormation**.
3. Abra o serviço.
4. Clique em **Create stack**.

## 3. Envio do Template

1. Escolha a opção de criar uma stack a partir de um template.
2. Informe o arquivo `templates/webserver-stack.yaml`.
3. Avance para a etapa de configuração.

## 4. Configuração da Stack

Preencha os parâmetros:

| Parâmetro | Descrição | Exemplo |
| --- | --- | --- |
| `LatestAmiId` | AMI Amazon Linux obtida pelo Parameter Store | `/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2` |
| `InstanceType` | Tipo da instância EC2 | `t2.micro` |
| `KeyName` | Par de chaves EC2 usado para SSH | `ChavesLinux` |
| `MyIP` | IP autorizado para acesso HTTP e SSH | `203.0.113.10/32` |

Nome definido para a stack:

```text
dio-primeira-stack-thassio
```

## Observação sobre SSH

Antes de criar ou atualizar a stack, crie um par de chaves no EC2 com o nome:

```text
ChavesLinux
```

O template usa esse par de chaves no parâmetro `KeyName`, associa a chave à instância EC2 e abre a porta 22 no Security Group apenas para o IP informado em `MyIP`.

Mais detalhes estão em:

[acesso-ssh.md](acesso-ssh.md)

## 5. Revisão e Criação

1. Revise as informações da stack.
2. Mantenha as opções padrão, se não houver necessidade de alteração.
3. Confirme a criação.
4. Aguarde o início do provisionamento.

## 6. Monitoramento

Na página da stack:

1. Acesse a aba **Events**.
2. Acompanhe os recursos sendo criados.
3. Aguarde o status `CREATE_COMPLETE`.

Se ocorrer erro:

- Verifique a coluna de motivo do erro.
- Confirme permissões IAM.
- Confirme se existe VPC padrão.
- Revise o valor de `MyIP`.
- Valide a sintaxe do YAML.

## 7. Validação da Aplicação

Após o status `CREATE_COMPLETE`:

1. Acesse a aba **Outputs**.
2. Copie o valor de `WebsiteURL`.
3. Abra a URL no navegador.
4. Confirme a exibição da página criada pelo script `UserData`.

## 8. Registro de Evidências

Capturas de tela sugeridas:

- Tela da stack criada.
- Eventos com `CREATE_COMPLETE`.
- Outputs da stack.
- Página web aberta no navegador.
- Exclusão da stack.

As imagens podem ser salvas na pasta:

```text
images/
```

## 9. Limpeza dos Recursos

Após finalizar os testes:

1. Selecione a stack no CloudFormation.
2. Clique em **Delete**.
3. Confirme a exclusão.
4. Aguarde o status de remoção.

Esse passo é importante para evitar cobranças desnecessárias.

## 10. Comandos Úteis com AWS CLI

Caso prefira usar a AWS CLI, os comandos abaixo podem ajudar.

Validar o template:

```bash
aws cloudformation validate-template --template-body file://templates/webserver-stack.yaml
```

Criar a stack:

```bash
aws cloudformation create-stack \
  --stack-name dio-primeira-stack-thassio \
  --template-body file://templates/webserver-stack.yaml \
  --parameters ParameterKey=InstanceType,ParameterValue=t2.micro ParameterKey=KeyName,ParameterValue=ChavesLinux ParameterKey=MyIP,ParameterValue=203.0.113.10/32
```

Consultar os eventos:

```bash
aws cloudformation describe-stack-events --stack-name dio-primeira-stack-thassio
```

Consultar os outputs:

```bash
aws cloudformation describe-stacks --stack-name dio-primeira-stack-thassio
```

Excluir a stack:

```bash
aws cloudformation delete-stack --stack-name dio-primeira-stack-thassio
```
