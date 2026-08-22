# 📋 PRD — Product Requirements Document

## Sarei e Doei

**Versão:** 1.3  
**Autor:** Lucas Aurélio  
**Data:** Abril 2026

---

## 1. Visão do Produto

O **Sarei e Doei** é uma plataforma web solidária que conecta pessoas que possuem equipamentos médicos de apoio disponíveis (muletas, cadeiras de rodas, andadores, camas hospitalares) com pessoas que precisam deles temporariamente ou definitivamente.

O problema que resolve: o empréstimo e doação desse tipo de equipamento acontece hoje de forma totalmente manual — via WhatsApp, ONGs presenciais e formulários físicos. Não existe uma plataforma digital centralizada, aberta e acessível para isso.

**Público-alvo:** enfermos em recuperação, com mobilidade reduzida temporária, familiares de pacientes, e qualquer pessoa que tenha equipamentos médicos parados em casa e queira destiná-los a quem precisa.

---

## 2. Escopo do MVP

O MVP contempla as funcionalidades essenciais de cadastro, navegação, fila, chat e transferência de equipamentos. Funcionalidades planejadas para versões futuras incluem:

- **Verificação por SMS** via Twilio
- **Login social** via Google OAuth
- **Atributos dinâmicos** por tipo de equipamento
- **Notificações push** e por e-mail
- **Geolocalização** automática
- **Criptografia ponta a ponta real** (E2E) no chat

---

## 3. Atores

| Ator | Descrição |
|---|---|
| **Visitante** | Acessa o feed e detalhe dos itens sem estar logado |
| **Usuário** | Cadastrado na plataforma. Pode navegar, buscar, entrar em filas e cadastrar itens |
| **Doador** | Papel assumido pelo Usuário ao cadastrar um item |
| **Admin** | Gerencia tipos de equipamentos, reativa itens indisponíveis e acessa dados completos |
| **Sistema** | Gerencia automaticamente filas, expirações, congelamentos e progressões |

---

## 4. Casos de Uso Narrativos

### 🙋 Alice — A Receptora

Alice se acidentou e precisa de uma muleta. Ela acessa o **Sarei e Doei** sem precisar criar conta e navega pelo feed de itens disponíveis. Ela pesquisa por "muleta" e encontra o item cadastrado por Bob. Clica no card, vê a foto do item em tela cheia, o estado de conservação, a descrição, o endereço de retirada, os horários disponíveis e quantas pessoas estão na fila.

Alice decide solicitar o item. Como ainda não tem conta, o sistema pede que ela se cadastre. Após o cadastro e login, ela clica em **"Solicitar"** e entra na fila. O sistema exibe sua posição atual.

Quando chega a vez de Alice, o sistema a notifica e abre automaticamente um **canal de chat** entre ela e Bob. Alice tem **1 semana** para buscar o item. Pelo chat, eles combinam o horário de retirada conforme os horários cadastrados por Bob. Alice vai até o endereço, pega a muleta e marca no app que **retirou o item**. Bob também confirma que **entregou**. Com ambas as confirmações, o item passa para o status **"em uso"** e o chat é encerrado automaticamente.

---

### 🎁 Bob — O Doador

Bob sarou de uma cirurgia no joelho e tem uma muleta parada em casa. Ele acessa o app, faz login e clica em **"Cadastrar item"**. Seleciona o tipo "Muleta" no select, adiciona fotos, escolhe o estado de conservação, adiciona uma descrição opcional e confirma o endereço de retirada. Bob também cadastra seus horários disponíveis para retirada: segunda e quarta das 18h às 20h.

O item é publicado e aparece no feed. Bob recebe uma notificação quando alguém entra na fila. Quando Alice é notificada, o chat é aberto. Bob combina o horário pelo chat, entrega a muleta e confirma no app que o item foi retirado. O item passa para "em uso".

Semanas depois, Bob tem uma cadeira de rodas para doar. Ele clica em **"Reutilizar item"**, seleciona a cadeira de rodas de um cadastro anterior e apenas atualiza o endereço. Um novo item é publicado independentemente, com seu próprio feed de fila e chat.

---

### ⏳ Carlos — O Próximo da Fila

Carlos também precisava de uma muleta e entrou na fila logo após Alice. O sistema exibe que ele está em **2º lugar**. Carlos aguarda. Quando Alice confirma que retirou o item e Bob confirma a entrega, o item passa para "em uso". A fila de Carlos fica ativa para quando o item for devolvido ou um novo item do mesmo tipo aparecer.

Se Alice não buscar o item em 1 semana após ser notificada, o sistema remove Alice da fila automaticamente, notifica Bob e passa Carlos para o **1º lugar**, abrindo o chat entre Carlos e Bob.

---

### 🔒 Dana — Item Indisponível

Dana tinha cadastrado um andador, mas ele foi extraviado durante uma mudança. Ela acessa o app e marca o item como **indisponível**, informando o motivo: "Andador extraviado durante mudança de residência". O item desaparece imediatamente do feed para todos os usuários. A fila existente fica **congelada** — ninguém perde sua posição.

O admin recebe o alerta, entra em contato com Dana e tenta resolver a situação. Quando Dana recupera ou conserta o item, o admin reativa o item. A fila é descongelada e o primeiro da fila é notificado para buscar o andador.

---

### 🛡️ Admin — Gestão da Plataforma

O admin acessa o painel e vê todos os itens, incluindo os indisponíveis. Ele pode visualizar o histórico completo de qualquer item com nomes reais e fotos dos usuários. Também gerencia os tipos de equipamentos disponíveis no select — adicionando, editando ou removendo tipos conforme necessário. O admin pode reativar itens indisponíveis após verificar que o problema foi resolvido.

---

### ⚙️ Sistema — Gestão Automática de Filas

A cada hora, o sistema verifica as entradas de fila com status "notificado" e prazo expirado. Para cada entrada expirada, o sistema remove o usuário da fila, avança para o próximo participante, abre um novo chat entre o próximo e o doador e notifica ambos. Entradas congeladas (item indisponível) são ignoradas pelo processo de expiração.

---

## 5. User Stories

### Autenticação e Perfil

- **US01** — Como visitante, quero me cadastrar com nome, e-mail, senha, telefone e endereço padrão.
- **US02** — Como usuário, quero fazer login com e-mail e senha.
- **US03** — Como usuário, quero editar meu perfil e endereço padrão.

### Navegação

- **US04** — Como visitante, quero visualizar o feed de itens disponíveis e em uso em layout de cards lado a lado, sem precisar estar logado.
- **US05** — Como visitante, quero pesquisar itens por nome ou tipo.
- **US06** — Como visitante, quero clicar em um item e ver seus detalhes: foto grande à esquerda e informações à direita.
- **US07** — Como visitante, quero ver o histórico público de um item com iniciais e foto anônima.

### Fila

- **US08** — Como usuário logado, quero solicitar um item clicando em "Solicitar" para entrar na fila.
- **US09** — Como usuário, quero ver quantas pessoas estão na fila de um item, sem saber quem são.
- **US10** — Como usuário notificado, quero ter 1 semana para buscar o item antes de ser removido da fila automaticamente.
- **US11** — Como sistema, quero passar automaticamente para o próximo da fila quando o prazo expirar.

### Chat

- **US12** — Como usuário notificado, quero ter acesso a um chat privado com o doador para combinar a retirada.
- **US13** — Como usuário, quero que o chat seja encerrado automaticamente após ambas as partes confirmarem a retirada.
- **US14** — Como usuário, quero que cada item tenha seu próprio canal de chat, sem misturar conversas.

### Doação

- **US15** — Como usuário, quero cadastrar um item escolhendo o tipo, adicionando fotos opcionais, estado de conservação e descrição opcional.
- **US16** — Como usuário, quero cadastrar horários disponíveis para retirada do item.
- **US17** — Como usuário, quero reutilizar um item que já cadastrei anteriormente.
- **US18** — Como doador, quero confirmar que entreguei o item ao solicitante.
- **US19** — Como receptor, quero confirmar que recebi o item para fechar o ciclo de transferência.

### Indisponibilidade

- **US20** — Como doador, quero marcar um item como indisponível informando o motivo.
- **US21** — Como admin, quero ver todos os itens indisponíveis e seus motivos.
- **US22** — Como admin, quero reativar um item indisponível quando o problema for resolvido.

### Administração

- **US23** — Como admin, quero cadastrar e gerenciar os tipos de equipamentos disponíveis.
- **US24** — Como admin, quero ver o histórico completo de um item com dados reais dos usuários.
- **US25** — Como admin, quero gerenciar usuários da plataforma.

---

## 6. Regras de Negócio

- **RN01** — Um usuário só pode cadastrar um item se tiver endereço no perfil ou informar um endereço específico para o item.
- **RN02** — Os tipos de equipamento são pré-definidos pelo admin. Usuários não podem criar tipos novos.
- **RN03** — Se o usuário não adicionar fotos ao item, o sistema exibe a imagem padrão do tipo de equipamento.
- **RN04** — A fila é absoluta e anônima. Apenas a quantidade é visível publicamente.
- **RN05** — O primeiro da fila tem 1 semana para buscar o item após ser notificado. Se não buscar, o sistema avança automaticamente.
- **RN06** — A transferência só é concluída quando doador e receptor confirmarem.
- **RN07** — Um usuário não pode solicitar um item que ele mesmo cadastrou.
- **RN08** — O histórico público exibe apenas iniciais e foto anônima. O admin vê dados completos via rota separada.
- **RN09** — O contador de usos é calculado automaticamente pelas transferências confirmadas.
- **RN10** — O doador deve cadastrar ao menos um horário de retirada ao publicar um item.
- **RN11** — A descrição do item é sempre opcional.
- **RN12** — Itens indisponíveis não aparecem no feed para nenhum usuário comum ou visitante. Apenas admins os visualizam.
- **RN13** — Ao marcar indisponível, a fila é congelada e nenhum participante perde sua posição.
- **RN14** — Apenas o admin pode reativar um item indisponível.
- **RN15** — O chat é aberto automaticamente quando o usuário é notificado que é sua vez na fila.
- **RN16** — O chat é encerrado automaticamente quando ambas as partes confirmam a retirada.
- **RN17** — Cada item tem seu próprio canal de chat por solicitação. Chats de itens diferentes nunca se misturam.
- **RN18** — Mensagens do chat são criptografadas em repouso no banco de dados.

---

## 7. Status dos Itens

| Status | Visível para visitante/usuário | Visível para admin | Fila |
|---|---|---|---|
| `available` | ✅ Sim | ✅ Sim | Ativa |
| `in_use` | ✅ Sim | ✅ Sim | Ativa |
| `unavailable` | ❌ Não | ✅ Sim | Congelada |

---

## 8. Entidades Principais

| Entidade | Descrição |
|---|---|
| `User` | Usuário da plataforma |
| `AssetType` | Tipos de equipamento pré-definidos pelo admin |
| `MedicalAsset` | Equipamento cadastrado por um usuário |
| `PickupSchedule` | Horários disponíveis para retirada |
| `QueueEntry` | Entrada de um usuário na fila |
| `AssetHandover` | Registro de transferência entre usuários |
| `ChatRoom` | Canal de chat entre doador e receptor por item/solicitação |
| `ChatMessage` | Mensagem individual dentro de um canal de chat |

---

## 9. Fora do Escopo (MVP)

- Verificação por SMS (Twilio)
- Login social via Google OAuth
- Atributos dinâmicos por tipo de equipamento
- Notificações push ou por e-mail
- Geolocalização automática
- Avaliação ou reputação de usuários
- Criptografia ponta a ponta real (E2E) no chat
- Itens descartáveis ou não hospitalares
- Pagamento ou caução
