## Passo 5: Criando uma Agent Skill

Você já personalizou o Copilot com instruções, prompts e um agente especializado. Agora, quer reutilizar um conhecimento pedagógico específico sem repeti-lo em cada prompt ou limitá-lo a um único agente.

Para isso, você criará uma Agent Skill que avalia a qualidade de enunciados antes que novas tarefas sejam publicadas para os alunos da Mergington High School.

### 📖 Teoria: Agent Skills

Agent Skills são pastas com instruções, referências, scripts e outros recursos que o Copilot carrega sob demanda para executar tarefas especializadas. Diferentemente das instruções, que definem regras gerais ou condicionais, uma Skill representa uma capacidade reutilizável. Diferentemente de um agente personalizado, ela não troca a personalidade nem as ferramentas da conversa atual.

Cada Skill contém um arquivo chamado `SKILL.md`. O frontmatter desse arquivo informa seu nome e descreve quando ela deve ser usada. O Copilot usa principalmente a `description` para descobrir Skills relevantes e carrega os demais recursos somente quando necessário.

O Visual Studio Code procura Skills do projeto em `.github/skills/<nome-da-skill>/SKILL.md`. O valor de `name` deve ser idêntico ao nome da pasta e usar apenas letras minúsculas, números e hifens.

> [!TIP]
> Saiba mais sobre Agent Skills:
>
> - [VS Code Docs: Agent Skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
> - [Agent Skills: especificação aberta](https://agentskills.io/)

### ⌨️ Atividade: Criar uma Skill para Avaliar Tarefas

Agora vamos criar uma Skill que usa uma rubrica para avaliar se o enunciado de uma tarefa está pronto para publicação.

1. Crie uma pasta chamada:

   ```text
   .github/skills/assignment-rubric/
   ```

1. Dentro dela, crie um arquivo chamado:

   ```text
   .github/skills/assignment-rubric/SKILL.md
   ```

   > ❕ **Importante:** O arquivo deve se chamar exatamente `SKILL.md`, com letras maiúsculas, e o campo `name` deve corresponder ao nome da pasta.

1. Adicione o seguinte conteúdo ao arquivo `SKILL.md`:

   ```markdown
   ---
   name: assignment-rubric
   description: Avalia enunciados de tarefas escolares quanto à clareza, aos requisitos verificáveis e à dificuldade. Use ao revisar uma tarefa antes da publicação.
   argument-hint: "[enunciado da tarefa]"
   ---

   # Avaliação de Tarefas

   Avalie o enunciado fornecido usando os critérios e pesos definidos em [rubric.md](./rubric.md).

   ## Procedimento

   1. Leia o enunciado completo sem alterá-lo.
   1. Atribua uma pontuação para cada critério da rubrica.
   1. Some as pontuações e compare o total com o limiar de publicação.
   1. Identifique informações ausentes ou ambíguas.
   1. Sugira exatamente três melhorias objetivas.

   ## Formato da Resposta

   Apresente:

   - A pontuação de cada critério e a pontuação total
   - As lacunas encontradas
   - A decisão `Pronto para publicar` ou `Precisa de revisão`
   - Exatamente três melhorias objetivas
   ```

1. Na mesma pasta, crie o arquivo:

   ```text
   .github/skills/assignment-rubric/rubric.md
   ```

1. Adicione a seguinte rubrica:

   ```markdown
   # Rubrica para Enunciados de Tarefas

   Avalie cada critério até o peso máximo indicado:

   | Critério | Peso máximo |
   | --- | ---: |
   | Objetivo de aprendizagem claro | 30 |
   | Entregável esperado definido | 25 |
   | Requisitos verificáveis | 25 |
   | Dificuldade adequada ao público | 20 |

   A tarefa está pronta para publicação quando atingir pelo menos 80 pontos e não obtiver zero em nenhum critério.
   ```

### ⌨️ Atividade: Testar a Agent Skill

1. Abra o Copilot Chat no VS Code e certifique-se de que está no modo **Agent**.

1. Comece uma nova conversa e envie o pedido abaixo. Ele não menciona o nome da Skill, permitindo observar se o Copilot a descobre pela descrição.

   > ![Static Badge](https://img.shields.io/badge/-Prompt-text?style=social&logo=github%20copilot)
   >
   > ```prompt
   > Avalie este enunciado antes da publicação: "Crie um programa Python sobre notas. Torne-o útil e inclua um relatório."
   > ```

1. Confira as referências da resposta para verificar se `assignment-rubric` e `rubric.md` foram utilizados. A resposta deve apresentar pontuação, lacunas, uma decisão e exatamente três melhorias.

1. Teste também a invocação manual. Digite `/assignment-rubric` no Chat e forneça o mesmo enunciado.

1. Faça o commit e envie (push) suas alterações para estes arquivos:

   ```text
   .github/skills/assignment-rubric/SKILL.md
   .github/skills/assignment-rubric/rubric.md
   ```

1. Aguarde a Mona verificar seu trabalho e publicar a revisão final!

<details>
<summary>Está com problemas? 🤷</summary><br/>

- Certifique-se de que o arquivo está em `.github/skills/assignment-rubric/SKILL.md` e usa exatamente esse nome, incluindo maiúsculas e minúsculas.
- Verifique se `name: assignment-rubric` corresponde ao nome da pasta.
- Confirme que o link para a rubrica usa o caminho relativo `./rubric.md`.
- Se a Skill não aparecer ao digitar `/`, recarregue a janela do VS Code.
- Execute **Chat: Open Customizations** para confirmar que a Skill foi descoberta. Consulte os diagnósticos do Chat para identificar erros no frontmatter.
- A descoberta automática pode variar conforme o modelo. A invocação `/assignment-rubric` permite testar a Skill diretamente.

</details>