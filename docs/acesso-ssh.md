# Usando Par de Chaves e Acessando a EC2 por SSH

Este documento registra como usar um par de chaves existente no Amazon EC2 e associá-lo à stack CloudFormation para acesso remoto à instância Linux.

## 1. Par de Chaves Utilizado

Foi utilizado um par de chaves já existente no EC2:

```text
ChavesLinux
```

O arquivo `.pem` correspondente já estava salvo em local seguro.

Caso fosse necessário criar um novo par de chaves, o caminho seria:

1. Acessar o serviço **EC2**.
2. Abrir **Rede e segurança > Pares de chaves**.
3. Clicar em **Criar par de chaves**.
4. Informar um nome para a chave.
5. Selecionar tipo `RSA`.
6. Selecionar formato `.pem`.
7. Criar e guardar o arquivo baixado.

## 2. Guardar a Chave Privada

O arquivo `.pem` é a chave privada. Ele deve ser guardado com segurança.

Pontos importantes:

- Não envie o arquivo `.pem` para o GitHub.
- Não compartilhe a chave privada.
- Se perder o arquivo, a AWS não permite baixá-lo novamente pelo console.
- O arquivo deve ficar fora do repositório do projeto.

## 3. Atualizar a Stack com o Par de Chaves

No CloudFormation, ao criar ou atualizar a stack, informe:

| Parâmetro | Valor |
| --- | --- |
| `KeyName` | `ChavesLinux` |
| `MyIP` | Seu IP público com `/32` |

Exemplo de `MyIP`:

```text
201.87.254.60/32
```

O template atualizado associa o par de chaves à EC2 e libera a porta 22 no Security Group apenas para o IP definido em `MyIP`.

## 4. Atenção ao Atualizar uma Stack Existente

Como a instância atual foi criada sem chave SSH, adicionar `KeyName` pode exigir substituição da instância EC2 durante a atualização da stack. Caso isso aconteça, o IP público e a URL pública da instância podem mudar.

Antes de atualizar, revise o conjunto de alterações no CloudFormation.

## 5. Conectar por SSH

No Windows, usando PowerShell com OpenSSH instalado, acesse a pasta onde a chave foi salva e execute:

```powershell
ssh -i .\ChavesLinux.pem ec2-user@ec2-3-87-82-229.compute-1.amazonaws.com
```

Substitua `ChavesLinux.pem` pelo nome exato do arquivo salvo no seu computador, caso ele esteja diferente. Substitua também o endereço DNS pelo valor atual exibido no output `SSHCommand` ou `WebsiteURL` da stack.

## 6. Usuário Padrão

Para Amazon Linux, o usuário padrão é:

```text
ec2-user
```

## 7. Troubleshooting

Se a conexão falhar, verifique:

- A instância está em estado `running`.
- O Security Group possui regra de entrada para a porta 22.
- A regra da porta 22 usa o seu IP público com `/32`.
- O arquivo `.pem` usado é o mesmo par de chaves associado à instância.
- O DNS público da instância está correto.
- Sua rede local permite conexões de saída pela porta 22.

## 8. Comandos de Validação

Após conectar na instância via SSH, os comandos abaixo ajudam a comprovar o funcionamento do ambiente:

```bash
whoami
hostname
pwd
cat /etc/os-release
sudo systemctl status httpd
httpd -v
sudo ss -tulpn | grep :80
cat /var/www/html/index.html
curl http://localhost
sudo tail -n 20 /var/log/httpd/access_log
sudo systemctl is-active httpd
sudo systemctl is-enabled httpd
```

Esses comandos validam o usuário conectado, o sistema operacional, o serviço Apache, a porta 80, o conteúdo publicado e os logs de acesso.

## 9. Erro ao Conectar pelo Console

O erro abaixo pode aparecer ao tentar conectar pelo botão **Conectar** do console EC2:

```text
Failed to connect to your instance
Error establishing SSH connection to your instance. Try again later.
```

Possíveis causas:

- A instância foi criada antes de associar o parâmetro `KeyName`.
- A stack ainda não foi recriada ou atualizada com o template que contém `KeyName`.
- A porta 22 não está liberada no Security Group.
- O parâmetro `MyIP` está diferente do IP público atual da sua rede.
- O DNS público mudou após recriar a instância.
- A tentativa foi feita via EC2 Instance Connect no navegador, mas o acesso esperado neste laboratório é pelo cliente SSH usando o arquivo `.pem`.

Para este laboratório, o teste recomendado é pelo PowerShell:

```powershell
ssh -i .\ChavesLinux.pem ec2-user@DNS_PUBLICO_DA_EC2
```

Se preferir usar o botão **Conectar** do console, confira se a aba selecionada é compatível com o método escolhido. Para conexão SSH tradicional, use a aba **Cliente SSH** e siga o comando informado pela AWS.

