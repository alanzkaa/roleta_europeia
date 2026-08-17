# Roleta Europeia — Grand Casino

Simulação de roleta europeia (single-zero) totalmente funcional, rodando em um único arquivo HTML — sem backend próprio, sem build e sem frameworks. O layout usa uma estética de cassino clássico (feltro verde, dourado, tipografia serifada) e persiste saldo, apostas e histórico de jogadas em um banco de dados na nuvem.

> **Simulação educativa.** Não envolve dinheiro real. O objetivo é demonstrar, na prática, como a vantagem da casa ("house edge") é embutida nos pagamentos das apostas, não no sorteio.

## Contexto acadêmico

Este projeto foi desenvolvido como **Projeto Integrador em grupo** do curso **Jovem Programador**, oferecido pelo **SENAC**.

## Preview

<img width="1919" height="905" alt="image" src="https://github.com/user-attachments/assets/61e5f280-667d-45d3-a812-6f3b474ff459" />

## Usuários de teste

O login é feito apenas pelo **nome de usuário** (sem senha) — o sistema consulta o banco de dados e carrega o saldo salvo daquele usuário. Para testar a aplicação, use um dos usuários já cadastrados:

| Usuário     |
|-------------|
| `um_111`    |
| `dois_222`  |
| `tres_333`  |
| `quatro_444`|
| `cinco_555` |

## Funcionalidades

- **Mesa de apostas completa**: números individuais (0–36), dúzias, vermelho/preto, par/ímpar e baixo/alto (1–18 / 19–36).
- **Roleta animada em Canvas**, com giro realista, destaque dourado nos números cobertos pela aposta atual e sons gerados via Web Audio API (giro, cliques, vitória e derrota).
- **Apostas somente em valores inteiros** — o campo bloqueia vírgula/ponto e exibe aviso caso o usuário tente digitar centavos.
- Botões de **Dobrar (×2)** e **Repetir última aposta**.
- **Painel de estatísticas e histórico** (gaveta lateral): total apostado, ganho, perdido, lucro, rodadas jogadas e taxa de acerto, além das últimas 30 rodadas.
- **Painel educativo** exibido após cada giro, explicando o resultado e reforçando que a vantagem da casa (~3%) está nos pagamentos, não no sorteio (que é honesto: 1/37 de chance por número).
- **Alerta automático** quando o jogador perde mais de 50% do saldo inicial.
- **Tela de Game Over** com resumo da sessão quando o saldo chega a zero, com opção de reiniciar.
- **Login/Logout por usuário**, com saldo, histórico e estatísticas persistidos no banco — o logout encerra apenas a sessão local, sem apagar dados.
- **Painel administrativo oculto** (atalho `Ctrl+Shift+A` ou `admin()` no console do navegador) para ajustar manualmente o saldo de qualquer usuário direto no banco.

## Regras e pagamentos

O sorteio é sempre justo (1 em 37 para cada número). A vantagem da casa é aplicada **reduzindo o pagamento** em relação ao valor matematicamente justo:

| Tipo de aposta          | Pagamento justo | Pagamento no jogo (com margem) |
|--------------------------|:---------------:|:-------------------------------:|
| Número individual         | 35:1             | 33.95:1                        |
| Dúzia (1–12, 13–24, 25–36)| 2:1              | 1.94:1                         |
| Cor / Par-Ímpar / Baixo-Alto | 1:1           | 0.97:1                         |

O valor mínimo de aposta é R$ 1,00 e todas as apostas devem ser números inteiros.

## Tecnologias

- **HTML5 Canvas** para o desenho e a animação da roleta
- **JavaScript puro (ES Modules)**, sem frameworks
- **CSS** com variáveis customizadas (design tokens) para o tema visual
- **Web Audio API** para os efeitos sonoros
- **[Turso](https://turso.tech/) (libSQL)** como banco de dados serverless compatível com SQLite, acessado diretamente do navegador via `@libsql/client` (import por CDN/`esm.sh`, conexão WebSocket — sem necessidade de backend ou CORS)

### Tabelas do banco de dados

| Tabela             | Finalidade                                              |
|---------------------|----------------------------------------------------------|
| `tb_usuario`         | Cadastro dos usuários (`nome_usuario`, `nome`, `saldo`) |
| `tb_movimentacoes`   | Registro de cada movimentação financeira do usuário     |
| `tb_jogada`          | Registro de cada jogada, vinculada a uma movimentação    |
| `tb_jogos`           | Saldo agregado acumulado pela "casa" no jogo de roleta   |

## Como executar

Por ser um projeto client-side (front-end puro), basta abrir o arquivo `index.html` diretamente no navegador ou servi-lo por qualquer servidor estático simples, por exemplo:

```bash
python -m http.server 8000
```

e acessar `http://localhost:8000`.

## Observação de segurança

As credenciais de acesso ao banco Turso estão embutidas diretamente no código do front-end. Isso é aceitável para fins didáticos/demonstração, mas **não é uma prática recomendada em produção**, já que qualquer pessoa pode visualizá-las inspecionando o código-fonte da página.
