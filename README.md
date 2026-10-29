# Maestro Mobile QA Lab

Laboratório de testes automatizados para Android com Maestro. Os fluxos em YAML exercitam o aplicativo de Configurações e a criação de contatos no aplicativo da Samsung.

## Estrutura do projeto

```text
.maestro/
├── tst.yaml                       # MOB-001: abrir Configurações
├── tst-interacao.yaml             # MOB-002: navegar nas configurações
└── regression/
    └── criar-contato.yaml         # MOB-003: criar e validar um contato
```

## Pré-requisitos

- Maestro CLI instalado e disponível no terminal como `maestro`.
- Java compatível com a versão instalada do Maestro.
- Android SDK Platform-Tools instalado, com `adb` disponível no terminal.
- Dispositivo Android conectado, desbloqueado e com depuração USB habilitada.
- Autorização de depuração USB aceita no aparelho.
- Interface do Android em português, conforme os textos usados nos testes.
- Para criar contatos: aplicativo Samsung Contatos (`com.samsung.android.app.contacts`).

Os seletores refletem a interface Samsung usada neste laboratório. Outros fabricantes ou versões do aplicativo podem exigir ajustes nos IDs e textos.

## Verificar a conexão

No PowerShell:

```powershell
adb devices -l
```

O aparelho deve aparecer com o status `device`.

| Resultado | Ação |
| --- | --- |
| `unauthorized` | Desbloqueie o aparelho e aceite a autorização de depuração USB. |
| `offline` | Reconecte o cabo e verifique novamente. |
| Lista vazia | Confira o cabo, a depuração USB e o driver do dispositivo no Windows. |

Se necessário, reinicie o ADB:

```powershell
adb kill-server
adb start-server
adb devices -l
```

## Executar os testes

Execute os comandos na raiz deste projeto.

Abrir Configurações:

```powershell
maestro test .maestro/tst.yaml
```

Navegar até **Sobre o telefone** e validar **Informações do software**:

```powershell
maestro test .maestro/tst-interacao.yaml
```

Criar um contato:

```powershell
maestro test .maestro/regression/criar-contato.yaml
```

Executar os fluxos da pasta:

```powershell
maestro test .maestro
```

## Fluxo de criação de contato

O arquivo `criar-contato.yaml` contém comentários por linha e exemplos de variações dos comandos usados. O fluxo:

1. Abre o Samsung Contatos e inicia o cadastro.
2. Seleciona o armazenamento **Telefone**, quando solicitado.
3. Preenche o nome e o telefone definidos no YAML.
4. Oculta o teclado e verifica o telefone antes de salvar.
5. Salva e valida o nome e o telefone na tela seguinte.
6. Registra a captura `contato-criado`.

O teste cria um contato real no aparelho e não o remove ao final. Execuções repetidas podem gerar contatos duplicados.

Para mudar os dados, edite os valores de `inputText` e as validações correspondentes de `assertVisible`. A validação do telefone usa expressão regular; mantenha-a coerente com o número e sua formatação na tela.

## Evidências e diagnóstico

Ao falhar, o Maestro informa no terminal a etapa e o diretório dos artefatos. Consulte a captura de tela, a hierarquia da interface e os logs dessa execução para identificar o seletor correto.

Neste ambiente, execuções anteriores geraram artefatos em:

```text
C:\Users\Desktop\.maestro\tests\<data_hora>\
```

Use o caminho informado pela execução atual, pois a localização pode variar conforme a configuração.

Quando ocorrer `Element not found`, confira o ID e o texto na hierarquia. Na tela de cadastro, **Telefone** identifica tanto o armazenamento no topo quanto a linha de número abaixo do nome; o fluxo combina `id` e `text` para distinguir esses elementos.

## Referência

[Documentação oficial do Maestro](https://docs.maestro.dev/)
# maestro-mobile-qa-lab
