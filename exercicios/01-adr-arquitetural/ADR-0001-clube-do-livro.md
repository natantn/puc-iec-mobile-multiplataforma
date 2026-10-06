# ADR-0001: Stack mobile para rede social de clubes de leitura

## Status

`Aceito`

**Data:** 2026-10-05
**Autor:** Natan Figueiredo

## Contexto

- **Produto:** MVP de uma rede social literária — o usuário descobre e se inscreve em vários clubes de leitura, participa de discussões, acompanha um feed e marca presença em encontros
  - **Clubes gratuitos:** moderados por IA, comunidade ou por indicação (generalistas ou temáticos)
  - **Clubes patrocinados:** curadoria, exemplares de lançamento, encontros com autores
- **Escala alvo:** MVP pequeno (~50 clubes, ~2 mil leitores), mas precisa crescer para muito mais clubes e usuários sem reescrever o app
- **Time:** 2 devs experientes em mobile nativo (Kotlin/Android e Swift/iOS)
- **Prazo:** 4 meses até o MVP nas duas lojas
- **Requisitos críticos:** app com **muitas telas** (feed, perfil, clube, discussão, agenda), push de interações e encontros, convites por link (crescimento orgânico), e **leve** — precisa rodar bem em Android de entrada
- **Fora do escopo deste ADR:** backend e moderação por IA (o app consome APIs; a escala de dados fica no servidor)

## Decisão

Adotaremos **Kotlin Multiplatform com Compose Multiplatform**: lógica e UI compartilhadas em Kotlin, com código nativo (SwiftUI/Android) só onde a plataforma exigir.

## Alternativas consideradas

| Alternativa | Prós | Contras |
|---|---|---|
| **KMP + Compose Multiplatform** (escolhida) | 1 codebase para lógica e UI, essencial num app de muitas telas; Kotlin e Compose já são a base do dev Android; dá para usar código nativo pontualmente (push, deep links, compartilhamento) | Dev iOS precisa aprender Compose; Compose no iOS é mais recente que no Android e tem menos bibliotecas prontas; app iOS fica um pouco maior (renderizador próprio) |
| Nativo (Kotlin + Swift) | Melhor UX e performance; time já domina | Todas as telas feitas 2 vezes: inviável em 4 meses com 2 devs; cada dev vira o único dono de uma plataforma (bus factor 1) |
| Flutter | 1 codebase; UI performática e consistente; ecossistema social maduro (Firebase, chat) | Time recomeça do zero em Dart; perde a experiência nativa justo quando o prazo é curto; integração nativa via plugins/platform channels |
| PWA | Link abre direto no navegador (ótimo para convites); sem loja; deploy imediato | Push no iOS só com o app instalado na tela inicial; experiência e presença em loja inferiores para uma rede social de uso diário |

## Consequências

**Positivas:**
- As telas são feitas uma vez → o MVP cabe em 4 meses e os 2 devs trabalham no mesmo código (sem bus factor 1)
- Kotlin aproveita a base do time; UI nativa pode ser usada onde a experiência pedir
- Escala por módulos (feed, clubes, eventos, patrocínio) → novas features sem reescrita

**Negativas:**
- Curva de aprendizado do dev iOS nas primeiras semanas
- Risco de faltar biblioteca pronta para Compose no iOS → parte das integrações (push, compartilhamento, pagamento futuro dos patrocinados) vai em código nativo
- Se a UX no iOS ficar abaixo do esperado, telas críticas podem precisar ser refeitas em SwiftUI — revisar esta decisão após o MVP

## Referências

- JetBrains — *Kotlin Multiplatform* (doc oficial). https://kotlinlang.org/docs/multiplatform.html
- JetBrains — *Compose Multiplatform* (doc oficial). https://www.jetbrains.com/compose-multiplatform/
- Netflix Technology Blog (2020). *Netflix Android and iOS Studio Apps — now powered by Kotlin Multiplatform*. https://netflixtechblog.com/netflix-android-and-ios-studio-apps-kotlin-multiplatform-d6d4d8d25d23
- Charland, A.; Leroux, B. (2011). *Mobile Application Development: Web vs. Native*. Communications of the ACM, 54(5).
- Nygard, M. (2011). *Documenting Architecture Decisions* — formato ADR. https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions
