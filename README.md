# Desafio DIO: Implementando Minha Primeira Stack com AWS CloudFormation

Este repositório documenta a implementação prática de uma infraestrutura básica na AWS utilizando o **AWS CloudFormation**. O objetivo foi consolidar o conhecimento em **Infraestrutura como Código (IaC)**, criando e gerenciando um conjunto de recursos (*Stack*) de forma declarativa e automatizada.

Desenvolvido como parte dos estudos e desafios do **Bootcamp Santander Code Girls | DIO**.

---

## Princípios de Infraestrutura como Código (IaC)

O CloudFormation atua como o **motor de orquestração declarativa** da AWS. Em vez de provisionar recursos manualmente pelo console, a infraestrutura é definida em um arquivo de *template* (YAML ou JSON), garantindo que os ambientes sejam **repetíveis, auditáveis e seguros**.

| Termo Chave | Função no CloudFormation | Benefício Prático |
| :--- | :--- | :--- |
| **Template** | O arquivo YAML/JSON com a descrição completa do ambiente. | Garante a **padronização** e elimina erros de configuração manual. |
| **Stack** | O agrupamento lógico de todos os recursos (EC2, S3, etc.) criados pelo *template*. | Permite o gerenciamento e a **exclusão segura** de todos os recursos com uma única ação. |
| **Rollback** | Mecanismo automático de reversão. | Se a criação ou atualização falhar, o CloudFormation restaura o ambiente ao último estado estável. |

---

## A Stack Implementada: Criação de Ambiente Básico

O template principal deste desafio (`templates/infrastructure-stack.yaml`) provisionou recursos fundamentais, demonstrando o conceito de dependência e orquestração:

### Recursos Provisionados
* **Computação (EC2):** Uma instância base (t2.micro), servindo como servidor de aplicação.
* **Rede (SecurityGroup):** Regras de firewall para permitir acesso **SSH (Porta 22)** à instância EC2.
* **Armazenamento (S3):** Um bucket simples para armazenamento de objetos.
* **Identidade (IAM):** Criação de um `IAMUser` e `IAMGroup` para gestão de acesso.

---

## Funcionalidades Avançadas e Aprendizados do Template

O foco do laboratório foi utilizar as seções e funções avançadas do CloudFormation para criar um template flexível e robusto:

| Seção do Template | Função Técnica Aprendida | Aplicação e Importância |
| :--- | :--- | :--- |
| `Parameters` | Variáveis de entrada (ex: `InstanceType`). | Torna o template **reutilizável**; o tipo de instância pode ser alterado no momento do *deployment*. |
| `Mappings` | Tabelas de consulta para valores dinâmicos (ex: `UbuntuMap`). | Garante a **compatibilidade entre Regiões**, buscando o ID correto da AMI (Imagem de Máquina Amazon) para cada localidade. |
| `!Ref` (Referência) | Função para conectar a saída de um recurso à entrada de outro (ex: o ID do `SecurityGroup` à `EC2`). | **Gerencia dependências** implicitamente, garantindo a ordem correta de criação dos recursos. |
| `UserData` | Campo combinado com `Fn::Base64` para scripts de inicialização. | Permite a **automação de configuração** no primeiro boot da instância, como a instalação de pacotes (e.g., Apache, Python). |

### Exemplo Complementar: Servidor Web Rápido

O template auxiliar (`webserver-apache.yaml`) ilustrou a capacidade do CloudFormation de entregar um ambiente funcional. Ele orquestrou:
1. Criação do *Security Group* para acesso HTTP (80).
2. Criação da Instância EC2, referenciando o *Security Group*.
3. Execução de um script de `UserData` para **instalar e iniciar o Apache Web Server**, tudo em uma única transação de Stack.
